# Deployment

Deployment is Kustomize-based.

DEV application flow:

```mermaid
sequenceDiagram
  participant CI as GitHub Actions
  participant GHCR as GHCR
  participant GitOps as homelab-gitops
  participant Argo as Argo CD
  participant K8s as Kubernetes

  CI->>GHCR: Push ghcr.io/dumberdore/upskill-api:sha-xxxxxxx
  CI->>GitOps: kustomize edit set image in DEV overlay
  Argo->>GitOps: Pull desired state
  Argo->>K8s: Run PreSync migration Job
  Argo->>K8s: Roll out Deployment
```

The app ServiceAccount is also created as an early PreSync prerequisite so migration Jobs can run without using the namespace default ServiceAccount.

GitHub Actions never runs `kubectl` and never needs cluster credentials.

PostgreSQL is deployed from this repo to emphasize that environment dependencies and app code are separate concerns.
