# CloudFormation StackSets Operations Runbook

*Last updated: 2026-02-23T17:49:52.489751 UTC*

## Overview

StackSets allow deploying CloudFormation stacks across multiple AWS
accounts and regions.

------------------------------------------------------------------------

## Create StackSet

``` bash
aws cloudformation create-stack-set   --stack-set-name <name>   --template-body file://template.yaml   --capabilities CAPABILITY_NAMED_IAM
```

------------------------------------------------------------------------

## Deploy to Accounts

``` bash
aws cloudformation create-stack-instances   --stack-set-name <name>   --accounts <account-id>   --regions us-east-1 us-west-2
```

------------------------------------------------------------------------

## Monitor StackSet

``` bash
aws cloudformation describe-stack-set   --stack-set-name <name>
```

------------------------------------------------------------------------

## Troubleshooting

-   Ensure AWS Organizations integration enabled
-   Verify execution role exists in target accounts
-   Check service-managed vs self-managed permissions
