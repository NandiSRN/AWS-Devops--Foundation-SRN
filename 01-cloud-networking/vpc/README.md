# VPC – Virtual Private Cloud (The Foundation)

A VPC is the **starting point of everything in AWS networking**.

Without a VPC:
- No EC2
- No EKS
- No RDS
- No Load Balancer

Think of a VPC as:
> Your own private data center inside AWS.
📄 01-cloud-networking/vpc/vpc-basics.md
# What Is a VPC? (Explained From Zero)

## Start With the Real World

In a traditional data center:
- You own a building
- You design internal networks
- You control who can enter and exit

In AWS, you do not own buildings.
So AWS gives you a **logical network boundary** instead.

That boundary is called a **VPC**.

---

## What a VPC Actually Is

A VPC is:
- A logically isolated network
- With its own IP address range
- Fully controlled by you

Nothing can enter or leave a VPC unless **you explicitly allow it**.

---

## Why AWS Uses VPCs

VPCs provide:
- Isolation between customers
- Network-level security
- Full control over traffic flow

This is why AWS is considered secure by design.

---

## Important Mental Model

A VPC is NOT:
- A server
- A firewall
- A subnet

A VPC is:
> A container for networking components.
📄 01-cloud-networking/vpc/cidr-design.md
# CIDR – How IP Addressing Works

## Why IP Addresses Exist

Every machine on a network needs an address.
This address is called an **IP address**.

Without IP addresses:
- Machines cannot find each other
- Communication is impossible

---

## What CIDR Means

CIDR stands for:
> Classless Inter-Domain Routing

CIDR defines:
- How many IPs a network has
- How those IPs are grouped

Example:
10.0.0.0/16


This means:
- Network starts at 10.0.0.0
- Contains ~65,536 IP addresses

---

## Why CIDR Planning Is Critical

CIDR decisions are:
- Hard to change later
- Affect scaling
- Affect peering and VPNs

Bad CIDR design = future pain.

---

## Production Rule

Always plan CIDR ranges **larger than current needs**.
📄 01-cloud-networking/vpc/subnet-basics.md
# Subnets – Dividing the Network

## Why Subnets Exist

Putting everything in one network is dangerous.

Subnets allow you to:
- Separate workloads
- Control access
- Reduce blast radius

---

## What a Subnet Is

A subnet is:
> A smaller network inside a VPC.

Each subnet:
- Uses part of the VPC CIDR
- Belongs to exactly one Availability Zone

---

## Public vs Private Subnets (Conceptual)

Public subnet:
- Has internet access

Private subnet:
- No direct internet access

This separation is **intentional and critical** for security.

---

## Production Insight

Most production architectures use:
- Public subnets → load balancers
- Private subnets → applications & databases
📄 01-cloud-networking/vpc/route-table.md
# Route Tables – Deciding Where Traffic Goes

## What Routing Means

Routing answers one simple question:
> “Where should this traffic go?”

Without routing:
- Traffic is dropped
- Communication fails

---

## What a Route Table Is

A route table contains rules like:
- Destination
- Next hop

Example:
0.0.0.0/0 → Internet Gateway


This means:
- Send all unknown traffic to the internet

---

## Important Truth

Subnets do not decide routing.
**Route tables do.**

---

## Production Rule

Always understand routing before debugging connectivity.
📄 01-cloud-networking/vpc/public-private-logic.md
# How a Subnet Becomes Public or Private

## Common Confusion

There is no such thing as a “public subnet” by default.

A subnet becomes public ONLY if:
1. It has a route to Internet Gateway
2. Instances have public IPs
3. Security rules allow traffic

Miss any one → no internet access.

---

## Why This Matters

This design prevents:
- Accidental exposure
- Security mistakes

AWS forces you to be explicit.


# VPC Connectivity – How Traffic Enters and Leaves a VPC

A VPC by default is **completely isolated**.

No internet.
No inbound traffic.
No outbound traffic.

This is intentional.

Connectivity components exist to **explicitly control**:
- Who can talk to the internet
- Who should stay private
- How traffic flows securely

In this section, we explain:
- Internet Gateway
- NAT Gateway
- Elastic IP
- Security Groups vs NACLs

These are **core daily DevOps concepts**.
📄 01-cloud-networking/vpc/connectivity/internet-gateway.md
# Internet Gateway (IGW)

## Start With a Simple Question

How does traffic from the internet reach your VPC?

The answer is:
> It cannot, unless an Internet Gateway exists.

---

## What an Internet Gateway Really Is

An Internet Gateway (IGW) is a **managed AWS component** that:
- Connects a VPC to the public internet
- Allows inbound and outbound internet traffic

Think of IGW as:
> The front door of your VPC.

---

## What IGW Does NOT Do (Very Important)

IGW does NOT:
- Automatically expose resources
- Act as a firewall
- Decide who can access what

IGW only provides a **path**.
Security controls still apply.

---

## How IGW Works (Flow)

1. User sends request from internet
2. Request reaches IGW
3. Route table decides where traffic goes
4. Security groups decide if traffic is allowed

If any step blocks traffic → request fails.

---

## How a Subnet Uses IGW

A subnet becomes internet-accessible only when:
0.0.0.0/0 → Internet Gateway

exists in its route table.

Without this route:
- IGW is useless for that subnet.

---

## Production Use Cases

IGW is typically used for:
- Application Load Balancers
- Bastion hosts
- Public APIs

❌ Not for databases  
❌ Not for internal services  

---

## Failure & Design Insight

If IGW is removed:
- All internet access stops
- Internal VPC traffic still works

---

## Interview One-Liner

> An Internet Gateway enables internet connectivity for a VPC by allowing traffic between the VPC and the public internet.
📄 01-cloud-networking/vpc/connectivity/nat-gateway.md
# NAT Gateway – Controlled Internet Access for Private Subnets

## The Problem NAT Solves

Private subnets should NOT be reachable from the internet.

But resources in private subnets still need to:
- Download updates
- Pull Docker images
- Access external APIs

This is where NAT Gateway exists.

---

## What NAT Gateway Does

A NAT Gateway allows:
- Outbound internet access from private subnets
- ❌ No inbound internet access

This means:
> Internet can never initiate a connection to private resources.

---

## How NAT Gateway Works (Traffic Flow)

1. Private EC2 sends request to internet
2. Route table sends traffic to NAT Gateway
3. NAT Gateway forwards traffic via IGW
4. Response returns back safely

Private EC2 never has a public IP.

---

## Where NAT Gateway Must Live

A NAT Gateway:
- Must be in a **public subnet**
- Must have an **Elastic IP**
- Uses IGW internally

---

## Availability Zone Design (Very Important)

Production rule:
> One NAT Gateway per Availability Zone.

Why?
- Avoid cross-AZ dependency
- Improve fault tolerance

---

## Cost Reality

NAT Gateways are:
- Highly available
- Managed
- But not cheap

Cost-conscious designs still prioritize **reliability**.

---

## Interview One-Liner

> A NAT Gateway allows private subnet resources to access the internet securely without exposing them to inbound traffic.
📄 01-cloud-networking/vpc/connectivity/elastic-ip.md
# Elastic IP – Stable Public Identity

## Why Elastic IP Exists

Public IPs in AWS are:
- Dynamic
- Can change on restart

Elastic IP (EIP) solves this by providing:
> A static, public IPv4 address.

---

## What an Elastic IP Is

An Elastic IP:
- Belongs to your AWS account
- Can be attached to different resources
- Does not change unless you release it

---

## Common Use Cases

Elastic IP is commonly used for:
- NAT Gateways
- Bastion hosts
- Legacy systems
- Whitelisted integrations

---

## Cost & Discipline

AWS charges for:
- Unused Elastic IPs
- EIPs not attached to running resources

This encourages responsible usage.

---

## Security Insight

Elastic IP alone does NOT:
- Open ports
- Allow traffic

Security Groups still control access.

---

## Interview One-Liner

> An Elastic IP is a static public IP address that provides a stable endpoint for AWS resources.
📄 01-cloud-networking/vpc/connectivity/security-groups-vs-nacls.md
# Security Groups vs Network ACLs (NACLs)

## Why Two Security Layers Exist

AWS follows **defense in depth**.

Security exists at:
- Instance level
- Subnet level

---

## Security Groups (Instance-Level Firewall)

Security Groups:
- Are stateful
- Allow rules only
- Apply to ENIs (instances)

Stateful means:
- Return traffic is automatically allowed

Used daily by DevOps engineers.

---

## Network ACLs (Subnet-Level Firewall)

NACLs:
- Are stateless
- Require explicit allow & deny
- Apply to subnets

Stateless means:
- Inbound and outbound rules are separate

---

## When to Use What

Security Groups:
- Primary security control
- Simple and flexible

NACLs:
- Additional compliance
- Blocking specific CIDRs

---

## Production Rule

> Use Security Groups by default.  
> Use NACLs only when required.

---

## Interview One-Liner

> Security Groups provide stateful instance-level security, while NACLs provide stateless subnet-level traffic control.


# VPC Advanced Connectivity – Connecting Networks Safely

A single VPC is rarely enough in real production.

As systems grow, you need to connect:
- Multiple VPCs
- Multiple AWS accounts
- On-premise data centers
- Shared services VPCs

Advanced connectivity components exist to:
- Scale networking cleanly
- Avoid routing chaos
- Maintain security boundaries

This section explains how AWS solves **network-to-network connectivity**.
📄 01-cloud-networking/vpc/advanced-connectivity/vpc-peering.md
# VPC Peering – Simple VPC-to-VPC Connectivity

## The Problem VPC Peering Solves

By default:
- VPCs are completely isolated
- Even inside the same AWS account

VPC Peering allows:
> Two VPCs to communicate privately.

---

## What VPC Peering Is

VPC Peering creates a **direct network connection** between two VPCs.

Traffic:
- Uses private IPs
- Never traverses the public internet
- Stays inside AWS backbone

---

## Key Characteristics (Very Important)

VPC Peering:
- Is **one-to-one**
- Is **non-transitive**
- Requires **CIDR ranges not to overlap**

Non-transitive means:
VPC-A ↔ VPC-B
VPC-B ↔ VPC-C

VPC-A ❌ cannot talk to VPC-C


---

## Routing Requirement

Peering alone does nothing.

You must:
- Update route tables in BOTH VPCs

Without routes → no traffic.

---

## Production Use Case

VPC Peering is good for:
- Small environments
- Simple architectures
- Dev / test connectivity

Not suitable for large-scale networks.

---

## Interview One-Liner

> VPC Peering enables private communication between two VPCs but is non-transitive and does not scale well.
📄 01-cloud-networking/vpc/advanced-connectivity/transit-gateway.md
# Transit Gateway – Scalable Network Hub

## Why Transit Gateway Exists

As VPC count grows:
- Peering becomes messy
- Route tables explode
- Operations become risky

Transit Gateway solves this by acting as:
> A central hub for network connectivity.

---

## What Transit Gateway Is

A Transit Gateway (TGW):
- Acts like a cloud router
- Connects multiple VPCs
- Supports on-prem VPN & Direct Connect

---

## Hub-and-Spoke Model

     VPC-A
       |
VPC-B — TGW — VPC-C
|
On-Prem


All traffic flows through TGW.

---

## Key Benefits

- Centralized routing
- Transitive connectivity
- Simplified operations
- Scales to hundreds of VPCs

---

## Production Reality

Most **enterprise AWS environments** use:
> Transit Gateway, not VPC Peering.

---

## Interview One-Liner

> Transit Gateway provides scalable, centralized connectivity between multiple VPCs and on-prem networks.
📄 01-cloud-networking/vpc/advanced-connectivity/vpc-endpoints.md
# VPC Endpoints – Private Access to AWS Services

## The Problem

By default:
- Accessing AWS services uses the public internet
- Even if traffic stays inside AWS

This increases:
- Security exposure
- Dependency on IGW / NAT

---

## What a VPC Endpoint Is

A VPC Endpoint allows:
> Private connectivity to AWS services without internet access.

Traffic:
- Never leaves AWS network
- Does not require IGW or NAT

---

## Types of VPC Endpoints

### Gateway Endpoints
- Used for S3 and DynamoDB
- Added to route tables

### Interface Endpoints
- Used for most AWS services
- Create ENIs with private IPs

---

## Production Use Case

VPC Endpoints are used for:
- Secure S3 access from private subnets
- Locking down internet access
- Compliance requirements

---

## Security Benefit

You can:
- Block internet completely
- Still access AWS services

---

## Interview One-Liner

> VPC Endpoints enable private access to AWS services without using the public internet.
📄 01-cloud-networking/vpc/advanced-connectivity/privatelink.md
# AWS PrivateLink – Private Service Exposure

## Why PrivateLink Exists

Sometimes you want to:
- Expose a service
- But NOT expose the VPC
- And NOT use public IPs

PrivateLink solves this.

---

## What PrivateLink Is

PrivateLink allows:
> One VPC to expose a service to another VPC privately.

The consumer:
- Does not know provider VPC CIDR
- Does not peer networks
- Only accesses the service

---

## How PrivateLink Works (Conceptual)

- Provider creates a Network Load Balancer
- Service is exposed via endpoint service
- Consumer creates an Interface Endpoint

---

## Security Advantage

- No VPC peering
- No routing complexity
- Least-privilege network access

---

## Production Use Case

PrivateLink is commonly used for:
- Shared internal services
- SaaS offerings
- Secure multi-account architectures

---

## Interview One-Liner

> AWS PrivateLink allows private, secure access to services across VPCs without exposing network topology.
📄 01-cloud-networking/vpc/advanced-connectivity/when-to-use-what.md
# Choosing the Right Connectivity Option

## Simple Comparison

| Scenario | Best Option |
|--------|------------|
| 2 VPCs, simple setup | VPC Peering |
| Many VPCs | Transit Gateway |
| Private AWS service access | VPC Endpoint |
| Expose service privately | PrivateLink |

---

## Production Rule of Thumb

- Small → Peering
- Medium → Transit Gateway
- Secure AWS access → Endpoints
- Service sharing → PrivateLink

Choosing wrong leads to:
- Complex routing
- Security risks
- Difficult scaling
