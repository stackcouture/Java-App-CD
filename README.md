## Java-App-CD

This repository is the **GitOps Continuous Deployment (CD)** source for the Java Web Application, using **Argo CD**, **Helm**, and **GitHub Actions** to promote applications across environments (`dev` → `stage` → `prod`).

---

### 📦 Overview

This repository hosts:

- Helm charts for the Spring Boot application
- Environment-specific configuration (`dev`, `stage`, `prod`)
- Argo CD ApplicationSet for dynamic GitOps deployments
- GitHub Actions workflows to automate promotion between environments

---

### 🚀 Promotion Flow

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