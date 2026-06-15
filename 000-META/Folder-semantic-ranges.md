---
type: policy
domain: documentation-architecture
priority: high
ai-context: high
title: Semantic Ranges
status: review
audience: all
version: 1.0.0
tags:
  - vault
aliases:
  - folders categories
creation-date: 2026-06-10
last-reviewed: 2026-06-13
---
# Semantic Ranges (3-digit)

| Range       | Category                   | Purpose                                                        |
| ----------- | -------------------------- | -------------------------------------------------------------- |
| **000–099** | **Governance**             | Meta-documentation, templates, conventions, policies           |
| **100–199** | **Business Context**       | Vision, narrative, brand, core concepts, strategic positioning |
| **200–299** | **Domain Knowledge**       | Data models, inference logic, use cases, business rules        |
| **300–399** | **Technical Architecture** | System design, ADRs, integrations, modules, infrastructure     |
| **400–499** | **Compliance**             | Privacy, security, legal, regulatory requirements              |
| **500–599** | **Project Boundaries**     | Scope, risks, limitations, roadmap, delivery constraints       |
| **600–699** | **AI Operations**          | Agent sessions, daily ceremonies, automation, prompts          |
| **700–899** | *(Reserved)*               | Available for future categories                                |
| **900–999** | **Historical & External**  | Deprecated docs, critique logs, references, external sources   |
## Full Range Reference
```
000   Governance
├── 000-META

100   Business Context
├── 100-NARRATIVE
├── 110-BRAND-POSITIONING
├── 120-CORE-CONCEPTS

200   Domain Knowledge
├── 200-DATA-MODEL
├── 210-INFERENCE-FRAMEWORK
├── 220-USE-CASES

300   Technical Architecture
├── 300-ARCHITECTURE

400   Compliance
├── 400-PRIVACY
├── 410-SECURITY
├── 420-LEGAL

500   Project Boundaries
├── 500-SCOPE
├── 510-RISKS
├── 520-LIMITATIONS

600   AI Operations
├── 600-SESSIONS
├── 610-DAILY

900   Historical & External
├── 900-CRITIQUE-ITERATION
├── 980-DEPRECATED
├── 990-REFERENCES
```
