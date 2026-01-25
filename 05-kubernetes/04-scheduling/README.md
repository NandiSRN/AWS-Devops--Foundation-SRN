Kubernetes Scheduling
From First Principles to Production Reality

A practical, production-grade explanation of how Kubernetes schedules pods — from basic concepts to advanced placement, isolation, and eviction behavior.

Table of Contents

Mental Model

Node Labels

Node Selector

Node Affinity

Pod Affinity

Pod Anti-Affinity

Taints

Tolerations

Resource Requests

Resource Limits

QoS Classes

Complete Production YAML

Final Mapping & Takeaways

1. Mental Model

Think in terms of a real data center.

You have multiple machines (nodes).
Each node has CPU, memory, disk, and possibly special hardware.

You deploy applications as pods. Kubernetes must:

Decide where a pod runs

Enforce placement rules

Respect resource availability

Handle runtime pressure

Decide eviction priority

Scheduler Phases
Phase	Responsibility
Filtering	Which nodes are allowed
Scoring	Which allowed node is best
Binding	Pod is assigned to a node
2. Node Labels

Node labels are metadata attached to nodes.

kubectl label node node-1 disktype=ssd
kubectl label node node-2 disktype=hdd


Key facts:

Labels do nothing by themselves

Scheduler never creates labels

Labels must exist before scheduling

Existing pods are not rescheduled if labels change

3. Node Selector

Node Selector is the simplest scheduling filter.

spec:
  nodeSelector:
    disktype: ssd


Behavior:

Nodes without the label are discarded

No match → pod remains Pending

Limitations:

No OR logic

No weights

No expressions

Used mainly in small or legacy setups.

4. Node Affinity

Node Affinity provides advanced placement control.

Required Node Affinity (Hard Rule)
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd


If no node matches → pod is never scheduled.

Preferred Node Affinity (Soft Rule)
preferredDuringSchedulingIgnoredDuringExecution:
- weight: 80
  preference:
    matchExpressions:
    - key: topology.kubernetes.io/zone
      operator: In
      values:
      - ap-south-1a


Used only during scoring.
Never blocks scheduling.

5. Pod Affinity

Pod Affinity defines relationships between pods.

podAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - labelSelector:
      matchLabels:
        app: backend
    topologyKey: kubernetes.io/hostname

Common Topology Keys
Key	Scope
kubernetes.io/hostname	Same node
topology.kubernetes.io/zone	Same AZ
topology.kubernetes.io/region	Same region
6. Pod Anti-Affinity

Ensures pods do not run together.

podAntiAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - labelSelector:
      matchLabels:
        app: frontend
    topologyKey: kubernetes.io/hostname


Used for:

High availability

Failure isolation

Blast-radius reduction

7. Taints

Taints allow nodes to reject pods.

kubectl taint node node-1 dedicated=database:NoSchedule

Taint Effects
Effect	Behavior
NoSchedule	Pod is blocked
PreferNoSchedule	Avoid if possible
NoExecute	Evict running pods
8. Tolerations

Tolerations allow pods to accept tainted nodes.

tolerations:
- key: dedicated
  operator: Equal
  value: database
  effect: NoSchedule


Pods without tolerations are rejected.

9. Resource Requests

Requests are the scheduler’s currency.

resources:
  requests:
    cpu: "500m"
    memory: "512Mi"


Key rule:

Scheduler uses requests only

Scheduler ignores limits completely

10. Resource Limits

Limits are enforced at runtime.

limits:
  cpu: "1"
  memory: "1Gi"


Behavior:

Resource	Result
CPU	Throttled
Memory	OOMKilled
11. QoS Classes

QoS is assigned automatically.

Class	Description	Eviction Priority
Guaranteed	requests == limits	Last
Burstable	requests < limits	Medium
BestEffort	no requests	First

Eviction order:

BestEffort → Burstable → Guaranteed

12. Complete Production Pod YAML
apiVersion: v1
kind: Pod
metadata:
  name: payments-api
  labels:
    app: payments
spec:
  nodeSelector:
    nodepool: backend

  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values: [ssd]

      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 80
        preference:
          matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values: [ap-south-1a]

    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: payments
        topologyKey: kubernetes.io/hostname

  tolerations:
  - key: dedicated
    operator: Equal
    value: backend
    effect: NoSchedule

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

13. Final Takeaways

Scheduler filters → scores → binds

Limits do not affect scheduling

Requests determine placement

BestEffort pods are eviction victims

Affinity and taints control real production behavior
