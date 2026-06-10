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
#### Example

| Branch             | Purpose                                                                                                                                                    | Protection                                                                                          |
| :----------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| **`main`**         | **Stable, Published, Auditable.** Only clean, reviewed content. Receives merges only from `develop`.                                                       | **Protected:** Require PR + 1 Review + Status Checks. No force push.                                |
| **`develop`**      | **Integration branch.** Collects multiple `topic/<slug>` branches before release. Daily work base.                                                         | **Light protection:** Require Status Checks. Direct pushes allowed for topic merges.                |
| **`topic/<slug>`** | **Short-lived** branches for specific changes: `topic/legal-gdpr-update`, `topic/security-threat-model-v2`, `topic/intent-scope-change`. Branch from `develop`, merge back to `develop` via PR. | **Non-protected.** One topic per branch.                                                            |
