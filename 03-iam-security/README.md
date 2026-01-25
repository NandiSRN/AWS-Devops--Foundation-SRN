This is one of the most important sections in DevOps.
If IAM is wrong → security breach.
If IAM is right → everything else becomes safe.

Take this slow. Read it like a personal DevOps notebook.

📁 03-iam-security/README.md
# IAM & Security – Who Can Do What (The Heart of AWS Security)

In AWS:
- Nothing is allowed by default
- Everything must be explicitly permitted

IAM decides:
- Who you are
- What you can do
- On which resources
- Under what conditions

IAM is the foundation of AWS security.
Every DevOps engineer works with IAM daily.

📄 03-iam-security/iam-basics.md
# IAM Basics – From Zero

## First Principle (Very Important)

AWS follows:
> DENY BY DEFAULT

If you do not explicitly allow something,
it is automatically denied.

This is intentional for security.

---

## What IAM Stands For

IAM = Identity and Access Management

It answers four questions:
1. Who are you?
2. What action do you want to perform?
3. On which resource?
4. Is it allowed?

---

## IAM Is Global

IAM:
- Is NOT region-specific
- Applies across the entire AWS account

This means:
A mistake affects everything.

📄 03-iam-security/iam-identities.md
# IAM Identities – Who Can Access AWS

IAM provides three main identities:

1. Users
2. Groups
3. Roles

Each serves a different purpose.

📄 03-iam-security/iam-users-groups.md
# IAM Users & Groups

## IAM User

An IAM user represents:
> A human or application needing AWS access.

User can have:
- Console access
- Programmatic access (Access Key + Secret)

---

## IAM Group

A group is:
> A collection of users with shared permissions.

Groups do NOT:
- Log in
- Have credentials

They only assign permissions.

---

## Production Best Practice

- Do NOT attach policies directly to users
- Always use groups

---

## Real Example

DevOps-Admins group:
- Full access
- Used by limited users

ReadOnly group:
- View-only access
- Used by auditors

📄 03-iam-security/iam-roles.md
# IAM Roles – Temporary, Secure Access

## Why Roles Exist

Storing long-term credentials is dangerous.

IAM Roles solve this by:
> Providing temporary credentials.

---

## What an IAM Role Is

A role:
- Has permissions
- Has NO credentials
- Is assumed when needed

---

## Common Role Usage

- EC2 accessing S3
- EKS accessing AWS APIs
- CI/CD pipelines deploying infra
- Cross-account access

---

## Production Rule

Applications should use:
> IAM Roles, not access keys

📄 03-iam-security/trust-policy.md
# Trust Policy – Who Can Assume a Role

## Two Policies in IAM (Important)

1. Trust Policy → WHO can assume
2. Permission Policy → WHAT can be done

---

## Trust Policy Example (Simple)

This trust policy says:
"EC2 is allowed to assume this role"

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}

Key Insight

No trust policy = role is useless.


---

## 📄 `03-iam-security/iam-policies.md`

```md
# IAM Policies – Permission Definition

## What Is an IAM Policy?

A policy defines:
> What actions are allowed or denied on which resources.

---

## Policy Structure

Every policy has:
- Effect (Allow/Deny)
- Action
- Resource
- Optional Conditions

---

## Example Policy (Read S3 Only)

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}

Production Rule

Always follow:

Least Privilege Principle


---

## 📄 `03-iam-security/managed-vs-inline.md`

```md
# Managed vs Inline Policies

## Managed Policies

- Reusable
- Versioned
- Recommended

---

## Inline Policies

- Attached to one identity
- Harder to manage
- Not reusable

---

## Production Rule

Use managed policies whenever possible.

📄 03-iam-security/sts-assume-role.md
# STS & AssumeRole – Temporary Credentials

## What Is STS?

STS = Security Token Service

It issues:
- Temporary credentials
- With limited lifetime

---

## Why STS Matters

- No permanent secrets
- Reduced blast radius
- Safer automation

---

## Common Usage

- Cross-account access
- CI/CD deployments
- Kubernetes service accounts

📄 03-iam-security/irsa.md
# IRSA – IAM Roles for Service Accounts (EKS)

## The Problem

Pods should not:
- Share node IAM role
- Have broad permissions

---

## What IRSA Solves

IRSA allows:
> One IAM role per Kubernetes service account

Each pod gets:
- Exact permissions it needs
- Nothing more

---

## Production Insight

IRSA is mandatory for secure EKS setups.

📄 03-iam-security/kms.md
# AWS KMS – Encryption Control

## Why KMS Exists

Encryption without key control is incomplete.

KMS manages:
- Encryption keys
- Key rotation
- Access control

---

## What Uses KMS

- EBS
- S3
- RDS
- Secrets Manager

---

## Production Rule

Always use customer-managed keys for sensitive data.

📄 03-iam-security/cloudtrail.md
# CloudTrail – Audit Everything

## What CloudTrail Does

CloudTrail records:
- Every API call
- Who did it
- When it happened

---

## Why CloudTrail Is Critical

- Security investigations
- Compliance
- Incident response

---

## Production Rule

CloudTrail must ALWAYS be enabled.

📄 03-iam-security/aws-config.md
# AWS Config – Resource Compliance

## What AWS Config Does

AWS Config:
- Tracks resource changes
- Evaluates compliance
- Detects drift

---

## Example Use Case

Alert if:
- S3 bucket becomes public
- Security group opens 0.0.0.0/0

---

## DevOps Insight

Config answers:
> “Who changed what and why?”

📄 03-iam-security/guardduty.md
# Amazon GuardDuty – Threat Detection

## What GuardDuty Does

GuardDuty analyzes:
- Logs
- Network traffic
- DNS activity

Detects:
- Credential compromise
- Crypto mining
- Malicious activity

---

## Production Rule

GuardDuty should be enabled in all accounts.

📄 03-iam-security/waf-shield.md
# WAF & Shield – Application Protection

## AWS WAF

Protects against:
- SQL injection
- XSS attacks
- Bad bots

---

## AWS Shield

Protects against:
- DDoS attacks

---

## Real Production Setup

Route 53
 ↓
CloudFront
 ↓
WAF
 ↓
ALB

📄 03-iam-security/production-mindset.md
# IAM & Security – Production Mindset

Security is NOT a one-time setup.
It is continuous.

---

## Golden Rules

- Least privilege always
- No hardcoded credentials
- Audit everything
- Assume breach mindset

Security mistakes are expensive.

✅ WHERE WE ARE NOW

You now have solid IAM & security foundations:

✔ Users, groups, roles
✔ Policies & trust relationships
✔ STS & IRSA
✔ Encryption (KMS)
✔ Auditing & threat detection

This is real-world DevOps security knowledge.
