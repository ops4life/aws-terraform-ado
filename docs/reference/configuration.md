# Configuration Reference

This page documents the configuration files and their settings used in this template.

## Terraform Configuration

### Version Constraints (`versions.tf`)

```hcl
terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

- **Terraform version**: >= 1.0 (exact version managed by `.terraform-version`)
- **AWS provider**: ~> 5.0 (minor version updates allowed)

### Backend Configuration

=== "Local (Default)"

    ```hcl
    # No backend block needed - defaults to local state
    ```

=== "S3 (Production)"

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

### Input Variables (`variables.tf`)

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `env` | `string` | (required) | Environment name (dev, qa, prod) |
| `prefix` | `string` | (required) | Resource name prefix |
| `region` | `string` | `us-east-1` | AWS region for deployment |

## TFLint Configuration (`.tflint.hcl`)

### AWS Plugin

```hcl
plugin "aws" {
  enabled = true
  version = "0.38.0"
  source  = "github.com/terraform-linters/tflint-ruleset-aws"
}
```

### Enabled Rules

| Rule | Description |
|------|-------------|
| `terraform_required_version` | Require version constraints |
| `terraform_required_providers` | Require provider version pinning |
| `terraform_typed_variables` | Require variable types |
| `terraform_documented_variables` | Require variable descriptions |
| `terraform_documented_outputs` | Require output descriptions |
| `terraform_naming_convention` | Enforce snake_case naming |
| `terraform_deprecated_interpolation` | Flag deprecated syntax |
| `terraform_unused_declarations` | Detect unused declarations |
| `terraform_module_pinned_source` | Require pinned module sources |
| `terraform_standard_module_structure` | Enforce module file structure |

## Checkov Configuration (`.checkov.yml`)

```yaml
framework:
  - terraform
  - terraform_plan
quiet: true
```

Add skip-check entries to suppress specific findings:

```yaml
skip-check:
  - CKV_AWS_XXX  # Document the reason for suppression
```

## Pre-commit Hooks (`.pre-commit-config.yaml`)

### File Quality Hooks

| Hook | Action |
|------|--------|
| `trailing-whitespace` | Removes trailing whitespace |
| `end-of-file-fixer` | Ensures files end with newline |
| `check-yaml` | Validates YAML syntax |

### Terraform Hooks

| Hook | Action |
|------|--------|
| `terraform_fmt` | Auto-formats `.tf` files |
| `terraform_validate` | Validates configuration syntax |
| `terraform_tflint` | Runs TFLint with AWS rules |
| `terraform_checkov` | Security and compliance scanning |

### Security Hooks

| Hook | Action |
|------|--------|
| `gitleaks` | Detects secrets in commits |

## Semantic Release (`.releaserc.json`)

Configured for:

- Conventional commit analysis
- Automatic version bumping
- CHANGELOG.md generation
- GitHub release creation
- Branches: `main`, `master`

## Editor Configuration (`.editorconfig`)

Ensures consistent formatting across editors:

- UTF-8 encoding
- LF line endings
- Spaces for indentation
- Trailing whitespace trimming

## Environment Variables

### Environment-Specific Settings

| Setting | Dev | QA | Prod |
|---------|-----|----|------|
| Instance size | t3.micro | t3.small | t3.medium+ |
| Instance count | 1 | 2 | 3+ (multi-AZ) |
| Backups | Minimal | Standard | Full retention |
| Monitoring | Basic | Standard | Enhanced |
| Deletion protection | No | No | Yes |
