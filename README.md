# infra-tools — Infrastructure Bootstrapping

Multi-command infrastructure initialization, one-off deployments, and health checks.

## Installation

```bash
make install    # Installs infra-init, deploy-one-off, deploy-service, open-dashboard, add-import-permissions to ~/.local/bin
```

## Prerequisites

- `terraform` for infrastructure provisioning
- `kubectl` with cluster access
- `aws` CLI with configured profile
- Git (for submodule hydration)

## Configuration

All configuration lives in `infra-config.yaml` with credentials in `.envrc.k8.dc` (see [k8-lib README](../k8-lib/README.md) for setup). Every tool accepts `--config <path>` to specify an alternative config file.

### Relevant Sections

In `infra-config.yaml`:

```yaml
aws:
  profile: terraformer
  region: us-east-1
paths:
  terraform_dir: terraform
terraform:
  state_bucket: my-tf-state
```

In `.envrc.k8.dc` secrets layer:

```bash
# In .envrc.k8.dc
export K8_AWS_ACCOUNT_ID="123456789012"
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
| `deploy-service` | Build → push → chart bump → helm upgrade pipeline |
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

### deploy-service

Headless build → push → values.yaml bump → helm upgrade pipeline. Reads deploy targets from `infra-config.yaml` project `helm:` blocks — no CLI mapping needed.

```bash
deploy-service my-image                        # Build, push, bump, deploy
deploy-service my-image --dry-run              # Preview planned actions
deploy-service my-image --skip-deploy          # Build + bump only, no helm upgrade
deploy-service my-image --tag v2.1.0           # Override the tag
deploy-service my-image --no-cache             # Force fresh build
deploy-service backend frontend                # Batch: build both, single helm upgrade
deploy-service codefre.sh/backend              # Composite project target
```

Requires `docker-build`, `helm-upgrade`, `yq` on PATH. Uses `paths.projects_dir` from `infra-config.yaml` to discover project configurations.
