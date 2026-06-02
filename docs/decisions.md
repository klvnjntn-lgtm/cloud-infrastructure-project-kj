## ECS Fargate Architecture vs Kubernetes (EKS)

This project uses Amazon ECS with Fargate instead of Kubernetes (EKS) to prioritize operational simplicity, faster delivery, and reduced infrastructure overhead, while still maintaining production-grade patterns such as scaling, observability, identity management, and fault tolerance.

The goal is not to avoid Kubernetes complexity, but to design a cloud-native architecture using AWS-managed primitives.

---

## Decision Drivers

### Lower Operational Complexity

ECS Fargate removes the need to manage infrastructure components such as:

- Kubernetes control plane
- Worker nodes and cluster autoscaling
- Add-ons (CNI plugins, CoreDNS, ingress controllers)

All compute is fully managed by AWS.

In contrast, EKS introduces additional operational responsibilities:

- Cluster version upgrades and compatibility management
- Node lifecycle and scaling strategies
- Networking complexity (CNI behavior, service discovery, ingress configuration)

This project intentionally avoids these layers to focus on core AWS architecture concepts:

- Networking design (VPC, subnets, routing)
- Service-to-service communication
- Failure handling and resilience patterns
- IAM-based security design

---

### Faster Iteration

ECS enables faster infrastructure iteration due to its simplified abstraction model.

Key advantages:

- Task definitions are simpler than Kubernetes manifests
- No cluster-level orchestration required
- Direct integration with ALB and IAM
- Faster deployment feedback loop

This makes ECS more suitable for:

- Architecture-focused portfolio projects
- Rapid infrastructure experimentation
- Small to medium-scale systems

---

### Cost Efficiency (Small to Medium Scale)

Fargate uses a pay-per-use model based on CPU and memory allocation per task.

This avoids:

- Idle EC2 worker nodes
- Overprovisioned cluster capacity

At lower traffic levels, this results in more efficient baseline cost management compared to maintaining always-on infrastructure.

---

### Identity & Security Model (OIDC-style IAM Integration)

This architecture uses IAM Roles for Service Accounts (OIDC-style task roles) to manage workload identity.

Each ECS task assumes a dedicated IAM role at runtime, enabling secure access to AWS services.

This allows:

- No static AWS credentials inside containers
- Fine-grained permission control per service
- Secure access to SSM Parameter Store, CloudWatch, and SNS
- Clear separation of identity between services

This design replaces traditional credential-based access with identity-based workload authorization.

---

### Secrets Management (SSM Parameter Store)

AWS Systems Manager (SSM) is used for centralized secrets management.

Key benefits:

- Secrets are not stored in application code or container images
- ECS tasks retrieve secrets at runtime using IAM permissions
- Centralized management of configuration values
- Native integration with AWS KMS encryption

This reduces operational overhead while maintaining secure configuration handling.

---

### Observability Layer (CloudWatch + Grafana)

To achieve production-grade visibility, the system uses:

- CloudWatch → logs, metrics, infrastructure signals
- Grafana → centralized visualization layer

#### Why this matters in ECS

Unlike Kubernetes, ECS does not expose cluster-level introspection tools. As a result, observability becomes a first-class architectural concern.

This setup enables:

- Monitoring per-task CPU and memory usage
- Tracking ALB latency and request patterns
- Debugging container failures via logs
- Detecting deployment regressions early

Grafana acts as the primary operational dashboard, while CloudWatch serves as the source of truth for metrics and logs.

---

### Tradeoffs Summary

#### Advantages

- Lower operational complexity compared to Kubernetes
- Faster deployment cycles
- Fully managed compute and orchestration layer
- Strong AWS-native security integration (IAM + SSM)
- Efficient cost model for small-to-medium workloads

#### Disadvantages

- Less flexibility compared to Kubernetes ecosystem
- Vendor lock-in to AWS ECS
- Limited control over scheduling and orchestration internals
- Fewer extensibility options compared to EKS

---

### Final Design Intent

This architecture intentionally prioritizes:

- AWS-native simplicity over orchestration complexity
- Managed services over self-operated infrastructure
- Identity-based security over static credentials
- Observability as a core requirement, not an afterthought

The result is a production-style system that focuses on real-world cloud architecture patterns without requiring Kubernetes-level operational overhead.