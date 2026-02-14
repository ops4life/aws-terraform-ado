# AWS Terraform Template

A structured Terraform project template for AWS infrastructure deployments with comprehensive CI/CD integration, security scanning, and best practices enforcement.

---

## Features

- **Production-Ready Structure** - Organized file layout following Terraform best practices
- **Multi-Environment Support** - Pre-configured dev, qa, and prod environments
- **Security Scanning** - Multi-layer scanning with Checkov, TFLint, and Gitleaks
- **CI/CD Pipelines** - GitHub Actions workflows for validation, deployment, and release management
- **Auto-Documentation** - Automated Terraform docs generation with terraform-docs
- **Semantic Releases** - Automated versioning and changelog generation
- **Reusable Modules** - Example S3 bucket module with security best practices
- **Cost Estimation** - Infracost integration for PR cost impact analysis

## Quick Links

| Resource | Description |
|----------|-------------|
| [Quick Start](getting-started/quick-start.md) | Get up and running in minutes |
| [Template Usage](getting-started/usage.md) | How to use this template for your project |
| [CI/CD Workflows](guides/workflows.md) | Understanding the automation pipelines |
| [Security Practices](guides/security.md) | Security scanning and compliance |
| [Repository Structure](reference/repository-structure.md) | File and directory layout |
| [Configuration Reference](reference/configuration.md) | Tool configurations explained |

## Architecture Overview

```text
.
├── environments/          # Environment-specific configurations
│   ├── dev/              # Development settings
│   ├── qa/               # QA/staging settings
│   └── prod/             # Production settings
├── modules/              # Reusable Terraform modules
├── examples/             # Usage examples and patterns
├── .github/workflows/    # CI/CD pipeline definitions
└── docs/                 # Documentation source files
```

## Technology Stack

| Tool | Purpose |
|------|---------|
| Terraform >= 1.0 | Infrastructure as Code |
| AWS Provider ~> 5.0 | Cloud provider |
| TFLint | Linting and best practices |
| Checkov | Security and compliance scanning |
| Gitleaks | Secret detection |
| Semantic Release | Automated versioning |
| Infracost | Cost estimation |
| Pre-commit | Git hook management |

## License

This project is licensed under the [MIT License](about.md).
