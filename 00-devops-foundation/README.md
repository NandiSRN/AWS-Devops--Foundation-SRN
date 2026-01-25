📁 00-devops-foundation/README.md
# DevOps Foundation – Thinking Before Tools

DevOps is NOT a tool.
DevOps is NOT Jenkins.
DevOps is NOT Kubernetes.

DevOps is a **way of building, delivering, and operating software** so that:
- Changes are fast
- Failures are small
- Systems are reliable

Before touching cloud, Docker, or Kubernetes, we must understand:
- Why DevOps exists
- What problems it solves
- How production systems behave

This folder builds that mindset.

📄 00-devops-foundation/what-is-devops.md
# What Is DevOps? (Explained From Zero)

## The Problem Before DevOps

Traditionally, software worked like this:

- Developers wrote code
- Operations teams deployed and ran it
- Each team blamed the other when things failed

This caused:
- Slow releases
- Fear of change
- Production outages
- No ownership

---

## What DevOps Changes

DevOps removes the wall between:
- Development
- Operations

DevOps encourages:
- Shared ownership
- Automation
- Fast feedback
- Continuous improvement

---

## Important Truth

DevOps is NOT:
- A job title
- A single tool
- Just CI/CD

DevOps IS:
> A culture supported by automation and cloud-native practices

---

## Real Production Meaning

In real companies:
- DevOps engineers design platforms
- Enable teams to deploy safely
- Reduce downtime
- Improve reliability

---

## Interview One-Liner

> DevOps is a set of practices that combine development and operations to deliver software faster and more reliably.

📄 00-devops-foundation/devops-lifecycle.md
# DevOps Lifecycle – How Software Lives

Software does not end at “code is written”.

It follows a continuous lifecycle.

---

## The DevOps Lifecycle Stages

1. Plan  
2. Code  
3. Build  
4. Test  
5. Release  
6. Deploy  
7. Operate  
8. Monitor  
9. Improve  

This loop never ends.

---

## Why This Matters

Failures usually happen because:
- Monitoring is missing
- Feedback is ignored
- Manual steps exist

DevOps focuses on **closing the loop**.

---

## Production Insight

If monitoring does not feed back into planning:
> The system will slowly degrade.

---

## Interview One-Liner

> The DevOps lifecycle is a continuous loop that covers planning, development, deployment, operation, and improvement.

📄 00-devops-foundation/ci-vs-cd.md
# CI vs CD – Commonly Confused Concepts

## Continuous Integration (CI)

CI means:
- Developers frequently merge code
- Automated builds run
- Automated tests validate changes

CI answers:
> “Is this code safe to merge?”

---

## Continuous Delivery (CD)

CD means:
- Code is always deployable
- Deployment is automated
- Release is a decision, not a task

CD answers:
> “Can this code go to production safely?”

---

## Continuous Deployment (Advanced)

Every change that passes tests:
- Goes directly to production
- Without manual approval

Used only in mature systems.

---

## Interview One-Liner

> CI focuses on integrating and testing code, while CD focuses on automated and reliable delivery to production.

📄 00-devops-foundation/devops-vs-sre.md
# DevOps vs SRE – Relationship Explained

## DevOps

Focuses on:
- Speed
- Automation
- Delivery pipelines

---

## Site Reliability Engineering (SRE)

Focuses on:
- Reliability
- Availability
- Error budgets

---

## Key Relationship

DevOps builds systems fast.
SRE ensures systems stay reliable.

They are complementary, not competing.

---

## Interview One-Liner

> DevOps focuses on delivery velocity, while SRE focuses on reliability using engineering principles.

📄 00-devops-foundation/real-production-mindset.md
# Real Production Mindset (Most Important File)

## Production Is Not a Lab

In production:
- Failures are normal
- Humans make mistakes
- Systems must heal themselves

---

## Core Production Principles

- Everything fails
- Automate recovery
- Reduce blast radius
- Prefer managed services
- Design for observability

---

## DevOps Engineer’s Real Job

Not:
- Writing YAML all day

But:
> Designing systems that fail safely and recover automatically

---

## Interview One-Liner

> Production systems must be designed for failure, automation, and observability.
