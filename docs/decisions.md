## ECS Fargate Architecture vs Kubernetes (EKS)

This project uses Amazon ECS with Fargate instead of Kubernetes (EKS) to prioritize operational simplicity, faster delivery, and reduced infrastructure overhead, while still maintaining production-grade patterns (scaling, observability, and fault tolerance).

## Decision Drivers
### Lower Operational Complexity

ECS Fargate removes the need to manage:

Kubernetes control plane
Worker nodes / cluster scaling
Add-ons (CNI, CoreDNS, ingress controllers)

Instead, compute is fully managed by AWS.

Kubernetes (EKS), while more powerful, introduces additional operational concerns:

cluster upgrades and version compatibility
node lifecycle management
networking complexity (CNI tuning, service discovery, ingress setup)

This project intentionally avoids that complexity to focus on core cloud fundamentals:

networking, scaling behavior, and failure handling in AWS-native services

### Faster Iteration

ECS enables faster deployment cycles with fewer moving parts:

Task definitions are simpler than Kubernetes manifests
No cluster-level orchestration required
Direct integration with ALB and IAM

This makes ECS more suitable for:

small-to-medium scale systems
rapid infrastructure iteration
portfolio projects focused on architecture clarity

### Cost Efficiency (at small scale)

Fargate uses a pay-per-use model (CPU / memory per task), avoiding:

idle EC2 worker nodes
overprovisioned clusters

At early-stage workloads, this reduces unnecessary baseline compute cost.

### Observability Layer (Grafana Stack)

To ensure production-like visibility, the system integrates a monitoring stack using Grafana.

Key Components:
Grafana → visualization layer for metrics and system health
CloudWatch → logs and infrastructure metrics source
(Optional) Prometheus-style metrics depending on configuration

Why this matters in ECS architecture:

Since ECS is abstracted (no node access like Kubernetes), observability becomes critical for:

Debugging task failures

Monitoring CPU/memory usage per service

Tracking ALB response patterns

Identifying deployment regressions

Grafana acts as the central dashboard for system visibility, replacing the introspection tools typically available in Kubernetes environments.