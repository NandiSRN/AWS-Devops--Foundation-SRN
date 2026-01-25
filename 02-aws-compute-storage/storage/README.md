hink of this section as:
👉 “Where data lives, how long it lives, and what happens when things fail.”

📁 02-aws-compute-storage/storage/README.md
# AWS Storage – Where Data Lives

Compute runs applications.
Storage preserves data.

Without storage:
- Applications lose state
- Databases cannot exist
- Logs disappear
- Backups are impossible

AWS provides multiple storage services because
**not all data behaves the same**.

📄 02-aws-compute-storage/storage/storage-basics.md
# Storage Basics – From Zero

## First Question to Ask

When an application writes data:
- Where does it go?
- How long should it stay?
- Does it need to survive restarts?

Storage decisions depend on:
- Performance
- Durability
- Cost
- Access pattern

---

## Two Fundamental Storage Types

### Block Storage
- Looks like a disk
- Used by operating systems
- Attached to one machine

### Object Storage
- Stores objects (files)
- Accessed via APIs
- Not mounted as disks

File storage sits between these two.

📄 02-aws-compute-storage/storage/ebs.md
# Amazon EBS – Elastic Block Store

## What Is EBS?

EBS is:
> Block storage for EC2 instances.

Think of it as:
- A virtual hard disk
- Attached to a server

---

## Key Characteristics

- Attached to one EC2 at a time
- Lives in a single Availability Zone
- Persists after instance reboot

---

## Common Use Cases

- Root disk for EC2
- Databases
- Application data

---

## Important Production Truth

If EC2 is terminated:
- EBS may or may not survive
(depending on configuration)

Always verify delete-on-termination flag.

📄 02-aws-compute-storage/storage/ebs-types.md
# EBS Volume Types

## Why Types Exist

Different workloads need:
- Speed
- Throughput
- Cost efficiency

---

## Common Types

### gp3 (General Purpose)
- Default choice
- Balanced performance
- Cost-effective

### io2 (Provisioned IOPS)
- High IOPS
- Mission-critical databases

### st1 / sc1
- Throughput optimized
- Large sequential workloads

---

## Production Rule

Use gp3 unless you clearly need io2.

📄 02-aws-compute-storage/storage/ebs-snapshots.md
# EBS Snapshots – Backup Strategy

## What Is a Snapshot?

A snapshot is:
> A point-in-time backup of an EBS volume.

---

## Where Snapshots Live

- Stored in S3 (managed by AWS)
- Incremental by nature

---

## Why Snapshots Matter

- Disaster recovery
- AMI creation
- Migration

---

## Production Rule

Automate snapshots.
Manual backups are unreliable.

📄 02-aws-compute-storage/storage/s3.md
# Amazon S3 – Object Storage

## What Is S3?

S3 is:
> Object storage for unlimited data.

It stores:
- Files
- Backups
- Logs
- Artifacts

---

## Key Characteristics

- Virtually unlimited
- Extremely durable (11 nines)
- Accessible over HTTP

---

## What S3 Is NOT

- Not a file system
- Not block storage
- Not mounted like EBS

---

## Common Use Cases

- Static websites
- CI/CD artifacts
- Backups
- Data lakes

📄 02-aws-compute-storage/storage/s3-storage-classes.md
# S3 Storage Classes

## Why Storage Classes Exist

Not all data is accessed equally.

---

## Common Classes

### Standard
- Frequent access

### IA (Infrequent Access)
- Lower cost
- Retrieval fee

### Glacier
- Archival
- Minutes to hours retrieval

---

## Production Strategy

Move old data to cheaper tiers automatically.

📄 02-aws-compute-storage/storage/s3-versioning-lifecycle.md
# S3 Versioning & Lifecycle

## Versioning

- Keeps multiple versions of objects
- Protects from accidental deletion

---

## Lifecycle Rules

Automates:
- Tier transitions
- Object deletion

---

## Production Rule

Enable versioning on critical buckets.

📄 02-aws-compute-storage/storage/efs.md
# Amazon EFS – Shared File Storage

## What Is EFS?

EFS is:
> Network file system for multiple EC2s.

---

## Key Characteristics

- Shared across instances
- Auto-scales
- Regional service

---

## Common Use Cases

- Shared application files
- CMS systems
- Container shared volumes

---

## Kubernetes Insight

EFS is often used with:
- StatefulSets
- Shared persistent volumes

📄 02-aws-compute-storage/storage/fsx.md
# Amazon FSx – Specialized File Systems

## What FSx Is

FSx provides:
- Managed file systems
- Optimized for specific workloads

---

## Variants

- FSx for Windows
- FSx for Lustre

---

## Use Cases

- Windows applications
- HPC workloads

📄 02-aws-compute-storage/storage/backups-snapshots.md
# Backup & Recovery Mindset

## Storage Is Not Backup

Live data ≠ Backup.

---

## Production Best Practices

- Snapshot regularly
- Store backups cross-region
- Test restore procedures

---

## Golden Rule

If you’ve never restored it,
it’s not a backup.

✅ WHERE WE ARE NOW

✔ Storage explained from zero
✔ Block vs object storage clarified
✔ EBS, S3, EFS, FSx covered
✔ Backup mindset explained
