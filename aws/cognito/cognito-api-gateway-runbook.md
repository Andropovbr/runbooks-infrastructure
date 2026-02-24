# Amazon Cognito + API Gateway Runbook

## Overview

Operational guide for protecting APIs using Cognito Authorizers.

------------------------------------------------------------------------

## Configuration Steps

1.  Create User Pool.
2.  Create App Client.
3.  Configure API Gateway Authorizer:
    -   Type: Cognito
    -   Select User Pool
4.  Attach Authorizer to API method.

------------------------------------------------------------------------

## Test with curl

``` bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>"   https://api-id.execute-api.region.amazonaws.com/prod/resource
```

------------------------------------------------------------------------

## Troubleshooting

### 1. 401 Unauthorized

Check: - Expired token - Wrong header format - Incorrect region

### 2. 403 Forbidden

Check: - IAM permission - Resource policy - Scope mismatch

### 3. Token Not Recognized

Verify: - Token from correct User Pool - API Gateway authorizer linked
to correct pool
