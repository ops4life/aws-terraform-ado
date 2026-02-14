# CI/CD Workflows

This project uses GitHub Actions for continuous integration and deployment. All workflows are defined in `.github/workflows/`.

## Pull Request Checks

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

## Deployment Workflow

**File:** `.github/workflows/terraform-aws.yml`

The main deployment workflow supports manual triggering with environment and action selection.

### Inputs

| Input | Options | Description |
|-------|---------|-------------|
| Environment | `dev`, `qa`, `prod` | Target deployment environment |
| Action | `plan`, `apply`, `destroy` | Terraform action to perform |

### Workflow Steps

1. **Checkout** - Clone the repository
2. **Setup Terraform** - Install the specified Terraform version
3. **Format Check** - Verify `terraform fmt` compliance
4. **Init** - Initialize Terraform with backend configuration
5. **Validate** - Run `terraform validate`
6. **Plan** - Generate and upload the execution plan
7. **Apply/Destroy** - Execute the selected action (if not `plan`)

### Environment Protection

!!! tip "Production Safety"
    Configure approval requirements for the `prod` environment in **Settings > Environments** to require manual approval before production deployments.

### Required Secrets

| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | AWS access key |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key |
| `AWS_REGION` | Target AWS region |

## Release Workflow

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

## Other Workflows

| Workflow | File | Purpose |
|----------|------|---------|
| Stale | `stale.yaml` | Marks and closes stale issues/PRs |
| Dependabot Auto-merge | `automerge.yaml` | Auto-merges dependency updates |
| Pre-commit Auto-update | `pre-commit-auto-update.yaml` | Updates pre-commit hook versions |
| Template Sync | `template-repo-sync.yaml` | Syncs changes from the template repository |
| License Update | `update-license.yml` | Updates license year and owner |
| Docs Deploy | `docs-deploy.yaml` | Builds and deploys documentation to GitHub Pages |
| Docs Validate | `docs-validate.yaml` | Validates documentation builds on PRs |
