---
type: reference
domain: documentation-architecture
priority: critical
status: in-progress
audience: all
version: 1.0.0
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

| Value                        | Description                                    |
|------------------------------|------------------------------------------------|
| `documentation-architecture` | How the vault itself works                     |
| `git`                        | Version control workflow and conventions        |
| `narrative`                  | Project story and vision                        |
| `brand`                      | Identity and market positioning                 |
| `core-concept`               | Foundational ideas and principles               |
| `data-model`                 | Data structures and schema definitions          |
| `inference`                  | AI reasoning and computational models           |
| `privacy`                    | Data protection and compliance                  |
| `security`                   | Security architecture and protocols             |
| `legal`                      | Legal considerations and licensing              |
| `architecture`               | System design and technical blueprint           |
| `risks`                      | Potential issues and mitigation                 |
| `limitations`                | Known constraints and boundaries                |
| `scope`                      | Project scope and objectives                    |
| `use-cases`                  | Practical applications and examples             |
| `critique`                   | Review and improvement cycles                   |
| `references`                 | External sources and bibliography               |

## `priority` — AI context importance (single value)

| Value      | AI Behavior                                                   |
|------------|---------------------------------------------------------------|
| `critical` | Always load. Core context for any task.                        |
| `high`     | Load by default when studying the domain.                     |
| `medium`   | Load when the domain is relevant to the current task.          |
| `low`      | Load only when the specific topic is addressed.                |
| `optional` | Skip unless explicitly requested. Tooling, infra, workflows.   |

## `status` — Maturity level (single value)

| Value          | Description                                                |
|----------------|------------------------------------------------------------|
| `raw`          | Unprocessed draft, ideas without structure                  |
| `in-progress`  | Being actively developed                                   |
| `in-review`    | Under review after a significant update                    |
| `completed`    | Stable version                                             |
| `deprecated`   | Superseded, kept for historical reference                   |

## `audience` — Who needs this content (single value)

| Value       | Description                                 |
|-------------|---------------------------------------------|
| `all`       | Everyone (default)                          |
| `ai`        | Primarily for AI agent consumption          |
| `developer` | Engineering and implementation team         |
| `legal`     | Legal and compliance review                 |
| `product`   | Product management and strategy             |

## `tags` — Supplementary labels (list)

Free-form tags for cross-cutting concerns not captured by `domain`.
Examples: `gdpr`, `rag`, `embeddings`, `on-device`, `eu-ai-act`.

## `version` — File version (optional)

Semantic version string: `1.0.0`, `2.1.0`.
Bump when the file's content has a significant change.

## `owner` — Person responsible (optional)

GitHub handle or name: `@cohorte6`

---

## Example

```yaml
---
type: concept
domain: privacy
priority: high
status: raw
audience: all
version: 0.1.0
tags:
  - gdpr
  - on-device
---
```
