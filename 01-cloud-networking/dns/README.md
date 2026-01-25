📁 01-cloud-networking/dns/README.md
# DNS – Domain Name System (How the Internet Finds Things)

Humans remember names.
Computers understand numbers.

DNS exists to translate **human-friendly names** into **machine-usable IP addresses**.

Without DNS:
- Websites would not work
- APIs would not resolve
- Kubernetes services would break
- Cloud infrastructure would be unusable

DNS is one of the **most critical systems on the internet**.

📄 01-cloud-networking/dns/dns-basics.md
# DNS Basics – Explained From Zero

## Start With a Simple Question

When you type:
https://google.com

How does your laptop know **where Google is**?

The answer is:
> DNS

---

## What DNS Really Does

DNS translates:


google.com → 142.250.182.14


Computers communicate using IP addresses.
Humans communicate using names.

DNS bridges that gap.

---

## Important Truth

DNS does NOT:
- Host websites
- Store application data
- Control traffic

DNS only answers one question:
> “What IP address belongs to this name?”

📄 01-cloud-networking/dns/how-dns-works.md
# How DNS Resolution Works (Step by Step)

Let’s resolve:
www.example.com

---

## Step 1: Browser Cache

Your browser checks:
> “Have I already resolved this recently?”

If yes → done.

---

## Step 2: OS Cache

Operating system checks its DNS cache.

If found → done.

---

## Step 3: Recursive Resolver (ISP / Cloud DNS)

If not found locally:
- Request goes to a DNS resolver
- Usually provided by ISP or cloud provider

---

## Step 4: Root DNS Server

Resolver asks:
> “Who knows about .com?”

Root server responds:
> “Ask the .com nameserver”

---

## Step 5: TLD Server (.com)

Resolver asks:
> “Who knows example.com?”

TLD responds:
> “Ask the authoritative server for example.com”

---

## Step 6: Authoritative DNS Server

Authoritative server replies:
> “www.example.com = 93.184.216.34”

---

## Step 7: Response Returned

IP address is cached and returned to browser.

Website loads.

---

## Key Insight

DNS resolution is:
- Hierarchical
- Distributed
- Extremely fast

📄 01-cloud-networking/dns/dns-record-types.md
# DNS Record Types (Very Important)

DNS records define **how a domain behaves**.

---

## A Record

Maps a name to an IPv4 address.

Example:


example.com → 93.184.216.34


---

## AAAA Record

Maps a name to an IPv6 address.

---

## CNAME Record

Alias for another domain.

Example:


www.example.com
 → example.com


Important:
- Cannot coexist with other record types

---

## MX Record

Defines mail servers for a domain.

Used for email delivery.

---

## TXT Record

Stores text data.

Commonly used for:
- Domain verification
- SPF / DKIM
- SSL validation

---

## NS Record

Defines authoritative name servers.

Critical for delegation.

---

## Interview Rule

If you don’t understand A vs CNAME,
you don’t understand DNS.

📄 01-cloud-networking/dns/ttl-propagation.md
# TTL – Time To Live (Why DNS Changes Take Time)

## What TTL Means

TTL defines:
> How long DNS responses are cached.

Example:


TTL = 300 seconds


Resolvers will cache result for 5 minutes.

---

## Why TTL Exists

- Reduces DNS load
- Improves performance
- Increases reliability

---

## DNS Propagation Myth

DNS does NOT "propagate".

What really happens:
- Old cache expires
- New query fetches updated value

---

## Production Strategy

Before migrations:
- Lower TTL in advance
- Make changes
- Increase TTL later

This avoids downtime.

📄 01-cloud-networking/dns/authoritative-vs-recursive.md
# Authoritative vs Recursive DNS

## Recursive DNS

- Used by clients
- Resolves queries on behalf of users
- Caches results

Example:
- ISP DNS
- Google DNS (8.8.8.8)

---

## Authoritative DNS

- Source of truth
- Stores actual DNS records
- Does NOT cache

Example:
- Route 53
- Cloudflare

---

## Key Difference

Recursive:
> “Let me find that for you”

Authoritative:
> “I own the answer”

📄 01-cloud-networking/dns/common-dns-failures.md
# Common DNS Problems in Production

## Wrong Record Type
- Using CNAME at root domain

## TTL Too High
- Slow rollback during outages

## DNS Cache Issues
- Old IPs still resolving

## Missing Health Checks
- Traffic sent to dead endpoints

---

## DevOps Reality

When apps are unreachable:
DNS is always suspected early.

📄 01-cloud-networking/dns/production-mindset.md
# DNS in Real Production Systems

DNS is:
- Globally distributed
- Highly cached
- Extremely sensitive

Mistakes in DNS:
- Affect all users
- Are hard to roll back
- Cause large outages

---

## Golden Rule

DNS changes should be:
- Planned
- Verified
- Reversible

Treat DNS with respect.

✅ WHERE WE ARE NOW

✔ DNS explained from zero
✔ How resolution works
✔ Record types explained clearly
✔ TTL & caching clarified
✔ Production mindset included



📁 01-cloud-networking/dns/route53/README.md
# Amazon Route 53 – DNS the AWS Way

DNS tells the internet **where your application lives**.
Route 53 is AWS’s managed DNS service that does this
**reliably, globally, and at massive scale**.

If VPC is your private network,
Route 53 is how the outside world finds it.

📄 01-cloud-networking/dns/route53/what-is-route53.md
# What Is Amazon Route 53? (From Zero)

## Start Simple

When someone types:
www.myapp.com

The internet must know:
- Which server?
- Which IP?
- Which load balancer?

Route 53 answers these questions.

---

## What Route 53 Actually Is

Amazon Route 53 is:
- An authoritative DNS service
- Highly available
- Globally distributed

It stores DNS records and responds to DNS queries.

---

## Why the Name “Route 53”?

- Route → routing traffic
- 53 → DNS runs on port 53

That’s it. Nothing fancy.

---

## What Route 53 Is NOT

Route 53 does NOT:
- Host your website
- Run applications
- Replace load balancers

It only decides **where traffic should go**.

📄 01-cloud-networking/dns/route53/hosted-zones.md
# Hosted Zones – DNS Ownership

## What Is a Hosted Zone?

A hosted zone is:
> A container for DNS records for a domain.

If you own:
example.com

Route 53 needs a hosted zone for:
example.com

---

## Types of Hosted Zones

### Public Hosted Zone
- Used for internet-facing domains
- Resolves from anywhere on the internet

### Private Hosted Zone
- Used inside a VPC only
- Not accessible from the public internet

---

## Real Example

Public hosted zone:


example.com


Private hosted zone:


internal.example.com


---

## Production Insight

Most real systems use:
- Public hosted zone → users
- Private hosted zone → microservices

📄 01-cloud-networking/dns/route53/delegation-nameservers.md
# Domain Delegation – Connecting Your Domain to Route 53

## The Critical Step Everyone Misses

Buying a domain is NOT enough.

You must tell the internet:
> “Route 53 is the authority for my domain.”

---

## How Delegation Works

1. Route 53 gives you NS records
2. You copy those NS records
3. Paste them into your domain registrar (GoDaddy, Namecheap, etc.)

This step is called **delegation**.

---

## Without Delegation

- DNS records exist
- But nobody asks Route 53
- Domain does not resolve

---

## Production Rule

Always verify NS records after creation.

📄 01-cloud-networking/dns/route53/record-management.md
# Managing DNS Records in Route 53

## Record = Instruction

A DNS record tells Route 53:
> “When this name is asked, respond with this answer.”

---

## Common Records in AWS

### A Record
Maps domain → IP

Used when:
- You have a static IP

---

### Alias Record (AWS Special)

Maps domain → AWS resource

Example:


www.example.com
 → ALB


Benefits:
- No IP management
- Auto updates
- Free DNS queries

---

### CNAME Record
Maps name → name

Example:


api.example.com → api-alb.amazonaws.com


Cannot be used at root domain.

---

## Production Rule

Prefer **Alias records** for AWS services.

📄 01-cloud-networking/dns/route53/routing-policies.md
# Route 53 Routing Policies (Very Important)

DNS is not just name → IP.
Route 53 can make decisions.

---

## Simple Routing
- One record
- One destination

---

## Weighted Routing
- Split traffic by percentage
- Used for canary releases

Example:
- v1 → 90%
- v2 → 10%

---

## Latency Routing
- Route user to nearest region
- Improves performance

---

## Failover Routing
- Primary / secondary
- Health-check based

---

## Geolocation Routing
- Route based on country

---

## Production Insight

Route 53 is often the **first traffic decision layer**.

📄 01-cloud-networking/dns/route53/health-checks.md
# Route 53 Health Checks

## Why Health Checks Exist

DNS without health checks is blind.

Route 53 health checks allow:
- Detect unhealthy endpoints
- Automatically reroute traffic

---

## How It Works

1. Route 53 checks endpoint health
2. If unhealthy → stop returning it
3. Route traffic elsewhere

---

## Common Use Cases

- Active-passive failover
- Multi-region DR
- Zero-downtime upgrades

📄 01-cloud-networking/dns/route53/private-dns.md
# Private DNS with Route 53

## Why Private DNS Exists

Inside a VPC:
- Services should not use public DNS
- Internal names improve security

---

## Example



backend.internal → 10.0.2.45


Only resolvable inside VPC.

---

## Kubernetes Connection

EKS uses:
- CoreDNS for pods
- Route 53 for VPC-level DNS

They complement each other.

📄 01-cloud-networking/dns/route53/production-failures.md
# Route 53 Failures in Real Life

## Common Mistakes

- Wrong hosted zone
- Missing NS delegation
- High TTL during outage
- Incorrect routing policy

---

## Golden Rule

DNS mistakes cause:
- Global outages
- Slow recovery
- High impact

Always double-check.

✅ WHERE WE ARE NOW

✔ Route 53 explained from zero
✔ Hosted zones (public/private)
✔ Delegation clarified
✔ Routing policies explained
✔ Production mindset included

01-cloud-networking/tls-ssl/README.md
# TLS / SSL – Securing Traffic on the Internet

DNS helps users find your application.
TLS makes sure the communication is:
- Encrypted
- Authentic
- Trusted

Without TLS:
- Data can be stolen
- Passwords can be sniffed
- Browsers will show warnings

Modern production systems DO NOT exist without TLS.

📄 01-cloud-networking/tls-ssl/https-tls-basics.md
# TLS / SSL Basics – From Zero

## Start With a Simple Question

When you open:
https://www.example.com

Why do you see a 🔒 lock icon?

That lock represents:
> TLS encryption.

---

## What TLS Really Is

TLS (Transport Layer Security):
- Encrypts data in transit
- Prevents eavesdropping
- Prevents tampering

SSL is the old name.
TLS is the modern protocol.

People still say “SSL”, but **TLS is what actually runs**.

---

## What TLS Protects Against

Without TLS:
- Anyone on the network can read traffic
- Man-in-the-middle attacks are possible

With TLS:
- Data is encrypted
- Identity is verified
- Integrity is ensured

📄 01-cloud-networking/tls-ssl/how-https-works.md
# How HTTPS Works (Step by Step)

Let’s break the HTTPS handshake simply.

---

## Step 1: Client Hello

Browser says:
> “I want to connect securely. Here are cipher options.”

---

## Step 2: Server Hello

Server replies:
> “Here is my certificate and chosen cipher.”

---

## Step 3: Certificate Validation

Browser checks:
- Is certificate issued by trusted CA?
- Is domain name correct?
- Is certificate expired?

If any check fails → browser warning.

---

## Step 4: Key Exchange

Browser and server:
- Agree on a shared secret key
- Only they know this key

---

## Step 5: Encrypted Communication

All further traffic is:
- Encrypted
- Secure

---

## Key Insight

TLS uses:
- Asymmetric encryption (handshake)
- Symmetric encryption (data transfer)

📄 01-cloud-networking/tls-ssl/certificates.md
# TLS Certificates – Identity on the Internet

## What Is a Certificate?

A TLS certificate:
- Proves identity of a domain
- Contains public key
- Is signed by a trusted authority

Think of it as:
> A digital passport for your website.

---

## Who Issues Certificates?

Certificates are issued by:
- Certificate Authorities (CA)
Examples:
- Let’s Encrypt
- DigiCert
- GlobalSign
- Amazon ACM

---

## What a Certificate Contains

- Domain name
- Public key
- Issuer
- Expiry date
- Digital signature

---

## Why Expiry Exists

Certificates expire to:
- Reduce long-term risk
- Force rotation
- Improve security hygiene

📄 01-cloud-networking/tls-ssl/trust-chain.md
# Certificate Trust Chain (Very Important)

## Why Browsers Trust Certificates

Browsers come with:
- Pre-installed trusted root CAs

Trust flows like this:


Root CA
↓
Intermediate CA
↓
Server Certificate


If chain breaks → browser error.

---

## Common Browser Errors Explained

- NET::ERR_CERT_AUTHORITY_INVALID
- CERT_COMMON_NAME_INVALID
- CERT_DATE_INVALID

These mean:
> Trust validation failed.

---

## Production Rule

Never use:
- Self-signed certs in production
- Expired certificates

📄 01-cloud-networking/tls-ssl/ssl-termination.md
# SSL Termination – Where TLS Ends

## What Is SSL Termination?

SSL termination is:
> The point where encrypted traffic is decrypted.

---

## Common Termination Points

1. Load Balancer (most common)
2. Reverse Proxy (NGINX)
3. Application (least common)

---

## AWS Best Practice

Terminate TLS at:
- Application Load Balancer
- Network Load Balancer (TLS mode)

This:
- Simplifies apps
- Centralizes cert management

📄 01-cloud-networking/tls-ssl/acm.md
# AWS Certificate Manager (ACM)

## Why ACM Exists

Managing certificates manually is painful.

ACM:
- Issues certificates
- Renews automatically
- Integrates with AWS services

---

## What ACM Can Be Used With

ACM works with:
- ALB
- NLB (TLS)
- CloudFront
- API Gateway

---

## What ACM Cannot Do

ACM certificates:
- Cannot be exported (public AWS)
- Cannot be installed on EC2 manually

---

## Production Insight

ACM is:
> The standard way to handle TLS in AWS.

📄 01-cloud-networking/tls-ssl/certificate-validation.md
# Certificate Validation Methods

## DNS Validation (Recommended)

- Add a TXT record
- Automatic
- No downtime
- Preferred for production

---

## Email Validation

- CA sends email
- Manual approval
- Error-prone

---

## Production Rule

Always use:
> DNS validation

📄 01-cloud-networking/tls-ssl/cert-manager-vs-acm.md
# cert-manager vs ACM (Kubernetes Context)

## ACM

- AWS-managed
- Works with ALB / CloudFront
- Not native to Kubernetes

---

## cert-manager

- Kubernetes-native
- Works with Let’s Encrypt
- Issues certs for Ingress

---

## Real Production Pattern

- External LB → ACM
- Inside Kubernetes → cert-manager

Both are used together.

📄 01-cloud-networking/tls-ssl/production-mindset.md
# TLS in Real Production Systems

TLS is not optional.
Browsers enforce it.
APIs require it.
Compliance demands it.

---

## Golden Rules

- Always HTTPS
- Auto-renew certificates
- Monitor expiry
- Test before rotation

TLS failures = customer-visible outages.

✅ CLOUD NETWORKING – COMPLETE 🎉

You have now fully completed:

✔ VPC basics
✔ VPC connectivity
✔ Advanced VPC connectivity
✔ DNS
✔ Route 53
✔ TLS / SSL

This is a rock-solid foundation.
