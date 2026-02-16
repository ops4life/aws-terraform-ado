# CI/CD Workflows

This project uses **Azure Pipelines** for Terraform deployment and **GitHub Actions** for pull request validation and release management.

## Terraform Deployment (Azure Pipelines)

**File:** `.azure-pipelines/azure-pipelines.yaml`

The main deployment pipeline runs on Azure DevOps and handles Terraform operations against AWS.

### Triggers

- **Branch trigger**: Runs on pushes to `main` when Terraform files (`*.tf`, `*.tfvars`, `.terraform.lock.hcl`) or pipeline files change
- **PR trigger**: Runs on pull requests targeting any branch

### Pipeline Steps

1. **Install Terraform** - Installs the latest Terraform version via `TerraformInstaller@0`
2. **Init** - Initializes Terraform with the AWS service connection
3. **Validate** - Runs `terraform validate` to check configuration syntax
4. **Plan** - Generates an execution plan (`-out=tfplan`) and publishes the plan results

### Azure DevOps Configuration

#### Service Connection

The pipeline requires an AWS service connection named `AWSServiceConnection` configured in Azure DevOps:

1. Go to **Project Settings > Service connections**
2. Create a new **AWS** service connection
3. Name it `AWSServiceConnection`
4. Provide your AWS access key and secret key

#### Pipeline Variables

| Setting | Value | Description |
|---------|-------|-------------|
| VM Image | `ubuntu-latest` | Agent pool image |
| AWS Region | `ap-southeast-1` | Target AWS region |
| Service Connection | `AWSServiceConnection` | AWS credentials |

#### Required Extensions

Install the following extensions from the Azure DevOps Marketplace:

| Extension | Purpose |
|-----------|---------|
| [Terraform](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks) | `TerraformInstaller@0` and `TerraformCLI@0` tasks |

### Environment-Specific Deployments

To deploy to specific environments, modify the plan step to include the appropriate tfvars file:

```yaml
- task: TerraformCLI@0
  displayName: 'terraform plan - dev'
  inputs:
    providerServiceAws: 'AWSServiceConnection'
    providerAwsRegion: 'ap-southeast-1'
    workingDirectory: '$(System.DefaultWorkingDirectory)'
    command: 'plan'
    commandOptions: '-var-file=environments/dev/dev.tfvars -out=tfplan -input=false'
    publishPlanResults: 'tfplan'
```

!!! tip "Environment Approvals"
    Configure [approval gates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals) in Azure DevOps Environments for production deployments.

## Pull Request Checks (GitHub Actions)

The following checks run automatically on every pull request:

### Pre-commit CI

**File:** `.github/workflows/pre-commit-ci.yaml`

Runs all pre-commit hooks including:

- Terraform formatting (`terraform fmt`)
- Configuration validation (`terraform validate`)
- Linting (`tflint`)
- Security scanning (`checkov`)
- Secret detection (`gitleaks`)
- File quality checks (whitespace, YAML, merge conflicts)

### Terraform Docs

**File:** `.github/workflows/tf-docs.yaml`

Automatically generates Terraform documentation in `TF-DOCS.md` using terraform-docs.

### Infracost

**File:** `.github/workflows/infracost.yaml`

Adds cost estimation comments to pull requests, showing the cost impact of infrastructure changes.

!!! note "API Key Required"
    Requires the `INFRACOST_API_KEY` secret to be configured.

### Checkov Security Scan

**File:** `.github/workflows/checkov.yaml`

Standalone security and compliance scanning for Terraform configurations.

### GitLeaks

**File:** `.github/workflows/gitleaks.yaml`

Scans for secrets and sensitive data in the codebase.

### PR Title Lint

**File:** `.github/workflows/lint-pr.yaml`

Validates that PR titles follow the conventional commit format.

## Release Workflow (GitHub Actions)

**File:** `.github/workflows/release.yaml`

Automated release management using Semantic Release:

1. Analyzes commit messages since the last release
2. Determines the version bump (major/minor/patch)
3. Generates changelog entries
4. Creates a GitHub release with release notes
5. Updates `CHANGELOG.md`

### Version Bumps

| Commit Type | Version Bump | Example |
|-------------|-------------|---------|
| `fix:` | Patch (1.0.x) | Bug fixes |
| `feat:` | Minor (1.x.0) | New features |
| `BREAKING CHANGE:` | Major (x.0.0) | Breaking changes |

## Other Workflows (GitHub Actions)

| Workflow | File | Purpose |
|----------|------|---------|
| Stale | `stale.yaml` | Marks and closes stale issues/PRs |
| Dependabot Auto-merge | `automerge.yaml` | Auto-merges dependency updates |
| Pre-commit Auto-update | `pre-commit-auto-update.yaml` | Updates pre-commit hook versions |
| Template Sync | `template-repo-sync.yaml` | Syncs changes from the template repository |
| License Update | `update-license.yml` | Updates license year and owner |
| Docs Deploy | `docs-deploy.yaml` | Builds and deploys documentation to GitHub Pages |
| Docs Validate | `docs-validate.yaml` | Validates documentation builds on PRs |
