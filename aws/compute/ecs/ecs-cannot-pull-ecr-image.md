# ECS Runbook: Cannot Pull Image from ECR (Timeout)

## Overview

This runbook describes how to diagnose and resolve ECS Fargate tasks failing to pull container images from Amazon ECR when running in private subnets without a NAT Gateway.

Typical error:

```
CannotPullContainerError:
dial tcp <public-ip>:443: i/o timeout
```

Example:

```
dial tcp 52.216.x.x:443: i/o timeout
```

---

## Architecture Context

- ECS Fargate in private subnets
- No NAT Gateway
- ECR Interface Endpoints (api + dkr)
- S3 Gateway Endpoint
- Restrictive Security Group egress rules

---

## Mental Model

Image pull flow without NAT:

1. Task contacts ECR API (GetAuthorizationToken)
2. Task contacts ECR DKR (image manifest)
3. Task downloads image layers from S3 (pre-signed URLs)

If S3 egress is blocked, image pull fails.

---

## Step 1 – Confirm Error

```bash
aws ecs describe-tasks \
  --cluster <cluster-name> \
  --tasks <task-id> \
  --query 'tasks[0].{stoppedReason:stoppedReason}'
```

If the error references a public IP (e.g., 52.x.x.x), likely S3 connectivity issue.

---

## Step 2 – Validate VPC Endpoints

Check ECR DKR endpoint:

```bash
aws ec2 describe-vpc-endpoints \
  --filters Name=service-name,Values=com.amazonaws.us-east-1.ecr.dkr
```

Ensure:

- State = available
- PrivateDnsEnabled = true

Check S3 Gateway endpoint:

```bash
aws ec2 describe-vpc-endpoints \
  --filters Name=service-name,Values=com.amazonaws.us-east-1.s3
```

Ensure:

- Type = Gateway
- Private route table associated

---

## Step 3 – Validate Route Table

```bash
aws ec2 describe-route-tables \
  --filters Name=association.subnet-id,Values=<private-subnet-id>
```

Ensure subnet uses route table associated with S3 endpoint.

---

## Step 4 – Validate Security Group Egress

```bash
aws ec2 describe-security-groups \
  --group-ids <app-sg-id> \
  --query 'SecurityGroups[0].IpPermissionsEgress'
```

If no rule allows outbound TCP 443 to S3, image pull will fail.

---

## Resolution

Allow outbound HTTPS (TCP 443) to the S3 prefix list:

- Protocol: TCP
- Port: 443
- Destination: com.amazonaws.us-east-1.s3 (prefix list)

Conceptually:

```
type = "egress"
from_port = 443
to_port = 443
protocol = "tcp"
prefix_list_ids = [S3_PREFIX_LIST_ID]
```

---

## Redeploy Service

```bash
aws ecs update-service \
  --cluster <cluster-name> \
  --service <service-name> \
  --force-new-deployment
```

---

## Validation

Confirm:

- Task transitions to RUNNING
- CloudWatch logs appear
- Target group registers target
- Health endpoint returns HTTP 200

---

## Root Cause

Security Group did not allow outbound HTTPS to S3.

Even with correct ECR endpoints and route tables, S3 egress must be explicitly permitted.

---

## Prevention

When running ECS/Fargate without NAT:

Always include:

- ECR API Interface Endpoint
- ECR DKR Interface Endpoint
- S3 Gateway Endpoint
- Security Group egress to S3 prefix list
