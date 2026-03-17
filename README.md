🛡️ Security Engineering Platform

This repository provides reusable security workflows, policies, and scanning frameworks for application and platform engineering teams.

It acts as a centralized DevSecOps control plane, enabling consistent security enforcement across repositories.

🎯 Purpose

The goal of this repository is to:

Standardize security controls across projects

Provide reusable GitHub Actions workflows

Enable shift-left security in CI/CD pipelines

Enforce supply chain security and runtime validation

🧩 What This Repo Provides
🔐 Supply Chain Security

Cosign signing & verification

Identity-based verification (GitHub OIDC)

Trusted image enforcement

🧪 Static & Dependency Scanning

SAST (Semgrep)

Secrets scanning (Gitleaks)

Dependency & vulnerability checks

🐳 Container Security

Trivy image scanning

SBOM generation (CycloneDX)

SBOM vulnerability scanning

📦 Infrastructure & Kubernetes Security

IaC scanning (Terraform, Kubernetes manifests)

Helm chart lint + manifest scan

Deep RBAC risk detection

☸️ Platform Security

Pod Security Admission (PSA) validation

CIS Kubernetes Benchmark scanning

Policy enforcement workflows (Kyverno-ready)

📁 Repository Structure
.github/workflows/
  ├── secrets-scan.yaml
  ├── product-static-scans.yaml
  ├── gitleaks.yaml
  ├── image-post-build.yaml
  ├── platform-deploy-gates.yaml
  ├── helm-chart-scan.yaml
  ├── cis-benchmark.yaml
  ├── psa-alignment-check.yaml

security/
  ├── kyverno/
  ├── falco/
  └── policies/
🚀 How to Use

Other repositories can consume these workflows using workflow_call.

Example: Image Verification
jobs:
  image-signing-verification:
    uses: security-org-230982/security-engineering/.github/workflows/image-signing-verification.yaml@main
    with:
      image-ref: ghcr.io/org/app:tag
      certificate-identity: "https://github.com/org/repo/.github/workflows/build.yaml@refs/heads/main"
Example: Helm Chart Scan
jobs:
  helm-chart-scan:
    uses: security-org-230982/security-engineering/.github/workflows/helm-chart-scan.yaml@main
    with:
      chart-path: helm/app
      values-file: helm/app/values.yaml
Example: CIS Benchmark
jobs:
  cis-benchmark:
    uses: security-org-230982/security-engineering/.github/workflows/cis-benchmark.yaml@main
    with:
      eks-cluster-name: ${{ vars.EKS_CLUSTER_NAME }}
      aws-region: us-east-1
    secrets:
      aws-role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
Example: PSA Alignment Check
jobs:
  psa-check:
    uses: security-org-230982/security-engineering/.github/workflows/psa-alignment-check.yaml@main
    with:
      eks-cluster-name: ${{ vars.EKS_CLUSTER_NAME }}
      namespace: simple-game
      expected-version: v1.30
    secrets:
      aws-role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
🔑 Key Concepts
🔐 OIDC-Based Authentication

All workflows use GitHub OIDC → AWS IAM role assumption, eliminating static credentials.

🔏 Image Trust Model

Images must be:

Signed using Cosign

Verified using:

OIDC issuer

Workflow identity

Only trusted workflows can produce deployable images
📊 Security Summaries

Each workflow provides:

GitHub Step Summary

Detailed findings (CVE, RBAC, CIS)

SARIF uploads to GitHub Security tab

🧱 Design Principles

Reusable → consumed across multiple repos

Composable → combine workflows as needed

Fail-fast → block insecure changes early

Observable → provide summaries and artifacts

Policy-driven → enforce security as code

🛠️ Workflows Overview
Workflow	Purpose
secrets-scan.yaml	Detect leaked secrets
product-static-scans.yaml	SAST + dependency scanning
image-post-build.yaml	Image scan + SBOM + signing
platform-deploy-gates.yaml	Image trust validation
helm-chart-scan.yaml	Helm + manifest + RBAC scan
cis-benchmark.yaml	Kubernetes CIS compliance
psa-alignment-check.yaml	Pod Security validation
⚠️ Important Notes

This repo does not deploy applications

It provides security controls only

Platform repos must:

call these workflows

pass correct inputs

configure required secrets

🔮 Future Enhancements

Kyverno policy enforcement (admission control)

Falco runtime threat detection

Supply chain policy gates (SLSA)

Central security dashboard aggregation

🤝 Contributing

Contributions should:

follow workflow modularity

include summary outputs

avoid breaking existing consumers

maintain backward compatibility

🏁 Summary

This repository enables:

Secure → Consistent → Scalable DevSecOps workflows

across your organization.
