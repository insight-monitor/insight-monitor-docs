---
type: process
title: "Documentation Workflow Guide"
domain: documentation-architecture
priority: high
ai-context: high
status: draft
audience: all
slug: documentation-workflow-guide
tags:
  - ai-input
aliases:
  - workflow guide
last-reviewed: 2026-06-09
---
# 📖 Documentation Workflow Guide
> **Audience:** Product, Legal, Design, Devs, AI Agents (Cursor/Copilot/GPT), QA.
> **Repo:** `insight-monitor-docs` (Obsidian Vault)
> **Golden Rule:** **Documentation is the Source of Truth.** Code implements what lives here first.

---

## 1. The Vault Structure (Mental Map)
We use a **numbered prefix** to enforce reading order & dependency logic.

| Folder | Purpose | Who Owns It | AI Context Weight |
| :--- | :--- | :--- | :--- |
| **00-META** | *How to use this vault, templates, governance* | **Tech Lead / Docs Owner** | High (Instructions for AI) |
| **01-NARRATIVE** | Vision, Story, "Why we exist" | Founder / PO | **Critical** (Sets context for all AI tasks) |
| **02-BRAND-POSITIONING** | Voice, Tone, Market Fit | Marketing / PO | Medium |
| **03-CORE-CONCEPT** | First Principles, Definitions, Glossary | Architect / PO | **Critical** (Grounding definitions) |
| **04-DATA-MODEL** | Entities, Relations, Schemas (Mermaid/DBML) | Architect / Backend | **Critical** (Schema generation) |
| **05-INFERENCE-FRAMEWORK** | Logic rules, Algorithms, Decision Trees | Data Science / Backend | High (Business Logic) |
| **06-PRIVACY** | GDPR, Data Mapping, DPIA, Retention | **Legal / DPO** | **Mandatory** (Compliance Gate) |
| **07-SECURITY** | Threat Models, AuthZ/AuthN, Crypto | **Security / Architect** | **Mandatory** (Security Gate) |
| **08-LEGAL** | Contracts, Terms, Licenses, IP | **Legal Counsel** | **Mandatory** (Legal Gate) |
| **09-ARCHITECTURE** | System Design, ADRs, Infra (C4/PlantUML) | Architect / Tech Lead | High (Implementation Guide) |
| **10-RISKS** | Risk Register, Mitigations | PM / Tech Lead | Medium |
| **11-LIMITATIONS** | Known Constraints, "Won't Do" | PO / Tech Lead | Medium (Scope Guardrails) |
| **12-SCOPE** | Phases, Milestones, In/Out of Scope | PM / PO | High (Planning) |
| **13-USE-CASES-EXAMPLES** | User Stories, Scenarios, Test Cases | PO / QA / UX | **Critical** (QA/Acceptance Criteria Source) |
| **14-CRITIQUE-ITERATION** | Retros, Decision Logs, Rejected Ideas | Team | Low (History) |
| **15-DIAGRAMS** | Source files (`.drawio`, `.mmd`, `.plantuml`) | Architect | High (Visual Context) |
| **98-DEPRECATED** | **Never delete.** Move stale docs here. | Anyone | None |
| **99-REFERENCES** | PDFs, Links, Papers, External Specs | Anyone | Medium (RAG Context) |
| **INDEX.md** | Auto-generated Map of Content (MOC) | Automation | Navigation |
| **README.md** | Entry Point / Quick Start | Docs Owner | Orientation |

---

## 2. The "Two-Phase" Workflow (Issue → PR)

We **do not** edit `main` directly. We **do not** open PRs for *ideas*.

### Phase 1: Alignment (The "Git Issue" Phase)
**Goal:** Get Human Sign-off (Legal, PO, Security) on *Content & Intent*.
**Tool:** GitHub **Issue** (Template: `Documentation Review Request`).
**Branch:** `topic/<short-slug>` (e.g., `topic/legal-gdpr-update`). Branch from `develop`.

1.  **Create Branch** locally in Obsidian (Terminal or Git plugin).
2.  **Write/Edit** in Obsidian. Use `[[Wikilinks]]` freely. Embed diagrams `![[diagram.excalidraw]]`.
3.  **Commit Often.** Messages: `feat: add data retention policy`, `fix: correct liability cap amount`.
4.  **Open Issue** using the **📄 Documentation Review Request** template.
    *   Paste the **Obsidian Preview URL** (if you have Obsidian Publish/Sync) or just list changed files.
    *   **Tag Humans:** `@legal-team`, `@security-lead`, `@product-owner`.
    *   **Ask Specific Questions:** "Is the 30-day retention in Section 4 approved?"
5.  **Discuss & Iterate** on the Issue. Push new commits to the *same branch*.
6.  **✅ Sign-off:** Required reviewers comment `/approve` or "LGTM" on the Issue.

### Phase 2: Integration (The "Pull Request" Phase)
**Goal:** Technical Quality (Links, Lint, Format, Merge Safety).
**Tool:** GitHub **Pull Request** (Draft → Ready).
**Automation:** **Runs automatically on PR open.**

1.  **Convert:** Author clicks "Create Pull Request" from the branch (or CLI `gh pr create`).
2.  **CI Runs Instantly:**
    *   🔗 **Link Check:** No broken `[[wikilinks]]` or `https://` links.
    *   ✍️ **Vale Lint:** Spelling, Legal Capitalization ("GDPR" not "gdpr"), Forbidden Words ("guarantee", "unhackable").
    *   📐 **Format:** Frontmatter validity, Heading hierarchy.
    *   🤖 **AI Context Check:** Validates `ai-context` tags exist for RAG ingestion.
3.  **Fix Failures Locally** → Push → CI turns Green ✅.
4.  **Mark "Ready for Review"**.
5.  **Tech/QA Review:** Check diffs, rendering, diagram exports.
6.  **Merge:** Squash & Merge to `develop`. Branch auto-deleted.
7.  **Release (on demand):** When `develop` has accumulated enough changes, open a PR `develop` → `main`. After review, Squash & Merge. Issue auto-closed.

---

### Hotfix (Emergency)
**Use sparingly.** Only for urgent fixes (broken links, compliance deadlines, security patches).

1. Branch directly from `main`: `topic/hotfix-<short-slug>`
2. Fix + commit on hotfix branch
3. Open PR `topic/hotfix-<slug>` → `main` for expedited review
4. After merge, immediately backport to `develop` via cherry-pick or PR `topic/hotfix-<slug>` → `develop`
5. Delete the hotfix branch

---

## 3. Writing Standards for AI & Humans

### Frontmatter (Required on **Every** Note)
```yaml
---
title: "Data Retention Policy"       # Human Title
slug: "legal-data-retention"         # URL/ID slug (kebab-case)
status: "draft"                      # draft | in-progress | review | accepted | deprecated
tags: [legal, gdpr, privacy, policy] # Lowercase, plural
ai-context: "high"                   # high | medium | low (RAG retrieval weight)
owner: "@legal-counsel"              # GitHub handle
last-reviewed: "2024-01-15"          # YYYY-MM-DD
related:                             # Explicit graph links for AI
  - "06-PRIVACY/data-mapping.md"
  - "04-DATA-MODEL/user-schema.md"
---