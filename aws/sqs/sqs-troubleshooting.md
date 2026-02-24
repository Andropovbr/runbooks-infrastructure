# Amazon SQS --- Troubleshooting Runbook

Generated on: 2026-02-24

------------------------------------------------------------------------

# 1. Messages Being Processed Twice

### Possible Causes:

-   Visibility timeout too low
-   Consumer crashes before delete

### Fix:

-   Increase visibility timeout
-   Ensure delete-message is executed only after success

------------------------------------------------------------------------

# 2. Messages Stuck in Queue

### Check:

-   Consumer running?
-   IAM permissions correct?
-   Security groups allow outbound traffic?

### Command:

``` bash
aws sqs get-queue-attributes   --queue-url <QUEUE_URL>   --attribute-names ApproximateNumberOfMessages
```

------------------------------------------------------------------------

# 3. Messages Going to DLQ

### Check:

-   maxReceiveCount threshold
-   Application errors
-   Dependency failures (DB, API)

### Inspect DLQ:

``` bash
aws sqs receive-message   --queue-url <DLQ_URL>
```

------------------------------------------------------------------------

# 4. High Queue Backlog

### Causes:

-   Insufficient consumer capacity
-   Downstream bottleneck

### Solutions:

-   Scale ECS Service or EC2 Auto Scaling
-   Tune batch size
-   Optimize processing logic

------------------------------------------------------------------------

# 5. High Empty Receive Calls (Cost Issue)

### Cause:

Short polling

### Fix:

Enable long polling (10--20 seconds)

------------------------------------------------------------------------

# 6. FIFO Throughput Limitation

### Cause:

Single MessageGroupId

### Fix:

Use multiple MessageGroupId values
