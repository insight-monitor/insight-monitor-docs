---
type: reference
domain: documentation-architecture
priority: critical
status: accepted
audience: all
version: 2.0.0
aliases:
  - Frontmatter
---
# Metadata: Frontmatter Schema

Every file in this vault MUST have frontmatter (YAML between `---` delimiters).
This page defines every valid field and value.

---

## `type` — What the file IS (single value)

| Value       | Description                                                |
|-------------|------------------------------------------------------------|
| `concept`   | Defines an idea, principle, or mental model                |
| `policy`    | Rules, standards, or directives to follow                  |
| `process`   | Sequenced steps or instructions to execute                 |
| `reference` | Schema, template, catalog, or lookup data                  |
| `decision`  | Record of an architectural or design choice (ADR)          |
| `truth`     | Foundational, non-negotiable principle                     |
| `index`     | Navigation file (MOC, table of contents)                   |

## `domain` — What SUBJECT the file belongs to (single value)

| Value                        | Description                              |
| ---------------------------- | ---------------------------------------- |
| `documentation-architecture` | How the vault itself works               |
| `git`                        | Version control workflow and conventions |
| `narrative`                  | Project story and vision                 |
| `brand`                      | Identity and market positioning          |
| `core-concept`               | Foundational ideas and principles        |
| `data-model`                 | Data structures and schema definitions   |
| `inference`                  | AI reasoning and computational models    |
| `privacy`                    | Data protection and compliance           |
| `security`                   | Security architecture and protocols      |
| `legal`                      | Legal considerations and licensing       |
| `architecture`               | System design and technical blueprint    |
| `risks`                      | Potential issues and mitigation          |
| `limitations`                | Known constraints and boundaries         |
| `scope`                      | Project scope and objectives             |
| `use-cases`                  | Practical applications and examples      |
| `critique`                   | Review and improvement cycles            |
| `references`                 | External sources and bibliography        |

## `priority` — AI context importance (single value)

| Value      | AI Behavior                                                   |
|------------|---------------------------------------------------------------|
| `critical` | Always load. Core context for any task.                        |
| `high`     | Load by default when studying the domain.                     |
| `medium`   | Load when the domain is relevant to the current task.          |
| `low`      | Load only when the specific topic is addressed.                |
| `optional` | Skip unless explicitly requested. Tooling, infra, workflows.   |

## `status` — Maturity level (single value)

| Value         | Description                                |
| ------------- | ------------------------------------------ |
| `draft`       | Unprocessed initial ideas without structure |
| `in-progress` | Being actively developed                    |
| `review`      | Under review after a significant update     |
| `accepted`    | Stable, approved version                    |
| `deprecated`  | Superseded, kept for historical reference   |

## `audience` — Who needs this content (single value)

| Value       | Description                                 |
|-------------|---------------------------------------------|
| `all`       | Everyone (default)                          |
| `ai`        | Primarily for AI agent consumption          |
| `developer` | Engineering and implementation team         |
| `legal`     | Legal and compliance review                 |
| `product`   | Product management and strategy             |

## `title` — Human-readable display name (single value, required)

Free text with spaces, punctuation, and capitalization. Used for rendering in Obsidian,
MOCs, and plugins. Not to be confused with `aliases`, which are alternative link targets.

## `slug` — Unique stable identifier (optional)

kebab-case identifier for stable cross-referencing across renames and moves.
Useful for URLs, APIs, and RAG pipelines. Falls back to the filename if omitted.

## `ai-context` — RAG retrieval weight (single value, required)

| Value    | AI Behavior                                                  |
|----------|--------------------------------------------------------------|
| `high`   | Prioritize in RAG retrieval results.                         |
| `medium` | Include when domain-relevant.                                |
| `low`    | Downrank in retrieval; include only if highly relevant.       |

## `tags` — Supplementary labels (list)

Free-form tags for cross-cutting concerns not captured by `domain`.
Examples: `gdpr`, `rag`, `embeddings`, `on-device`, `eu-ai-act`.

## `last-reviewed` — Last review date (optional)

ISO 8601 date (`YYYY-MM-DD`). Records the last human or automated review.
Recommended for legal, security, and compliance documents where audit trails matter.
Can be automated via git hooks or CI.

## `related` — Explicit graph links (optional list)

Array of relative paths or `[[wikilinks]]` to other notes.
Provides explicit edge information for graph-based RAG and AI context resolution.

## `aliases` — Alternative names (optional list)

Free-form alternative names for `[[alias]]` wikilink resolution.
Enables linking via alternate names (e.g. `workflow guide` links to `Workflow-guide.md`).

## `version` — File version (optional)

Semantic version string: `1.0.0`, `2.1.0`.
Bump when the file's content has a significant change.

## `owner` — Person responsible (optional)

GitHub handle or name: `@SrLampi1001`
## `creation-date` — Creation date
Creation date, relevant for session and daily related files.  Format DD/MM/YYYY.

---

## Example

```yaml
---
type: concept
title: "Data Privacy Principles"
domain: privacy
priority: high
ai-context: high
status: draft
audience: all
slug: data-privacy-principles
tags:
  - gdpr
  - on-device
last-reviewed: 2026-06-09
related:
  - "06-PRIVACY/data-mapping.md"
  - "04-DATA-MODEL/user-schema.md"
aliases:
  - privacy principles
  - data privacy
version: 0.1.0
---
```
