📁 06-ci-cd/README.md
# CI/CD – From Code to Production Automatically

CI/CD is the automation layer that connects:

Developer → Git → Build → Test → Scan → Deploy → Monitor

Without CI/CD:
- Deployments are manual
- Errors are frequent
- Rollbacks are painful

With CI/CD:
- Deployments are repeatable
- Failures are caught early
- Releases are fast and safe

📄 06-ci-cd/cicd-concepts.md
# CI/CD Concepts – From Zero

## What Is CI (Continuous Integration)?

CI means:
> Automatically integrating code changes frequently.

Every code push should:
- Build
- Run tests
- Validate quality

---

## What Is CD (Continuous Delivery / Deployment)?

CD means:
> Automatically delivering code to environments.

Two types:
- Continuous Delivery → manual approval to prod
- Continuous Deployment → auto deploy to prod

---

## Why CI/CD Exists

Manual deployment causes:
- Human errors
- Inconsistent environments
- Long downtime

CI/CD removes humans from repetitive tasks.

📄 06-ci-cd/pipeline-basics.md
# CI/CD Pipeline – Core Idea

## What Is a Pipeline?

A pipeline is:
> A sequence of automated steps.

Typical stages:
1. Checkout code
2. Build
3. Test
4. Scan
5. Package
6. Deploy

---

## Key Principle

If any stage fails:
👉 Pipeline must stop

This prevents bad code from reaching production.

📄 06-ci-cd/tools-overview.md
# CI/CD Tools Overview

CI/CD tools provide:
- Automation engine
- Logs
- History
- Integrations

Common tools:
- Jenkins
- GitHub Actions
- GitLab CI
- Azure DevOps

We focus on Jenkins and GitHub Actions.

📁 06-ci-cd/jenkins/

Now we go deep into Jenkins, from zero.

📄 06-ci-cd/jenkins/jenkins-architecture.md
# Jenkins Architecture – From Zero

## What Is Jenkins?

Jenkins is:
> An open-source automation server.

It runs pipelines and manages jobs.

---

## Core Components

- Jenkins Controller (Master)
- Jenkins Agents (Workers)

---

## Controller Responsibilities

- UI
- Job scheduling
- Pipeline orchestration
- Credential management

---

## Agent Responsibilities

- Execute build steps
- Run commands
- Build images

---

## Production Rule

Never run heavy builds on controller.
Always use agents.

📄 06-ci-cd/jenkins/pipeline.md
# Jenkins Pipeline – Core Concept

## What Is a Jenkins Pipeline?

A pipeline:
> Code that defines build steps.

Stored as:
- Jenkinsfile
- Version-controlled

---

## Pipeline Types

- Declarative (recommended)
- Scripted (advanced)

---

## Why Pipeline-as-Code Matters

- Versioned
- Reviewable
- Reproducible

📄 06-ci-cd/jenkins/declarative-pipeline.md
# Declarative Pipeline – From Zero

## Basic Jenkinsfile Example

```groovy
pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building application'
      }
    }

    stage('Test') {
      steps {
        echo 'Running tests'
      }
    }
  }
}

Key Concepts

agent → where pipeline runs

stages → logical steps

steps → actual commands

Production Insight

Declarative pipelines are:

Easier to read

Safer

Standard in enterprises


---

## 📄 `06-ci-cd/jenkins/credentials.md`

```md
# Jenkins Credentials – Security from Zero

## Why Credentials Matter

Pipelines need access to:
- Git
- Docker registry
- Cloud APIs

Never hardcode secrets.

---

## Jenkins Credential Types

- Username/password
- Secret text
- SSH keys
- Tokens

---

## Best Practice

Access credentials using:
withCredentials blocks

Never echo secrets.
