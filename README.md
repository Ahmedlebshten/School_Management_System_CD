# School Management System – GitOps Deployment on EKS

## Overview

## This repository represents the GitOps (CD) layer of the School Management System.

## It contains all Kubernetes manifests and Helm values used by ArgoCD to automatically deploy:
	•	School Web Application
	•	MySQL Database (with PVC)
	•	Monitoring Stack (Prometheus, Grafana, Loki, Promtail)

Deployed on Amazon EKS using a GitOps workflow.

⸻

## Architecture Flow
	1.	Jenkins (Infra Pipeline)
	•	Installs ArgoCD
	•	Creates the Root Application
	2.	ArgoCD
	•	Watches this repository
	•	Automatically syncs changes to EKS
	•	Deploys all applications
	3.	EKS Cluster
	•	Runs application + database
	•	Runs monitoring stack
	•	Applies self-healing & auto-sync

⸻ 

## Repository Structure
School_Management_System_CD/
│
├── applications/
│   ├── school-app.yaml
│   └── monitoring-app.yaml
│
├── school/
│   ├── mysql-configmap.yaml
│   ├── mysql-secret.yaml
│   ├── mysql-pvc.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── mysql-init.yaml
│   ├── school-deployment.yaml
│   └── school-service.yaml
│
├── monitoring/
│   └── values.yaml   # Helm values for monitoring stack
│
└── README.md

## Applications Deployed

1️⃣ School Web Application
	•	Namespace: school
	•	Image: ahmedlebshten/school_management_system:24
	•	Service Type: LoadBalancer
	•	Uses:
	•	ConfigMap (non-sensitive DB config)
	•	Secret (DB credentials)
	•	Includes:
	•	Liveness & Readiness Probes
	•	Resource Requests & Limits

⸻

2️⃣ MySQL Database
	•	Namespace: school
	•	Service Type: ClusterIP (internal only)
	•	Uses:
	•	ConfigMap (DB name, host)
	•	Secret (DB password)
	•	PersistentVolumeClaim (5Gi storage)
	•	Initialization SQL mounted via ConfigMap

⸻

3️⃣ Monitoring Stack (Helm-Based)
	•	Namespace: monitoring
	•	Installed via Helm (values.yaml)
	•	Components:
	•	Prometheus
	•	Grafana
	•	Loki
	•	Promtail
	•	Internal services (ClusterIP)

⸻

## GitOps Deployment Flow
	1.	Update Kubernetes manifests in this repo
	2.	Push changes to GitHub
	3.	ArgoCD detects changes
	4.	Cluster automatically syncs
	5.	OutOfSync resources are reconciled (self-heal enabled)

## Accessing Services

```
kubectl get svc -A
```
School App (LoadBalancer):
```
kubectl get svc -n school
```
Port-forward example:
```
kubectl -n monitoring port-forward svc/prometheus 9090:9090
kubectl -n monitoring port-forward svc/grafana 3000:3000
```
⸻

## Technologies Used
	•	Kubernetes
	•	Amazon EKS
	•	ArgoCD (GitOps CD)
	•	Jenkins (Infra Bootstrap)
	•	Docker
	•	Helm
	•	Prometheus
	•	Grafana
	•	Loki

⸻

## Key Production-Oriented Improvements
	•	✅ LoadBalancer for public access
	•	✅ MySQL Persistent Storage (PVC)
	•	✅ Secrets for credentials
	•	✅ ConfigMaps for configuration
	•	✅ Resource limits & requests
	•	✅ Liveness & Readiness probes
	•	✅ Automated GitOps sync