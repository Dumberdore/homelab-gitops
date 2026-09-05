# Argo CD

Argo CD is installed inside the private homelab cluster using Kustomize:

```bash
kubectl apply -k platform/argocd/bootstrap
```

It is not exposed publicly. Local admin access:

```bash
kubectl port-forward -n argocd svc/argocd-server 8080:443
```

The repo currently uses explicit `Application` resources instead of `ApplicationSet`.

Because `homelab-gitops` is private, Argo CD needs a read-only SSH deploy key registered as an Argo repository Secret. That Secret is created directly in the cluster and is not committed to Git.

Reason:

- There is one service, one database dependency, and three logical environments.
- Explicit Applications are easier to troubleshoot during initial setup.
- The generated matrix is small enough that ApplicationSet would add more abstraction than value.

When to move to ApplicationSet:

- Multiple services share the same environment pattern.
- Multiple clusters are introduced.
- Preview environments are generated automatically.
- The number of Argo Applications becomes tedious to maintain manually.

DEV auto-syncs and self-heals. STAGING and PROD are intentionally manual for promotion control.
