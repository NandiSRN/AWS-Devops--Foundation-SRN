📁 01-cloud-networking/README.md
# Cloud Networking – Understanding How Traffic Moves

Before containers, Kubernetes, or CI/CD,
**traffic must be able to move correctly**.

Most production issues are not application bugs.
They are **network misunderstandings**.

Cloud networking answers four fundamental questions:

1. Where does traffic come from?
2. Where does it go?
3. Who is allowed to talk to whom?
4. How is access controlled securely?

This folder explains cloud networking from first principles,
slowly and practically, using AWS as the reference platform.

📄 01-cloud-networking/why-networking-matters.md
# Why Networking Matters (Before Any Cloud Service)

## The Core Truth

Every distributed system is a networked system.

No matter how good your code is:
- If traffic cannot reach it → app is down
- If traffic reaches wrong place → security issue
- If traffic is slow → users leave

---

## What Networking Really Means

Networking is simply:
> How machines discover and communicate with each other.

This includes:
- IP addresses
- Routing
- Firewalls
- DNS
- Encryption

Cloud networking abstracts hardware,
but the principles remain the same.

---

## Production Reality

In real production:
- 80% outages involve networking
- Debugging requires understanding packet flow
- Guessing does not work

Understanding networking turns debugging from panic into logic.
