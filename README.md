# Production-Grade AWS Infrastructure (ECS Fargate)

## Overview

This repository demonstrates a production-style AWS architecture built using ECS Fargate and Terraform.

The infrastructure is designed to remain available during common failure scenarios such as:

* Container crashes
* Failed health checks
* Availability Zone outages
* Database failover events

The project emphasizes infrastructure automation, high availability, scalability, and operational simplicity using managed AWS services.

---

## Architecture

![AWS Architecture Diagram](docs/AWS-DIAGRAM.png)

### Architecture Summary

Client → Internet Gateway → Application Load Balancer → ECS Fargate → RDS Multi-AZ

Key characteristics:

* Multi-AZ deployment for high availability
* Private subnet isolation for backend resources
* Application Load Balancer with health checks
* ECS Fargate for serverless container orchestration
* RDS Multi-AZ for database resilience
* Infrastructure managed with Terraform
* Automated deployments through GitHub Actions

[Deep Dive Documentation](docs/infrastructure.md)

---

## Core Components

### ECS Fargate

Runs containerized workloads without managing EC2 instances.

### Application Load Balancer (ALB)

Distributes traffic across healthy targets and Availability Zones.

### Amazon RDS (Multi-AZ)

Provides automatic failover and database high availability.

### Amazon VPC

Network segmentation using public and private subnets.

### Terraform

Infrastructure as Code for repeatable and version-controlled deployments.

### GitHub Actions

Automated CI/CD pipeline for infrastructure and application deployments.

---

## Failure Handling

### Container Failure

If a container crashes, ECS automatically launches a replacement task.

### Health Check Failure

Unhealthy tasks are removed from the load balancer target group until healthy again.

### Availability Zone Failure

Traffic is automatically routed to healthy targets in the remaining Availability Zone.

### Database Failure

RDS Multi-AZ performs automatic failover to the standby instance.

---

## Project Documentation

### Challenges Faced

[View Challenges](docs/challenges.md)

### Design Decisions

[View Decisions](docs/decisions.md)

### Installation Guide

[Getting Started](docs/installation.md)

### Future Improvements

[Future Plans](docs/future-projects.md)

---

## Deployment

### 1. Clone Repository

```bash
git clone https://github.com/klvnjntn-lgtm/ecs-lab.git

cd ecs-lab
```

### 2. Initialize Terraform

```bash
terraform init
```

### 3. Review Planned Changes

```bash
terraform plan
```

### 4. Deploy Infrastructure

```bash
terraform apply
```

---

## Deployment Strategy

The project supports production-style deployment patterns:

* Rolling deployments through ECS Services
* Blue/Green deployment architecture using dual target groups
* Health-check-based traffic shifting
* Zero-downtime application releases

---

## Skills Demonstrated

* AWS Networking (VPC, Subnets, Route Tables, NAT Gateway)
* ECS Fargate
* Application Load Balancer
* RDS Multi-AZ
* Terraform
* Infrastructure as Code
* CI/CD Automation
* High Availability Design
* Failure Recovery
* Cloud Cost Awareness
* Production Architecture Design


<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | ~> 5.0 |
| <a name="requirement_grafana"></a> [grafana](#requirement\_grafana) | ~> 4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | 5.100.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_alb"></a> [alb](#module\_alb) | ./modules/alb | n/a |
| <a name="module_ecr"></a> [ecr](#module\_ecr) | ./modules/ecr | n/a |
| <a name="module_ecs"></a> [ecs](#module\_ecs) | ./modules/ecs | n/a |
| <a name="module_monitoring"></a> [monitoring](#module\_monitoring) | ./modules/monitoring | n/a |
| <a name="module_network"></a> [network](#module\_network) | ./modules/network | n/a |
| <a name="module_rds"></a> [rds](#module\_rds) | ./modules/rds | n/a |

## Resources

| Name | Type |
|------|------|
| [aws_budgets_budget.monthly_limit](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/budgets_budget) | resource |
| [aws_iam_policy.enforce_mfa](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role_policy.ecs_task_secrets_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_availability_zones"></a> [availability\_zones](#input\_availability\_zones) | List of AZs to deploy into | `list(string)` | n/a | yes |
| <a name="input_container_name"></a> [container\_name](#input\_container\_name) | n/a | `string` | `"my-app-container"` | no |
| <a name="input_monthly_budget_limit"></a> [monthly\_budget\_limit](#input\_monthly\_budget\_limit) | n/a | `string` | `"10.0"` | no |
| <a name="input_project_name"></a> [project\_name](#input\_project\_name) | The prefix for all resources in this project | `string` | `"Kelvin-Cloud-Project"` | no |
| <a name="input_public_subnet_cidrs"></a> [public\_subnet\_cidrs](#input\_public\_subnet\_cidrs) | CIDR blocks for the public subnets | `list(string)` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_active_target_groups"></a> [active\_target\_groups](#output\_active\_target\_groups) | n/a |
| <a name="output_alb_public_url"></a> [alb\_public\_url](#output\_alb\_public\_url) | The public URL to access Kelvin's Web Server |
| <a name="output_container_name"></a> [container\_name](#output\_container\_name) | n/a |
| <a name="output_ecs_cluster_name"></a> [ecs\_cluster\_name](#output\_ecs\_cluster\_name) | n/a |
| <a name="output_ecs_service_name"></a> [ecs\_service\_name](#output\_ecs\_service\_name) | n/a |
| <a name="output_ecs_task_definition_family"></a> [ecs\_task\_definition\_family](#output\_ecs\_task\_definition\_family) | n/a |
<!-- END_TF_DOCS -->
