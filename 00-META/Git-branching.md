---
type: instructions
tags:
  - how-to
  - example
---
# Branch strategy: Main + Docs-in-Progress
Do not over-engineer branches. 
A new branch must have only one topic updated.
#### Example

| Branch             | Purpose                                                                                                                                  | Protection                                                                            |
| :----------------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------ |
| **`main`**         | **Stable, Published, Auditable.** Only clean, reviewed content. Represents the "current official state" of the project.                  | **Protected:** Require PR + 1 Review + Status Checks (lint/links). No force push.     |
| **`topic/<slug>`** | **Short-lived** branches for specific changes: `topic/legal-gdpr-update`, `topic/security-threat-model-v2`, `topic/intent-scope-change`. | **Non-protected:** Can be created at any time to any topic, just ONE topic at a time. |
