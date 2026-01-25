08 – Monitoring & Logging (From Zero, Real Production View)

Monitoring and logging together are called Observability.

Observability answers three life-saving questions in production:

Is my system healthy right now?

What changed when it broke?

Why did it break?

If you cannot answer these within minutes, your system is not production-ready.

1. Observability Fundamentals (Start From Zero)

There are three pillars of observability:

1️⃣ Metrics

Numbers that change over time.

Examples:

CPU usage

Memory usage

Request count

Error rate

Latency

Metrics answer:
“How bad is it?”

2️⃣ Logs

Detailed text records of events.

Examples:

Application errors

Stack traces

Authentication failures

Request logs

Logs answer:
“What exactly happened?”

3️⃣ Traces

End-to-end request journey.

Examples:

API → backend → database

Where latency occurred

Traces answer:
“Where did it slow or fail?”

Golden Production Rule

Metrics tell you something is wrong
Logs tell you what is wrong
Traces tell you where it is wrong

2. Monitoring vs Logging (Very Important)
Monitoring

Proactive

Alert-driven

Numeric

Lightweight

You use monitoring to detect problems early.

Logging

Reactive

Investigation-driven

Text-heavy

Storage-intensive

You use logging to debug problems deeply.

In production, monitoring wakes you up
Logging helps you fix it

3. Monitoring Architecture (Big Picture)

A real production monitoring system has:

Metric collection agents

Central metric store

Visualization layer

Alerting engine

Without all four, monitoring is incomplete.

4. Prometheus (The Heart of Kubernetes Monitoring)
What Prometheus Is

Prometheus is:

A time-series database

A metrics collector

An alert engine

Prometheus does NOT do:

Log storage

Long-term analytics (by default)

How Prometheus Works (Very Important)

Prometheus uses a pull model.

Meaning:

Prometheus pulls metrics

Applications do NOT push metrics

This is a design choice, not a limitation.

Why Pull Model Is Better

Prometheus controls scrape frequency

Failing apps don’t overload the system

Easy service discovery with Kubernetes

5. Prometheus Architecture (From Zero)

Prometheus architecture has these components:

Prometheus Server

Scrapes metrics

Stores time-series data

Evaluates alert rules

Targets

Applications

Nodes

Kubernetes components

Exporters

Convert system metrics into Prometheus format

Alertmanager

Handles alerts

Sends notifications

6. Metrics (What Are We Actually Measuring?)

Metrics are numeric values with labels.

Examples:

CPU usage per node

Memory usage per pod

HTTP requests per service

Error rate per endpoint

Metrics must be:

Cheap to collect

Fast to query

Meaningful

Common Metric Types

Counter → only increases (requests served)

Gauge → goes up and down (CPU, memory)

Histogram → distribution (latency)

Summary → percentile-based metrics

7. Node Exporter (Infrastructure Metrics)
Why Node Exporter Exists

Kubernetes runs on nodes (VMs or instances).
You must monitor the node itself, not just pods.

Node Exporter collects:

CPU usage

Memory usage

Disk I/O

Network traffic

Filesystem usage

Production Insight

If nodes are unhealthy:

Pods will fail

Scheduling will break

Cluster becomes unstable

Node Exporter is non-negotiable in production.

8. Kubernetes Metrics You Must Monitor
Cluster-level

Node CPU & memory

Disk pressure

Node readiness

Pod-level

Pod restarts

CPU throttling

Memory OOM kills

Application-level

Request rate

Error rate

Latency

This is called the RED method:

Rate

Errors

Duration

9. Grafana (Visualization Layer)
What Grafana Is

Grafana is:

A visualization tool

A dashboard platform

NOT a data store

Grafana reads data from:

Prometheus

CloudWatch

Loki

OpenSearch

Why Grafana Is Critical

Humans understand graphs, not raw numbers.

Grafana:

Turns metrics into insight

Shows trends

Helps correlate failures

Production Dashboard Strategy

Good dashboards:

Show symptoms, not raw data

Focus on SLIs

Avoid clutter

Bad dashboards:

Hundreds of panels

No clear signal

10. Alerting (When to Wake Humans)
Alerting Philosophy

Alerts should be:

Actionable

Rare

Urgent

If an alert fires often:

People will ignore it

It becomes noise

Alertmanager (Prometheus Component)

Alertmanager:

Receives alerts

Groups them

Routes them

Sends notifications

Destinations:

Slack

Email

PagerDuty

OpsGenie

Production Rule

Do NOT alert on CPU alone.
Alert on symptoms, not causes.

11. Logging Fundamentals (From Zero)

Logs are records of events, not metrics.

Logs are:

High volume

High cardinality

Expensive to store

You must be selective.

Types of Logs

Application logs

Errors

Business events

System logs

Node issues

Kernel messages

Audit logs

Security events

API access

12. Kubernetes Logging Reality

Kubernetes does NOT store logs permanently.

By default:

Logs live on node disk

Lost if pod or node dies

Therefore:
👉 Centralized logging is mandatory.

13. Log Collection (Agents)

You need agents that:

Run on every node

Collect logs

Forward logs centrally

Common agents:

Fluent Bit

Fluentd

Vector

These usually run as DaemonSets.

14. Logging Backends (Where Logs Go)
ELK Stack

Elasticsearch → storage & search

Logstash → processing

Kibana → visualization

Powerful but heavy.

OpenSearch

AWS-managed alternative to Elasticsearch

Better cost control

Easier scaling in AWS

CloudWatch Logs

Fully managed

Easy integration with AWS

Limited search flexibility

15. Log Retention Strategy (Critical)

Logs grow fast.

Production rules:

Short retention for debug logs

Longer retention for security logs

Archive old logs to S3

Uncontrolled logs = surprise AWS bill.

16. Correlating Metrics and Logs

The real power is correlation.

Example:

Alert fires for high latency

Grafana shows spike

Logs show database timeout

Root cause identified quickly

Without correlation:

You guess

You waste time

17. Tracing (Brief but Important)

Tracing shows:

Request flow across services

Where latency accumulates

Which service failed

Common tools:

OpenTelemetry

Jaeger

AWS X-Ray

Tracing becomes essential in microservices.

18. Real Production Monitoring Flow

What actually happens in prod:

Alert fires

Engineer checks Grafana

Identify affected service

Inspect logs

Correlate metrics + logs

Fix or rollback

Monitoring without logging = blind
Logging without monitoring = slow

19. Production Monitoring Mindset (Very Important)

Monitor user experience, not infrastructure vanity metrics

Alert on symptoms, not resources

Dashboards should tell a story

Logs should be structured

Always think in failure scenarios

WHERE YOU ARE NOW

You now deeply understand monitoring & logging:

Observability pillars

Prometheus architecture

Node Exporter & metrics

Grafana dashboards

Alerting philosophy

Kubernetes logging reality

ELK / OpenSearch / CloudWatch

Production troubleshooting flow

This is real on-call DevOps knowledge, not theory.
