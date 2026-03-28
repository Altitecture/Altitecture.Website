# Azure Static Web Apps Deployment — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deploy the Altitecture static website to Azure Static Web Apps with GitHub Actions CI/CD and custom domains `altitecture.nl` + `www.altitecture.nl`.

**Architecture:** Azure Static Web App (Free tier, West Europe) serves the static HTML site. GitHub Actions deploys on every push to `main` using the official Azure action. Custom domains are configured via DNS at TransIP (CNAME for www, ALIAS for apex) with free managed SSL.

**Tech Stack:** Azure Static Web Apps, GitHub Actions, Azure CLI, TransIP DNS

---

### Task 1: Authenticate with Azure CLI

**Context:** All Azure commands require an authenticated session. The user needs to be logged in.

- [ ] **Step 1: Check if Azure CLI is installed**

Run:
```bash
az --version
```
Expected: Version info for `azure-cli`. If not installed, install from https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows

- [ ] **Step 2: Log in to Azure**

Run:
```bash
az login
```
Expected: Browser opens for authentication. After login, terminal shows a list of subscriptions.

- [ ] **Step 3: Verify correct subscription is active**

Run:
```bash
az account show --query "{name:name, id:id}" -o table
```
Expected: Shows the subscription you want to use. If wrong, run:
```bash
az account set --subscription "<subscription-id>"
```

---

### Task 2: Create Azure Resource Group

**Context:** All Azure resources live in a resource group.

- [ ] **Step 1: Create resource group `rg-altitecture`**

Run:
```bash
az group create --name rg-altitecture --location westeurope
```
Expected: JSON output with `"provisioningState": "Succeeded"`.

---

### Task 3: Create Azure Static Web App

**Context:** This creates the SWA resource and links it to the GitHub repo. Azure needs a GitHub token to set up the connection.

- [ ] **Step 1: Create a GitHub personal access token**

Go to https://github.com/settings/tokens and create a classic token with `repo` and `workflow` scopes. Copy the token value.

- [ ] **Step 2: Create the Static Web App**

Run (replace `<github-token>` with your token):
```bash
az staticwebapp create \
  --name altitecture-website \
  --resource-group rg-altitecture \
  --source https://github.com/Altitecture/Altitecture.Website \
  --branch main \
  --app-location "/" \
  --output-location "/" \
  --login-with-github \
  --sku Free
```
Expected: JSON output with the SWA resource details including a `defaultHostname` like `<random>.azurestaticapps.net`.

- [ ] **Step 3: Note the default hostname**

Run:
```bash
az staticwebapp show --name altitecture-website --resource-group rg-altitecture --query "defaultHostname" -o tsv
```
Expected: Something like `random-name-abc123.azurestaticapps.net`. Save this — you need it for DNS records.

---

### Task 4: Set Up GitHub Actions Deployment

**Context:** We need the deployment token as a GitHub secret, and the workflow file in the repo.

- [ ] **Step 1: Get the deployment token**

Run:
```bash
az staticwebapp secrets list --name altitecture-website --resource-group rg-altitecture --query "properties.apiKey" -o tsv
```
Expected: A long token string.

- [ ] **Step 2: Store token as GitHub secret**

Run (replace `<token>` with the value from Step 1):
```bash
gh secret set AZURE_STATIC_WEB_APPS_API_TOKEN --repo Altitecture/Altitecture.Website --body "<token>"
```
Expected: `✓ Set secret AZURE_STATIC_WEB_APPS_API_TOKEN for Altitecture/Altitecture.Website`

If `gh` CLI is not installed, alternatively add the secret manually at:
https://github.com/Altitecture/Altitecture.Website/settings/secrets/actions

- [ ] **Step 3: Create the workflow file**

Create file `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Azure Static Web Apps

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Azure Static Web Apps
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "/"
          skip_app_build: true
```

- [ ] **Step 4: Commit and push**

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow for Azure SWA deployment"
git push origin main
```
Expected: Push succeeds, GitHub Actions workflow triggers.

- [ ] **Step 5: Verify deployment**

Run:
```bash
gh run list --repo Altitecture/Altitecture.Website --limit 1
```
Expected: Shows the run with status `completed` and conclusion `success`.

Then visit the default hostname from Task 3 Step 3 in your browser to confirm the site is live.

---

### Task 5: Configure DNS at TransIP

**Context:** This is done manually in the TransIP control panel. You need the SWA default hostname from Task 3.

- [ ] **Step 1: Add CNAME record for `www`**

In TransIP DNS settings for `altitecture.nl`:
- **Type:** CNAME
- **Name:** `www`
- **Value:** `<default-hostname>.azurestaticapps.net.` (note trailing dot)
- **TTL:** 3600 (or default)

- [ ] **Step 2: Add ALIAS record for apex**

In TransIP DNS settings for `altitecture.nl`:
- **Type:** ALIAS (or ANAME if that's how TransIP labels it)
- **Name:** `@` (or leave empty for apex)
- **Value:** `<default-hostname>.azurestaticapps.net.`
- **TTL:** 3600 (or default)

Note: If TransIP does not support ALIAS/ANAME records, we'll need an alternative approach (e.g., A record with Azure's IP, or redirect apex to www).

- [ ] **Step 3: Verify DNS propagation**

Run:
```bash
nslookup www.altitecture.nl
nslookup altitecture.nl
```
Expected: Both resolve to the Azure SWA IP. May take a few minutes to propagate.

---

### Task 6: Register Custom Domains in Azure SWA

**Context:** After DNS is pointing correctly, we register the domains in Azure so it provisions SSL certificates.

- [ ] **Step 1: Add `www.altitecture.nl`**

Run:
```bash
az staticwebapp hostname set \
  --name altitecture-website \
  --resource-group rg-altitecture \
  --hostname www.altitecture.nl
```
Expected: JSON output confirming the hostname was added.

- [ ] **Step 2: Add `altitecture.nl`**

Run:
```bash
az staticwebapp hostname set \
  --name altitecture-website \
  --resource-group rg-altitecture \
  --hostname altitecture.nl
```
Expected: JSON output confirming the hostname was added.

- [ ] **Step 3: Verify SSL certificates**

Run:
```bash
az staticwebapp hostname list \
  --name altitecture-website \
  --resource-group rg-altitecture \
  -o table
```
Expected: Both hostnames listed with SSL status. Certificates may take a few minutes to provision.

- [ ] **Step 4: Test in browser**

Visit both URLs and verify:
- https://altitecture.nl loads the site with valid SSL
- https://www.altitecture.nl loads the site with valid SSL
