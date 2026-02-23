# Amazon EFS -- Overview & Core Concepts

## What is EFS?

Amazon Elastic File System (EFS) is a fully managed NFS-based file
system designed for Linux workloads that require shared, elastic,
multi-AZ storage.

------------------------------------------------------------------------

## Core Characteristics

-   NFSv4 compatible
-   Multi-AZ high availability
-   Elastic capacity (auto grows & shrinks)
-   POSIX compliant
-   Integrated with Security Groups
-   Encryption at rest and in transit supported

------------------------------------------------------------------------

## When to Use EFS

✔ Shared storage across multiple EC2 instances\
✔ Shared storage for ECS / Fargate tasks\
✔ WordPress / CMS uploads directory\
✔ Legacy applications requiring NFS\
✔ Distributed workloads needing shared file system

------------------------------------------------------------------------

## When NOT to Use EFS

✘ Storing container source code\
✘ Large backup archives (prefer S3)\
✘ Log aggregation (prefer CloudWatch or S3)\
✘ Single-instance block storage (use EBS instead)

------------------------------------------------------------------------

## Performance Modes

-   General Purpose (default)
-   Max I/O

## Throughput Modes

-   Bursting
-   Provisioned
-   Elastic

------------------------------------------------------------------------

## Architecture Components

-   File System
-   Mount Targets (one per AZ)
-   Security Groups
