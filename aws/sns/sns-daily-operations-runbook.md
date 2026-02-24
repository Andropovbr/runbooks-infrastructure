# Amazon SNS --- Daily Operations Runbook

*Last updated: 2026-02-24 14:12 UTC*

------------------------------------------------------------------------

## 1. Create a Topic

``` bash
aws sns create-topic --name my-topic
```

For FIFO:

``` bash
aws sns create-topic --name my-topic.fifo --attributes FifoTopic=true,ContentBasedDeduplication=true
```

------------------------------------------------------------------------

## 2. List Topics

``` bash
aws sns list-topics
```

------------------------------------------------------------------------

## 3. Subscribe an Endpoint

### Subscribe SQS

``` bash
aws sns subscribe   --topic-arn <TOPIC_ARN>   --protocol sqs   --notification-endpoint <SQS_QUEUE_ARN>
```

### Subscribe Email

``` bash
aws sns subscribe   --topic-arn <TOPIC_ARN>   --protocol email   --notification-endpoint user@example.com
```

------------------------------------------------------------------------

## 4. Publish a Message

``` bash
aws sns publish   --topic-arn <TOPIC_ARN>   --message "Test message"
```

For FIFO:

``` bash
aws sns publish   --topic-arn <TOPIC_ARN>   --message "Test message"   --message-group-id group1
```

------------------------------------------------------------------------

## 5. Enable Server-Side Encryption (KMS)

``` bash
aws sns set-topic-attributes   --topic-arn <TOPIC_ARN>   --attribute-name KmsMasterKeyId   --attribute-value alias/aws/sns
```

------------------------------------------------------------------------

## 6. Configure Dead Letter Queue

``` bash
aws sns set-subscription-attributes   --subscription-arn <SUBSCRIPTION_ARN>   --attribute-name RedrivePolicy   --attribute-value '{"deadLetterTargetArn":"<DLQ_ARN>"}'
```

------------------------------------------------------------------------

## 7. Delete Topic

``` bash
aws sns delete-topic --topic-arn <TOPIC_ARN>
```

------------------------------------------------------------------------

## Operational Checklist

-   Confirm topic exists
-   Confirm subscriptions are confirmed
-   Validate IAM permissions
-   Check CloudWatch metrics
-   Validate DLQ configuration
