# Amazon Cognito - User Pool Runbook

## Overview

This runbook covers common operational commands and troubleshooting
procedures for Amazon Cognito User Pools.

------------------------------------------------------------------------

## Common AWS CLI Commands

### List User Pools

``` bash
aws cognito-idp list-user-pools --max-results 20
```

### Create User Pool

``` bash
aws cognito-idp create-user-pool --pool-name my-user-pool
```

### Create App Client

``` bash
aws cognito-idp create-user-pool-client   --user-pool-id <POOL_ID>   --client-name my-app-client   --no-generate-secret
```

### List Users

``` bash
aws cognito-idp list-users --user-pool-id <POOL_ID>
```

### Admin Create User

``` bash
aws cognito-idp admin-create-user   --user-pool-id <POOL_ID>   --username user@example.com
```

### Admin Set Permanent Password

``` bash
aws cognito-idp admin-set-user-password   --user-pool-id <POOL_ID>   --username user@example.com   --password 'Password123!'   --permanent
```

------------------------------------------------------------------------

## Operational Procedures

### Enable MFA

1.  Update User Pool MFA configuration.
2.  Configure software token or SMS.
3.  Validate using test account.

### Add User to Group

``` bash
aws cognito-idp admin-add-user-to-group   --user-pool-id <POOL_ID>   --username user@example.com   --group-name admins
```

------------------------------------------------------------------------

## Troubleshooting Scenarios

### 1. User Cannot Log In

Check: - Account status (CONFIRMED?) - Password reset required? - MFA
misconfiguration?

Command:

``` bash
aws cognito-idp admin-get-user   --user-pool-id <POOL_ID>   --username user@example.com
```

------------------------------------------------------------------------

### 2. Invalid Token Errors

Check: - Token expiration - Correct User Pool region - Correct App
Client ID

Validate JWT using: https://jwt.io

------------------------------------------------------------------------

### 3. Too Many Requests Exception

Cause: - Rate limiting

Mitigation: - Implement exponential backoff - Reduce login retries
