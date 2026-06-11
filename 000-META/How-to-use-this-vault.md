---
type: process
domain: documentation-architecture
priority: critical
status: accepted
audience: all
version: 2.0.0
---
# How to use this vault

This file discusses how to create, modify and remove documentation files. Follow [[Docs-truth]]

## What to use

If the git repository was cloned:
- Install Obsidian
- Use community plugins.
  - Continuous reading
- Review [[Metadata]] for the frontmatter schema

## How to read

This documentation is highly modular. To navigate:
- **Folder naming** — Folders are prefixed with 3-digit semantic numbers for scope ordering. See [[Vault-structure]] for the full folder map.
- **MOCs (Map of Content)** — Each folder has an `MOC-<Folder>.md` that links all files in that domain.
- **Priority** — Files with `priority: critical` and `priority: high` are the primary reading path. Files with `priority: low` or `optional` should only be read when relevant.

## How to create
Make sure to follow the [[File-lifecycle|File lifecycle]]

### File naming

- First letter capital, remaining words separated by `-` (e.g., `Data-model.md`).
- Update or create an `MOC-<Folder>.md` to add file.
- Add frontmatter following the schema in [[Metadata]].

### Folder naming

- Folders use a 3-digit semantic prefix + descriptive name (e.g., `100-NARRATIVE`, `220-USE-CASES`).
- See [[Folder-creation-algorithm]] for the full policy on creating new folders.

### File editing
follow the [[File-lifecycle|File lifecycle]].
git is an important tool inside this vault, make sure to make the corresponding edits inside a separated branch, and commit regularly. See [[Workflow-guide]] for the detailed documentation workflow.
