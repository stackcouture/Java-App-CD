# Java-App-CD

This repository serves as the **GitOps-driven Continuous Deployment (CD)** pipeline for the Java Web Application managed through Jenkins and **ArgoCD**. It integrates secure DevSecOps practices, automated quality gates, image signing, and deployment promotion workflows.

---

## 📦 Project Overview

This repository is part of a CI/CD ecosystem:

- **CI Repo:** [Java-WebAPP-CI](https://github.com/stackcouture/Java-WebAPP-CI)  
- **CD Repo (this repo):** Contains Kubernetes manifests or Helm charts used by **ArgoCD** to deploy application updates.

---

## 🚀 CD Pipeline Summary

The Jenkins pipeline performs the following actions:

1. **Checkout CD Repository**
2. **Wait for Manual Approval** before deployment
3. **Update `imageTag` in deployment manifests**
4. **Commit and Push Changes** to this GitOps repository
5. **ArgoCD Syncs Automatically** to deploy the new version

---

## 🔧 Technologies Used

| Tool           | Purpose                                  |
|----------------|------------------------------------------|
| **Jenkins**    | Orchestrates the CD workflow             |
| **ArgoCD**     | GitOps-based deployment engine           |
| **AWS ECR**    | Stores signed Docker images              |
| **Cosign**     | Signs Docker images (Sigstore)           |
| **GitHub**     | Source control and GitOps integration    |

---
