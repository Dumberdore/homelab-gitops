# Production Mapping

- Talos Kubernetes -> AWS EKS
- GHCR -> ECR
- Kubernetes PostgreSQL -> RDS PostgreSQL
- Local ingress -> AWS Load Balancer Controller or managed ingress
- Kubernetes Secrets -> AWS Secrets Manager plus External Secrets Operator
- Kubernetes ServiceAccount -> EKS Pod Identity
- local-path storage -> EBS or EFS depending on workload semantics
- Argo CD in homelab -> Argo CD for EKS
- GitHub Actions -> same general CI model

The homelab reproduces the operating model and deployment architecture. It does not pretend that a single-node cluster provides production-grade availability or isolation.
