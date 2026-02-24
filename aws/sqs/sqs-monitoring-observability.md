# Amazon SQS --- Monitoring & Observability

Generated on: 2026-02-24

------------------------------------------------------------------------

# Key CloudWatch Metrics

-   ApproximateNumberOfMessagesVisible
-   ApproximateNumberOfMessagesNotVisible
-   ApproximateAgeOfOldestMessage
-   NumberOfMessagesSent
-   NumberOfMessagesDeleted

------------------------------------------------------------------------

# Alarm Example (High Backlog)

Trigger when:

ApproximateNumberOfMessagesVisible \> 1000

------------------------------------------------------------------------

# Scaling Pattern

ECS Auto Scaling based on:

ApproximateNumberOfMessagesVisible

------------------------------------------------------------------------

# SRE Checklist

-   DLQ configured?
-   Visibility timeout aligned with processing time?
-   Long polling enabled?
-   CloudWatch alarms configured?
-   IAM least privilege enforced?
