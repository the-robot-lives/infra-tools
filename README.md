# infra-utils

**Repo:** https://github.com/the-robot-lives/infra-tools

Infrastructure bootstrap helpers plus the `deploy-service` release pipeline: build an image, push it, bump Helm values, deploy the release.

## What

Commands installed to `~/.local/bin`:

| Command | Purpose |
|---------|---------|
| `infra-init` | Bootstrap repos, Terraform, Terraformer imports, dependency checks |
| `deploy-service` | Build → push (`docker-build --push`) → `values.yaml` tag bump → `helm-upgrade --include <release>` |
| `deploy-one-off` | One-off Kubernetes deployment helper |
| `open-dashboard` | Open configured dashboards |
| `add-import-permissions` | IAM setup for Terraformer imports |

## Why

Getting a service from source to a running release touches four tools and three config files. `deploy-service` collapses that into one command with a declarative image→values wiring, so per-service releases are repeatable and `--dry-run`-able. `infra-init` handles the once-per-cluster bootstrap chores.

## Getting Started

Prerequisites: `terraform`, `kubectl`, `aws`, `git`, `docker-build`, `helm-upgrade`, `yq` (the latter two come from the sibling helm-utils/docker-utils packages).

```bash
make install    # → ~/.local/bin (make install-completions for completions)
```

```bash
infra-init doctor | all | terraform | repos | import | import --force

deploy-service easy-peasy
deploy-service easy-peasy --dry-run | --skip-build | --skip-deploy | --no-cache
deploy-service easy-peasy --tag v2.1.0 | --stage | --prod
deploy-service codefre.sh/backend codefre.sh/frontend   # composite projects
```

## How It Works

- **Config**: `infra-config.yaml` (shared paths/scalars, e.g. `paths.projects_dir`) + per-project `project.yaml` files declaring Helm releases and image→values wiring. AWS account/profile/region come from `.envrc.k8.dc` (`K8_AWS_ACCOUNT_ID`, `K8_AWS_PROFILE`, `K8_AWS_REGION`). Note `deploy-service` differs from `docker-build`, which reads Docker targets from the merged `infra-config.yaml` `project:` section.
- **project.yaml shapes**: flat (`helm.release`, `docker.images[].helm.{chart_path, values_path, format}`) or composite (`type: composite`, `projects[].domain`, image keys `<domain>/<service>`). Multiple images on one chart update all configured values, then run a single Helm upgrade.
- **Reverse-map**: the current image's `helm.chart_path` must match `PROJECT_DIR/helm.path`, else "Could not reverse-map chart_path".
- **Field defaults**: `helm.tier` → 5, `helm.timeout` → 10m, `format` → `tag` (or `image` for full image string).

Common failure modes ("No project.yaml declares helm for image", "values.yaml not found", reverse-map mismatch) and their fixes are documented in the FAQ: `docs/PROJ-FAQ.md`. Full field reference: `docs/PROJ-SCHEMA.md`.

## Repo Layout

- `bin/` — command entrypoints
- `completions/` — shell completions
- `docs/` — PROJ-ARCH / PROJ-SCHEMA / PROJ-FAQ / howto / arch docs
