# Amazon Cognito - Identity Pool Runbook

## Overview

Runbook for Cognito Identity Pools (Federated Identities).

------------------------------------------------------------------------

## Common AWS CLI Commands

### List Identity Pools

``` bash
aws cognito-identity list-identity-pools --max-results 20
```

### Create Identity Pool

``` bash
aws cognito-identity create-identity-pool   --identity-pool-name my-identity-pool   --allow-unauthenticated-identities
```

### Get Identity ID

``` bash
aws cognito-identity get-id   --identity-pool-id <IDENTITY_POOL_ID>
```

### Get Temporary Credentials

``` bash
aws cognito-identity get-credentials-for-identity   --identity-id <IDENTITY_ID>
```

------------------------------------------------------------------------

## Operational Procedures

### Attach IAM Roles

1.  Create IAM roles (authenticated / unauthenticated).
2.  Attach trust policy for Cognito.
3.  Attach roles to identity pool.

------------------------------------------------------------------------

## Troubleshooting Scenarios

### 1. Access Denied to S3

Check: - IAM role policy - Trust relationship - Correct role mapping

### 2. No Credentials Returned

Check: - Identity pool ID correct? - Provider configured? - User
authenticated?

------------------------------------------------------------------------

### 3. Invalid Identity Pool Configuration

Verify: - Region consistency - Linked User Pool configuration
