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

Because the repositories were created as private, GHCR image pulls require either a public package or an image pull secret. The app ServiceAccount references `ghcr-pull` for private-image pulls.

Create one `ghcr-pull` Secret per environment using a token with `read:packages` access:

```bash
export GITHUB_USER="Dumberdore"
export GITHUB_PAT_READ_PACKAGES="<token-with-read-packages>"

for env in dev staging prod; do
  namespace="upskill-${env}"
  kubectl create secret docker-registry ghcr-pull \
    -n "${namespace}" \
    --docker-server=ghcr.io \
    --docker-username="${GITHUB_USER}" \
    --docker-password="${GITHUB_PAT_READ_PACKAGES}" \
    --dry-run=client -o yaml | kubectl apply -f -
done
```

Alternative: make the GHCR package public after the first image is published, then remove the `ghcr-pull` reference from the ServiceAccount.
