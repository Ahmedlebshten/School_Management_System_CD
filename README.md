# School Management System - CI/CD on EKS using ArgoCD

## Overview

A comprehensive Kubernetes-native CI/CD and monitoring solution for the School Management System deployed on Amazon EKS using ArgoCD for GitOps-based deployment and includes full monitoring stack with Prometheus, Grafana, and Loki.

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
├── applications/
│   ├── school-app.yaml               # School Management System ArgoCD App
│   ├── prometheus-app.yaml           # Prometheus ArgoCD App
│   ├── grafana-app.yaml              # Grafana ArgoCD App
│   └── loki-app.yaml                 # Loki ArgoCD App
│
├── school/
│   ├── deployment.yaml               # Kubernetes Deployment manifest
│   └── service.yaml                  # Kubernetes Service manifest
│
└── monitoring/
    ├── prometheus.yaml               # Prometheus configuration
    ├── grafana.yaml                  # Grafana configuration
    ├── loki.yaml                     # Loki log aggregation config
    └── promtail.yaml                 # Promtail log shipper config
```

---

## Applications Deployed

### 1. School Management System
- **Namespace:** school
- **Container Image:** ahmedlebshten/school_management_system:4
- **Port:** 2020
- **Replicas:** 1

### 2. Prometheus
- **Namespace:** monitoring
- **Retention:** 30 days
- **Resources:** 1 CPU / 2GB RAM (min), 2 CPU / 4GB RAM (max)
- **Service Type:** ClusterIP (Port 9090)

### 3. Grafana
- **Namespace:** monitoring
- **Data Source:** Prometheus
- **Purpose:** Visualization and dashboards

### 4. Loki
- **Namespace:** monitoring
- **Purpose:** Centralized log aggregation
- **Log Shipper:** Promtail

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
kubectl port-forward -n monitoring svc/prometheus 9090:9090
kubectl port-forward -n monitoring svc/grafana 3000:3000
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
