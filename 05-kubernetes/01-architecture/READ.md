📁 05-kubernetes/01-architecture/README.md
# Kubernetes Architecture – From Zero (Deep Dive)

Kubernetes is NOT a deployment tool.
Kubernetes is NOT Docker replacement.

Kubernetes is:
> A distributed system for running containers reliably at scale.

It solves problems that appear when:
- You have many containers
- Across many machines
- With failures happening all the time

Kubernetes assumes failure.
And it is designed to survive it.

📄 05-kubernetes/01-architecture/kubernetes-overview.md
# Kubernetes – What Problem Does It Solve?

## Before Kubernetes

Teams faced:
- Manual container restarts
- No self-healing
- No scheduling logic
- No service discovery
- No scaling automation

Containers alone were not enough.

---

## What Kubernetes Does

Kubernetes:
- Schedules containers
- Restarts failed workloads
- Scales applications
- Handles networking
- Manages configuration

Kubernetes acts as:
> An operating system for your cluster.

📄 05-kubernetes/01-architecture/control-plane.md
# Control Plane – The Brain of Kubernetes

The control plane makes decisions.

It:
- Knows desired state
- Knows current state
- Works continuously to reconcile both

If control plane is down:
> Cluster becomes read-only.

🔹 kube-apiserver (MOST IMPORTANT COMPONENT)
📄 05-kubernetes/01-architecture/kube-apiserver.md
# kube-apiserver – The Front Door of Kubernetes

Nothing happens in Kubernetes without the API server.

---

## What It Is

kube-apiserver is:
> The central management point of the cluster.

Every request goes through it.

---

## Responsibilities

- Accepts all requests:
  - kubectl
  - controllers
  - scheduler
  - kubelet
- Authenticates users
- Authorizes actions (RBAC)
- Validates requests
- Persists state to etcd

---

## What Happens If API Server Is Down?

- kubectl stops working
- Controllers stop reacting
- Nodes keep running current pods
- No new changes allowed

📌 **If API server is down → cluster is effectively dead**

---

## Production Insight

- Must be highly available
- Must be protected
- Must be monitored

🔹 etcd – Cluster Memory
📄 05-kubernetes/01-architecture/etcd.md
# etcd – Source of Truth

## What etcd Is

etcd is:
> A distributed key-value store.

It stores:
- Cluster state
- Configurations
- Secrets
- Desired workloads

---

## What etcd Does NOT Do

- Does not schedule pods
- Does not run containers

It only stores data.

---

## Failure Scenario

If etcd is lost:
- Cluster state is lost
- Recovery is extremely difficult

---

## Production Rule

- etcd backups are mandatory
- etcd access must be restricted

🔹 kube-scheduler – Pod Placement Brain
📄 05-kubernetes/01-architecture/kube-scheduler.md
# kube-scheduler – Deciding Where Pods Run

## The Scheduling Problem

When a pod is created:
- Which node should run it?

Scheduler answers this.

---

## What Scheduler Considers

- CPU availability
- Memory availability
- Node labels
- Taints & tolerations
- Affinity rules

---

## What Scheduler Does NOT Do

- Does not start containers
- Does not monitor runtime

It only decides placement.

---

## Production Insight

Bad scheduling rules = inefficient clusters.

🔹 kube-controller-manager – Reconciliation Engine
📄 05-kubernetes/01-architecture/kube-controller-manager.md
# kube-controller-manager – Making Reality Match Desired State

## What Controllers Do

Controllers:
- Watch cluster state
- Detect drift
- Take corrective action

---

## Examples

- Pod dies → controller creates new pod
- Node fails → pods rescheduled
- Replicas mismatch → fixed automatically

---

## Key Insight

Controllers NEVER stop working.
They constantly reconcile state.

🔹 cloud-controller-manager
📄 05-kubernetes/01-architecture/cloud-controller-manager.md
# cloud-controller-manager – Cloud Integration

## Why It Exists

Kubernetes must work across clouds.

Cloud controller:
- Integrates cloud APIs
- Manages load balancers
- Handles node lifecycle

---

## AWS Example

- Creates ALB/NLB
- Attaches EBS volumes
- Manages node IPs

🔹 Worker Nodes – Where Work Happens
📄 05-kubernetes/01-architecture/worker-node.md
# Worker Nodes – Running the Workloads

Worker nodes:
- Run containers
- Host pods
- Execute workloads

They are the muscle.

🔹 kubelet
📄 05-kubernetes/01-architecture/kubelet.md
# kubelet – Node Agent

## What kubelet Does

kubelet:
- Talks to API server
- Starts containers
- Monitors pod health
- Reports node status

---

## If kubelet Stops

- Node becomes NotReady
- Pods are rescheduled

🔹 kube-proxy
📄 05-kubernetes/01-architecture/kube-proxy.md
# kube-proxy – Network Traffic Manager

## What kube-proxy Does

kube-proxy:
- Manages service networking
- Handles IP tables
- Routes traffic to pods

---

## Key Insight

Without kube-proxy:
- Services break
- Pod-to-pod traffic fails

🔹 Container Runtime
📄 05-kubernetes/01-architecture/container-runtime.md
# Container Runtime – Running Containers

## What It Is

Container runtime:
- Pulls images
- Starts containers
- Stops containers

Examples:
- containerd
- CRI-O

Docker is NOT required anymore.

📄 05-kubernetes/01-architecture/request-flow.md
# Kubernetes Request Flow (Very Important)

kubectl apply
 ↓
kube-apiserver
 ↓
etcd (store desired state)
 ↓
scheduler (select node)
 ↓
kubelet (start container)
 ↓
container runtime

This is the life of every pod.

✅ WHERE WE ARE NOW

You now understand:

✔ What Kubernetes is
✔ Why it exists
✔ Control plane components
✔ Worker node components
✔ Failure scenarios
✔ Real production behavior

This is senior-level Kubernetes foundation.
