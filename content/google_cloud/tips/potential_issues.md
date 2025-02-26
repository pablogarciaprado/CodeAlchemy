◀️ [Home](../../../README.md)

## Fixing GCP Project Mismatch Issue in BigQuery
If you encounter an error where your active GCP project does not match your Application Default Credentials (ADC) quota project, follow these steps:

### Problem
You changed your GCP project using:

```sh
gcloud config set project locaria-dev-finance-reports
```
However, you received this warning:
```
WARNING: Your active project does not match the quota project in your local Application Default Credentials file. This might result in unexpected quota issues.
```
Besides causing quota or billing issues, this can prevent you from running certain scripts.

### Solution
#### 1. Set the Active GCP Project
```sh
gcloud config set project your-project-id
```
Verify running:
```sh
gcloud config get-value project
```

#### 2. Update ADC (Application Default Credentials) Quota Project
Fix the quota mismatch:
```sh
gcloud auth application-default set-quota-project your-project-id
```

#### 3. Verify Authentication & Credentials
Check if the quota project is updated:
```sh
gcloud auth application-default print-access-token
```
If the issue persists, re-authenticate:
```sh
gcloud auth application-default login
```
