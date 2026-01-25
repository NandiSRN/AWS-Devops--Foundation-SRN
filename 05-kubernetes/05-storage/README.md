📁 05-kubernetes/05-storage/README.md
# Kubernetes Storage – Making Data Survive Pod Death

Pods are ephemeral.
Nodes are replaceable.
Containers restart.

👉 **Data must survive all of this.**

Kubernetes storage exists to answer:
- Where is data stored?
- Who owns the data?
- What happens when a pod dies?

📄 05-kubernetes/05-storage/storage-basics.md
# Kubernetes Storage – From Zero

## First Principle

Containers are stateless by design.

If a pod:
- Restarts
- Moves to another node
- Is recreated

👉 **All local container data is lost**

---

## Why This Is a Problem

Applications need:
- Logs
- Uploaded files
- Database data
- Cache (sometimes)

Without storage abstraction,
Kubernetes would be useless for real apps.

📄 05-kubernetes/05-storage/volumes.md
# Volumes – Attaching Storage to Pods

## What a Volume Is

A volume:
> A directory shared into a pod.

Volumes:
- Exist as long as the pod exists
- Can be shared between containers in the same pod

---

## Important Limitation

Most basic volumes:
- Die with the pod
- Are not persistent

Volumes alone are NOT enough.

📄 05-kubernetes/05-storage/persistent-volume.md
# PersistentVolume (PV) – Cluster Storage Resource

## What a PV Is

A PersistentVolume:
> A piece of storage provisioned for the cluster.

It is:
- Independent of pods
- Managed by cluster admin or cloud provider

---

## PV Represents

- EBS volume
- EFS file system
- NFS mount
- Disk block storage

---

## Key Insight

PV exists even when pods are deleted.

📄 05-kubernetes/05-storage/persistent-volume-claim.md
# PersistentVolumeClaim (PVC) – Requesting Storage

## What a PVC Is

PVC:
> A request for storage by a pod.

Pod does NOT ask for PV directly.
It asks for PVC.

---

## Analogy

PV = Warehouse  
PVC = Rental agreement  
Pod = Tenant

---

## Why This Abstraction Exists

It separates:
- Storage provisioning
- Storage usage

📄 05-kubernetes/05-storage/pv-pvc-binding.md
# PV–PVC Binding Process

## How Binding Works

1. PVC created
2. Kubernetes finds matching PV
3. PV bound to PVC
4. Pod uses PVC

---

## Matching Criteria

- Storage size
- Access mode
- StorageClass

---

## Production Insight

Binding is one-to-one by default.

📄 05-kubernetes/05-storage/access-modes.md
# Access Modes – Who Can Mount Storage

## Common Access Modes

### ReadWriteOnce (RWO)
- One node can mount
- Most common (EBS)

### ReadWriteMany (RWX)
- Multiple nodes
- Used with EFS / NFS

### ReadOnlyMany (ROX)
- Rare use

---

## AWS Mapping

- EBS → RWO
- EFS → RWX

📄 05-kubernetes/05-storage/storageclass.md
# StorageClass – Dynamic Storage Provisioning

## The Old Problem

Manual PV creation:
- Slow
- Error-prone
- Not scalable

---

## What StorageClass Does

StorageClass:
> Automatically creates PVs on demand.

---

## Example (EKS – EBS)

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3
provisioner: ebs.csi.aws.com
parameters:
  type: gp3
volumeBindingMode: WaitForFirstConsumer

Production Rule

Always use dynamic provisioning.


---

## 📄 `05-kubernetes/05-storage/csi-driver.md`

```md
# CSI – Container Storage Interface

## Why CSI Exists

Kubernetes does NOT implement storage itself.

CSI allows:
- Cloud providers
- Storage vendors

To plug storage into Kubernetes.

---

## CSI Responsibilities

- Create volumes
- Attach volumes to nodes
- Mount volumes into pods

---

## AWS CSI Examples

- EBS CSI Driver
- EFS CSI Driver

Without CSI → no persistent storage.

📄 05-kubernetes/05-storage/statefulset-storage.md
# Storage with StatefulSets

## Why StatefulSets Need Special Storage

Stateful apps need:
- Stable identity
- Stable storage

---

## How StatefulSets Handle Storage

Each replica gets:
- Its own PVC
- Its own PV

Example:


db-0 → pvc-db-0
db-1 → pvc-db-1


---

## Production Rule

Never share one volume across replicas unless required.

📄 05-kubernetes/05-storage/efs-vs-ebs.md
# EBS vs EFS in Kubernetes

| Feature | EBS | EFS |
|------|----|----|
| Type | Block | File |
| Access | RWO | RWX |
| Performance | High | Moderate |
| Use Case | Databases | Shared files |

---

## Production Mapping

- Database → EBS
- Shared uploads → EFS

📄 05-kubernetes/05-storage/real-production-patterns.md
# Kubernetes Storage – Production Patterns

## Golden Rules

- Never store data inside container filesystem
- Always use PVCs
- Use correct access mode
- Monitor disk usage
- Backup volumes

---

## Common Failures

- Pod stuck in Pending (PVC not bound)
- Node failure → EBS detach delay
- Wrong access mode

Storage failures cause data loss.
Treat storage with respect.

✅ WHERE YOU ARE NOW

You now fully understand Kubernetes storage:

✔ Volumes vs PV vs PVC
✔ StorageClass & CSI
✔ EBS vs EFS
✔ StatefulSet storage
✔ Real production pitfalls

This is real EKS production knowledge, not theory.
