# Troubleshooting — AWS SWF

This file focuses on practical failure modes you’ll see in real life and how to isolate root cause fast.

---

## 1) “Workflows are stuck open”

### Symptoms
- `list-open-workflow-executions` shows many open executions.
- History shows activities scheduled but never started, OR decisions not being made.

### Triage checklist
1) Confirm the workflow is still receiving decision tasks:
- Look for recent `DecisionTaskScheduled` and `DecisionTaskStarted` events.
2) Confirm decider is running and polling the right task list:
```bash
aws swf poll-for-decision-task --domain my-domain --task-list name="deciders" --identity "debug-decider"
```
3) Confirm workers are polling the right activity task list:
```bash
aws swf poll-for-activity-task --domain my-domain --task-list name="workers" --identity "debug-worker"
```

**Interpretation:**
- Poll returns “empty” repeatedly → either no tasks, wrong task list, or tasks timing out/never scheduled.
- If decision tasks are being produced but never completed → decider logic crash, IAM denial, or network egress issue.

---

## 2) “poll-for-activity-task returns empty”

### Expected behavior (not always an error)
`poll-for-activity-task` does a long poll; it may return empty if no task arrives within ~60 seconds. citeturn0search1

### If it’s unexpected
Check:
- Is the activity being scheduled? (history)
- Is the worker polling the correct `taskList`?
- Is the activity type deprecated?
- Are timeouts (ScheduleToStart) expiring before pickup?

---

## 3) “Activities time out”

### Common root causes
- Worker scale too low (queue grows)
- Task list mismatch
- Activity execution takes longer than `StartToClose`
- Missing heartbeats (for long tasks) triggers heartbeat timeout

### How to confirm
In history, look for:
- `ActivityTaskScheduled` → `ActivityTaskStartFailed` or `ActivityTaskTimedOut`
- `ActivityTaskStarted` without completion

### Fix options
- Increase worker concurrency / autoscale
- Increase timeouts (ScheduleToStart / StartToClose / Heartbeat)
- Add periodic heartbeats during long processing:
  - via SDK or `record-activity-task-heartbeat`

---

## 4) “Decisions time out / no progress”

### Root causes
- Decider not running
- Decider can’t reach SWF endpoint (private subnet without NAT)
- Decider IAM lacks `swf:PollForDecisionTask` or `swf:RespondDecisionTaskCompleted`
- Decider processes history too slowly (huge history)

### Quick debug
- Run a debug poll with an identity; if you get a DecisionTask, the service is producing tasks.
```bash
aws swf poll-for-decision-task --domain my-domain --task-list name="deciders" --identity "debug"
```

If you get a task:
- Inspect `events` and identify the last meaningful event.
- If tasks repeat without progress, your decider logic is likely not emitting the correct decisions.

---

## 5) “AccessDenied / UnauthorizedOperation”

### What it means
Your IAM principal lacks SWF permissions for the action.

### Common fixes
- Attach the right IAM policy to the role running the CLI/worker/decider
- Ensure policy scope includes the correct domain
- Verify you’re in the correct AWS account and region:
```bash
aws sts get-caller-identity
aws configure get region
```

---

## 6) “Domain / type is deprecated”

### Symptoms
- Starting new workflow executions fails
- Registering new types fails

### Confirm
```bash
aws swf list-domains --registration-status DEPRECATED
aws swf list-workflow-types --domain my-domain --registration-status DEPRECATED
aws swf list-activity-types --domain my-domain --registration-status DEPRECATED
```

### Fix
- Undeprecate if appropriate:
```bash
aws swf undeprecate-domain --name my-domain
aws swf undeprecate-workflow-type --domain my-domain --workflow-type name="my-workflow",version="1.0"
aws swf undeprecate-activity-type  --domain my-domain --activity-type name="resize-image",version="1.0"
```

---

## 7) “History is huge; debugging is painful”

### Symptoms
- Decider slow
- Getting history times out or returns partial pages

### Fix tactics
- Use `--reverse-order` to see latest events first
- Paginate history using `--next-page-token`
- Improve application-level logging (correlate workflowId/runId in logs)

---

## 8) “Throttling / Rate exceeded”

### Root cause
Too many API calls (polling too frequently without backoff, high parallelism, retries).

### Fix
- Add jittered backoff on retries
- Reduce poll concurrency
- Use long polling as intended (don’t tight-loop)

---

## 9) “It works on my laptop, fails in prod”

### Usual culprits
- Different region
- Missing network egress (private subnets)
- Role mismatch (least-privilege differences)
- Different task list names via config drift

### Fast comparison
Capture and diff:
- region, account
- domain name
- workflow/activity type name+version
- task list names
- IAM role policies
- VPC routing/egress
