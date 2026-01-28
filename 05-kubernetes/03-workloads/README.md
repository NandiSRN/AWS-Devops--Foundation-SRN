📁 05-kubernetes/03-workloads/README.md
# Kubernetes Workloads – How Applications Run

Kubernetes does not run applications directly.
It runs **workload objects** that describe:

- What to run
- How many copies
- How to recover from failure
- How updates should happen

If you understand workloads,
you understand Kubernetes.

📄 05-kubernetes/03-workloads/pod.md
# Pod – The Smallest Deployable Unit

## Start From Zero

A Pod is NOT a container.

A Pod is:
> A wrapper around one or more containers.

---

## Why Pods Exist

Containers alone:
- Cannot share networking cleanly
- Cannot share storage easily

Pods solve this by:
- Sharing IP address
- Sharing volumes
- Sharing lifecycle

---

## Pod Characteristics

- One IP per pod
- Containers inside pod communicate via localhost
- Pods are ephemeral

---

## Important Truth

Pods are NOT meant to be created manually in production.
They are managed by higher-level controllers.

📄 05-kubernetes/03-workloads/replicaset.md
# ReplicaSet – Keeping Pods Alive

## The Problem ReplicaSet Solves

Pods can:
- Crash
- Be deleted
- Lose nodes

ReplicaSet ensures:
> A fixed number of pod replicas always exist.

---

## How ReplicaSet Works

- Watches pod count
- Creates pods if missing
- Deletes extra pods if needed

---

## Production Reality

You almost never create ReplicaSets directly.
They are created by Deployments.

📄 05-kubernetes/03-workloads/deployment.md
# Deployment – Managing Stateless Applications

## What Deployment Is

Deployment is:
> The most common Kubernetes workload.

It manages:
- ReplicaSets
- Rolling updates
- Rollbacks

---

## Why Deployments Exist

They solve:
- Zero-downtime deployments
- Version control
- Scaling

---

## Example Deployment (Simple)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:1.25
        ports:
        - containerPort: 80

Production Insight

