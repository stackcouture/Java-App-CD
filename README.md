# Java-App-CD

This repository serves as the **GitOps-driven Continuous Deployment (CD)** pipeline for the Java Web Application managed through Jenkins and **ArgoCD**. It integrates secure DevSecOps practices, automated quality gates, image signing, and deployment promotion workflows.

---

## 📦 Project Overview

This repository is part of a CI/CD ecosystem:

- **CI Repo:** [Java-APP-CI](https://github.com/stackcouture/Java-APP-CI)  
- **CD Repo (this repo):** Helm charts used by **ArgoCD** to deploy application updates.

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
