# Template Usage Guide

This guide explains how to customize the template for your specific AWS infrastructure project.

## Using the Template

### 1. Repository Setup

After creating your repository from the template:

1. Update `CODEOWNERS` with your team members
2. Configure GitHub repository settings (branch protection, environments)
3. Set up required secrets for CI/CD

### 2. Backend Configuration

By default, the template uses a local backend. For team collaboration, configure an S3 backend in `versions.tf`:

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"
    key            = "your-project/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

!!! warning "State Bucket"
    Create the S3 bucket and DynamoDB table before configuring the backend. These resources should be managed separately.

### 3. Provider Configuration

The AWS provider is configured in `providers.tf` with auto-tagging:

```hcl
provider "aws" {
  region = var.region

  default_tags {
    tags = {
      ManagedBy   = "Terraform"
      Environment = var.env
    }
  }
}
```

Additional tags can be merged via `locals.tf`.

### 4. Variables and Locals

- **`variables.tf`** - Define input variables with descriptions and validation
- **`locals.tf`** - Computed values, naming patterns, and environment detection

The naming convention follows: `{prefix}-{env}-{resource-type}-{name}`

### 5. Creating Modules

Use the existing `modules/s3-bucket` as a reference. Each module should include:

```text
modules/your-module/
├── main.tf           # Resources
├── variables.tf      # Inputs with validation
├── outputs.tf        # Outputs with descriptions
└── README.md         # Documentation
```

!!! tip "Module Best Practices"
    - Use typed variables with descriptions
    - Add validation blocks for inputs
    - Enable security features by default
    - Include comprehensive outputs

### 6. Environment Configuration

Each environment has its own tfvars file with appropriate settings:

=== "Development"

    ```hcl
    # environments/dev/dev.tfvars
    env    = "dev"
    prefix = "myproject"
    region = "us-east-1"
    # Cost-optimized, single instance, minimal features
    ```

=== "QA"

    ```hcl
    # environments/qa/qa.tfvars
    env    = "qa"
    prefix = "myproject"
    region = "us-east-1"
    # Production-like, reduced capacity, full testing
    ```

=== "Production"

    ```hcl
    # environments/prod/prod.tfvars
    env    = "prod"
    prefix = "myproject"
    region = "us-east-1"
    # High availability, multi-AZ, full monitoring
    ```

## Development Workflow

### Local Development

```bash
# Format Terraform files
terraform fmt -recursive

# Validate configuration
terraform validate

# Run linter
tflint --config=.tflint.hcl

# Run security scan
checkov --config-file .checkov.yml

# Run all pre-commit hooks
pre-commit run --all-files
```

### Dev Container

A pre-configured development container is available:

1. Install the **Dev Containers** extension in VSCode
2. Open the repository in VSCode
3. Reopen in the dev container when prompted

The container includes all required tools and extensions.

## Examples

The `examples/` directory contains production-ready patterns:

| Example | Description |
|---------|-------------|
| `data-sources/` | AWS data source usage (VPC, AMI, account info) |
| `module-composition/` | Module reuse and composition patterns |
| `complete-infrastructure/` | Full stack integration template |

Each example includes a README with usage instructions.
