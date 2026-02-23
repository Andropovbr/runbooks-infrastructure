# Amazon EFS -- Troubleshooting Guide

## Issue 1: Mount Timeout

Symptoms: - Mount command hangs - Connection timeout errors

Checklist: - Confirm Mount Target exists in same AZ - Validate Security
Group allows NFS (port 2049) - Confirm subnet routing - Ensure NACL
allows traffic

------------------------------------------------------------------------

## Issue 2: Permission Denied

Symptoms: - Cannot write to mounted directory

Checklist: - Check POSIX permissions - Validate UID/GID alignment -
Verify Access Point configuration

------------------------------------------------------------------------

## Issue 3: High Latency

Possible Causes: - Using Bursting mode with insufficient credits -
Excessive concurrent writes - Wrong performance mode selected

Actions: - Switch to Provisioned or Elastic throughput - Review
CloudWatch metrics - Evaluate workload pattern

------------------------------------------------------------------------

## Issue 4: AZ Failure

Expected Behavior: - EFS remains available - Traffic shifts to other
Mount Targets

Validation: - Confirm multiple mount targets configured - Test failover
scenario

------------------------------------------------------------------------

## Metrics to Monitor

-   BurstCreditBalance
-   PercentIOLimit
-   ClientConnections
