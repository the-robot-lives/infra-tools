# infra-tools — Infrastructure Bootstrapping

Multi-command infrastructure initialization, one-off deployments, and health checks.

## Installation

```bash
make install    # Installs infra-init, deploy-one-off, open-dashboard, add-import-permissions to ~/.local/bin
```

## Prerequisites

- `terraform` for infrastructure provisioning
- `kubectl` with cluster access
- `aws` CLI with configured profile
- Git (for submodule hydration)

## Configuration

All configuration lives in `k8-util-config.yaml` with credentials in `.k8-secrets.yaml` (see [k8-lib README](../k8-lib/README.md) for setup). Every tool accepts `--config <path>` to specify an alternative config file.

### Relevant Sections

In `k8-util-config.yaml`:

```yaml
aws:
  profile: terraformer
  region: us-east-1
paths:
  terraform_dir: terraform
terraform:
  state_bucket: my-tf-state
```

In `.k8-secrets.yaml` (gitignored):

```yaml
aws:
  account_id: "123456789012"
```

## Tools

| Command | Purpose |
|---------|---------|
| `infra-init terraform` | Terraform provider setup + init |
| `infra-init repos` | Hydrate git submodules |
| `infra-init all` | repos then terraform |
| `infra-init doctor` | Health check all dependencies |
| `infra-init import` | Batch AWS import via Terraformer |
| `deploy-one-off` | One-off deployment |
| `open-dashboard` | Open monitoring dashboard |
| `add-import-permissions` | Set up IAM permissions for Terraformer |

## Usage

```bash
infra-init doctor               # Check all prerequisites
infra-init all                  # Full bootstrap (repos + terraform)
infra-init terraform            # Terraform init only
infra-init import               # Import existing AWS resources
infra-init import --force       # Re-import all groups
```
