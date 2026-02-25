# AWS Lambda -- Event Sources Runbook

## Add SQS Trigger

``` bash
aws lambda create-event-source-mapping   --function-name my-function   --event-source-arn arn:aws:sqs:us-east-1:<account-id>:my-queue   --batch-size 10
```

## Add S3 Trigger (via console recommended)

Ensure: - Bucket notification configured - Lambda permission added

------------------------------------------------------------------------

## Troubleshooting

### Messages not consumed from SQS

✔ Check visibility timeout ✔ Check batch size ✔ Check IAM permissions

### Duplicate processing

✔ Ensure idempotent logic ✔ Validate retry behavior
