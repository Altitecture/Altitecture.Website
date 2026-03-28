# Azure Static Web Apps Deployment — Design Spec

## Overview

Deploy the Altitecture website (single static HTML page) to Azure Static Web Apps using GitHub Actions, with custom domains `altitecture.nl` and `www.altitecture.nl`.

## Architecture

```
GitHub (main branch)
  └─ push triggers ──▶ GitHub Actions workflow
                          └─ Azure/static-web-apps-deploy@v1
                              └─▶ Azure Static Web App (Free tier, West Europe)
                                    ├── altitecture.nl (ALIAS record)
                                    ├── www.altitecture.nl (CNAME record)
                                    └── Free managed SSL for both
```

## Azure Resource

- **Resource group:** `rg-altitecture` (create if not exists)
- **Resource type:** Static Web App
- **SKU:** Free
- **Region:** West Europe
- **Source:** GitHub repo `Altitecture/Altitecture.Website`, branch `main`
- **App location:** `/` (root of repo)
- **API location:** none
- **Build:** skipped (no build step — static HTML with Tailwind CDN)

## GitHub Actions Workflow

- **File:** `.github/workflows/deploy.yml`
- **Trigger:** push to `main`
- **Secret:** `AZURE_STATIC_WEB_APPS_API_TOKEN` (deployment token from Azure SWA)
- **Action:** `Azure/static-web-apps-deploy@v1`
- **Config:** `app_location: "/"`, `skip_app_build: true`

## Custom Domains

### `www.altitecture.nl`
- **DNS (TransIP):** CNAME record pointing to `<swa-name>.azurestaticapps.net`
- **Azure:** Add as custom domain via `az staticwebapp hostname set`
- **SSL:** Free managed certificate, provisioned automatically after DNS validation

### `altitecture.nl` (apex)
- **DNS (TransIP):** ALIAS/ANAME record pointing to `<swa-name>.azurestaticapps.net`
- **Azure:** Add as custom domain via `az staticwebapp hostname set`
- **SSL:** Free managed certificate, provisioned automatically after DNS validation

### Behavior
- Both domains serve the same site
- Both have independent SSL certificates managed by Azure
- No automatic redirect between apex and www (both work independently)

## Implementation Steps

1. Create Azure resource group `rg-altitecture` (if not exists)
2. Create Azure Static Web App linked to GitHub repo
3. Retrieve deployment token, store as GitHub secret `AZURE_STATIC_WEB_APPS_API_TOKEN`
4. Create `.github/workflows/deploy.yml` in the repo
5. Push workflow — triggers first deployment
6. Add CNAME record for `www.altitecture.nl` at TransIP
7. Add ALIAS record for `altitecture.nl` at TransIP
8. Register both custom domains in Azure SWA
9. Verify SSL certificates are provisioned for both domains
