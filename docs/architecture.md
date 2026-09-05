# Architecture

```mermaid
flowchart LR
  app[upskill-api repo] --> ci[GitHub Actions]
  ci --> ghcr[GHCR sha image]
  ci --> gitops[homelab-gitops]
  argocd[Argo CD in Talos] --> gitops
  argocd --> ns[Namespaces]
  argocd --> pg[PostgreSQL]
  argocd --> api[upskill-api]
  api --> pg
```

Repository ownership:

- `upskill-api`: app code, tests, Alembic migrations, Dockerfile, image release workflow.
- `homelab-gitops`: Kustomize deployment state, Argo CD applications, environment promotion.

This deliberately mirrors a production split where application teams publish immutable artifacts and platform/GitOps state declares where those artifacts run.
