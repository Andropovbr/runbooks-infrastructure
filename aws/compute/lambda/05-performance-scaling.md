# AWS Lambda -- Performance & Scaling

## Update Memory

``` bash
aws lambda update-function-configuration   --function-name my-function   --memory-size 1024
```

Higher memory = more CPU

------------------------------------------------------------------------

## Configure Reserved Concurrency

``` bash
aws lambda put-function-concurrency   --function-name my-function   --reserved-concurrent-executions 10
```

------------------------------------------------------------------------

## Troubleshooting

### Slow Execution

✔ Increase memory ✔ Optimize dependencies ✔ Reduce cold start

### Cost Too High

✔ Analyze duration ✔ Reduce memory if overprovisioned ✔ Consider ECS if
workload is constant
