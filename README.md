# 🛡️ OrganiStation Shared CI Workflows

This repository serves as the **Centralized Security Engine** for the entire OrganiStation organization. It contains the reusable GitHub Actions logic that enforces DevSecOps best practices across all microservices.

---

## 🏗️ The Reusable Strategy

Instead of maintaining separate build logic in 8+ repositories, we use a single source of truth:
**`reusable-ci.yaml`**

By referencing this file, every microservice automatically benefits from:
1.  **Consolidated Logic**: One fix here updates all microservices instantly.
2.  **Standardized Security**: Every build must pass the exact same quality gates.
3.  **Audit Trail**: Centralized logging for organization-wide compliance.
4.  **Immutable Tags**: Images are pushed to ACR with the first 7 characters of the commit SHA (e.g. `a1b2c3d`), not a static version.

---

## 🔒 Security Gate Lifecycle (5 Layers)

| Stage | Tool | Purpose |
| :--- | :--- | :--- |
| **Linting** | **Hadolint** | Validates Dockerfile security (Anti-Root, Fixed Versions). |
| **SCA** | **Snyk (Python)** | Scans dependencies for known CVEs (Fails on HIGH/CRITICAL). |
| **Secrets** | **Trivy (FS)** | Detects API Keys, Secrets, and Credentials in the codebase. |
| **Quality** | **SonarCloud** | Enforces Code Quality, Coverage, and Reliability Gates. |
| **Container** | **Trivy (Image)** | Final audit of the built Docker Image layers before pushing. |

---

## 🚀 How to use in a Microservice

Add this file to your microservice at `.github/workflows/build.yaml`:

```yaml
jobs:
  ci:
    uses: OrganiStation-org/shared-workflows/.github/workflows/reusable-ci.yaml@develop
    with:
      image_name: your-service-name
      language: node
      sonar_org: your-sonar-org
      sonar_project_key: your-project-key
    secrets: inherit

  # Optional: read the SHA tag pushed to ACR
  deploy-hint:
    needs: ci
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deployed image tag ${{ needs.ci.outputs.image_tag }}"
```

---

## 🔑 Required Organization Secrets

For the pipeline to authenticate, the following Org-level secrets are mandatory:
*   **Azure OIDC**: `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID`.
*   **Audit Tools**: `SONAR_TOKEN`, `SNYK_TOKEN`.
*   **Registry**: `ACR_LOGIN_SERVER`.
*   **Alerts**: `SOURCE_EMAIL`, `EMAIL_PASSWORD`, `TARGET_EMAIL`.
