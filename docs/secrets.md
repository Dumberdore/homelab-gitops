# Secrets

Plaintext secrets are not committed to Git.

Initial homelab approach:

- Kubernetes Secrets are created by a local cluster admin.
- Kustomize manifests reference those Secrets by name.
- This can later move to AWS Secrets Manager plus External Secrets Operator.

Create PostgreSQL/app secrets per environment after namespaces exist:

```bash
for env in dev staging prod; do
  namespace="upskill-${env}"
  password="$(openssl rand -base64 32)"
  kubectl create secret generic upskill-postgres \
    -n "${namespace}" \
    --from-literal=POSTGRES_USER=upskill \
    --from-literal=POSTGRES_PASSWORD="${password}" \
    --from-literal=POSTGRES_DB=upskill \
    --from-literal=DATABASE_URL="postgresql+psycopg://upskill:${password}@postgres.${namespace}.svc.cluster.local:5432/upskill"
done
```

If GHCR packages remain private, create image pull secrets and add `imagePullSecrets` to the app manifests. For this reference setup, prefer making the published sample image public once the first package exists.
