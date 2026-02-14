# Contributing

Thank you for your interest in contributing to this project. This guide will help you get started.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Install pre-commit hooks: `pre-commit install`
4. Initialize TFLint: `tflint --init`
5. Create a feature branch from `main`

## Branching Strategy

All changes must be made in feature branches and merged via pull requests.

**Branch naming conventions:**

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feat/` | New features | `feat/add-rds-module` |
| `fix/` | Bug fixes | `fix/subnet-cidr-calculation` |
| `chore/` | Maintenance | `chore/update-provider-version` |
| `docs/` | Documentation | `docs/add-vpc-examples` |

!!! danger "Never Commit to Main"
    Direct commits to the `main` branch are not allowed. All changes must go through pull requests.

## Development Workflow

### 1. Create a Feature Branch

```bash
git checkout main
git pull origin main
git checkout -b feat/your-feature-name
```

### 2. Make Changes

Follow these guidelines:

- Use [conventional commits](commit-conventions.md) for all commit messages
- Run `pre-commit run --all-files` before pushing
- Keep changes focused and atomic

### 3. Submit a Pull Request

1. Push your branch to the remote repository
2. Create a pull request targeting `main`
3. Fill in the PR template with a description of changes
4. Wait for all CI checks to pass
5. Request review from code owners

### 4. Code Review

- Address all review comments
- Keep the PR updated with the latest `main` branch
- Squash commits if requested

## Quality Standards

### Terraform Code

- All variables must have types and descriptions
- All outputs must have descriptions
- Use snake_case for all naming
- Follow the naming pattern: `{prefix}-{env}-{resource-type}-{name}`
- Include validation blocks for input variables

### Modules

Every new module should include:

- `main.tf` - Resource definitions
- `variables.tf` - Input variables with validation
- `outputs.tf` - Output values with descriptions
- `README.md` - Documentation with usage examples

### Security

- All resources must pass Checkov scans
- Enable encryption by default
- Block public access by default
- No hardcoded credentials or secrets

## Running Checks Locally

```bash
# Format check
terraform fmt -recursive -check

# Validation
terraform validate

# Linting
tflint --config=.tflint.hcl

# Security scan
checkov --config-file .checkov.yml

# All pre-commit hooks
pre-commit run --all-files
```

## Reporting Issues

Use the [issue template](https://github.com/username/github-repo-template/issues/new) to report bugs or request features. Include:

- Clear description of the issue
- Steps to reproduce (for bugs)
- Expected vs actual behavior
- Terraform and provider versions
