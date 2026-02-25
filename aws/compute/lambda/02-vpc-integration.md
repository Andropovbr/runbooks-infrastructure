# AWS Lambda -- VPC Integration Runbook

## Attach Lambda to VPC

``` bash
aws lambda update-function-configuration   --function-name my-function   --vpc-config SubnetIds=subnet-123,subnet-456,SecurityGroupIds=sg-123
```

## Common Requirements

-   Private subnet
-   Route to NAT Gateway for internet access
-   Security group egress rule enabled

------------------------------------------------------------------------

## Troubleshooting

### Timeout when accessing internet

✔ Check NAT Gateway ✔ Check route table ✔ Check security group egress

### Cold start delay

✔ Use Provisioned Concurrency ✔ Reduce VPC dependency if possible
