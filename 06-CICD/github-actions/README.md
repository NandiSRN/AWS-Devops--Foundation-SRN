📁 06-ci-cd/github-actions/

Now we move to GitHub Actions, modern and cloud-native.

📄 06-ci-cd/github-actions/workflows.md
# GitHub Actions – From Zero

## What Is GitHub Actions?

GitHub Actions is:
> GitHub-native CI/CD platform.

Runs pipelines based on:
- Git events

---

## Workflow Triggers

- push
- pull_request
- schedule
- manual

---

## Folder Structure

```text
.github/workflows/pipeline.yml


---

## 📄 `06-ci-cd/github-actions/runners.md`

```md
# Runners – Where Jobs Execute

## What Is a Runner?

Runner:
> A machine that executes workflow jobs.

---

## Types

- GitHub-hosted
- Self-hosted

---

## Production Insight

Self-hosted runners:
- Needed for private infra
- Needed for heavy workloads

📄 06-ci-cd/github-actions/secrets.md
# Secrets in GitHub Actions

## What Secrets Are

Secrets:
- Encrypted
- Masked in logs
- Scoped

---

## Example Usage

```yaml
env:
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}

Production Rule

Rotate secrets regularly.


---

## 📄 `06-ci-cd/ci-cd-production-patterns.md`

```md
# CI/CD in Real Production

## Typical Flow

Code push
↓
CI pipeline
↓
Docker image build
↓
Security scan
↓
Push to registry
↓
Deploy to Kubernetes

---

## Golden Rules

- Fail fast
- Keep pipelines fast
- Secure credentials
- Automate rollback

CI/CD is the heartbeat of DevOps.

✅ WHERE YOU ARE NOW

You now fully understand CI/CD from zero:

✔ CI vs CD
✔ Pipeline concepts
✔ Jenkins architecture & pipelines
✔ GitHub Actions workflows
✔ Secrets & security
✔ Production CI/CD patterns

This is interview-ready + real-job-ready knowledge.
