# Promotion

Artifact flow:

```text
main -> DEV -> STAGING -> PROD
```

Rules:

- Build once.
- Promote the same image tag.
- Never rebuild between environments.
- DEV can auto-sync.
- STAGING and PROD require deliberate Git changes and manual Argo sync.

Promote DEV to STAGING:

```bash
cd apps/upskill-api/overlays/staging
kustomize edit set image ghcr.io/dumberdore/upskill-api=ghcr.io/dumberdore/upskill-api:<dev-sha-tag>
```

Promote STAGING to PROD by applying the same tag to `apps/upskill-api/overlays/prod/kustomization.yaml`.
