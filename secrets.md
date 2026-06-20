# 🔑 GitHub Actions Configuration Checklist

Before triggering any CI/CD pipelines, you must configure the following **Organization-Level Secrets and Variables** in GitHub (`Settings -> Secrets and variables -> Actions`).

---

## 🔐 1. Deployment Credentials (Secrets)
*Add these under the **Secrets** tab.*

| Secret Name | Purpose | Value to Provide |
| :--- | :--- | :--- |
| `AZURE_CLIENT_ID` | OIDC Auth | Service Principal Client (App) ID |
| `AZURE_TENANT_ID` | OIDC Auth | Azure AD Tenant ID |
| `AZURE_SUBSCRIPTION_ID` | OIDC Auth | Target Azure Subscription ID |
| `ACR_LOGIN_SERVER` | Registry Auth | e.g. `youracr.azurecr.io` |
| `ACR_USERNAME` | Registry Auth | ACR Admin/SPN Username |
| `ACR_PASSWORD` | Registry Auth | ACR Admin/SPN Password |

---

## 🛡️ 2. Security Audit Tokens (Secrets)
*Add these under the **Secrets** tab.*

| Secret Name | Provider | Link |
| :--- | :--- | :--- |
| `SONAR_TOKEN` | SonarCloud | [Retrieve from Account Security](https://sonarcloud.io) |
| `SNYK_TOKEN` | Snyk.io | [Retrieve from Account Settings](https://snyk.io) |

---

## 📧 3. Notification Config (Secrets)
*Add these under the **Secrets** tab.*

| Secret Name | Purpose | Example |
| :--- | :--- | :--- |
| `SOURCE_EMAIL` | Sender Email | `ops-alerts@gmail.com` |
| `EMAIL_PASSWORD` | App Password | Gmail 16-digit App Password |
| `TARGET_EMAIL` | Alerts Recipient | `admin@company.com` |

---

## ⚙️ 4. Deployment Environment (Variables)
*Add these under the **Variables** tab.*

| Variable Name | Purpose | Example Value |
| :--- | :--- | :--- |
| `BACKEND_RG` | Terraform Backend | `infra-state-rg` |
| `BACKEND_STORAGE` | Terraform Backend | `tfstateunique123` |
| `AKS_RESOURCE_GROUP` | Deployment RG | `organistation-rg-prod` |
| `AKS_CLUSTER_NAME` | Target Cluster | `organistation-aks-prod` |

---

## 🚀 Permission Instructions
1.  Navigate to **Org Settings -> Actions -> General**.
2.  Under **Workflow permissions**, ensure "Read and write permissions" is selected.
3.  Under **Actions permissions**, select "Allow all actions and reusable workflows".
