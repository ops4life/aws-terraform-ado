# Security Practices

This template implements multiple layers of security scanning and enforcement to ensure infrastructure code meets security best practices.

## Security Scanning Tools

### Checkov

[Checkov](https://www.checkov.io/) performs policy-as-code scanning against Terraform configurations.

**Configuration:** `.checkov.yml`

```yaml
# Key settings
framework:
  - terraform
  - terraform_plan
quiet: true
```

Checkov checks for:

- Encryption at rest and in transit
- Public access configurations
- IAM policy best practices
- Logging and monitoring
- Network security rules

!!! info "Suppressing Findings"
    Add skip-check entries in `.checkov.yml` to suppress specific findings when justified:
    ```yaml
    skip-check:
      - CKV_AWS_XXX  # Reason for suppression
    ```

### TFLint

[TFLint](https://github.com/terraform-linters/tflint) provides linting with AWS-specific rules.

**Configuration:** `.tflint.hcl`

Key rules enforced:

| Rule | Purpose |
|------|---------|
| `terraform_required_version` | Version constraint required |
| `terraform_required_providers` | Provider versions pinned |
| `terraform_typed_variables` | All variables must have types |
| `terraform_documented_variables` | All variables must have descriptions |
| `terraform_documented_outputs` | All outputs must have descriptions |
| `terraform_naming_convention` | Snake case naming enforced |
| `terraform_unused_declarations` | No unused variables/locals |

The AWS plugin enables cloud-specific checks like valid instance types, deprecated features, and resource configuration best practices.

```bash
# Initialize TFLint plugins (first time)
tflint --init

# Run TFLint
tflint --config=.tflint.hcl
```

### Gitleaks

[Gitleaks](https://github.com/gitleaks/gitleaks) detects secrets and credentials in the codebase.

- Integrated as a pre-commit hook
- Runs in CI/CD pipeline
- Blocks commits containing secrets

## Secure-by-Default Patterns

### S3 Bucket Module

The included S3 bucket module (`modules/s3-bucket`) demonstrates security best practices:

- **Encryption**: AES256 server-side encryption enabled by default
- **Public Access**: All public access blocked by default
- **Versioning**: Configurable object versioning
- **Lifecycle Rules**: Automated object management

### Auto-Tagging

All resources are automatically tagged via the AWS provider:

```hcl
default_tags {
  tags = {
    ManagedBy   = "Terraform"
    Environment = var.env
  }
}
```

This ensures:

- Resource ownership tracking
- Cost allocation
- Compliance auditing
- Environment identification

## Pre-commit Security Hooks

The following security hooks run before every commit:

1. **terraform_checkov** - Security policy scanning
2. **gitleaks** - Secret detection
3. **detect-private-key** - Private key detection
4. **check-added-large-files** - Prevents large file commits

## CI/CD Security

### Pull Request Checks

Every PR is scanned by:

- Checkov (standalone workflow)
- GitLeaks (standalone workflow)
- Pre-commit CI (includes all security hooks)

### Deployment Security (Azure Pipelines)

- Azure DevOps approval gates for production environments
- Manual approval requirements via pipeline environments
- Plan review before apply (plan results published in Azure DevOps)
- AWS credentials managed via Azure DevOps service connections
- Encrypted state storage (when using S3 backend)

## Recommendations

!!! warning "Sensitive Data"
    - Never commit `.tfvars` files containing secrets
    - Use AWS Secrets Manager or SSM Parameter Store for sensitive values
    - Configure `*.tfvars` in `.gitignore` (already done in this template)

!!! tip "State File Security"
    - Enable encryption for S3 backend
    - Use DynamoDB for state locking
    - Restrict access to the state bucket with IAM policies
