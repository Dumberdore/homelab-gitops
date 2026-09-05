# Governance

This repository is the Kubernetes source of truth.

Initial ownership uses `@Dumberdore` as the sole CODEOWNER.

Intended production shape:

- Platform owns `platform/`, `argocd/`, shared deployment conventions, and cluster-level guardrails.
- Application teams own their app overlays within approved platform conventions.
- DEV can be updated by release automation.
- STAGING and PROD promotions should require PR review and GitHub Environment approval.
- Argo CD deploys only what is committed to Git.

Current automation exception:

- The `upskill-api` release workflow uses a write deploy key to update `apps/upskill-api/overlays/dev/kustomization.yaml`.
- This enables automatic DEV deployment without giving GitHub Actions Kubernetes credentials.

Future improvement:

- Replace the deploy key with a GitHub App or dedicated service account for stronger auditability and more precise bypass rules.

Current repository controls:

- Repository visibility is public.
- `main` requires pull requests.
- CODEOWNERS review is required.
- The `kustomize` status check is required.
- Force pushes and branch deletion are blocked.
- The `upskill-api` release deploy key may bypass for DEV image-tag automation.
- `@Dumberdore` may bypass only through pull requests for single-owner operability.

GitHub Environments:

- `dev`: unrestricted; used for automation-friendly development deployment.
- `staging`: required reviewer is `@Dumberdore`.
- `prod`: required reviewer is `@Dumberdore`.
