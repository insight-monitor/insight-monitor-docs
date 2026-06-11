---
type: reference
domain: git
priority: low
status: accepted
audience: developer
slug: git-branch-prefixes
aliases:
  - branch prefixes
---

# Branch Prefixes

This is a **documentation-only** repository. No code — only content files and minimal CI/CD config (`.github/`).

Valid branch prefixes:

| Prefix | Purpose | Example |
| :----- | :------ | :------ |
| `topic/<slug>` | General docs changes | `topic/narrative-vision-update` |
| `feat/<slug>` | New sections, policies, diagrams | `feat/gdpr-retention-policy` |
| `fix/<slug>` | Corrections, broken links, typos | `fix/security-model-diagram` |
| `refactor/<slug>` | Restructuring without changing meaning | `refactor/vault-docs-merge` |
| `chore/<slug>` | CI/CD config, Obsidian plugins, automation | `chore/vale-legal-caps` |
| `security/<slug>` | Threat models, access control **(mandatory prefix)** | `security/authz-decision-tree` |
| `legal/<slug>` | Contracts, compliance, terms **(mandatory prefix)** | `legal/dpa-template-v2` |

> `security/` and `legal/` changes **must** use their own prefix. Never `topic/security-*` or `topic/legal-*`. These domains require explicit traceability.
