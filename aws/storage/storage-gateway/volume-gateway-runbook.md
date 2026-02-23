# AWS Storage Gateway --- Volume Gateway Runbook

## Overview

Volume Gateway presents iSCSI volumes backed by AWS (EBS Snapshots).

------------------------------------------------------------------------

## Common Operational Commands

### List Volumes

``` bash
aws storagegateway list-volumes --gateway-arn <gateway-arn>
```

### Describe Volume

``` bash
aws storagegateway describe-storediSCSIVolumes   --volume-arns <volume-arn>
```

### Create Snapshot

``` bash
aws storagegateway create-snapshot   --volume-arn <volume-arn>   --snapshot-description "Manual snapshot"
```

------------------------------------------------------------------------

## Routine Procedures

### Attach iSCSI Volume (Linux)

``` bash
iscsiadm -m discovery -t sendtargets -p <gateway-ip>
iscsiadm -m node --login
```

------------------------------------------------------------------------

## Troubleshooting Scenarios

### iSCSI Target Not Discoverable

-   Verify port 3260 open
-   Check initiator configuration
-   Confirm CHAP authentication settings

### Snapshot Fails

-   Verify IAM permissions
-   Check AWS service limits
-   Inspect CloudWatch logs

### Performance Degradation

-   Evaluate cache disk performance
-   Confirm bandwidth availability
