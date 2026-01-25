📁 05-kubernetes/02-networking/README.md
# Kubernetes Networking – How Pods Talk to Each Other

Kubernetes networking answers four critical questions:

1. How do pods communicate with each other?
2. How do pods communicate across nodes?
3. How do users access applications?
4. How does DNS work inside the cluster?

If networking is broken:
- Pods run but apps don’t work
- Services fail silently
- Ingress shows 502 / timeout errors

Networking is the nervous system of Kubernetes.

📄 05-kubernetes/02-networking/pod-networking.md
# Pod Networking – The Fundamental Rule

## The Most Important Kubernetes Networking Rule

👉 **Every Pod gets its own IP address**

This IP is:
- Unique across the entire cluster
- Routable to every other pod

No NAT.
No port mapping.
No localhost tricks.

---

## Why This Rule Exists

Kubernetes treats pods like:
> Virtual machines on a flat network

This simplifies:
- Service discovery
- Scaling
- Debugging

---

## Pod-to-Pod Communication

Pod A → Pod B  
Works:
- Same node
- Different node

Without any proxies.

---

## Production Insight

If pod IPs are NOT routable:
- Kubernetes networking is broken

📄 05-kubernetes/02-networking/cni.md
# CNI – Container Network Interface

## Why CNI Exists

Kubernetes does NOT implement networking itself.

Instead, it defines rules and lets plugins implement them.

This is CNI.

---

## What CNI Does

CNI plugin:
- Assigns IPs to pods
- Configures routing
- Enables cross-node communication

---

## Common CNI Plugins

- AWS VPC CNI (EKS)
- Calico
- Flannel
- Cilium

---

## AWS VPC CNI (EKS Insight)

Pods get:
- IPs from VPC subnet
- Direct AWS network routing

This improves performance but consumes IPs.

📄 05-kubernetes/02-networking/services.md
# Kubernetes Services – Stable Access to Pods

## The Problem Services Solve

Pods:
- Are ephemeral
- Get destroyed and recreated
- IPs change frequently

Clients need a stable endpoint.

---

## What a Service Is

A Service:
> A stable virtual IP that routes to pods.

It selects pods using labels.

---

## Service Never Talks Directly

Traffic flow:
Client → Service → Pod

Service load-balances automatically.

📄 05-kubernetes/02-networking/clusterip.md
# ClusterIP – Internal Services

## What ClusterIP Is

ClusterIP:
- Default service type
- Internal-only access

Only reachable inside cluster.

---

## Use Cases

- Backend services
- Internal APIs
- Databases

---

## Production Rule

Most services should be ClusterIP.

📄 05-kubernetes/02-networking/nodeport.md
# NodePort – Exposing Services via Nodes

## What NodePort Does

NodePort:
- Exposes service on every node IP
- Uses fixed port range (30000–32767)

---

## Example



NodeIP:30080 → Service → Pod


---

## Problems with NodePort

- No TLS
- No hostname routing
- Manual port management

---

## Production Insight

NodePort is rarely used directly in production.

📄 05-kubernetes/02-networking/loadbalancer.md
# LoadBalancer – Cloud-Native Exposure

## What LoadBalancer Does

Creates:
- Cloud load balancer (ALB/NLB)
- Routes traffic to service

---

## AWS EKS Example

Service type LoadBalancer:
→ AWS NLB created automatically

---

## Production Use

- Simple public services
- Non-HTTP workloads

Ingress is still preferred for HTTP.

📄 05-kubernetes/02-networking/headless-service.md
# Headless Services – No Load Balancing

## What Headless Means

A headless service:
- Has no ClusterIP
- Directly returns pod IPs

---

## Why This Exists

Some apps need:
- Pod identity
- Direct connections
- Stable network identity

---

## Common Use Case

StatefulSets:
- Databases
- Kafka
- Zookeeper

📄 05-kubernetes/02-networking/ingress.md
# Ingress – HTTP Routing Layer

## Why Ingress Exists

Using one LoadBalancer per service:
- Is expensive
- Is hard to manage

Ingress provides:
> Central HTTP routing

---

## What Ingress Is

Ingress:
- Defines routing rules
- Uses hostnames and paths

Ingress does NOT handle traffic itself.

📄 05-kubernetes/02-networking/ingress-controller.md
# Ingress Controller – The Real Worker

## Critical Truth

Ingress without controller = useless YAML.

---

## What Controller Does

Ingress Controller:
- Watches Ingress resources
- Configures proxy (NGINX / ALB)
- Routes traffic

---

## Common Controllers

- NGINX Ingress
- AWS Load Balancer Controller
- Traefik

---

## Production Rule

Always know which controller you are using.

📄 05-kubernetes/02-networking/coredns.md
# CoreDNS – Cluster DNS Brain

## What CoreDNS Does

CoreDNS:
- Resolves service names
- Resolves pod DNS
- Enables service discovery

---

## Example Resolution



backend.default.svc.cluster.local


---

## Without CoreDNS

- Services unreachable by name
- Applications fail silently

---

## Production Insight

DNS issues are among the hardest to debug.

📄 05-kubernetes/02-networking/networking-flow.md
# Kubernetes Networking Flow (End-to-End)

User
 ↓
DNS (Route53)
 ↓
Load Balancer
 ↓
Ingress Controller
 ↓
Service
 ↓
Pod

Every layer must work.

Failure anywhere = outage.

✅ WHERE WE ARE NOW

You now deeply understand:

✔ Pod networking rules
✔ CNI and AWS VPC CNI
✔ Services (ClusterIP, NodePort, LoadBalancer)
✔ Headless services
✔ Ingress & controllers
✔ CoreDNS

This is production-grade Kubernetes networking knowledge.
