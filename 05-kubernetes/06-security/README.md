📁 05-kubernetes/06-security/README.md
# Kubernetes Security – Controlling Access & Trust

Kubernetes security answers five fundamental questions:

1. Who are you?
2. What are you allowed to do?
3. What resources can you access?
4. How are secrets handled?
5. How do pods communicate safely?

If security is weak:
- Anyone can delete clusters
- Secrets can leak
- Pods can attack each other

Security is layered. No single control is enough.

📄 05-kubernetes/06-security/security-basics.md
# Kubernetes Security – From Zero

## First Principle

Kubernetes is an API-driven system.

Every action:
- Goes through kube-apiserver
- Is authenticated
- Is authorized

No authentication → no access  
No authorization → action denied

📄 05-kubernetes/06-security/authentication.md
# Authentication – Who Are You?

## What Authentication Means

Authentication answers:
> "Who is making this request?"

---

## Common Authentication Methods

- Certificates
- Tokens
- ServiceAccounts
- IAM (in EKS)

---

## Important Truth

Kubernetes does NOT manage users internally.
It trusts external identity systems.

📄 05-kubernetes/06-security/serviceaccount.md
# ServiceAccount – Pod Identity

## What a ServiceAccount Is

A ServiceAccount:
> An identity for pods.

Pods use ServiceAccounts to:
- Call Kubernetes API
- Access cluster resources

---

## Default Behavior

Every namespace has:
- A default ServiceAccount

Pods automatically use it if none specified.

---

## Production Rule

Never rely on default ServiceAccount.

📄 05-kubernetes/06-security/rbac.md
# RBAC – Role-Based Access Control

## What RBAC Does

RBAC answers:
> "What is this identity allowed to do?"

---

## RBAC Controls

- Read
- Write
- Delete
- Update

On:
- Pods
- Services
- Secrets
- Nodes

📄 05-kubernetes/06-security/role-rolebinding.md
# Role & RoleBinding – Namespace-Level Permissions

## Role

A Role defines:
- Allowed actions
- On specific resources
- Inside one namespace

---

## RoleBinding

RoleBinding:
> Assigns Role to a user or ServiceAccount

---

## Example

```yaml
kind: Role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list"]

Production Insight

Use Roles for application-level permissions.


---

## 📄 `05-kubernetes/06-security/clusterrole.md`

```md
# ClusterRole & ClusterRoleBinding

## Why ClusterRole Exists

Some resources are cluster-wide:
- Nodes
- PersistentVolumes
- Namespaces

---

## ClusterRole

Defines permissions across entire cluster.

---

## Production Rule

Grant cluster-level access very carefully.

📄 05-kubernetes/06-security/network-policy.md
# NetworkPolicy – Pod-to-Pod Security

## Default Kubernetes Networking

By default:
- All pods can talk to all pods

This is dangerous.

---

## What NetworkPolicy Does

NetworkPolicy:
> Restricts traffic between pods.

---

## Example Use

- Frontend can talk to backend
- Backend cannot talk to database directly

---

## Production Insight

Zero-trust networking starts here.

📄 05-kubernetes/06-security/pod-security-standards.md
# Pod Security Standards (PSS)

## Why PSS Exists

Containers can:
- Run as root
- Access host filesystem
- Escalate privileges

---

## Pod Security Levels

- Privileged
- Baseline
- Restricted

---

## Production Rule

Most workloads should be:
> Restricted or Baseline

📄 05-kubernetes/06-security/secrets.md
# Secrets – Sensitive Data Handling

## What Secrets Store

- Passwords
- Tokens
- API keys
- Certificates

---

## Important Warning

Secrets are:
- Base64 encoded
- NOT encrypted by default

---

## Production Best Practice

Use:
- KMS encryption
- External secret managers

📄 05-kubernetes/06-security/irsa.md
# IRSA – IAM Roles for Service Accounts (EKS)

## The Old Problem

Pods needed AWS access.
Credentials were stored as secrets.
This was insecure.

---

## What IRSA Solves

IRSA allows:
> Pods to assume IAM roles directly.

No static credentials.
No secrets.

---

## How It Works

ServiceAccount
→ IAM Role
→ Temporary AWS credentials

---

## Production Rule

Always use IRSA in EKS.

📄 05-kubernetes/06-security/real-production-security.md
# Kubernetes Security – Production Mindset

## Layered Security Model

1. Authentication
2. Authorization (RBAC)
3. Network isolation
4. Pod security
5. Secret management

---

## Golden Rules

- Least privilege always
- Separate namespaces
- Audit logs enabled
- No admin access for apps

Security is not one feature.
It is a system.

✅ WHERE YOU ARE NOW

You now deeply understand Kubernetes security:

✔ Authentication vs authorization
✔ ServiceAccounts
✔ RBAC (Role, RoleBinding, ClusterRole)
✔ NetworkPolicies
✔ Pod security
✔ IRSA (AWS critical)

This is real production-grade Kubernetes security knowledge.
