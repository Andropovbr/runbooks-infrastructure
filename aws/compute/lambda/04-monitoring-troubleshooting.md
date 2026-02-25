# AWS Lambda -- Monitoring & Observability

## View Logs

``` bash
aws logs describe-log-groups
aws logs filter-log-events   --log-group-name /aws/lambda/my-function
```

## Key Metrics (CloudWatch)

-   Invocations
-   Duration
-   Errors
-   Throttles
-   ConcurrentExecutions

------------------------------------------------------------------------

## Troubleshooting

### High Error Rate

✔ Check CloudWatch logs ✔ Validate environment variables ✔ Check
dependency timeouts

### Throttling

✔ Increase reserved concurrency ✔ Request limit increase ✔ Optimize
function duration
