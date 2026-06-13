---
type: process
title: Documentation Workflow Guide
domain: documentation-architecture
priority: high
ai-context: high
status: in-progress
audience: all
slug: documentation-workflow-guide
tags:
  - ai-input
aliases:
  - workflow guide
last-reviewed: 2026-06-09
---
# Documentation Workflow Guide

See [[Vault-Structure]] for the complete folder map.

---

## 1. The "Two-Phase" Workflow (Issue → PR)

We **do not** edit `main` directly. We **do not** open PRs for *ideas*.

### Phase 1: Alignment (The "Git Issue" Phase)
**Goal:** Get Human Sign-off (Legal, PO, Security) on *Content & Intent*.
**Tool:** GitHub **Issue** (Template: `Documentation Review Request`).
**Branch:** `<prefix>/<short-slug>`. See [[Git-branch-prefixes]] for valid prefixes. Branch from `develop`.

1.  **Create Branch** locally in Obsidian (Terminal or Git plugin).
2.  **Write/Edit** in Obsidian. Use `[[Wikilinks]]` freely. Embed diagrams `![[diagram.excalidraw]]`.
3.  **Commit Often.** Messages: [[Git-commit|git commit]]
4.  **Open Issue** using the **[[Git-issue|Documentation Review Request]]** template.
    *   Paste the **Obsidian Preview URL** (if you have Obsidian Publish/Sync) or just list changed files.
    *   **Tag Humans:** `@legal-team`, `@security-lead`, `@product-owner`.
    *   **Ask Specific Questions:** "Is the 30-day retention in Section 4 approved?"
5.  **Discuss & Iterate** on the Issue. Push new commits to the *same branch*.
6.  **Sign-off:** Required reviewers comment `/approve` or "LGTM" on the Issue.

### Phase 2: Integration (The "Pull Request" Phase)
**Goal:** Technical Quality (Links, Lint, Format, Merge Safety).
**Tool:** GitHub **Pull Request** (In-progress → Ready).
**Automation:** **Runs automatically on PR open.**

1.  **Convert:** Author clicks "Create Pull Request" from the branch (or CLI `gh pr create`).
2.  **CI Runs Instantly:**
    *  **Link Check:** No broken `[[wikilinks]]` or `https://` links.
    *   **Vale Lint:** Spelling, Legal Capitalization ("GDPR" not "gdpr"), Forbidden Words ("guarantee", "unhackable").
    *  **Format:** Frontmatter validity, Heading hierarchy.
    *  **AI Context Check:** Validates `ai-context` tags exist for RAG ingestion.
3.  **Fix Failures Locally** → Push → CI turns Green.
4.  **Mark "Ready for Review"**.
5.  **Tech/QA Review:** Check diffs, rendering, diagram exports. Self-approval allowed when reviewer is the author.
6.  **Merge:** Squash & Merge to `develop`. Branch auto-deleted. See [[Git-branching#merge-process|Git-branching: Merge Process]] for full merge rules.
7.  **Release (on demand):** When `develop` has accumulated enough changes, open a PR `develop` → `main`. After review, Squash & Merge. Issue auto-closed.
### Hotfix
see [[Git-branching#Hotfix (Emergency)|Hotfix branches]]
1. Branch directly from `main`: `hotfix/<short-slug>`
2. Fix + commit on hotfix branch
3. Open PR `hotfix/<short-slug>` → `main` for expedited review
4. After merge, immediately backport to `develop` via cherry-pick or PR `hotfix/<short-slug>` → `develop`
5. Delete the hotfix branch
---

## 2. Writing Standards for AI & Humans

### [[Metadata|Frontmatter]] (Required on every note)
### [[Docs-truth|Docs golden rules]] (Required on every note except drafts)
