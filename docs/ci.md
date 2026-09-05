# CI

The GitOps repository has a validation workflow that renders every Kustomize overlay.

It does not contact the Kubernetes API and does not need kubeconfig.

Validated paths:

- `platform/namespaces`
- `apps/postgres/overlays/dev`
- `apps/postgres/overlays/staging`
- `apps/postgres/overlays/prod`
- `apps/upskill-api/overlays/dev`
- `apps/upskill-api/overlays/staging`
- `apps/upskill-api/overlays/prod`
- `argocd`
- `platform/argocd/bootstrap`
