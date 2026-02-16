# Quick Start

Get up and running with this Terraform template in just a few steps.

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/install) >= 1.0 (recommended: 1.10.5)
- [Pre-commit](https://pre-commit.com/#install)
- [TFLint](https://github.com/terraform-linters/tflint)
- [Checkov](https://www.checkov.io/2.Basics/Installing%20Checkov.html)
- AWS CLI configured with appropriate credentials
- Docker (optional, for Dev Container)

## Step 1: Create Your Repository

Use this template to create a new repository:

1. Click **"Use this template"** on the GitHub repository page
2. Choose a name for your new repository
3. Clone your new repository locally

```bash
git clone https://github.com/your-org/your-repo.git
cd your-repo
```

## Step 2: Install Pre-commit Hooks

```bash
pre-commit install
```

This ensures all quality checks run automatically before each commit.

## Step 3: Initialize Terraform

```bash
# Initialize TFLint plugins (first time only)
tflint --init

# Initialize Terraform
terraform init
```

## Step 4: Configure Your Environment

Edit the environment-specific variable files:

```bash
# Development
environments/dev/dev.tfvars

# QA/Staging
environments/qa/qa.tfvars

# Production
environments/prod/prod.tfvars
```

## Step 5: Plan and Apply

```bash
# Plan for development environment
terraform plan -var-file=environments/dev/dev.tfvars

# Apply when ready
terraform apply -var-file=environments/dev/dev.tfvars
```

## Step 6: Set Up CI/CD

### Azure Pipelines (Terraform Deployment)

Configure the Azure DevOps pipeline for Terraform operations:

1. Install the [Terraform extension](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks) from the Azure DevOps Marketplace
2. Create an AWS service connection named `AWSServiceConnection` in **Project Settings > Service connections**
3. Create a new pipeline pointing to `.azure-pipelines/azure-pipelines.yaml`

### GitHub Actions (Validation & Release)

Configure the following GitHub secrets for PR checks and releases:

| Secret | Description |
|--------|-------------|
| `INFRACOST_API_KEY` | (Optional) For cost estimation on PRs |

!!! tip "Environment Approvals"
    Configure [approval gates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals) in Azure DevOps Environments for production deployment safety.

## Next Steps

- Read the [Template Usage Guide](usage.md) for detailed customization
- Review [CI/CD Workflows](../guides/workflows.md) to understand the automation
- Check [Security Practices](../guides/security.md) for compliance requirements
