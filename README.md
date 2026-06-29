# cicd-pipeline-templates

Reusable GitHub Actions workflows. Drop them into your repo and have a production-grade pipeline in an afternoon.

## Workflows

| File | What it does |
|------|-------------|
| `docker-build-scan.yml` | Build image, scan with Trivy, push to ECR |
| `terraform-plan-apply.yml` | `terraform plan` on PR, `apply` on merge with manual approval gate |
| `helm-deploy-eks.yml` | Helm upgrade/install to EKS with kubeconfig via IRSA |
| `sast-semgrep.yml` | Static analysis on every PR, blocks merge on HIGH findings |
| `secret-scan.yml` | TruffleHog scan — fails pipeline on detected credentials |
| `release-please.yml` | Conventional commits → automated changelog + GitHub release |

## Usage

Reference a workflow from your repo:

```yaml
# .github/workflows/deploy.yml
jobs:
  deploy:
    uses: Botoxx/cicd-pipeline-templates/.github/workflows/helm-deploy-eks.yml@main
    with:
      cluster-name: my-eks-cluster
      namespace: production
      chart-path: ./helm/myapp
    secrets: inherit
```

## Design principles

- **No hardcoded credentials** — all AWS auth via OIDC + IRSA
- **Least-privilege** — each workflow documents its required IAM permissions
- **Fail fast** — security scans run before build steps, not after
- **Approval gates** — Terraform apply to prod requires explicit GitHub environment approval
- **Composable** — workflows are independent; use only what you need

## Status

🚧 Work in progress — Docker and Terraform workflows complete, Helm deploy and SAST in progress.

---

*Part of the [Apex Lab](https://cloudgeist.cloud) cloud engineering portfolio — [github.com/Botoxx](https://github.com/Botoxx).*
