# Amazon EFS -- Common Procedures

## 1. Create an EFS File System (CLI)

aws efs create-file-system --creation-token my-efs

## 2. Create Mount Target

aws efs create-mount-target\
--file-system-id fs-12345678\
--subnet-id subnet-abc123\
--security-groups sg-abc123

## 3. Install NFS Client (Amazon Linux)

sudo yum install -y amazon-efs-utils

## 4. Mount EFS

sudo mount -t efs fs-12345678:/ /mnt/efs

## 5. Verify Mount

df -h

## 6. Enable Encryption in Transit

sudo mount -t efs -o tls fs-12345678:/ /mnt/efs

## 7. Automatic Mount via /etc/fstab

fs-12345678:/ /mnt/efs efs defaults,\_netdev 0 0

------------------------------------------------------------------------

## ECS Fargate Integration

-   Define EFS Volume in Task Definition
-   Attach Access Point if needed
-   Ensure correct Security Group rules
