# Secure Delivery Sample Workflow

This repository contains a minimal web app and a GitHub Actions workflow that deploys it to Azure App Service through two gates: a pre-deploy configuration check and a manual review approval. It accompanies the Pluralsight lab **Secure an Azure Delivery Workflow with Gates, Secrets, and Policy**.

## What is in this repository

| Path | Purpose |
| --- | --- |
| `src/index.html` | The minimal web app that is deployed to App Service |
| `deploy/config.json` | The deployment configuration the pre-deploy check validates |
| `.github/workflows/deploy.yml` | The secured workflow: configuration gate, then approval gate, then deploy |

## Before you run it

The repository must be **public** so the required reviewer approval is available on a free GitHub account. Complete these in your own copy:

1. Enable GitHub Actions on the repository if it is not already enabled.
2. Add a repository secret named `AZURE_PUBLISH_PROFILE` containing the publish profile downloaded from your App Service.
3. Create an environment named `production` and add yourself as a required reviewer.

The app name is read from the publish profile, so no other Azure credential is required.

## How the gates behave

The workflow runs in two stages.

1. The configuration check reads `deploy/config.json` and fails if `enforceHttps` is not `true`. The file ships set to `false`, so the first run fails the check and blocks the deployment. This is the intended starting point.
2. After you set `enforceHttps` to `true` and run the workflow again, the check passes and the deploy job pauses for review. Approve the deployment to let it proceed.

Once approved, the workflow deploys the contents of `src/` to your App Service.

> The detailed, step-by-step instructions are in the lab guide. This README is a short orientation to the repository itself.
