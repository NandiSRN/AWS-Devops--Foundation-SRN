📁 04-containers/README.md
# Containers – Running Applications the Modern Way

Before containers:
- Applications were installed directly on servers
- Dependencies conflicted
- Deployments were risky
- “Works on my machine” was common

Containers solve these problems.

Containers are the foundation of:
- Docker
- Kubernetes
- CI/CD pipelines
- Cloud-native architecture

📁 04-containers/docker/
📄 04-containers/docker/docker-basics.md
# Docker Basics – From Zero

## Start With the Core Problem

Imagine:
- App A needs Java 11
- App B needs Java 17
- App C needs Java 21

All on the same server.

Result:
- Dependency conflicts
- Broken deployments
- Unstable systems

Docker solves this.

---

## What Docker Really Is

Docker is:
> A containerization platform.

It packages:
- Application
- Dependencies
- Runtime
- Configuration

Into a single unit called a **container**.

---

## Key Idea

Containers isolate applications,
NOT hardware.

They share:
- Host OS kernel

But isolate:
- Filesystem
- Processes
- Network

📄 04-containers/docker/docker-vs-vm.md
# Docker vs Virtual Machines (Deep Understanding)

## Virtual Machines

VMs:
- Virtualize hardware
- Run full OS
- Heavy and slow

Each VM includes:
- OS
- Kernel
- Libraries
- App

---

## Containers

Containers:
- Virtualize OS
- Share host kernel
- Lightweight and fast

Each container includes:
- App
- Dependencies
- Runtime

---

## Production Reality

VMs are still used.
Containers run **inside VMs**.

This is normal.

📄 04-containers/docker/docker-architecture.md
# Docker Architecture – How Docker Works

## Docker Client

- CLI tool (`docker`)
- Used by developers and CI/CD

---

## Docker Daemon

- Runs in background
- Manages containers and images

---

## Docker Image

- Read-only template
- Used to create containers

---

## Docker Container

- Running instance of an image

---

## Flow

docker build → image  
docker run → container

📄 04-containers/docker/dockerfile.md
# Dockerfile – Blueprint of a Container

## What Is a Dockerfile?

A Dockerfile is:
> A text file that defines how to build an image.

---

## Simple Example (Java App)

```dockerfile
FROM openjdk:17-jdk
WORKDIR /app
COPY target/app.jar app.jar
CMD ["java", "-jar", "app.jar"]

Key Instructions

FROM → base image

COPY → add files

RUN → execute commands

CMD → default container command

Production Rule

Keep Dockerfiles:

Small

Predictable

Secure


---

## 📄 `04-containers/docker/docker-build.md`

```md
# Docker Build – Creating Images

## What Happens During Build

Docker:
- Reads Dockerfile line by line
- Caches layers
- Produces an image

---

## Build Command

```bash
docker build -t myapp:1.0 .

Image Layers Insight

Each instruction creates a layer.

Fewer layers = faster builds.


---

## 📄 `04-containers/docker/docker-run.md`

```md
# Docker Run – Starting Containers

## Running a Container

```bash
docker run -d -p 8080:8080 myapp:1.0

What Happens Internally

Image is pulled

Container is created

Process starts

Port is exposed

Containers Are Ephemeral

Stop container → data lost
(unless volumes are used)


---

## 📄 `04-containers/docker/docker-volumes.md`

```md
# Docker Volumes – Persisting Data

## The Problem

Containers are ephemeral.
Data disappears on restart.

---

## Volumes Solve This

Volumes:
- Store data outside container
- Persist across restarts

---

## Example

```bash
docker run -v mydata:/data myapp

Production Rule

Never store important data inside containers.


---

## 📄 `04-containers/docker/docker-networking.md`

```md
# Docker Networking – Container Communication

## Default Bridge Network

- Containers get private IPs
- Can talk to each other

---

## Port Mapping

```bash
-p 80:8080


Host port → container port

Production Insight

Networking is isolated by default.
Explicit exposure is required.


---

## 📄 `04-containers/docker/docker-compose.md`

```md
# Docker Compose – Multi-Container Apps

## Why Compose Exists

Applications often need:
- App
- DB
- Cache

Compose runs them together.

---

## Example docker-compose.yml

```yaml
version: "3"
services:
  app:
    image: myapp
    ports:
      - "8080:8080"
  db:
    image: postgres

DevOps Usage

Compose is great for:

Local development

Testing


---

# 📁 `04-containers/registry/`

---

## 📄 `04-containers/registry/what-is-registry.md`

```md
# Container Registry – Image Storage

A registry is:
> A place to store Docker images.

Examples:
- Docker Hub
- Amazon ECR

CI builds images.
Registry stores them.
Kubernetes pulls them.

📄 04-containers/registry/ecr.md
# Amazon ECR – Elastic Container Registry

## What Is ECR?

ECR is:
- AWS-managed container registry
- Integrated with IAM
- Secure by default

---

## Production Flow

CI/CD → build image  
Push → ECR  
EKS → pull image

---

## Security Insight

Access is controlled via IAM.

📄 04-containers/registry/image-tagging.md
# Image Tagging – Versioning Containers

## Why Tags Matter

Avoid:
- latest tag in production

---

## Good Tag Examples

- 1.0.0
- git-sha
- build-number

---

## Production Rule

Tags must be immutable.

📄 04-containers/registry/image-scanning.md
# Image Scanning – Container Security

## Why Scan Images

Images may contain:
- Vulnerable libraries
- Malware
- Outdated packages

---

## Tools

- Trivy
- ECR image scanning

---

## Production Insight

Security scanning is mandatory in pipelines.

✅ WHERE WE ARE NOW

You now fully understand containers:

✔ Why Docker exists
✔ Docker architecture
✔ Dockerfile & images
✔ Volumes & networking
✔ Registries & ECR
✔ Security mindset

This is real production container knowledge.
