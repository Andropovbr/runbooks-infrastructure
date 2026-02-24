# Amazon SNS --- Troubleshooting Runbook

*Last updated: 2026-02-24 14:12 UTC*

------------------------------------------------------------------------

# 1. Messages Not Delivered

## Step 1 --- Check Subscription Status

``` bash
aws sns list-subscriptions-by-topic --topic-arn <TOPIC_ARN>
```

Ensure status is 'Confirmed'.

------------------------------------------------------------------------

## Step 2 --- Check CloudWatch Metrics

Key metrics:

-   NumberOfMessagesPublished
-   NumberOfNotificationsDelivered
-   NumberOfNotificationsFailed

------------------------------------------------------------------------

## Step 3 --- Verify IAM Permissions

Common issue: SQS queue policy missing permission for SNS.

Example SQS Policy snippet:

``` json
{
  "Effect": "Allow",
  "Principal": {"Service": "sns.amazonaws.com"},
  "Action": "sqs:SendMessage",
  "Resource": "<QUEUE_ARN>",
  "Condition": {
    "ArnEquals": {"aws:SourceArn": "<TOPIC_ARN>"}
  }
}
```

------------------------------------------------------------------------

# 2. AccessDenied Errors

Check:

-   Topic policy
-   IAM role permissions
-   KMS key policy

------------------------------------------------------------------------

# 3. FIFO Ordering Issues

Verify:

-   message-group-id is set
-   No multiple publishers using same group incorrectly

------------------------------------------------------------------------

# 4. DLQ Growing

Investigate:

-   Endpoint availability
-   Lambda errors
-   SQS permission issues

------------------------------------------------------------------------

# 5. Cross-Account Delivery Failing

Check:

-   Resource policy on topic
-   Queue policy in target account
-   Region mismatch

------------------------------------------------------------------------

# Advanced Debug Checklist

-   Validate region
-   Confirm ARN correctness
-   Inspect retry policies
-   Review delivery logs (CloudWatch)
