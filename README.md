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

### Required (in config.env)

```bash
K8_AWS_PROFILE="terraformer"
K8_AWS_ACCOUNT_ID="123456789012"
K8_AWS_REGION="us-east-1"
K8_TF_DIR="/path/to/terraform"
K8_TF_STATE_BUCKET="my-tf-state"
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
