# School Management System - CI/CD on EKS using ArgoCD

## Overview

This repository represents the GitOps (CD) layer of the School Management System project.

It contains all Kubernetes manifests and Helm values used by ArgoCD to automatically deploy:

- School Application
- MySQL Database
- Monitoring Stack (Prometheus, Grafana, Loki, Promtail)

Deployed on Amazon EKS using a GitOps workflow.

---

## Project Architecture

### Flow
1. **ArgoCD (CD)**
   - Watches this GitHub repository
   - Syncs changes automatically to EKS
   - Deploys applications and monitoring stack

2. **EKS (Kubernetes)**
   - Runs the School Management System application
   - Exposes it using a Service
   - Includes monitoring and logging infrastructure

3. **Monitoring Stack**
   - **Prometheus** - Metrics collection and alerting
   - **Grafana** - Metrics visualization and dashboards
   - **Loki** - Log aggregation and querying
   - **Promtail** - Log shipper for Loki

---

## Repository Structure

```
School_Management_System_CD/
│
├── applications/
│   ├── school-app.yaml          # ArgoCD App for school
│   └── monitoring-app.yaml      # ArgoCD App for monitoring stack
│
├── school/
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── mysql-init.yaml
│   ├── school-deployment.yaml
│   ├── school-service.yaml
│   └── school-env.yaml
│
├── monitoring/
│   └── values.yaml              # Helm values for monitoring stack
│
└── README.md
```

---

## Applications Deployed

### 1. School Management System
- **Namespace:** school
- **Container Image:** ahmedlebshten/school_management_system:image-tag
- **Service Type:** ClusterIP
- **Replicas:** 1

### 2.MySQL Database 
- **Namespace:** school 
- **Service Type:** ClusterIP 
- Internal only (not exposed externally) 
- Currently without PVC (ephemeral storage)

### 3.Monitoring Stack (Helm-Based) 
Installed using Helm values: 
- **Namespace:** monitoring
- **Service Types:** ClusterIP 
- Prometheus 
- Grafana 
- Loki 
- Promtail  

---

## How Deployment Works

1. **Push code to GitHub** - Triggers automated build
2. **Update YAML manifests** - Commit to this repo
3. **ArgoCD detects changes** - Automatically syncs
4. **EKS applies configuration** - Deploys updated applications and monitoring

---

## Accessing Applications

```bash
# Get all services
kubectl get svc -A

# School App
kubectl get svc -n school

# Monitoring Stack
kubectl get svc -n monitoring

# Port forwarding (example)
kubectl -n argocd port-forward svc/argocd-server 9090:443
```

---

## Used Technologies

- **Kubernetes** - Container orchestration
- **Amazon EKS** - Managed Kubernetes service
- **ArgoCD** - GitOps continuous deployment
- **Docker** - Containerization
- **Prometheus** - Metrics and alerting
- **Grafana** - Data visualization
- **Loki** - Log aggregation
- **Promtail** - Log shipping

---

## Prerequisites

- Amazon EKS cluster (configured)
- ArgoCD installed on the cluster
- kubectl configured to access the cluster
- Docker Hub account for container images

---

## Quick Start

1. Add this repository to ArgoCD
2. Create the root Application to deploy all components
3. Monitor deployment progress in ArgoCD UI
4. Access applications through port-forwarding or configured ingress

---

## Notes

- All applications use automated sync with pruning enabled
- Namespaces are created automatically (CreateNamespace=true)
- Self-healing is enabled for all applications
- Monitoring stack collects metrics from all namespaces
