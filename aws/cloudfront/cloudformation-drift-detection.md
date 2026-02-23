# CloudFormation Drift Detection Runbook

*Last updated: 2026-02-23T17:49:52.489750 UTC*

## Purpose

Detect configuration drift between stack template and deployed
resources.

------------------------------------------------------------------------

## Step 1 -- Start Drift Detection

``` bash
aws cloudformation detect-stack-drift --stack-name <stack-name>
```

------------------------------------------------------------------------

## Step 2 -- Check Detection Status

``` bash
aws cloudformation describe-stack-drift-detection-status   --stack-drift-detection-id <detection-id>
```

------------------------------------------------------------------------

## Step 3 -- List Drifted Resources

``` bash
aws cloudformation describe-stack-resource-drifts   --stack-name <stack-name>
```

------------------------------------------------------------------------

## Operational Guidance

-   Drifted resources should be reconciled
-   Prefer updating template instead of manual console changes
-   Investigate IAM or manual emergency changes
