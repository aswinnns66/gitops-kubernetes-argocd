# gitops-kubernetes-argocd

# GitOps Workflow with ArgoCD on Kubernetes (Project 10)

An automated Continuous Deployment (CD) pipeline implementing the GitOps methodology on Kubernetes using ArgoCD. All application configurations and infrastructure states are declaratively tracked in Git, enabling automated synchronization, drift detection, and automated rollback capabilities.

---

## Architecture Overview

+---------------------+         +---------------------------------------------------+
|                     |         |                 AWS EC2 (K3s Node)                |
|  GitHub Repository  |         |                                                   |
| (Manifests / Truth) |<--------|--+  [ArgoCD Application Controller]               |
+----------+----------+  Poll/  |  |                 |                              |
|             Sync   |  |                 v (Reconcile)                  |
| Commit             |  |  +------------------------------------------+  |
v                    |  |  | Kubernetes Workload (default namespace)  |  |
+--------------+            |  |  |                                          |  |
| Deployment / |            |  +--|  - Deployment: gitops-webapp             |  |
| Service YAML |            |     |  - ReplicaSet: 2 Pods (Nginx)            |  |
+--------------+            |     |  - Service: NodePort (Port 30080)        |  |
|     +------------------------------------------+  |
+---------------------------------------------------+
|
v
HTTP User / Browser (Port 30080)


---

## Repository Structure

├── manifests/
│   ├── deployment.yaml      # Kubernetes Deployment manifest (2 Replicas, Nginx)
│   └── service.yaml         # NodePort Service exposing port 80 on 30080
├── argocd/
│   └── application.yaml     # ArgoCD Custom Resource (CRD) defining sync policy
└── README.md
