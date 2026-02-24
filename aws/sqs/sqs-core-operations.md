# Amazon SQS --- Core Operations Runbook

Generated on: 2026-02-24

------------------------------------------------------------------------

## 1. Create a Standard Queue (AWS CLI)

``` bash
aws sqs create-queue   --queue-name my-standard-queue   --attributes VisibilityTimeout=30
```

------------------------------------------------------------------------

## 2. Create a FIFO Queue

``` bash
aws sqs create-queue   --queue-name my-fifo-queue.fifo   --attributes FifoQueue=true,ContentBasedDeduplication=true
```

------------------------------------------------------------------------

## 3. Send a Message

``` bash
aws sqs send-message   --queue-url <QUEUE_URL>   --message-body "Hello World"
```

FIFO example:

``` bash
aws sqs send-message   --queue-url <FIFO_QUEUE_URL>   --message-body "Order Created"   --message-group-id "orders"
```

------------------------------------------------------------------------

## 4. Receive Messages

``` bash
aws sqs receive-message   --queue-url <QUEUE_URL>   --max-number-of-messages 1   --wait-time-seconds 10
```

------------------------------------------------------------------------

## 5. Delete Message (After Successful Processing)

``` bash
aws sqs delete-message   --queue-url <QUEUE_URL>   --receipt-handle <RECEIPT_HANDLE>
```

------------------------------------------------------------------------

## 6. Configure Dead Letter Queue

``` bash
aws sqs set-queue-attributes   --queue-url <QUEUE_URL>   --attributes RedrivePolicy='{"deadLetterTargetArn":"<DLQ_ARN>","maxReceiveCount":"5"}'
```

------------------------------------------------------------------------

## 7. Adjust Visibility Timeout

``` bash
aws sqs change-message-visibility   --queue-url <QUEUE_URL>   --receipt-handle <RECEIPT_HANDLE>   --visibility-timeout 60
```

------------------------------------------------------------------------

## 8. List Queues

``` bash
aws sqs list-queues
```

------------------------------------------------------------------------

## 9. Delete Queue

``` bash
aws sqs delete-queue   --queue-url <QUEUE_URL>
```

------------------------------------------------------------------------

# Operational Best Practices

-   Always delete messages after successful processing.
-   Use long polling (wait-time-seconds \> 0).
-   Monitor ApproximateNumberOfMessagesVisible.
-   Configure DLQ for all production queues.
