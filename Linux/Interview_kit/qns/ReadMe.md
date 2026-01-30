🔥 ADVANCED DEVOPS SCENARIO INTERVIEW QUESTIONS (PRODUCTION)
1️⃣ LINUX (ADVANCED SCENARIOS)

Production latency spikes, CPU and memory look normal — what Linux-level metrics do you check next?

Application logs stop writing, but disk has free space — how do you debug?

top shows low CPU usage, but load average is very high — explain why.

A process keeps restarting but no systemd service exists — how do you trace it?

How do you identify kernel-level OOM kills in production?

A process is consuming file descriptors rapidly — how do you detect and mitigate?

Network is slow only for one application — how do you debug at OS level?

What happens when inode usage reaches 100%?

How does Linux handle TCP connection exhaustion?

You rebooted a server and performance degraded — what changed?

2️⃣ GIT (ADVANCED TEAM / PROD WORKFLOWS)

Two teams accidentally deploy from the same branch — how do you prevent this permanently?

A bad commit was merged and deployed — how do you roll back safely?

How do you manage Git repos for multi-tenant environments?

What happens if Git history is rewritten in a GitOps repo?

How do you audit exactly which commit is running in production?

How do you enforce commit signing in enterprises?

How do you manage secrets accidentally committed to Git history?

How do you design Git branching for regulated environments?

What happens if Git is down during a production incident?

How do you prevent force-pushes in critical repos?

3️⃣ CI/CD (ADVANCED PIPELINE SCENARIOS)

CI pipeline is green, but production is broken — how do you investigate?

Multiple pipelines trigger simultaneously — how do you prevent race conditions?

How do you design pipelines for zero-downtime DB migrations?

What happens if your CI system is compromised?

How do you version and promote artifacts across environments?

How do you stop a running deployment safely?

Pipeline takes 40 minutes — how do you optimize without skipping tests?

How do you prevent developers from bypassing CI?

How do you handle partial pipeline failures?

Why should pipelines be idempotent?

4️⃣ DOCKER (ADVANCED CONTAINER SCENARIOS)

Container works locally but fails in Kubernetes — why?

Image size is small, but startup time is slow — why?

How do you debug a container that exits immediately?

What happens when container memory limit is hit?

Why does running as root cause security issues?

How do you handle secrets without rebuilding images?

How do you patch vulnerabilities without downtime?

What breaks when Docker images are mutable?

How do you debug DNS issues inside containers?

How do you design multi-stage builds for production?

5️⃣ Kubernetes (ADVANCED SCENARIOS)
Scheduling & Stability

Pods are Pending even though nodes have free resources — why?

How does Kubernetes behave during node pressure?

What happens when a node suddenly disappears?

How do you reduce blast radius during node failure?

Why do pods restart even when limits are not hit?

Networking

Service exists but traffic doesn’t reach pods — debug steps?

Why is DNS resolution slow inside the cluster?

How do you restrict traffic between namespaces?

Why shouldn’t Services be used for RDS?

How do you debug packet drops inside the cluster?

Scaling

HPA is not scaling — where do you look?

Why does Cluster Autoscaler sometimes not add nodes?

How do you avoid scaling oscillations?

How do you design for uneven traffic spikes?

When does vertical scaling make sense?

6️⃣ Amazon Web Services (ADVANCED AWS SCENARIOS)
IAM & SSO

How do you revoke access instantly for a compromised user?

Why should IAM users never be used in production?

How does SSO work internally with STS?

How do you audit cross-account access?

How do you enforce MFA for all access?

Networking

Why is NAT Gateway often the biggest hidden cost?

VPC Peering vs Transit Gateway — real decision criteria?

Why does traffic fail across VPCs even when peered?

How do EKS nodes reach RDS securely?

How do you debug asymmetric routing?

Compute & Storage

When NOT to use Fargate?

How do you right-size EC2 without downtime?

EBS vs EFS — production pitfalls?

How do you rotate AMIs safely?

How do you handle AZ outages?

7️⃣ TERRAFORM / OPENTOFU (ADVANCED SCENARIOS)

Terraform apply failed halfway — what do you do?

State file is corrupted — recovery steps?

How do you manage secrets in Terraform safely?

How do you prevent accidental resource deletion?

How do you design reusable modules?

How do you manage drift outside Terraform?

How do you handle multi-region deployments?

What happens if two applies run simultaneously?

How do you roll back infrastructure changes?

How do you audit infra changes?

8️⃣ Argo CD / GITOPS (ADVANCED)

What happens if someone changes resources manually?

How does ArgoCD detect and fix drift?

Auto-sync vs manual sync — real decision?

How do you deploy to multiple clusters safely?

How do you handle secrets in GitOps?

What happens if Git is unavailable?

How do you roll back a failed deployment?

How do you manage dependencies between apps?

How do you restrict ArgoCD access?

How do you monitor ArgoCD health?

9️⃣ Prometheus / Grafana (ADVANCED)

CPU is fine but app latency is high — why?

How do you avoid alert fatigue?

What metrics actually matter for SLOs?

Difference between metrics, logs, traces?

How do you debug without SSH access?

How do you monitor cost-related metrics?

How do you detect memory leaks?

What happens when Prometheus storage fills?

How do you design meaningful dashboards?

How do you handle noisy alerts?

🔟 SECURITY (ADVANCED SCENARIOS)

A secret was leaked — what are your immediate steps?

How do you rotate credentials without downtime?

How do you prevent privilege escalation?

How do you restrict dashboard access?

What is defense-in-depth in Kubernetes?

How do you secure container images?

How do you audit access in AWS?

How do you isolate workloads securely?

How do you respond to a breach?

How do you enforce compliance?

1️⃣1️⃣ COST OPTIMIZATION (ADVANCED)

AWS bill suddenly spikes — how do you investigate?

Which EKS components usually cost the most?

How do you reduce NAT Gateway cost?

When are Spot instances safe?

How do you detect idle resources?

How do you enforce cost limits?

How do you explain cloud bills to management?

How do you track cost per service?

How do you prevent over-provisioning?

How do you forecast cloud spend?

1️⃣2️⃣ INCIDENTS & FAILURE SCENARIOS (INTERVIEW FAVORITES)

Describe a production outage you handled.

How do you do root cause analysis?

How do you prevent recurrence?

How do you communicate during incidents?

How do you prioritize fixes?

What automation did you add after failure?

How do you handle on-call stress?

What metrics matter during incidents?

How do you test failure scenarios?

How do you learn from outages?

✅ HOW TO USE THIS EFFECTIVELY

You now have 3 levels of prep:

Basic questions (earlier list)

Advanced scenario questions (this list)

Deep-dive answers (we can do next)

Next best options (pick ONE):

🔥 “Give detailed answers for Kubernetes scenarios only”

🔥 “Run mock interview — ask me questions, I answer”

🔥 “Create a 30-day DevOps interview prep plan”

🔥 “Convert this into PDF / notes format”

Just tell me what you want next.
