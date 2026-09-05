# homelab-gitops

Kubernetes desired state for the homelab reference platform.

This repository owns deployment state. Application code, tests, migrations, and Dockerfile live in `upskill-api`.

## Structure

```text
platform/
  argocd/bootstrap/       # Kustomize bootstrap for Argo CD itself
  namespaces/             # shared logical environment namespaces
apps/
  postgres/               # deployment-owned PostgreSQL dependency
  upskill-api/            # FastAPI workload deployment state
argocd/
  projects/               # Argo CD projects
  applications/           # explicit Argo CD Applications
docs/
```

## Bootstrap Order

```bash
kubectl apply -k platform/argocd/bootstrap
kubectl apply -k platform/namespaces
```

Create environment secrets outside Git before syncing PostgreSQL and the API. See `docs/secrets.md`.

Then bootstrap Argo's root application definitions:

```bash
kubectl apply -k argocd
```

Argo CD remains private. Access it locally with:

```bash
kubectl port-forward -n argocd svc/argocd-server 8080:443
```

Open `https://localhost:8080`.

Docs:

- `docs/architecture.md`
- `docs/argocd.md`
- `docs/ci.md`
- `docs/deployment.md`
- `docs/governance.md`
- `docs/secrets.md`
- `docs/promotion.md`
- `docs/operations.md`
- `docs/production-mapping.md`
