---
type: process
domain: documentation-architecture
priority: critical
status: in-progress
audience: all
version: 1.0.0
---

# How to use this vault

This file discusses how to create, modify and remove documentation files.

## What to use

If the git repository was cloned:
- Install Obsidian
- Use community plugins (continuous mode)
- Review [[Metadata]] for the frontmatter schema

## How to read

This documentation is highly modular. To navigate:
- **Folder naming** — Folders are prefixed with numbers for scope ordering (00-META, 01-NARRATIVE, ...).
- **MOCs (Map of Content)** — Each folder has an `MOC-<Folder>.md` that links all files in that domain.
- **Priority** — Files with `priority: critical` and `priority: high` are the primary reading path. Files with `priority: low` or `optional` should only be read when relevant.

## How to create

### File naming

- First letter capital, remaining words separated by `-` (e.g., `Data-Model.md`).
- Update or create an `MOC-<Folder>.md` for the folder.
- Add frontmatter following the schema in [[Metadata]].

### File lifecycle

1. Start: add `status: raw` in frontmatter. Write ideas without worrying about structure.
2. Read [[Atomization]] and split concerns into separate files as needed.
3. Refactor `status` to `in-progress` once structured.
4. Once reviewed, set `status: completed`.
5. Commit and push per [[Git-norms]].
