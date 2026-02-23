# AWS Storage Gateway --- File Gateway Runbook

## Overview

File Gateway provides NFS or SMB access to objects stored in Amazon S3.

------------------------------------------------------------------------

## Common Operational Commands

### Check Gateway Status

``` bash
aws storagegateway list-gateways
aws storagegateway describe-gateway-information --gateway-arn <gateway-arn>
```

### List File Shares

``` bash
aws storagegateway list-file-shares --gateway-arn <gateway-arn>
```

### Describe File Share

``` bash
aws storagegateway describe-nfs-file-shares --file-share-arn <file-share-arn>
```

### Refresh Cache

``` bash
aws storagegateway refresh-cache --file-share-arn <file-share-arn>
```

------------------------------------------------------------------------

## Routine Procedures

### Create NFS File Share

``` bash
aws storagegateway create-nfs-file-share   --gateway-arn <gateway-arn>   --role <iam-role-arn>   --location-arn <s3-bucket-arn>
```

### Mount NFS Share (Linux)

``` bash
mount -t nfs <gateway-ip>:/share /mnt/storage
```

------------------------------------------------------------------------

## Troubleshooting Scenarios

### Share Not Mounting

-   Verify NFS port 2049 is open
-   Check security group rules
-   Validate client subnet access

### Data Not Appearing in S3

-   Check upload buffer usage
-   Verify IAM role permissions
-   Inspect CloudWatch metrics

### High Latency

-   Check local cache disk health
-   Verify bandwidth
-   Review CloudWatch metrics: CacheHitPercent
