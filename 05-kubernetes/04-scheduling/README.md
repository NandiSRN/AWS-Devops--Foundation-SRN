05-kubernetes/04-scheduling/README.md
# Kubernetes Scheduling – How Pods Get a Node

Scheduling answers one question:

👉 **On which node should this pod run?**

Kubernetes does NOT randomly place pods.

It considers:
- Resources
- Labels
- Constraints
- Preferences
- Restrictions

Bad scheduling = outages, noisy neighbors, wasted money.

📄 05-kubernetes/04-scheduling/scheduler-basics.md
# Kubernetes Scheduler – From Zero

## What the Scheduler Is

The kube-scheduler is a control-plane component.

Its job:
> Assign a node to every pod that does not have one.

---

## When Scheduling Happens

Scheduling occurs when:
- A new pod is created
- A pod is rescheduled after node failure

---

## High-Level Flow

1. Pod created
2. Scheduler filters nodes
3. Scheduler scores nodes
4. Best node selected
5. Pod bound to node

---

## Important Truth

Once scheduled, pods do NOT move automatically.

📄 05-kubernetes/04-scheduling/node-labels.md
# Node Labels – Attaching Meaning to Nodes

## What Labels Are

Labels are:
> Key-value metadata attached to nodes.

Example:


environment=prod
disk=ssd


---

## Why Labels Matter

Labels allow:
- Targeted scheduling
- Environment separation
- Hardware-based placement

---

## Real Production Use

- prod vs non-prod nodes
- GPU nodes
- SSD-backed nodes

📄 05-kubernetes/04-scheduling/node-selector.md
# Node Selector – Simple Scheduling Constraint

## What NodeSelector Does

NodeSelector:
> Forces pod to run ONLY on matching nodes.

---

## Example

```yaml
spec:
  nodeSelector:
    environment: prod

Behavior

Hard requirement

If no node matches → pod stays Pending

Production Insight

NodeSelector is simple but limited.
No logic.
No preference.


---

## 📄 `05-kubernetes/04-scheduling/node-affinity.md`

```md
# Node Affinity – Advanced Node Selection

## Why Node Affinity Exists

NodeSelector is too basic.

Node Affinity adds:
- Expressions
- Preferences
- Soft vs hard rules

---

## Types of Node Affinity

### RequiredDuringScheduling
Hard rule.
Must match.

### PreferredDuringScheduling
Soft rule.
Best effort.

---

## Example

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: environment
          operator: In
          values:
          - prod

Production Insight

Use:

Required → compliance

Preferred → optimization


---

## 📄 `05-kubernetes/04-scheduling/pod-affinity.md`

```md
# Pod Affinity & Anti-Affinity

## What Pod Affinity Does

Controls:
> Which pods should run together or apart.

---

## Use Cases

- Spread replicas across nodes
- Keep related services close

---

## Example Use

Anti-affinity:
- Avoid all replicas on same node

---

## Production Insight

Critical for:
- High availability
- Fault tolerance

📄 05-kubernetes/04-scheduling/taints.md
# Taints – Repelling Pods from Nodes

## What Taints Do

Taints say:
> "Pods are NOT allowed here unless they tolerate me."

---

## Example Taint

```bash
kubectl taint nodes node1 dedicated=prod:NoSchedule

Effects

NoSchedule

PreferNoSchedule

NoExecute

Production Use

Dedicated nodes

Control-plane isolation


---

## 📄 `05-kubernetes/04-scheduling/tolerations.md`

```md
# Tolerations – Allowing Pods onto Tainted Nodes

## What Tolerations Do

Tolerations:
> Allow pods to bypass taints.

---

## Example

```yaml
tolerations:
- key: "dedicated"
  operator: "Equal"
  value: "prod"
  effect: "NoSchedule"

Key Insight

Toleration ≠ placement.
It only allows scheduling.


---

## 📄 `05-kubernetes/04-scheduling/resource-requests-limits.md`

```md
# Resource Requests & Limits

## Requests

Minimum resources required.

Used by:
- Scheduler

---

## Limits

Maximum allowed usage.

Enforced by:
- Kubelet
- Container runtime

---

## Example

```yaml
resources:
  requests:
    cpu: "500m"
    memory: "512Mi"
  limits:
    cpu: "1"
    memory: "1Gi"

Production Rule

Always define requests.


---

## 📄 `05-kubernetes/04-scheduling/qos-classes.md`

```md
# QoS Classes – Quality of Service

## QoS Types

### Guaranteed
Requests = Limits

### Burstable
Requests < Limits

### BestEffort
No requests or limits

---

## Why QoS Matters

During pressure:
- BestEffort pods die first
- Guaranteed pods survive longest

---

## Production Rule

Critical workloads should be Guaranteed or Burstable.

📄 05-kubernetes/04-scheduling/real-production-patterns.md
# Scheduling in Real Production

## Common Pattern

- Label nodes by environment
- Taint prod nodes
- Use affinity + tolerations
- Define resource requests

---

## Why This Matters

Good scheduling:
- Improves stability
- Reduces cost
- Prevents noisy neighbors

✅ WHERE YOU ARE NOW

You now truly understand scheduling:

✔ How scheduler thinks
✔ Labels & selectors
✔ Node & pod affinity
✔ Taints & tolerations
✔ Requests, limits & QoS

This is interview-grade + production-grade knowledge.
