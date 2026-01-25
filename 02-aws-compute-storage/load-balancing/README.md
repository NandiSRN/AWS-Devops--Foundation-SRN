Think of this as:
👉 “How traffic is safely distributed to servers without users noticing failures.”

📁 02-aws-compute-storage/load-balancing/README.md
# Load Balancing – Handling Traffic the Right Way

Applications rarely run on a single server in production.

Why?
- Servers fail
- Traffic spikes
- Deployments cause restarts

Load balancers exist to:
- Distribute traffic
- Detect unhealthy servers
- Provide high availability

Without load balancing:
- Downtime is guaranteed
- Scaling is impossible

📄 02-aws-compute-storage/load-balancing/what-is-load-balancing.md
# What Is Load Balancing? (From Zero)

## Start With a Simple Problem

You have:
- 1 application
- 10,000 users
- 1 server

What happens?
- CPU spikes
- Requests fail
- Server crashes

---

## What Load Balancing Does

A load balancer:
- Sits in front of servers
- Receives all client traffic
- Distributes requests across servers

Clients never talk to servers directly.

---

## Core Responsibilities

A load balancer:
- Distributes traffic
- Performs health checks
- Removes unhealthy servers
- Improves availability

---

## Key Insight

Users talk to the **load balancer**, not to servers.

📄 02-aws-compute-storage/load-balancing/elb-overview.md
# AWS Elastic Load Balancing (ELB)

AWS provides managed load balancers so you don’t have to:
- Install software
- Maintain HA
- Handle failover

AWS manages:
- Scaling
- Availability
- Fault tolerance

---

## Types of AWS Load Balancers

1. Classic Load Balancer (CLB)
2. Application Load Balancer (ALB)
3. Network Load Balancer (NLB)

Each serves a different purpose.

📄 02-aws-compute-storage/load-balancing/clb.md
# Classic Load Balancer (CLB)

## What CLB Is

CLB is:
- Legacy load balancer
- Operates at Layer 4 & 7
- Older AWS service

---

## Why CLB Is Rarely Used Now

CLB:
- Lacks modern features
- Limited routing
- Not Kubernetes-friendly

---

## Production Rule

Do NOT use CLB for new systems.
It exists only for legacy workloads.

📄 02-aws-compute-storage/load-balancing/alb.md
# Application Load Balancer (ALB)

## What ALB Is

ALB is a:
> Layer 7 (HTTP/HTTPS) load balancer

It understands:
- URLs
- Hostnames
- HTTP headers

---

## Why ALB Is Popular

ALB supports:
- Path-based routing
- Host-based routing
- TLS termination
- WebSockets
- Kubernetes Ingress

---

## Example Routing



/api → backend service
/web → frontend service


---

## Production Use Cases

ALB is used for:
- Web applications
- Microservices
- APIs
- EKS Ingress

---

## Interview One-Liner

> ALB operates at Layer 7 and routes traffic based on HTTP attributes like path and host.

📄 02-aws-compute-storage/load-balancing/nlb.md
# Network Load Balancer (NLB)

## What NLB Is

NLB is a:
> Layer 4 (TCP/UDP) load balancer

It does NOT understand HTTP.

---

## Why NLB Exists

NLB provides:
- Ultra-low latency
- High throughput
- Static IP support

---

## Use Cases

NLB is used for:
- TCP services
- Databases
- gRPC
- TLS passthrough

---

## Production Insight

Use NLB when:
- Performance matters
- Protocol is non-HTTP

📄 02-aws-compute-storage/load-balancing/target-groups.md
# Target Groups – Where Traffic Goes

## What Is a Target Group?

A target group defines:
> The destination of load-balanced traffic.

Targets can be:
- EC2 instances
- IP addresses
- Kubernetes pods

---

## Health Checks

Load balancer:
- Periodically checks target health
- Stops sending traffic to unhealthy targets

---

## Key Insight

Load balancers do NOT talk to instances directly.
They talk to **target groups**.

📄 02-aws-compute-storage/load-balancing/health-checks.md
# Health Checks – Keeping Traffic Safe

## Why Health Checks Matter

Without health checks:
- Traffic goes to dead servers
- Users see errors

---

## How Health Checks Work

Load balancer:
- Sends periodic requests
- Expects healthy response
- Removes failed targets automatically

---

## Production Rule

Always:
- Use application-level health checks
- Avoid shallow TCP checks

📄 02-aws-compute-storage/load-balancing/ssl-and-load-balancers.md
# TLS with Load Balancers

## TLS Termination

Load balancers commonly:
- Terminate TLS
- Forward plain HTTP to backend

---

## Benefits

- Centralized certificate management
- Simplified backend services
- Better performance

---

## AWS Best Practice

Use:
- ACM + ALB
- Automatic renewal

📄 02-aws-compute-storage/load-balancing/production-patterns.md
# Load Balancing in Real Production

## Common Architecture

User
 ↓
Route 53
 ↓
ALB
 ↓
Auto Scaling Group
 ↓
EC2 / EKS

---

## Golden Rules

- Never expose instances directly
- Always use health checks
- Always plan for failure

Load balancers are mandatory, not optional.

✅ WHERE WE ARE NOW

✔ Load balancing explained from zero
✔ CLB vs ALB vs NLB clarified
✔ Target groups explained
✔ Health checks covered
✔ TLS termination included
