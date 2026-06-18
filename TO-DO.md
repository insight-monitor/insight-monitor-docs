---
title: to do
type: notice
status: draft
priority: medium
ai-context: medium
audience: all
domain: documentation
creation-date: 2026-06-17
last-reviewed: 2026-06-17
version: 1.0.0
---
- [x] Update REAME into a natural lecture guideline through the projects. It will be a narrative file but will be composed of inserted notes, instead of simple references, that guide the user or developer in a natural path through the different sections.
- [x] Update documentation to separate technical with non-technical concerns. Architecture folder must be re-factorized or deleted, and be changed for something else. 
      This is based on the existence of multiple repositories, separate the documentation completely into another repository and leave the [coding repository](https://github.com/insight-monitor/insight-monitor-code) without any.
      Better alternative: the [documentation repository](https://github.com/insight-monitor/insight-monitor-docs) contains non-technical documentation -> Narrative, Feature descriptions, Limitations, use, etc. And the [coding repository](https://github.com/insight-monitor/insight-monitor-code) contains the technical documentation -> Architecture, code, ERD, schemas, modularization, etc.
      Status: **Done** — 300-ARCHITECTURE, 210-INFERENCE-FRAMEWORK, Data-Acquisition, and Configuration-Model migrated to `insight-monitor-code/docs/`. Original files replaced with redirect notes.
- [ ] Add missing vault folders: 400-PRIVACY, 410-SECURITY, 420-LEGAL, 990-REFERENCES
- [ ] Implement CI/CD workflows in both repos (see Raw-ai-response.md)