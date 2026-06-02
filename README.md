# Production-Grade AWS Infrastructure (ECS Fargate)

## Overview

This repository demonstrates a production-style AWS architecture built using ECS Fargate and Terraform.

The infrastructure is designed to remain available during common failure scenarios such as:

* Container crashes

* Failed health checks

* Availability Zone outages

* Database failover events

The project also incorporates AWS-native security patterns including IAM Roles for Service Accounts (OIDC-based identity) and AWS Systems Manager for secrets management.

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

* IAM Roles for Service Accounts (OIDC integration)

* AWS Systems Manager (SSM) for secrets management


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

### IAM Roles for Service Accounts (OIDC)

Enables ECS tasks to securely assume IAM roles without static credentials, following a least-privilege security model.

### AWS Systems Manager (SSM)

Used for centralized storage and retrieval of configuration values and secrets at runtime.

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

## Security & Identity Design

This architecture uses AWS-native identity and secrets management patterns:

IAM Roles for Service Accounts (OIDC) for workload-level AWS access control

AWS Systems Manager (SSM) for secure parameter and secret storage

No long-lived AWS credentials stored in containers or application code

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