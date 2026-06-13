---
title: File life cycle
domain: documentation-architecture
type: process
status: review
priority: high
ai-context: medium
audience: all
version: 2.0.0
tags:
  - vault
aliases:
  - File lifecyle
creation-date: 2026-06-07
last-reviewed: 2026-06-13
---
## Creation
Start by creating a file at root, make sure to add the minimum frontmatter [[Metadata]]:
`status: draft`, `type`, `audience` and `domain`.
Firstly created documents don't need a strict structure, if you need to do a git commit, add `status(draft)`
## Optimization
1. Read [[Atomization]] and split concerns into separate files as needed.
2. Follow [[Folder-creation-algorithm|semantic ranges]] to position the files in the correct domain
3. Change `status` to `in-progress` once structured. (Make commits when necessary)
   > Make sure the files are connected, or at least appear in the MOC
4. When done, change `status: review` commit and create a [[Git-issue|git issue]] for everyone or the one responsible to review
   If you are the responsible, create the issue nevertheless, to create a throughout git history.
5. When reviewed, change `status: accepted` and make a [[Pull-request-template|PR]] to `develop`.
6. If deprecated, change `status: deprecated` and move to `980-DEPRECATED` folder.
## Deletion
If the files were on `develop` or `main` and were valid, DO NOT delete them, put them into `980-DEPRECATED`.
If the files only existed on disposable branches or were rejected on Review, they can be Deleted.
