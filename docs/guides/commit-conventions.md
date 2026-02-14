# Commit Conventions

This project follows the [Conventional Commits](https://www.conventionalcommits.org/) specification, enforced by Semantic Release for automated versioning.

## Format

```text
<type>(<scope>): <subject>

<body>
```

### Type (Required)

| Type | Description | Version Bump |
|------|-------------|-------------|
| `feat` | A new feature | Minor |
| `fix` | A bug fix | Patch |
| `perf` | Performance improvement | Patch |
| `refactor` | Code restructuring | Patch |
| `docs` | Documentation changes | None |
| `chore` | Maintenance tasks | None |
| `test` | Test additions/updates | None |
| `style` | Code style changes | None |

### Scope (Optional)

The part of the codebase affected by the change:

```text
feat(s3): add encryption configuration
fix(vpc): correct subnet CIDR calculation
docs(readme): update getting started guide
```

### Subject (Required)

A brief description using imperative mood:

- Use present tense: "add" not "added"
- Don't capitalize the first letter
- No period at the end

## Examples

### Feature

```text
feat(s3): add encryption configuration for buckets

Configure AES256 encryption for all S3 buckets to meet
security compliance requirements.
```

### Bug Fix

```text
fix(iam): correct policy attachment for read-only role

The read-only role was incorrectly attached to the admin
policy, granting excessive permissions.
```

### Breaking Change

```text
feat(vpc): redesign subnet allocation strategy

Migrate from /24 to /20 subnets to support larger workloads.

BREAKING CHANGE: Existing VPC configurations will need to be
updated to use the new subnet CIDR ranges.
```

### Documentation

```text
docs(modules): add usage examples for S3 module
```

### Maintenance

```text
chore(deps): update AWS provider to 5.30.0
```

## PR Titles

Pull request titles must also follow this format. The `lint-pr.yaml` workflow validates PR titles against the conventional commit specification.

## Tips

!!! tip "Write Good Commits"
    - Each commit should represent a single logical change
    - The subject line should explain **what** changed
    - The body should explain **why** the change was made
    - Reference related issues when applicable
