# AWS Storage Gateway --- Tape Gateway Runbook

## Overview

Tape Gateway emulates a Virtual Tape Library (VTL) storing data in S3
Glacier.

------------------------------------------------------------------------

## Common Operational Commands

### List Virtual Tapes

``` bash
aws storagegateway list-tapes
```

### Retrieve Tape

``` bash
aws storagegateway retrieve-tape-archive   --tape-arn <tape-arn>
```

### Describe Tape

``` bash
aws storagegateway describe-tapes   --tape-arns <tape-arn>
```

------------------------------------------------------------------------

## Routine Procedures

### Create Virtual Tape

``` bash
aws storagegateway create-tape-with-barcode   --gateway-arn <gateway-arn>   --tape-size-in-bytes <size>   --tape-barcode <barcode>
```

------------------------------------------------------------------------

## Troubleshooting Scenarios

### Backup Software Cannot See VTL

-   Confirm iSCSI connectivity
-   Verify backup server configuration
-   Check initiator name registration

### Tape Stuck in RETRIEVING

-   Validate Glacier retrieval time (Standard/Bulk/Expedited)
-   Monitor retrieval status via CLI

### Unexpected Costs

-   Review Glacier storage class
-   Check retrieval frequency
