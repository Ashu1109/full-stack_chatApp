# Deploying the backend

This document explains how to deploy the backend Helm chart manually and what secrets the GitHub Actions workflow expects.

## Manual Helm deploy

Prerequisites:
- Helm 3
- A kubeconfig with access to the target cluster

Commands:

1. Install or upgrade the chart (from repo root):

```bash
helm upgrade --install backend ./backend \
  --set image.repository=<your-repo/backend> \
  --set image.tag=<tag-or-sha> \
  --wait --timeout 5m
```

2. To rollback:

```bash
helm rollback backend 1
```

## GitHub Actions secrets (create these in the repo settings)

- REGISTRY_HOST (optional, defaults to docker.io)
- REGISTRY_USERNAME
- REGISTRY_PASSWORD
- IMAGE_REPOSITORY (e.g. yourdockerhubuser/backend)
- KUBECONFIG (the whole kubeconfig file content)

Notes:
- The provided workflow builds the image using the `backend/Dockerfile` and pushes it to `IMAGE_REPOSITORY:sha`.
- The workflow then runs `helm upgrade --install` against the `backend` chart in the repository.
