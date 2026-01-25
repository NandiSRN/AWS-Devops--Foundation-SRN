📁 05-kubernetes/07-troubleshooting/README.md
# Kubernetes Troubleshooting – When Things Break (They Will)

In real production:
- Pods fail
- Nodes disappear
- Images don’t pull
- DNS stops resolving
- Applications crash silently

This section teaches:
- How to THINK during incidents
- What commands to run first
- How to isolate root cause fast

Kubernetes troubleshooting is about **signal over noise**.

📄 05-kubernetes/07-troubleshooting/troubleshooting-mindset.md
# Troubleshooting Mindset – From Zero

## Golden Rule

Never panic.
Kubernetes always leaves clues.

---

## Always Ask in This Order

1. Is the pod running?
2. Is it Ready?
3. Is the node healthy?
4. Is networking working?
5. Is the application healthy?

Jumping steps = wasted time.

📄 05-kubernetes/07-troubleshooting/kubectl-debugging.md
# kubectl – Your Primary Debug Tool

## Must-Know Commands

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container>
kubectl exec -it <pod-name> -- /bin/sh

What Each Command Tells You

get → current state

describe → events & failures

logs → application errors

exec → inside container

Most issues are visible in describe.


---

## 📄 `05-kubernetes/07-troubleshooting/pod-crashloop.md`

```md
# CrashLoopBackOff – Pod Keeps Restarting

## What It Means

Pod starts → crashes → restarts → repeats.

---

## Common Causes

- Application crash
- Wrong environment variables
- Missing secrets
- Wrong command / entrypoint

---

## How to Debug

```bash
kubectl logs <pod>
kubectl describe pod <pod>

Production Fix

Fix app crash

Add readiness checks

Validate config before deploy


---

## 📄 `05-kubernetes/07-troubleshooting/imagepullbackoff.md`

```md
# ImagePullBackOff – Image Cannot Be Pulled

## What It Means

Kubernetes cannot download the container image.

---

## Common Causes

- Wrong image name or tag
- Image not pushed
- Private registry auth issue
- Missing IAM permissions (ECR)

---

## Debug Commands

```bash
kubectl describe pod <pod>


Look at Events.

AWS EKS Insight

Ensure:

Node IAM role has ECR access

Image exists in ECR


---

## 📄 `05-kubernetes/07-troubleshooting/dns-issues.md`

```md
# DNS Issues – Service Name Not Resolving

## Symptoms

- App cannot reach service
- Errors like: "host not found"

---

## Root Causes

- CoreDNS not running
- Wrong service name
- Wrong namespace
- NetworkPolicy blocking traffic

---

## Debug Steps

```bash
kubectl get pods -n kube-system
kubectl exec -it <pod> -- nslookup service-name

Production Tip

DNS failures break everything silently.
Always check CoreDNS first.


---

## 📄 `05-kubernetes/07-troubleshooting/node-not-ready.md`

```md
# Node NotReady – Cluster Capacity Issue

## What It Means

Node cannot run pods.

---

## Common Causes

- Network issue
- Disk pressure
- Memory pressure
- Kubelet crash
- Cloud instance issue

---

## Debug Commands

```bash
kubectl get nodes
kubectl describe node <node>

Production Insight

Pods stuck in Pending often mean:

Node issue

Scheduling constraints


---

## 📄 `05-kubernetes/07-troubleshooting/pending-pods.md`

```md
# Pods Stuck in Pending

## Why This Happens

Scheduler cannot find a suitable node.

---

## Common Reasons

- No free resources
- NodeSelector mismatch
- Taints without tolerations
- PVC not bound

---

## Debug

```bash
kubectl describe pod <pod>


Events will tell you WHY.


---

## 📄 `05-kubernetes/07-troubleshooting/service-not-working.md`

```md
# Service Exists but App Not Reachable

## Common Causes

- Pod labels don't match service selector
- Pod not Ready
- Wrong targetPort
- NetworkPolicy blocking

---

## Debug Checklist

```bash
kubectl get svc
kubectl describe svc <service>
kubectl get endpoints <service>

Key Insight

Service without endpoints = broken routing.


---

## 📄 `05-kubernetes/07-troubleshooting/ingress-issues.md`

```md
# Ingress Issues – 404 / 502 / Timeout

## Common Symptoms

- 404 Not Found
- 502 Bad Gateway
- Connection timeout

---

## Common Causes

- Ingress controller not running
- Wrong service port
- Backend pod not Ready
- DNS not pointing correctly

---

## Debug Steps

```bash
kubectl get ingress
kubectl describe ingress
kubectl logs -n ingress-nginx <controller-pod>

Production Rule

Ingress YAML alone does nothing.
Controller must be healthy.


---

## 📄 `05-kubernetes/07-troubleshooting/storage-issues.md`

```md
# Storage Issues – PVC Not Bound

## Symptoms

- Pod Pending
- PVC Pending

---

## Root Causes

- StorageClass missing
- Wrong access mode
- CSI driver not installed
- AZ mismatch (EBS)

---

## Debug

```bash
kubectl get pvc
kubectl describe pvc
kubectl get storageclass

Production Insight

Storage issues block scheduling.


---

## 📄 `05-kubernetes/07-troubleshooting/real-incident-flow.md`

```md
# Real Production Incident Flow

## Step-by-Step

1. kubectl get pods
2. Identify failing pods
3. kubectl describe pod
4. Check events
5. Check logs
6. Check node health
7. Check networking

---

## Never Guess

Kubernetes always tells you WHY.
You just need to listen.

✅ KUBERNETES COMPLETE 🎉

You now have end-to-end Kubernetes mastery:

✔ Architecture
✔ Networking
✔ Workloads
✔ Scheduling
✔ Storage
✔ Security
✔ Troubleshooting

This is interview-ready + on-call-ready knowledge.
