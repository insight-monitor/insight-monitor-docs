---
title: Product MVP Definition
type: concept
domain: narrative
priority: critical
ai-context: high
status: draft
audience: all
version: 1.0.0
tags:
  - mvp
  - product
  - definition
aliases:
  - Product MVP
  - Full MVP
creation-date: 2026-06-26
last-reviewed: 2026-06-26
---

# Product MVP Definition

> **Scope Clarification**: This document defines the **Product MVP** — the full product with ethical safeguards per [[Core-principles]]. The **Inference Pipeline v0.1** (June 15–29 sprint) delivers only the technical inference engine. See [[Inference-Pipeline-v0.1-vs-MVP]].

## Core Promise

A monitoring system where the **monitored person is the primary beneficiary**. Every inference is transparent, challengeable, and proportional. The system provides self-insight, not just surveillance.

## Target Audience for Product MVP

**Monitored persons** (workers, students, individuals) and **observers** (supervisors, educators, organizations) — with the monitored person's benefit as the north star.

## Product MVP in One Sentence

An end-to-end monitoring platform that captures activity, infers intent with contextual AI, and gives **both** the observer and the monitored person full transparency, challengeability, and self-insight — built on Observation > Control architecture.

## What Success Looks Like (Product MVP)

1. **Transparency View** — Monitored person sees all raw events, derived sessions, inferences, and evidence traces
2. **Challengeability** — "Correct this" button on every inference; feedback retrains the prompt
3. **Self-Insight Dashboard** — Focus patterns, break quality, task energy, progress over time
4. **Proportionality Controls** — Per-inference data justification; no indiscriminate logging
5. **Uncertainty UI** — "We don't know" surfaced as a feature, not hidden
6. **Power Distribution** — Opt-in, data ownership, no secret scores

## Product MVP Capabilities (Beyond Pipeline v0.1)

| Layer | Pipeline v0.1 Delivers | Product MVP Adds |
|-------|------------------------|------------------|
| **Capture** | Window title, screenshots, input freq | Browser extension, clipboard metadata, Wayland |
| **Inference** | Session → Intent (Riwi-aware) | Dynamic catalog, anomaly detection, NL query |
| **API** | REST endpoints for sessions/intent | Auth, multi-tenant, webhooks |
| **Dashboard** | Observer view only | **Self-view** (transparency, challenge, self-insight) |
| **Ethics** | Error philosophy (false positive near zero) | **Challengeability, proportionality, user control** |

## Ethical Guardrails (Non-Negotiable)

- **No inference without evidence trace** — Every classification links to source signals
- **No challenge without feedback loop** — Disputes improve the model
- **No data without proportionality justification** — Each signal tied to inference need
- **No dashboard without self-view** — Monitored person sees what observer sees
- **No "confident" without uncertainty** — Low confidence = "we don't know" UI

## Relationship to Inference Pipeline v0.1

The Pipeline v0.1 is the **technical foundation**. The Product MVP is the **ethical product**. Pipeline v0.1 proves the technical thesis; Product MVP proves the ethical thesis.

## References

- [[Inference-Pipeline-v0.1-vs-MVP]] — Detailed scope comparison
- [[Core-principles]] — Ethical principles driving Product MVP
- [[Stance-on-Surveillance]] — Observation vs Control philosophy
- [[Inference-Pipeline-Definition]] — Pipeline v0.1 narrative (legacy alias)