# School Management System – Enterprise-Grade GitOps Deployment on Amazon EKS

## Executive Summary

This repository represents the GitOps (Continuous Delivery) layer of a production-oriented School Management System deployed on Amazon EKS.

It demonstrates a fully automated, declarative, and self-healing Kubernetes delivery model using ArgoCD, AWS ECR, dynamic storage provisioning, and production-grade Kubernetes best practices aligned with CKA/CKAD domains.

The architecture separates CI and CD concerns while enforcing infrastructure immutability and operational reliability.

____

## High-Level Architecture

### CI Layer (Separate Repository)
- Jenkins builds optimized Docker image
- Image pushed to private AWS ECR repository
- Image tag automatically updated in CD repository

### CD Layer (This Repository)
- ArgoCD continuously monitors this Git repository
- Declarative manifests define desired cluster state
- Automated sync & self-healing enabled
- Drift detection enforced

### Runtime Platform
- Amazon EKS (Managed Kubernetes)
- Application + MySQL database
- AWS EBS-backed persistent storage
- Observability stack (Prometheus, Grafana, Loki)
- Network-level workload isolation

____

## GitOps Workflow

1. Developer pushes code changes
2. Jenkins builds new container image
3. Image pushed to AWS ECR
4. Image tag updated in this repository
5. ArgoCD detects commit
6. ArgoCD syncs cluster state
7. EKS reconciles workloads automatically

No manual kubectl apply required.

____

## Repository Structure
```
School_Management_System_CD/
│
├── applications/
│   ├── school-app.yaml
│   └── monitoring-app.yaml
│
├── school/
│   ├── namespace.yaml
│   ├── resource-quota.yaml
│   ├── network-policy.yaml
│   ├── storage-class.yaml
|   ├── mysql-init.yaml
│   ├── mysql-config.yaml
│   ├── mysql-secret.yaml
│   ├── mysql-pvc.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── school-deployment.yaml
│   └── school-service.yaml
│
├── monitoring/
│   └── values.yaml
│
└── README.md
```
____

## Application Deployment

### School Web Application

- Namespace: school
- Container Image: AWS ECR (Private Registry)
- Image Pattern:
  731628759499.dkr.ecr.us-east-1.amazonaws.com/school-management-system:<version>
- Service Type: LoadBalancer (public exposure)
- Production Features:
  - Liveness Probes
  - Readiness Probes
  - Resource Requests & Limits
  - Init Container (DB readiness validation)
  - ConfigMap for non-sensitive configuration
  - Secret for database credentials

____

### MySQL Database

- Namespace: school
- Service Type: ClusterIP (internal-only exposure)
- Persistent Storage:
  - StorageClass (AWS EBS dynamic provisioning)
  - PersistentVolumeClaim (5Gi)
- Initialization:
  - SQL bootstrap mounted via ConfigMap
- NetworkPolicy restricts inbound traffic
- Database isolated from public access

____

### Observability Stack (Helm-Based Deployment)

Namespace: monitoring

Components:
- Prometheus (metrics collection)
- Grafana (visualization)
- Loki (log aggregation)
- Promtail (log shipping)

Characteristics:
- Installed via Helm
- Fully GitOps-managed
- Internal services (ClusterIP)
- Designed for production observability

____

## Kubernetes Production Controls (CKA / CKAD Domains)

- Namespace isolation
- ResourceQuota enforcement
- NetworkPolicy (pod-level network control)
- StorageClass with dynamic provisioning (EBS)
- PersistentVolumeClaim
- Liveness & Readiness probes
- Init containers
- ConfigMaps & Secrets separation
- Declarative manifests
- GitOps auto-sync & drift reconciliation

____

## Security & Operational Hardening

- Private container registry (AWS ECR)
- IAM-based image pull authorization
- Database not externally exposed
- Secrets separated from configuration
- Resource governance enforced
- Cluster state reconciliation via ArgoCD
- Immutable container deployment model

____

## Infrastructure & Platform Stack

- Kubernetes
- Amazon EKS
- AWS ECR
- ArgoCD
- Jenkins
- Helm
- Docker
- Prometheus
- Grafana
- Loki

____

## Enterprise-Level Capabilities Demonstrated

- End-to-end CI/CD pipeline design
- GitOps-based Kubernetes delivery model
- Production-grade EKS architecture
- Secure container registry integration
- Dynamic storage provisioning (AWS EBS)
- Observability integration
- Infrastructure immutability principles
- Practical application of CKA & CKAD concepts
- Separation of CI and CD responsibilities
- Automated cluster reconciliation

____

## Portfolio Impact

This project demonstrates the ability to:

- Design and implement real-world Kubernetes architectures
- Apply GitOps methodologies in a production context
- Integrate AWS-native services with Kubernetes workloads
- Enforce operational best practices
- Deliver secure, scalable, and observable cloud-native systems