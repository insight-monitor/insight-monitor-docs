---
type: reference
title: "Vault Structure"
domain: documentation-architecture
priority: critical
status: accepted
audience: all
slug: vault-structure
aliases:
  - vault structure
  - folder map
last-reviewed: 2026-06-10
---

## The Vault Structure (Mental Map)

We use a **numbered prefix** to enforce reading order & dependency logic.

| Folder                     | Purpose                                        | Who Owns It                | AI Context Weight                            |
| :------------------------- | :--------------------------------------------- | :------------------------- | :------------------------------------------- |
| **00-META**                | *How to use this vault, templates, governance* | **Tech Lead / Docs Owner** | High (Instructions for AI)                   |
| **01-NARRATIVE**           | Vision, Story, "Why we exist"                  | Founder / PO               | **Critical** (Sets context for all AI tasks) |
| **02-BRAND-POSITIONING**   | Voice, Tone, Market Fit                        | Marketing / PO             | Medium                                       |
| **03-CORE-CONCEPT**        | First Principles, Definitions, Glossary        | Architect / PO             | **Critical** (Grounding definitions)         |
| **04-DATA-MODEL**          | Entities, Relations, Schemas (Mermaid/DBML)    | Architect / Backend        | **Critical** (Schema generation)             |
| **05-INFERENCE-FRAMEWORK** | Logic rules, Algorithms, Decision Trees        | Data Science / Backend     | High (Business Logic)                        |
| **06-PRIVACY**             | GDPR, Data Mapping, DPIA, Retention            | **Legal / DPO**            | **Mandatory** (Compliance Gate)              |
| **07-SECURITY**            | Threat Models, AuthZ/AuthN, Crypto             | **Security / Architect**   | **Mandatory** (Security Gate)                |
| **08-LEGAL**               | Contracts, Terms, Licenses, IP                 | **Legal Counsel**          | **Mandatory** (Legal Gate)                   |
| **09-ARCHITECTURE**        | System Design, ADRs, Infra (C4/PlantUML)       | Architect / Tech Lead      | High (Implementation Guide)                  |
| **10-RISKS**               | Risk Register, Mitigations                     | PM / Tech Lead             | Medium                                       |
| **11-LIMITATIONS**         | Known Constraints, "Won't Do"                  | PO / Tech Lead             | Medium (Scope Guardrails)                    |
| **12-SCOPE**               | Phases, Milestones, In/Out of Scope            | PM / PO                    | High (Planning)                              |
| **13-USE-CASES-EXAMPLES**  | User Stories, Scenarios, Test Cases            | PO / QA / UX               | **Critical** (QA/Acceptance Criteria Source) |
| **14-CRITIQUE-ITERATION**  | Retros, Decision Logs, Rejected Ideas          | Team                       | Low (History)                                |
| **15-DIAGRAMS**            | Source files (`.drawio`, `.mmd`, `.plantuml`)  | Architect                  | High (Visual Context)                        |
| **16-DAILY**               | Scrum Daily ceremonies and stopper reports     | PO, SM                     | Low (History)                                |
| **17-SESSIONS**            | AI agents sessions                             | Team                       | Medium (Work Progress Context)               |
| **98-DEPRECATED**          | **Never delete.** Move stale docs here.        | Anyone                     | None                                         |
| **99-REFERENCES**          | PDFs, Links, Papers, External Specs            | Anyone                     | Medium (RAG Context)                         |
| **INDEX.md**               | Auto-generated Map of Content (MOC)            | Automation                 | Navigation                                   |
| **README.md**              | Entry Point / Quick Start                      | Docs Owner                 | Orientation                                  |
