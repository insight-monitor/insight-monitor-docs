---
title: Vault Structure
type: reference
domain: documentation-architecture
priority: critical
ai-context: medium
status: accepted
audience: all
slug: vault-structure
tags:
  - vault
  - folders
aliases:
  - vault structure
  - folder map
creation-date: 2026-06-10
last-reviewed: 2026-06-12
version: 3.0.1
---
# Folder Map

| Folder                      | Purpose                                        | Who Owns It                | AI Context Weight                            |
| --------------------------- | ---------------------------------------------- | -------------------------- | -------------------------------------------- |
| **000-META**                | *How to use this vault, templates, governance* | **Tech Lead / Docs Owner** | High (Instructions for AI)                   |
| **100-NARRATIVE**           | Vision, Story, "Why we exist"                  | Founder / PO               | **Critical** (Sets context for all AI tasks) |
| **110-BRAND-POSITIONING**   | Voice, Tone, Market Fit                        | Marketing / PO             | Medium                                       |
| **120-CORE-CONCEPTS**       | First Principles, Definitions, Glossary        | Architect / PO             | **Critical** (Grounding definitions)         |
| **200-DATA-MODEL**          | Entities, Relations, Schemas (Mermaid/DBML)    | Architect / Backend        | **Critical** (Schema generation)             |
| **210-INFERENCE-FRAMEWORK** | Logic rules, Algorithms, Decision Trees (MIGRATED to code repo - redirect notes remain) | Data Science / Backend | Low (Redirect) |
| **220-USE-CASES**           | User Stories, Scenarios, Test Cases            | PO / QA / UX               | **Critical** (QA/Acceptance Criteria Source) |
| **300-ARCHITECTURE**        | System Design, ADRs, Infra (MIGRATED to code repo - redirect notes remain) | Architect / Tech Lead | Low (Redirect) |
| **400-PRIVACY**             | GDPR, Data Mapping, DPIA, Retention (placeholder) | **Legal / DPO**        | **Mandatory** (Compliance Gate)              |
| **410-SECURITY**            | Threat Models, AuthZ/AuthN, Crypto (placeholder) | **Security / Architect** | **Mandatory** (Security Gate)                |
| **420-LEGAL**               | Contracts, Terms, Licenses, IP (placeholder)     | **Legal Counsel**        | **Mandatory** (Legal Gate)                   |
| **500-SCOPE**               | Phases, Milestones, In/Out of Scope            | PM / PO                    | High (Planning)                              |
| **510-RISKS**               | Risk Register, Mitigations                     | PM / Tech Lead             | Medium                                       |
| **520-LIMITATIONS**         | Known Constraints, "Won't Do"                  | PO / Tech Lead             | Medium (Scope Guardrails)                    |
| **600-SESSIONS**            | AI agents sessions                             | Team                       | Medium (Work Progress Context)               |
| **610-DAILY**               | Scrum Daily ceremonies and stopper reports     | PO / SM                    | Low (History)                                |
| **900-CRITIQUE-ITERATION**  | Retros, Decision Logs, Rejected Ideas          | Team                       | Low (History)                                |
| **980-DEPRECATED**          | **Never delete.** Move stale docs here.        | Anyone                     | None                                         |
| **990-REFERENCES**          | PDFs, Links, Papers, External Specs            | Anyone                     | Medium (RAG Context)                         |
| **INDEX.md**                | Auto-generated Map of Content (MOC)            | Automation                 | Navigation                                   |
| **README.md**               | Entry Point / Quick Start                      | Docs Owner                 | Orientation                                  |
