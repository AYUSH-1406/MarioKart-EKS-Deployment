
# 🚀 Mario GitOps Deployment on AWS EKS

This repository contains Kubernetes manifests for deploying a containerized Mario application to Amazon EKS using a GitOps workflow powered by ArgoCD.

The application runs on private EKS worker nodes and is exposed publicly via AWS Application Load Balancer (ALB).

---

# 🏗 Architecture Overview

```
Internet
   ↓
AWS ALB (Public Subnets)
   ↓
EKS Worker Nodes (Private Subnets)
   ↓
Kubernetes Pods
```

Infrastructure is provisioned using Terraform (separate infra repository).
Application deployment is managed entirely via GitOps using ArgoCD.

---

# 🔁 GitOps Workflow

1. Third-party DockerHub image is mirrored to Amazon ECR.
2. Image reference is defined in Kubernetes manifests inside this repository.
3. ArgoCD continuously monitors this repository.
4. On every commit, ArgoCD automatically syncs the cluster state.
5. Deployment is updated in EKS without manual `kubectl apply`.

No direct CI-to-cluster access is used.

Git is the single source of truth.

---

# 📦 Repository Structure

```
mario-gitops/
│
├── deployment.yaml
├── service.yaml
└── ingress.yaml
```

* `deployment.yaml` – Defines the Mario application deployment.
* `service.yaml` – Exposes the application internally.
* `ingress.yaml` – Configures ALB ingress for public access.

---

# ☁️ Platform Stack

* AWS VPC (public + private subnets)
* Amazon EKS (Managed Node Group)
* Amazon ECR (private container registry)
* AWS ALB (Ingress)
* Terraform (Infrastructure as Code)
* ArgoCD (GitOps deployment)
* DockerHub → ECR mirroring

---

# 🔐 Security Design

* Worker nodes run in private subnets.
* NAT Gateway used for outbound internet access.
* ALB deployed in public subnets.
* Images stored in private ECR.
* No direct CI/CD access to Kubernetes cluster.
* ArgoCD handles cluster synchronization.

---

# 🚀 Deployment Flow

ArgoCD Application resource points to this repository:

```
spec:
  source:
    repoURL: https://github.com/AYUSH-1406/mario-gitops.git
    targetRevision: main
```

When changes are pushed:

```
Git Push
   ↓
ArgoCD detects change
   ↓
Automatic Sync
   ↓
EKS Updated
```

---

# 📸 Recommended Screenshots (Add to Repo)

Add these screenshots to demonstrate working setup:

1. `kubectl get nodes` output
<img width="896" height="226" alt="image" src="https://github.com/user-attachments/assets/4f8b4c0e-44fc-405d-ac9d-3102df2da682" />

2. ArgoCD Dashboard showing:
   * App Health: Healthy
   * Sync Status: Synced
<img width="1919" height="826" alt="image" src="https://github.com/user-attachments/assets/934cd688-7a59-4357-bde5-c04b75a1e8fc" />

3. AWS ALB console showing active load balancer
<img width="1573" height="359" alt="image" src="https://github.com/user-attachments/assets/a54287a6-6a7c-4391-8605-f69410b98935" />

4. Application accessible via ALB DNS
<img width="1914" height="932" alt="image" src="https://github.com/user-attachments/assets/4df4e2ec-ed10-43e7-861e-eb27e9d6d31f" />

5. ECR repository showing mirrored image
<img width="1567" height="486" alt="image" src="https://github.com/user-attachments/assets/965b19e2-655e-45cb-9736-f4055526929b" />


---

# 🎯 What This Project Demonstrates

* Production-style Kubernetes networking
* GitOps deployment strategy
* Secure EKS cluster design
* Infrastructure as Code (Terraform)
* CI image mirroring workflow
* ALB-based ingress configuration

---

# 👨‍💻 Author

Ayush Srivastava
DevOps | Cloud | Platform Engineering

---
