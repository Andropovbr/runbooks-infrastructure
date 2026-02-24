# AWS SWF — CLI Cheatsheet (Common Commands)

> These examples use AWS CLI v2: `aws swf ...` citeturn0search7

## 0) Prereqs

```bash
aws sts get-caller-identity
aws configure list
aws configure get region
```

SWF is **regional**. Make sure you’re in the correct region (or pass `--region`).

---

## 1) Domains

### List domains
```bash
aws swf list-domains --registration-status REGISTERED
aws swf list-domains --registration-status DEPRECATED
```

### Register domain
```bash
aws swf register-domain \
  --name my-domain \
  --workflow-execution-retention-period-in-days 30
```

### Deprecate / undeprecate domain
```bash
aws swf deprecate-domain --name my-domain
aws swf undeprecate-domain --name my-domain
```

---

## 2) Register Types (Workflow + Activity)

### Register workflow type
```bash
aws swf register-workflow-type \
  --domain my-domain \
  --name my-workflow \
  --version "1.0" \
  --default-task-list name="deciders" \
  --default-execution-start-to-close-timeout "3600" \
  --default-task-start-to-close-timeout "60"
```

### Register activity type
```bash
aws swf register-activity-type \
  --domain my-domain \
  --name resize-image \
  --version "1.0" \
  --default-task-list name="workers" \
  --default-task-start-to-close-timeout "300" \
  --default-task-heartbeat-timeout "60" \
  --default-task-schedule-to-start-timeout "120"
```

### List types
```bash
aws swf list-workflow-types --domain my-domain --registration-status REGISTERED
aws swf list-activity-types --domain my-domain --registration-status REGISTERED
```

---

## 3) Start / Signal / Cancel / Terminate Executions

### Start workflow execution
```bash
aws swf start-workflow-execution \
  --domain my-domain \
  --workflow-type name="my-workflow",version="1.0" \
  --workflow-id "order-123" \
  --task-list name="deciders" \
  --input '{"orderId":"123"}'
```

### Signal a running workflow
```bash
aws swf signal-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123" \
  --signal-name "ApprovalReceived" \
  --input '{"approved":true}'
```

### Request cancel / terminate
```bash
aws swf request-cancel-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123"

aws swf terminate-workflow-execution \
  --domain my-domain \
  --workflow-id "order-123" \
  --reason "manual stop"
```

---

## 4) Visibility: List / Count / Describe

### List open executions (time window required)
```bash
aws swf list-open-workflow-executions \
  --domain my-domain \
  --start-time-filter oldestDate="$(date -u -d '1 day ago' +%Y-%m-%dT%H:%M:%SZ)"
```

### List closed executions
```bash
aws swf list-closed-workflow-executions \
  --domain my-domain \
  --start-time-filter oldestDate="$(date -u -d '7 day ago' +%Y-%m-%dT%H:%M:%SZ)"
```

### Count open executions
```bash
aws swf count-open-workflow-executions \
  --domain my-domain \
  --start-time-filter oldestDate="$(date -u -d '1 day ago' +%Y-%m-%dT%H:%M:%SZ)"
```

### Describe an execution
```bash
aws swf describe-workflow-execution \
  --domain my-domain \
  --execution workflowId="order-123",runId="RUN_ID_HERE"
```

### Get execution history (debug gold)
```bash
aws swf get-workflow-execution-history \
  --domain my-domain \
  --execution workflowId="order-123",runId="RUN_ID_HERE" \
  --reverse-order
```

---

## 5) Polling (Deciders and Workers)

### Decider long poll
```bash
aws swf poll-for-decision-task \
  --domain my-domain \
  --task-list name="deciders" \
  --identity "decider-1"
```

### Worker long poll
```bash
aws swf poll-for-activity-task \
  --domain my-domain \
  --task-list name="workers" \
  --identity "worker-1"
```

> `poll-for-activity-task` does long polling and may return an “empty task” if nothing arrives within ~60s. citeturn0search1

---

## 6) Responding to tasks (you usually do this via SDK, but CLI exists)

### Activity completion / failure
```bash
aws swf respond-activity-task-completed --task-token "$TOKEN" --result '{"ok":true}'
aws swf respond-activity-task-failed --task-token "$TOKEN" --reason "failed" --details "stacktrace..."
```

### Activity heartbeat
```bash
aws swf record-activity-task-heartbeat --task-token "$TOKEN" --details "50% done"
```

### Decision completion
```bash
aws swf respond-decision-task-completed \
  --task-token "$DTOKEN" \
  --decisions file://decisions.json
```
