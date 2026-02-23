# CloudFormation Troubleshooting -- Failed Stack

*Last updated: 2026-02-23T17:49:52.489749 UTC*

## Scenario

Stack creation or update failed.

------------------------------------------------------------------------

## Step 1 -- Check Events

``` bash
aws cloudformation describe-stack-events --stack-name <stack-name>
```

Identify the first FAILED resource.

------------------------------------------------------------------------

## Step 2 -- Common Causes

-   IAM permission issues
-   Resource already exists
-   Invalid parameter values
-   Dependency misconfiguration

------------------------------------------------------------------------

## Step 3 -- Continue Update Rollback (if applicable)

If stack is in UPDATE_ROLLBACK_FAILED:

``` bash
aws cloudformation continue-update-rollback --stack-name <stack-name>
```

------------------------------------------------------------------------

## Step 4 -- Delete and Recreate (Last Resort)

``` bash
aws cloudformation delete-stack --stack-name <stack-name>
```

⚠ Only if no production data is attached.
