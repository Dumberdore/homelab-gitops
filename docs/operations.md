# Operations

Useful commands:

```bash
kubectl get applications -n argocd
kubectl get pods -n upskill-dev
kubectl get statefulset,pvc,svc -n upskill-dev
kubectl logs -n upskill-dev deploy/upskill-api
kubectl logs -n upskill-dev job/upskill-api-migrate
kubectl logs -n upskill-dev job/upskill-api-notify
```

Drift test after DEV is healthy:

```bash
kubectl scale deployment upskill-api -n upskill-dev --replicas=7
kubectl get deployment upskill-api -n upskill-dev -w
```

Expected result: Argo CD detects drift and reconciles back to the Git-defined replica count.

Single-node caveats:

- HPA can add pods but cannot add physical capacity.
- `local-path` storage is node-local, not highly available.
- `dev`, `staging`, and `prod` are logical environments only.
- Flannel does not enforce NetworkPolicy by itself; the manifests document intent for a future policy-capable CNI.

Metrics Server is not currently installed, so HPA objects will not report useful metrics until it is added as a platform component.
