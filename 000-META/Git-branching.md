---
type: reference
domain: git
priority: low
status: accepted
audience: developer
---
# Branch strategy: Main → Develop → Topic (GitFlow)
Do not over-engineer branches. 
A new branch must have only one topic updated.
See [[Git-branch-prefixes]] for all valid branch prefixes.

| Branch             | Purpose                                                                                                                                                    | Protection                                                                                          |
| :----------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| **`main`**         | **Stable, Published, Auditable.** Only clean, reviewed content. Receives merges only from `develop`.                                                       | **Protected:** Require PR + 1 Review + Status Checks. No force push.                                |
| **`develop`**      | **Integration branch.** Collects multiple branches before release. Daily work base.                                                                        | **Light protection:** Require Status Checks.                                                        |
| **`<prefix>/<slug>`** | **Short-lived** branches. Use a valid prefix from [[Git-branch-prefixes]]. Branch from `develop`, merge back to `develop` via PR.                      | **Non-protected.** One topic per branch.                                                            |

---

## Merge Process

All merges to `develop` must follow these rules:

| Rule | Details |
| :--- | :------ |
| **PR required** | All topic branches must be merged via PR (squash & merge). No direct `git merge` to `develop`. |
| **Rebase before merge** | Branch must be up-to-date with `develop` before merging. |
| **Merge commit message** | PR title must follow [[Git-commit|conventional commit format]] (e.g., `refactor(docs): extract vault-structure`). The squash commit becomes the merge commit. |
| **Self-approval** | Allowed when the PR author is the reviewer. |

### Trivial-fix exception

Changes **≤5 lines or ≤50 characters** that do not affect file meaning, key metadata (`slug`, `priority`, `domain`, `tags`), relationships, or concepts may skip the PR process and be pushed directly to `develop`.

Examples: typo fixes, date corrections, minor formatting, grammar fixes.
Non-examples: metadata changes, structural changes, additions of content.

### Hotfix

Hotfixes branch from `main` (not `develop`) and follow the [[Workflow-guide#hotfix-emergency|hotfix process]].
