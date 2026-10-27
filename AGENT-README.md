# AGENT-README

> Orientation for AI coding agents working in this repo. Human docs: README.md.

## Type

scripts+cli-tool

## Purpose

Infra bootstrap helpers plus the `deploy-service` release pipeline — build an image, push it, bump Helm values, deploy the release. Also `infra-init`, `deploy-one-off`, `open-dashboard`, `add-import-permissions`.

## Stack

Bash CLIs (`bin/`); prerequisites terraform, kubectl, aws, git, plus sibling `docker-build` and `helm-upgrade` binaries and `yq`; config from `infra-config.yaml` + per-project `project.yaml`.

## Dev mode

```bash
make install    # -> ~/.local/bin (make install-completions for shell completions)
infra-init doctor
deploy-service easy-peasy --dry-run
deploy-service easy-peasy --skip-build
```

## Ports / URLs

- none (library/CLI)

## Tests

```bash
make test   # no-op placeholder
```

## Gotchas

- Per-project `project.yaml` declares Helm releases and image→values wiring — flat (`helm.release`, `docker.images[].helm.*`) or composite (`type: composite`, keys `<domain>/<service>`); multiple images on one chart update all values, then run a single Helm upgrade.
- Reverse-map rule: the current image's `helm.chart_path` must match `PROJECT_DIR/helm.path`, else "Could not reverse-map chart_path".
- AWS account/profile/region come from `.envrc.k8.dc` (`K8_AWS_ACCOUNT_ID`, `K8_AWS_PROFILE`, `K8_AWS_REGION`).
- Field defaults: `helm.tier` → 5, `helm.timeout` → 10m, `format` → `tag`.
- Common failure modes and fixes live in `docs/PROJ-FAQ.md`; field reference in `docs/PROJ-SCHEMA.md`.
