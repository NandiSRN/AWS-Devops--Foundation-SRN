hink of this section as:
👉 “Where structured data lives, how it scales, and how DevOps engineers keep it reliable.”

📁 02-aws-compute-storage/databases/README.md
# Databases – Where Application Data Lives

Applications come and go.
Servers scale up and down.
Containers restart.

But **data must survive everything**.

Databases exist to:
- Store structured data
- Ensure consistency
- Handle concurrency
- Provide durability

This section explains how AWS provides **managed databases**
so DevOps teams don’t babysit servers.

📄 02-aws-compute-storage/databases/database-basics.md
# Database Basics – From Zero

## First Principles

A database exists to:
- Store data
- Retrieve data
- Update data
- Protect data

Applications should NOT:
- Store critical data on local disk
- Manage replication manually
- Handle backups themselves

That’s why managed databases exist.

---

## Two Major Database Categories

### Relational Databases (SQL)
- Structured schema
- ACID guarantees
- Strong consistency

### Non-Relational Databases (NoSQL)
- Flexible schema
- Horizontal scaling
- High availability

Choosing the wrong type causes pain later.

📄 02-aws-compute-storage/databases/rds.md
# Amazon RDS – Managed Relational Databases

## What Is RDS?

RDS (Relational Database Service) is:
> A managed SQL database service.

AWS manages:
- OS
- Patching
- Backups
- Replication

You manage:
- Schema
- Queries
- Indexes

---

## Supported Engines

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server

---

## Key Benefits

- Automated backups
- Multi-AZ high availability
- Monitoring built-in

---

## Production Rule

Never run production databases on EC2
unless you have a strong reason.

📄 02-aws-compute-storage/databases/rds-architecture.md
# RDS Architecture (Important)

## Single-AZ vs Multi-AZ

### Single-AZ
- One database instance
- No automatic failover
- Used for dev/test

### Multi-AZ
- Primary + standby
- Synchronous replication
- Automatic failover

---

## What Multi-AZ Is NOT

- Not read scaling
- Not performance improvement

It is for **availability**, not speed.

📄 02-aws-compute-storage/databases/rds-backups.md
# RDS Backups & Snapshots

## Automated Backups

- Daily snapshots
- Transaction logs
- Point-in-time recovery

---

## Manual Snapshots

- User-initiated
- Persist until deleted
- Used for migrations

---

## Production Rule

Always enable automated backups.

📄 02-aws-compute-storage/databases/aurora.md
# Amazon Aurora – Cloud-Native SQL

## What Is Aurora?

Aurora is:
> AWS-built, cloud-optimized relational database.

Compatible with:
- MySQL
- PostgreSQL

---

## Why Aurora Exists

Traditional databases:
- Scale vertically
- Replicate slowly

Aurora:
- Separates compute and storage
- Scales better
- Heals faster

---

## Key Features

- 6-way replication across AZs
- Fast failover
- Serverless option available

---

## Production Insight

Aurora is preferred for:
- High availability
- High performance
- Cloud-native workloads

📄 02-aws-compute-storage/databases/dynamodb.md
# Amazon DynamoDB – NoSQL at Scale

## What Is DynamoDB?

DynamoDB is:
> Fully managed NoSQL key-value database.

Designed for:
- Massive scale
- Low latency
- Zero maintenance

---

## Core Concepts

- Table
- Item
- Attribute
- Primary key

---

## Why DynamoDB Is Different

- No joins
- No complex queries
- Design around access patterns

---

## Production Use Cases

- Session storage
- User profiles
- Metadata storage
- Event-driven systems

📄 02-aws-compute-storage/databases/dynamodb-scaling.md
# DynamoDB Scaling & Capacity

## Capacity Modes

### On-Demand
- Auto scales
- Higher cost
- Best for unpredictable traffic

### Provisioned
- Fixed capacity
- Lower cost
- Requires planning

---

## Global Tables

- Multi-region replication
- Active-active setup

---

## DevOps Insight

DynamoDB removes:
- Capacity planning stress
- Scaling failures

📄 02-aws-compute-storage/databases/elasticache.md
# Amazon ElastiCache – In-Memory Datastore

## What Is ElastiCache?

ElastiCache is:
> Managed in-memory database.

Engines:
- Redis
- Memcached

---

## Why In-Memory?

RAM is:
- Faster than disk
- Faster than network

---

## Use Cases

- Caching
- Session management
- Leaderboards
- Rate limiting

---

## Production Rule

Cache improves performance,
but database remains source of truth.

📄 02-aws-compute-storage/databases/secrets-manager.md
# AWS Secrets Manager – Protecting Credentials

## The Problem

Hardcoding secrets:
- Is insecure
- Leads to leaks
- Breaks compliance

---

## What Secrets Manager Does

- Stores secrets securely
- Encrypts with KMS
- Rotates automatically

---

## Production Best Practice

Applications should:
- Fetch secrets dynamically
- Never store passwords in code

📄 02-aws-compute-storage/databases/production-patterns.md
# Databases in Real Production

## Common Architecture

Application
 ↓
Cache (ElastiCache)
 ↓
Primary DB (RDS / Aurora)

---

## Golden Rules

- Never expose DB publicly
- Always use private subnets
- Always enable backups
- Monitor connections & latency

Databases fail silently.
Monitoring is mandatory.

✅ SECTION COMPLETE: 02-aws-compute-storage 🎉

You now have strong foundations in:

✔ EC2 & compute
✔ Load balancing
✔ Storage
✔ Databases

This is real production DevOps knowledge, not theory.
