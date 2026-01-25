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

Deployments are ideal for:

APIs

Web apps

Microservices

Stateless only.


---



## 📄 `05-kubernetes/03-workloads/statefulset.md`

```md
# StatefulSet – Managing Stateful Applications

## The Problem StatefulSet Solves

Some apps need:
- Stable network identity
- Stable storage
- Ordered startup

Deployments cannot guarantee this.

---

## What StatefulSet Provides

- Fixed pod names (pod-0, pod-1)
- Stable DNS
- Persistent volumes per pod
- Ordered scaling

---

## Common Use Cases

- Databases
- Kafka
- Zookeeper
- Elasticsearch

---

## Production Rule

Never run databases using Deployments.
Use StatefulSets.

📄 05-kubernetes/03-workloads/daemonset.md
# DaemonSet – One Pod Per Node

## What DaemonSet Does

DaemonSet ensures:
> One pod runs on every node.

---

## Why This Is Needed

Some workloads are node-specific:
- Log collectors
- Monitoring agents
- Network plugins

---

## Examples

- Fluent Bit
- Node Exporter
- CNI agents

---

## Production Insight

DaemonSets scale automatically as nodes are added.

📄 05-kubernetes/03-workloads/job.md
# Job – Run Once Tasks

## What a Job Is

A Job:
> Runs a task until completion.

---

## Use Cases

- Database migrations
- Batch processing
- One-time scripts

---

## Behavior

- Retries on failure
- Stops after success

---

## Production Rule

Jobs should be idempotent.

📄 05-kubernetes/03-workloads/cronjob.md
# CronJob – Scheduled Jobs

## What CronJob Does

CronJob:
> Runs Jobs on a schedule.

---

## Example Use Cases

- Nightly backups
- Cleanup tasks
- Report generation

---

## Cron Format

```text
0 2 * * *


Runs every day at 2 AM.

Production Insight

Monitor CronJobs.
Silent failures are common.


---

## 📄 `05-kubernetes/03-workloads/workload-comparison.md`

```md
# Choosing the Right Workload Type

| Workload | Use Case |
|--------|---------|
| Pod | Testing only |
| Deployment | Stateless apps |
| StatefulSet | Databases |
| DaemonSet | Node-level agents |
| Job | One-time tasks |
| CronJob | Scheduled tasks |

---

## Golden Rule

Stateless → Deployment  
Stateful → StatefulSet

📄 05-kubernetes/03-workloads/production-mindset.md
# Kubernetes Workloads – Production Mindset

Kubernetes is declarative.

You tell Kubernetes:
> “This is what I want.”

Kubernetes figures out:
> “How to make it happen.”

---

## Golden Rules

- Never manage pods manually
- Always define desired state
- Design for failure
- Monitor everything

Workloads fail.
Controllers recover.

✅ WHERE WE ARE NOW

You now deeply understand Kubernetes workloads:

✔ Pod vs controllers
✔ Deployment vs StatefulSet
✔ DaemonSet logic
✔ Jobs & CronJobs
✔ Production patterns

This is real production Kubernetes knowledge.
