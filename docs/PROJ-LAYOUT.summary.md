# Project Layout — Summary

```
infra-utils/
├── bin/                          # CLI tools → ~/.local/bin
│   ├── add-import-permissions    # IAM policy sync for Terraformer imports
│   ├── deploy-one-off            # Template ordered helm deploy
│   ├── deploy-service            # Build → push → values bump → helm upgrade
│   ├── infra-config              # Manage .infra-config.yaml resources
│   ├── infra-init                # Setup/health: terraform, repos, import, doctor
│   └── open-dashboard            # Port-forward monitoring dashboards
├── completions/                  # bash + zsh completions (deploy-service, infra-config)
├── docs/
│   ├── PROJ-ARCH.md
│   ├── PROJ-ARCH.summary.md
│   ├── PROJ-FAQ.md               # FAQ index (→ faq/)
│   ├── PROJ-FAQ.summary.md
│   ├── PROJ-HOWTO.md             # How-to index (→ howto/)
│   ├── PROJ-HOWTO.summary.md
│   ├── PROJ-LAYOUT.md
│   ├── PROJ-LAYOUT.summary.md
│   ├── arch/scripts.md           # Per-script details
│   ├── faq/                      # Expanded FAQ answers
│   └── howto/                    # Expanded task guides
├── .gitignore
├── CHANGELOG.md                  # Milestone history (m1–m4)
├── Makefile                      # make install (scripts + completions)
├── README.md                     # Start here
└── merge-notes.md                # Historical merge notes
```
