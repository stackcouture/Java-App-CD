## Java-App-CD

This repository is the **GitOps Continuous Deployment (CD)** source for the Java Web Application, using **Argo CD**, **Helm**, and **GitHub Actions** to promote applications across environments (`dev` → `stage` → `prod`).

---

### Overview

This repository hosts:

- Helm charts for the Spring Boot application
- Environment-specific configuration (`dev`, `stage`, `prod`)
- Argo CD ApplicationSet for dynamic GitOps deployments
- GitHub Actions workflows to automate promotion between environments

---

### Promotion Flow

The promotion is automated and driven through GitHub Actions workflows and Argo CD:

dev-values.yaml → stage-values.yaml → prod-values.yaml


| File/Folder                     | Description                                  |
|--------------------------------|----------------------------------------------|
| `.github/workflows/`           | GitHub Actions workflows                     |
| `promote-dev-stage.yml`        | Promotes `dev` to `stage`                    |
| `promote-stage-prod.yml`       | Promotes `stage` to `prod`                   |
| `argoapps/`                    | Contains Argo CD ApplicationSet definitions  |
| `helm-charts/springboot/`      | Helm chart for Spring Boot app               |
| `templates/`                   | Helm templates: `deployment`, `service`, etc |
| `values/`                      | Environment-specific Helm values             |
| `Chart.yaml`                   | Helm chart metadata                          |

---

### Argo CD Integration

Argo CD monitors this repository and applies changes based on the `ApplicationSet` defined in:

```bash 
argoapps/
    app-project.yaml
    applicationset.yaml
```

```bash 
    kubectl apply -f argoapps/app-project.yaml 
    kubectl apply -f argoapps/applicationset.yaml
```

This setup allows Argo CD to:

- Automatically deploy to `dev`, `stage`, or `prod`
- Use a single Helm chart with different `values` files
- Handle multi-env GitOps from a central source

---

### GitHub Actions Workflows

| Workflow                     | Description                        |
|-----------------------------|------------------------------------|
| `promote-dev-stage.yml`     | Promotes latest dev image to stage |
| `promote-stage-prod.yml`    | Promotes stage image to prod       |

These workflows likely:
- Update the appropriate `values-*.yaml` file with a new `image.tag`
- Commit the change
- Argo CD then syncs the new configuration

---

### Helm Chart Structure

```bash
helm-charts/springboot/
├── Chart.yaml
├── templates/
│   ├── deployment.yaml
│   ├── ingress.yaml
│   └── service.yaml
└── values/
    ├── dev-values.yaml
    ├── stage-values.yaml
    └── prod-values.yaml
```

---
### How to Promote

You can promote deployments either manually or through automation using GitHub Actions. The promotion process works as follows:

1. Trigger the Promotion Workflow

Run the GitHub Actions workflow (manually or automatically) to start the promotion process.

2. Read Current Image Tag

The workflow reads the current image.tag from the source environment (e.g., dev or staging).

3. Copy to Target Environment

The same image tag is copied into the values file of the target environment (e.g., staging → prod).

4. Commit & Push Changes

The updated values file is committed and pushed to the Git repository.

5. Argo CD Sync

Argo CD detects the change and syncs the target environment with the updated configuration.

---
```bash 
    A[CI builds and pushes image to ECR]
    B[Jenkins or CI updates dev-values.yaml]
    C[Argo CD deploys to dev]
    D[GitHub Action: promote-dev-stage]
    E[Update stage-values.yaml]
    F[Argo CD deploys to stage]
    G[GitHub Action: promote-stage-prod]
    H[Update prod-values.yaml]
    I[Argo CD deploys to prod]

    A --> B --> C --> D --> E --> F --> G --> H --> I
```
---
📋 Step-by-Step Breakdown
| Step           | Description                                                               |
|----------------|---------------------------------------------------------------------------|
| 🏗️ 1. CI Build     | CI (e.g., Jenkins) builds the Docker image and pushes it to Amazon ECR.   |
| 📝 2. Dev Update   | The `dev-values.yaml` file is updated with the new `image.tag`.           |
| 🚀 3. Dev Deploy   | Argo CD detects the change and deploys to the `dev` environment.          |
| 🔄 4. Promote to Stage | GitHub Action `promote-dev-stage` copies the image tag to `stage-values.yaml`. |
| 🚀 5. Stage Deploy | Argo CD deploys to the `stage` environment.                               |
| 🔄 6. Promote to Prod  | GitHub Action `promote-stage-prod` updates `prod-values.yaml`.            |
| 🚀 7. Prod Deploy  | Argo CD syncs and deploys to the `prod` environment.                      |


```