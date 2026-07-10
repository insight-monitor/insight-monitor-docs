---
title: Inference Pipeline v0.1 vs Product MVP — Scope Comparison
type: reference
domain: scope
priority: critical
ai-context: high
status: accepted
audience: all
version: 2.0.0
tags:
  - mvp
  - scope-comparison
  - pipeline
  - context-awareness
  - product
aliases:
  - Pipeline v0.1 vs MVP
  - Scope Clarification
creation-date: 2026-06-24
last-reviewed: 2026-06-24
parent:
  - "[[500-SCOPE]]"
---
# Inference Pipeline v0.1 vs Product MVP — Scope Comparison

## Executive Summary

| Aspect | Inference Pipeline v0.1 (This Sprint) | Product MVP (Post-v0.1) |
|--------|--------------------------------------|------------------------|
| **Timeline** | June 15–29, 2026 (14 days) | Post-June 29 |
| **Core Deliverable** | Contextual AI inference engine | Full product with ethical safeguards |
| **Primary User** | Observer (supervisor, educator, org) | **Monitored person** (worker, student, individual) |
| **Key Differentiator** | Contextual classification > binary labels | Observation > Control architecture |

---

## Technical Capabilities

| Capability | Pipeline v0.1 | Product MVP |
|------------|---------------|-------------|
| Capture: window title, screenshots, input freq | ✅ | ✅ + browser extension, clipboard metadata, Wayland |
| Session detection (time-gap) | ✅ | ✅ + dynamic catalog, anomaly detection |
| LLM inference (Gemini 2.0 Flash) | ✅ | ✅ + model routing, local models |
| Structured intent output | ✅ | ✅ + NL query, report generation |
| REST API (sessions, intent) | ✅ | ✅ + auth, multi-tenant, webhooks |
| Dashboard: session list + detail | ✅ Observer view only | ✅ **Observer + Self-view** |
| Dev tools (simulate, seed) | ✅ | ✅ |

---

## Ethical / Product Capabilities (Core Principles)

| Core Principle | Pipeline v0.1 | Product MVP |
|----------------|---------------|-------------|
| **Transparency** | ❌ Not implemented | ✅ User sees all collected data & inferences |
| **Challengeability** | ❌ Not implemented | ✅ "Correct this" + evidence trace + feedback loop |
| **Proportionality** | ❌ Not implemented | ✅ Per-inference data justification |
| **Individual Benefit** | ❌ Dashboard for observer only | ✅ **Self-insight dashboard** (primary beneficiary) |
| **Uncertainty as Feature** | ⚠️ Confidence scores only | ✅ "We don't know" UI + explicit uncertainty surfacing |

---

## What "Inference Pipeline v0.1" Proves

The v0.1 demo validates the **technical thesis**:

> **Activity only has meaning relative to intent. Context can be estimated from lightweight signals.**

Demonstrated via:
- YouTube → "technical learning" when paired with VS Code + docs
- YouTube → "entertainment" when activity context suggests personal use
- Confidence scores + evidence traces for every classification
- Friction point detection (tab switching, error patterns)

---

## What Requires Product MVP (Post-v0.1)

The **ethical thesis** requires the Product MVP:

> **Monitoring designed around constraints that protect the observed person. The monitored person is the primary beneficiary.**

Requires:
1. **Transparency View** — User sees raw events, derived sessions, inferences, evidence
2. **Challengeability** — User disputes inference → system learns → retrains prompt
3. **Self-Insight Dashboard** — Focus patterns, break quality, task energy, progress over time
4. **Proportionality Controls** — Per-inference data collection justification
5. **Power Distribution** — Opt-in, data ownership, no secret scores

---

## Architecture Implications

| Layer | Pipeline v0.1 (Current Clean Arch) | Product MVP (Additions) |
|-------|-----------------------------------|------------------------|
| Domain | Session, IntentRecord, RawEvent | + Challenge, TransparencyLog, UserPreferences |
| Application | 6 use cases (ingest, build, infer, close, get, list) | + ChallengeInference, GetTransparency, GetSelfInsight |
| Infrastructure | SQLite, Gemini, FastAPI | + PostgreSQL, Auth, EventStore, Webhooks |
| Interfaces | Observer Dashboard (React) | + **Self Dashboard** (React Native / PWA) |

---

## References

- [[Inference-Pipeline-Definition]] — v0.1 narrative with explicit exclusions
- [[MVP-Definition]] — Product MVP definition with ethical safeguards
- [[Inference-Pipeline-Included]] — Technical capabilities in v0.1
- [[Inference-Pipeline-Excluded]] — Technical capabilities deferred
- [[Core-principles]] — Ethical principles driving Product MVP scope
- [[Stance-on-Surveillance]] — Observation vs Control philosophy
- Code repository Inference-Pipeline plan : [ARCH-0 Master Plan](https://github.com/insight-monitor/insight-monitor-code/issues/41)