📁 07-gitops/README.md
# GitOps – Operating Kubernetes the Right Way

GitOps is not a tool.
GitOps is an operating model.

Core idea:
> Git is the single source of truth for deployments.

If Git says it should run → it runs  
If cluster changes manually → it gets reverted

This eliminates:
- SSH to servers
- kubectl apply in production
- Configuration drift

📄 07-gitops/gitops-concepts.md
# GitOps Concepts – From Zero

## Traditional CD (Problem)

CI pipeline:
- Builds image
- Deploys directly to cluster

Problems:
- CI needs cluster access
- Hard to audit
- Drift possible
- Rollback is complex

---

## GitOps Model (Solution)

CI:
- Builds image
- Pushes image
- Updates Git repo

GitOps tool:
- Watches Git
- Syncs cluster automatically

CI never touches the cluster.

📄 07-gitops/gitops-principles.md
# GitOps Core Principles

1. Declarative
   - Desired state defined in YAML

2. Versioned
   - Git history = audit log

3. Automated
   - No manual kubectl apply

4. Self-healing
   - Drift is auto-corrected

5. Observable
   - Git + UI show state

If any of these is missing → not GitOps.

📁 07-gitops/argocd/

Now we deep dive into Argo CD, the most popular GitOps tool.

📄 07-gitops/argocd/argocd-architecture.md
# Argo CD Architecture – From Zero

## What Argo CD Is

Argo CD is:
> A Kubernetes-native GitOps controller.

It runs INSIDE the cluster.

---

## Core Components

- API Server
- Repository Server
- Application Controller
- UI / CLI

---

## Key Insight

Argo CD:
- Pulls from Git
- Pushes to Kubernetes

NOT the other way around.

📄 07-gitops/argocd/application.md
# Argo CD Application – The Heart of GitOps

## What an Application Is

An Application:
> Maps a Git path to a Kubernetes cluster/namespace

---

## Example Application YAML

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
spec:
  project: default
  source:
    repoURL: https://github.com/org/app-config.git
    path: helm/myapp
    targetRevision: main
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true

Production Insight

Application is the GitOps contract.


---

## 📄 `07-gitops/argocd/sync-policy.md`

```md
# Sync Policy – Automation Behavior

## Manual Sync

- Argo detects drift
- Human approves sync

---

## Automated Sync

- Changes auto-applied
- Drift auto-corrected

---

## Production Pattern

- Auto-sync for dev
- Manual sync for prod

📄 07-gitops/argocd/drift-detection.md
# Drift Detection – Self Healing

## What Is Drift?

Drift:
- Cluster state ≠ Git state

Example:
- kubectl edit deployment

---

## What Argo CD Does

- Detects difference
- Flags OutOfSync
- Reverts changes (if enabled)

---

## Production Insight

Drift = configuration bug.

📄 07-gitops/argocd/rollback.md
# Rollbacks – Git Is Your Time Machine

## How Rollback Works

- Revert Git commit
- Argo CD syncs automatically

---

## No Special Commands

Rollback = git revert

---

## Production Advantage

- Simple
- Auditable
- Fast

📁 07-gitops/helm/

GitOps and Helm are best friends.

📄 07-gitops/helm/helm-basics.md
# Helm – Package Manager for Kubernetes

## What Helm Solves

Raw YAML:
- Repetitive
- Hard to maintain

Helm introduces:
- Templates
- Values
- Reusability

---

## Helm = Kubernetes + Variables

📄 07-gitops/helm/chart-structure.md
# Helm Chart Structure

```text
myapp/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml

Production Insight

One chart → many environments.


---

## 📄 `07-gitops/helm/values-yaml.md`

```md
# values.yaml – Environment Control

values.yaml defines:
- Image tag
- Replicas
- Resources
- Environment variables

---

## CI + GitOps Flow

CI updates values.yaml
GitOps syncs deployment

📄 07-gitops/helm/helm-vs-kustomize.md
# Helm vs Kustomize

| Feature | Helm | Kustomize |
|------|------|----------|
| Templating | Yes | No |
| Complexity | Medium | Low |
| Reuse | High | Medium |
| Learning | Harder | Easier |

---

## Production Choice

Helm for apps  
Kustomize for overlays

📄 07-gitops/real-production-gitops-flow.md
# Real Production GitOps Flow

Developer pushes code
↓
CI builds image
↓
CI updates Git config repo
↓
Argo CD detects change
↓
Cluster reconciles state

No kubectl.
No SSH.
No manual deploys.

✅ WHERE YOU ARE NOW

You now understand GitOps completely:

✔ GitOps philosophy
✔ Argo CD architecture
✔ Applications & sync
✔ Drift detection
✔ Rollbacks
✔ Helm integration

This is how real companies run Kubernetes today.
