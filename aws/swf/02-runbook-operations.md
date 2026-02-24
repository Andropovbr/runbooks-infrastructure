# Runbook — AWS SWF Operations

This runbook is written for **operators** who need to inspect, diagnose, and safely intervene in SWF workflows.

---

## A) Daily / Common Procedures

### A1) Check “is SWF alive for this account/region?”
1. Confirm caller + region
```bash
aws sts get-caller-identity
aws configure get region
```
2. List domains
```bash
aws swf list-domains --registration-status REGISTERED
```

**If this fails:** likely IAM, region mismatch, or networking egress restrictions.

---

### A2) Find workflow executions for a domain (open + closed)

> Visibility calls require a **time window**.

**Open workflows (last 24h):**
```bash
aws swf list-open-workflow-executions \
  --domain my-domain \
  --start-time-filter oldestDate="$(date -u -d '1 day ago' +%Y-%m-%dT%H:%M:%SZ)"
```

**Closed workflows (last 7 days):**
```bash
aws swf list-closed-workflow-executions \
  --domain my-domain \
  --start-time-filter oldestDate="$(date -u -d '7 day ago' +%Y-%m-%dT%H:%M:%SZ)"
```

---

### A3) Drill into one workflow execution (fast path)

1. Get `runId` from list output.
2. Describe execution:
```bash
aws swf describe-workflow-execution \
  --domain my-domain \
  --execution workflowId="order-123",runId="RUN_ID"
```
3. Pull history (reverse is easier to read):
```bash
aws swf get-workflow-execution-history \
  --domain my-domain \
  --execution workflowId="order-123",runId="RUN_ID" \
  --reverse-order
```

**What you’re looking for:**
- Latest event: scheduled activity? activity timed out? decision timed out?
- Signals received?
- Cancel requested?
- Child workflows?

---

### A4) Check “are workers/deciders polling the right task lists?”

From your workflow/activity registration, identify task lists:
- `deciders` task list (DecisionTask)
- `workers` task list (ActivityTask)

Then confirm your services/processes (ECS, EC2, on-prem) are configured to poll **the same exact names**.

Common symptom of mismatch:
- Workflows stay open.
- History shows activities scheduled but never started.
- Polling returns empty.

---

### A5) Safe intervention options (in increasing severity)

#### 1) Send a signal
Use when the workflow is waiting for external input (approval, callback, etc.)
```bash
aws swf signal-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123" \
  --signal-name "ManualOverride" \
  --input '{"action":"continue"}'
```

#### 2) Request cancel
Use when the workflow should stop but you want graceful handling (decider can react).
```bash
aws swf request-cancel-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123"
```

#### 3) Terminate execution
Use as a last resort (immediate stop).
```bash
aws swf terminate-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123" \
  --reason "manual termination"
```

---

## B) Registration / Lifecycle Management

### B1) Deprecate old workflow/activity types
Use when you want to prevent new scheduling/executions with old versions.

```bash
aws swf deprecate-workflow-type --domain my-domain --workflow-type name="my-workflow",version="0.9"
aws swf deprecate-activity-type  --domain my-domain --activity-type name="resize-image",version="0.9"
```

### B2) Deprecate a domain (rare)
After deprecating, you **can’t** start new executions or register new types; visibility still works. citeturn0search3

```bash
aws swf deprecate-domain --name my-domain
```

---

## C) Operational Guardrails

### C1) Logging
SWF stores **history events**. Your decider/worker code should also emit logs (e.g., CloudWatch Logs) including:
- domain
- workflowId + runId
- task list
- task token correlation (do not log raw token to untrusted sinks)

### C2) Backpressure / scaling
- If open workflows grow and activities aren’t starting:
  - scale workers
  - verify schedule-to-start timeout is reasonable
  - verify task list correctness

### C3) Networking
SWF is a public AWS API. If your workers/deciders run in private subnets, they must have **egress** (e.g., NAT) to call SWF APIs.

---

## D) Minimal IAM (high level)

You typically need:
- Domain/type admin (register/list/deprecate)
- Execution control (start/signal/cancel/terminate)
- Workers/deciders (poll + respond + history)

Prefer least-privilege policies and separate roles for:
- workflow admin
- decider
- worker
