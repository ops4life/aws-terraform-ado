# Repository Structure

This page documents the complete file and directory layout of the repository.

## Directory Tree

```text
.
├── .devcontainer/
│   ├── Dockerfile                    # Development environment definition
│   └── devcontainer.json             # VSCode Dev Container configuration
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── issue_template.md         # GitHub issue template
│   ├── workflows/
│   │   ├── automerge.yaml            # Auto-merge dependency updates
│   │   ├── checkov.yaml              # Security scanning
│   │   ├── create-release.yaml       # Release creation
│   │   ├── docs-deploy.yaml          # Documentation deployment
│   │   ├── docs-validate.yaml        # Documentation validation
│   │   ├── gitleaks.yaml             # Secret detection
│   │   ├── infracost.yaml            # Cost estimation
│   │   ├── lint-pr.yaml              # PR title linting
│   │   ├── pre-commit-auto-update.yaml # Hook version updates
│   │   ├── pre-commit-ci.yaml        # Pre-commit CI checks
│   │   ├── release.yaml              # Semantic release
│   │   ├── stale.yaml                # Stale issue management
│   │   ├── template-repo-sync.yaml   # Template synchronization
│   │   ├── terraform-aws.yml         # Terraform deployment
│   │   ├── tf-docs.yaml              # Terraform docs generation
│   │   └── update-license.yml        # License year update
│   ├── dependabot.yml                # Dependabot configuration
│   └── pull_request_template.md      # PR template
├── docs/                             # MkDocs documentation source
├── environments/
│   ├── dev/
│   │   └── dev.tfvars                # Development variables
│   ├── qa/
│   │   └── qa.tfvars                 # QA variables
│   └── prod/
│       └── prod.tfvars               # Production variables
├── examples/
│   ├── complete-infrastructure/      # Full stack example
│   ├── data-sources/                 # Data source patterns
│   └── module-composition/           # Module reuse patterns
├── modules/
│   └── s3-bucket/                    # S3 bucket module
│       ├── main.tf
│       ├── outputs.tf
│       ├── variables.tf
│       └── README.md
├── .checkov.yml                      # Checkov configuration
├── .editorconfig                     # Editor settings
├── .gitignore                        # Git ignore rules
├── .pre-commit-config.yaml           # Pre-commit hooks
├── .releaserc.json                   # Semantic release config
├── .terraform-version                # Terraform version (tfenv/asdf)
├── .tflint.hcl                       # TFLint configuration
├── BEST-PRACTICES.md                 # Best practices summary
├── CHANGELOG.md                      # Release changelog
├── CLAUDE.md                         # AI assistant guidance
├── CODEOWNERS                        # Code ownership
├── LICENSE                           # MIT License
├── README.md                         # Project README
├── TF-DOCS.md                        # Auto-generated Terraform docs
├── backend.tf                        # Backend configuration (deprecated)
├── locals.tf                         # Local values and naming
├── main.tf                           # Main Terraform configuration
├── mkdocs.yml                        # MkDocs configuration
├── outputs.tf                        # Output values
├── providers.tf                      # Provider configuration
├── requirements.txt                  # Python dependencies for MkDocs
├── variables.tf                      # Input variables
└── versions.tf                       # Version constraints and backend
```

## Key Files

### Terraform Configuration

| File | Purpose |
|------|---------|
| `versions.tf` | Terraform and provider version constraints, backend config |
| `providers.tf` | AWS provider configuration with default tags |
| `variables.tf` | Input variable definitions |
| `locals.tf` | Computed values, naming patterns, environment detection |
| `main.tf` | Resource orchestration and module calls |
| `outputs.tf` | Output value definitions |

### Configuration Files

| File | Purpose |
|------|---------|
| `.tflint.hcl` | TFLint rules and AWS plugin configuration |
| `.checkov.yml` | Checkov security scanning settings |
| `.pre-commit-config.yaml` | Git pre-commit hook definitions |
| `.releaserc.json` | Semantic Release configuration |
| `.editorconfig` | Editor formatting standards |
| `mkdocs.yml` | Documentation site configuration |

### Documentation

| File | Purpose |
|------|---------|
| `README.md` | Primary project documentation |
| `BEST-PRACTICES.md` | Implemented best practices summary |
| `CLAUDE.md` | AI assistant context and guidance |
| `TF-DOCS.md` | Auto-generated Terraform documentation |
| `CHANGELOG.md` | Release history |
| `docs/` | MkDocs documentation source files |
