---
domain: documentation-architecture
type: process
priority: high
ai-context: medium
status: review
audience: all
aliases:
  - how to create a folder
  - folder naming convention
  - semantic ranges
---
# Folder Creation Algorithm

## Core Rule

> **Top-level folder numbers are semantic identifiers, not chronological identifiers.**
> Numbers encode the folder's role in the knowledge architecture and remain stable over time.
> New folders are assigned according to their semantic category, not their creation date.
> Reserved ranges must be maintained to allow future expansion without renumbering existing folders.

## Folder Creation Algorithm

### Step 1 — Determine Category

When creating a new top-level folder, classify its content:

| Question | Category |
|---|---|
| Is this about how the vault itself works? | **Governance** (000–099) |
| Does it describe why the project exists or who it serves? | **Business Context** (100–199) |
| Does it model the problem domain? | **Domain Knowledge** (200–299) |
| Does it describe how the system is built? | **Technical Architecture** (300–399) |
| Does it deal with legal or regulatory obligations? | **Compliance** (400–499) |
| Does it define what is being built and what constraints apply? | **Project Boundaries** (500–599) |
| Is it about AI agent workflows or team ceremonies? | **AI Operations** (600–699) |
| Is it historical, deprecated, or from external sources? | **Historical & External** (900–999) |
see [[Folder-semantic-ranges|folders categories]]
### Step 2 — Identify the Range

Once the category is determined, locate the appropriate numerical range from the table above.

**Example:** *Agent Evaluation Reports* → AI Operations → **600–699**

### Step 3 — Choose the Nearest Available Number

Within the range, pick a number that:
- Is not already in use
- Is close to related folders
- Leaves gaps for future additions

**Example within 600–699:**
```
600-SESSIONS
610-DAILY
```

A new folder *Agent Evaluation Reports* could become `620-EVALUATIONS`.

### Step 4 — Leave Gaps

Prefer leaving gaps between numbers to allow organic growth.

**Preferred:**
```
100
110
120
```
**Avoid (unless the range is already dense):**
```
100
101
102
```

The important invariant is that **ranges stay stable**. Individual numbers within a range can become dense over time, but a category must never overflow into another category's range.
## Examples

### Adding `API-CONTRACTS`

1. **Category:** Technical Architecture (API contracts describe system interfaces)
2. **Range:** 300–399
3. **Available number:** `310` (between `300-ARCHITECTURE` and reserved slots)
4. **Result:** `310-API-CONTRACTS`

### Adding `PROMPTS`

1. **Category:** AI Operations (prompts are used by AI agents)
2. **Range:** 600–699
3. **Available number:** `630` (after `620-EVALUATIONS`)
4. **Result:** `630-PROMPTS`

### Adding `KNOWLEDGE-GRAPHS`

1. **Category:** Domain Knowledge (knowledge graphs model the domain)
2. **Range:** 200–299
3. **Available number:** `230` (after `220-USE-CASES`)
4. **Result:** `230-KNOWLEDGE-GRAPHS`
