# Threat Model — Summary

Single-operator developer toolchain: six bash CLIs running on a workstation with the
operator's credentials (AWS, registry, kubeconfig). No server, no listener, no
multi-tenant surface. Crown jewels: the transitively-used credentials and the deploy
authority (whoever controls the scripts, their YAML config inputs, `K8_LIB_DIR`, or
`PATH` controls what ships to the cluster). Workstation is the trusted computing base.

Register: 7 findings — 2 mitigated (loopback-only port-forwards; no secrets in local
artifacts), 3 partial (registry/build supply chain T-002, IAM policy replacement
T-003, config-file tamper surface T-004 — mitigations live outside this repo),
2 open/accepted (K8_LIB_DIR code-injection trust T-001; unauthenticated local
promoted-tag ledger T-005). Every script: `set -euo pipefail`, no `eval`, no direct
network calls; effects delegated to audited sibling CLIs.

→ Full register, attack-surface diagram, and residual-risk statement: [THREAT-MODEL.md](THREAT-MODEL.md)
