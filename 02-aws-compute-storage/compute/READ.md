📁 02-aws-compute-storage/README.md
# AWS Compute & Storage – Where Applications Actually Run

Networking allows communication.
Compute is where applications execute.
Storage is where data lives.

Without compute:
- No applications run
- No containers start
- No Kubernetes nodes exist

This section explains how AWS provides **servers, scaling, and storage** in a controlled, production-ready way.

📁 02-aws-compute-storage/compute/
📄 02-aws-compute-storage/compute/ec2.md
# Amazon EC2 – Virtual Servers from Zero

## Start With the Fundamental Question

What is a server?

A server is simply:
- CPU (compute power)
- Memory (RAM)
- Disk (storage)
- Network interface
- Operating system

Amazon EC2 provides **virtual servers** on AWS infrastructure.

---

## What EC2 Really Is

EC2 (Elastic Compute Cloud) is:
> A service that lets you rent virtual machines in the cloud.

You choose:
- CPU size
- Memory size
- Disk
- OS
- Network placement

AWS manages the physical hardware.
You manage the virtual machine.

---

## EC2 vs Physical Server (Mindset)

Physical server:
- Buy hardware
- Rack it
- Power it
- Maintain it

EC2:
- Launch in minutes
- Pay per use
- Scale up or down
- Delete anytime

This is why cloud exists.

📄 02-aws-compute-storage/compute/instance-types.md
# EC2 Instance Types – Choosing the Right Machine

## Why Instance Types Exist

Not all workloads are the same.

Some need:
- More CPU
- More memory
- High network throughput

AWS offers instance families optimized for different needs.

---

## Common Instance Families

### General Purpose (t, m)
Balanced CPU and memory  
Used for:
- Web servers
- Application servers

### Compute Optimized (c)
High CPU power  
Used for:
- Batch processing
- High-performance apps

### Memory Optimized (r)
Large RAM  
Used for:
- Databases
- Caching systems

### Storage Optimized (i)
Fast local disks  
Used for:
- Big data
- Low-latency storage

---

## Production Rule

Do NOT over-provision.
Start small, monitor, then scale.

📄 02-aws-compute-storage/compute/ami.md
# AMI – Amazon Machine Image

## What Is an AMI?

An AMI is:
> A template used to create EC2 instances.

It contains:
- Operating system
- Pre-installed software
- Configuration

---

## Why AMIs Matter

AMIs ensure:
- Consistency
- Faster provisioning
- Reproducibility

Instead of configuring servers manually every time,
you launch from an AMI.

---

## Real Production Example

Golden AMI includes:
- OS patched
- Docker installed
- Monitoring agent installed
- Security hardening applied

---

## DevOps Insight

Immutable infrastructure starts with AMIs.

📄 02-aws-compute-storage/compute/asg.md
# Auto Scaling Group (ASG)

## The Problem ASG Solves

Traffic is unpredictable.

Manual scaling leads to:
- Downtime
- Over-provisioning
- Human error

---

## What ASG Does

Auto Scaling Group:
- Automatically adds instances
- Automatically removes instances
- Maintains desired capacity

---

## Scaling Triggers

ASG can scale based on:
- CPU usage
- Memory
- Custom metrics

---

## Production Reality

Most production EC2 workloads:
> NEVER run as a single instance.

Always use ASG.

📄 02-aws-compute-storage/compute/launch-template.md
# Launch Templates – Blueprint for EC2

## What Is a Launch Template?

A launch template defines:
- AMI
- Instance type
- Security groups
- User data
- IAM role

ASG uses launch templates to create instances.

---

## Why Launch Templates Exist

- Versioning
- Consistency
- Safer updates

---

## Production Rule

Never hardcode instance configs.
Always use launch templates.

📄 02-aws-compute-storage/compute/bastion-host.md
# Bastion Host – Secure Entry Point

## The Problem

Private instances should not:
- Have public IPs
- Be directly accessible

But admins still need access.

---

## What Is a Bastion Host?

A bastion host is:
> A hardened EC2 instance used to access private resources.

---

## How It Works

1. SSH into bastion (public subnet)
2. SSH from bastion to private instance

---

## Production Best Practice

- Minimal access
- Strong authentication
- Logging enabled

Modern alternative:
- AWS SSM Session Manager

✅ WHERE WE ARE NOW

✔ EC2 explained from zero
✔ Instance types clarified
✔ AMI importance explained
✔ Auto Scaling introduced
✔ Bastion access covered

This is real compute foundation.
