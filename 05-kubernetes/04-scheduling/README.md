Kubernetes Scheduling — From First Principles to Production Reality

This document explains how Kubernetes schedules pods in real production environments, from absolute basics to advanced placement, isolation, and eviction behavior.

If you understand everything in this README, you can:

Debug Pending pods confidently

Design highly available workloads

Reason about scheduler decisions under pressure

Build production-safe Kubernetes manifests

0️⃣ Mental Model (Lock This In First)

Imagine a real data center.

You have many machines (nodes).

Each node has:

CPU

Memory

Disk

Network

Special properties (GPU, SSD, AZ, instance type, dedicated purpose)

You deploy applications as pods, and Kubernetes must:

Decide which node gets the pod

Enforce placement rules

Respect resource availability

Handle runtime pressure

Decide who gets evicted first

All Kubernetes scheduling logic fits into three logical phases.

Kubernetes Scheduler Phases
Phase	Responsibility
Filtering	Which nodes are even allowed
Scoring	Which allowed node is best
Binding / Enforcement	Pod is bound; runtime rules apply

Keep this model in your head. Everything below maps to one of these phases.

1️⃣ Node Labels (The Absolute Foundation)
What Is a Node Label?

A node label is simple metadata attached to a node.

kubectl label node node-1 disktype=ssd
kubectl label node node-2 disktype=hdd


Result:

node-1 → disktype=ssd

node-2 → disktype=hdd

Critical Production Truths

Labels do nothing by themselves

They are only selectors

The scheduler never invents labels

Labels must exist before scheduling

If labels change later, existing pods are not rescheduled

📌 Think of labels as stickers on machines.

2️⃣ Node Selector (Simple, Strict, Legacy)
What It Is

A hard filter:

“This pod MUST run on a node with this label — or never run.”

spec:
  nodeSelector:
    disktype: ssd

Scheduler Behavior

Scheduler scans all nodes

Removes nodes without disktype=ssd

If zero nodes remain → pod stays Pending forever

Production Reality

❌ No OR logic
❌ No weights
❌ No expressions
❌ No fallback

✅ Fast
✅ Predictable

⚠️ Mostly avoided in modern production
➡️ Node Affinity replaced it

3️⃣ Node Affinity (Real Scheduling Control)

Node Affinity = NodeSelector on steroids.

Two types:

Required → hard rule (filtering)

Preferred → soft rule (scoring)

3.1 Required Node Affinity (Hard Gate)
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd


Meaning (Plain English):

“If a node does NOT match this rule, the pod will NEVER be scheduled there.”

Operators You Must Know
Operator	Meaning
In	Value must match
NotIn	Value must not match
Exists	Key must exist
DoesNotExist	Key must not exist
Production Use Cases

GPU workloads

AZ-specific services

Dedicated node pools

Compliance / isolation requirements

3.2 Preferred Node Affinity (Soft Scoring)
preferredDuringSchedulingIgnoredDuringExecution:
- weight: 80
  preference:
    matchExpressions:
    - key: topology.kubernetes.io/zone
      operator: In
      values:
      - ap-south-1a


Meaning:

“Try to place here first — but fall back if necessary.”

Key points:

Weight range: 1–100

Used only during scoring

Never blocks scheduling

📌 Preferred affinity is optimization, not safety.

4️⃣ Pod Affinity (Pods Attract Each Other)

Now we stop selecting nodes
and start defining relationships between pods.

What Is Pod Affinity?

“Place this pod near another pod.”

podAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - labelSelector:
      matchLabels:
        app: backend
    topologyKey: kubernetes.io/hostname


Meaning:

“Run this pod on the same node as pods with app=backend.”

topologyKey (Extremely Important)
Key	Scope
kubernetes.io/hostname	Same node
topology.kubernetes.io/zone	Same AZ
topology.kubernetes.io/region	Same region

⚠️ Overusing pod affinity reduces scheduler freedom
➡️ Leads to Pending pods.

5️⃣ Pod Anti-Affinity (Pods Repel Each Other)

Opposite of affinity.

podAntiAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - labelSelector:
      matchLabels:
        app: frontend
    topologyKey: kubernetes.io/hostname


Meaning:

“Never place two frontend pods on the same node.”

Why This Exists (Real Production Reason)

High availability

Blast-radius reduction

Node failure protection

This is mandatory for HA workloads.

6️⃣ Taints (Node Rejects Pods)

Until now:

Pods were choosing nodes

Now:

Nodes reject pods

What Is a Taint?

A node-level rejection rule.

kubectl taint node node-1 dedicated=database:NoSchedule


Meaning:

“I repel ALL pods unless they explicitly tolerate me.”

Taint Format
key=value:effect

Taint Effects (Critical)
Effect	Behavior
NoSchedule	Pod will not schedule
PreferNoSchedule	Avoid if possible
NoExecute	Evict existing pods
7️⃣ Tolerations (Pod Accepts the Risk)
tolerations:
- key: "dedicated"
  operator: "Equal"
  value: "database"
  effect: "NoSchedule"

Scheduler Logic
Node Tainted	Pod Tolerates	Result
Yes	No	❌ Rejected
Yes	Yes	✅ Allowed
No	N/A	✅ Allowed
Production Use Cases

Dedicated DB nodes

Infra / system workloads

GPU isolation

Platform services

8️⃣ Resource Requests (Scheduling Currency)
resources:
  requests:
    cpu: "500m"
    memory: "512Mi"


Core Rule:

Scheduler uses ONLY requests
Scheduler ignores limits completely

If a node cannot satisfy requests:

Pod is not scheduled

No requests → BestEffort class

9️⃣ Resource Limits (Runtime Enforcement)
limits:
  cpu: "1"
  memory: "1Gi"

Resource	When Exceeded
CPU	Throttled
Memory	OOMKilled
Production Reality

CPU limits are soft

Memory limits are lethal

Many teams:

Always set requests

Often avoid CPU limits

Carefully tune memory limits

🔟 QoS Classes (Who Dies First)

QoS is assigned automatically.

1️⃣ Guaranteed (VIP)

requests == limits (CPU & memory)

✅ Last to be killed
❌ Can waste capacity

2️⃣ Burstable (Most Production Workloads)

requests < limits

✅ Efficient
⚠️ Medium eviction priority

3️⃣ BestEffort (Eviction Victims)

No requests

No limits

❌ First to die
❌ No guarantees
❌ Highly unstable

Eviction Order (Critical)
BestEffort → Burstable → Guaranteed

🔥 Full Production-Grade Pod YAML (All Concepts Combined)
apiVersion: v1
kind: Pod
metadata:
  name: payments-api
  labels:
    app: payments
    tier: backend
spec:

  # Node Selector (simple hard filter)
  nodeSelector:
    nodepool: backend

  affinity:
    nodeAffinity:
      # Required Node Affinity (hard rule)
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd

      # Preferred Node Affinity (soft rule)
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 80
        preference:
          matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values:
            - ap-south-1a

    # Pod Affinity (soft attraction)
    podAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 50
        podAffinityTerm:
          labelSelector:
            matchLabels:
              app: redis
          topologyKey: topology.kubernetes.io/zone

    # Pod Anti-Affinity (hard separation)
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: payments
        topologyKey: kubernetes.io/hostname

  # Toleration for tainted nodes
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "backend"
    effect: "NoSchedule"

  containers:
  - name: payments-api
    image: myrepo/payments-api:1.0.0
    resources:
      requests:
        cpu: "500m"
        memory: "512Mi"
      limits:
        cpu: "1"
        memory: "1Gi"

🧠 Final Production Mapping
Feature	Controls
Node Labels	Node metadata
Node Selector	Simple hard filter
Required Node Affinity	Hard placement rules
Preferred Node Affinity	Soft placement preferences
Pod Affinity	Pods attract
Pod Anti-Affinity	Pods repel
Taints	Node rejects pods
Tolerations	Pod bypasses rejection
Requests	Scheduling eligibility
Limits	Runtime enforcement
QoS	Eviction priority
🧯 Production Truths (Read This Twice)

Scheduler filters → scores → binds

Scheduler never considers limits

Pending pods are usually caused by:

Affinity conflicts

Missing tolerations

Insufficient requests capacity

BestEffort pods are designed to die first
