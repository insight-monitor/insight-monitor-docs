---
type: policy
domain: documentation-architecture
priority: critical
status: accepted
audience: all
version: 1.0.0
---

# Atomization

Maintain a single concern per file.

## Examples

| Instead of one file covering...                  | Split into                                         |
|--------------------------------------------------|-----------------------------------------------------|
| Data collection, storage, and deletion           | `Data-Collection.md`, `Data-Storage.md`, `Data-Deletion.md` |
| GDPR compliance + database architecture          | One file for GDPR rules, another for the DB schema   |
| Product features + technical implementation      | Separate `Scope.md` from `Architecture.md`            |

## Exceptions

- **MOC/index files** — By design they reference multiple concerns.
- **Policy bundles** — A cohesive set of rules that are meaningless in isolation.
