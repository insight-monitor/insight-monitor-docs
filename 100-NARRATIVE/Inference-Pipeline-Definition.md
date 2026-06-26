---
title: Inference Pipeline v0.1 Definition
type: concept
domain: narrative
priority: critical
ai-context: high
status: draft
audience: all
version: 2.0.0
tags:
  - inference-pipeline
  - definition
  - index
aliases:
  - Inference Pipeline v0.1 Definition
  - MVP-Definition (legacy alias)
creation-date: 2026-06-15
last-reviewed: 2026-06-24
---
# Inference Pipeline v0.1 Definition

> **Scope Clarification**: This document describes the **Inference Pipeline v0.1** — the core technical engine delivered in the 14-day sprint (June 15–29). The full **Product MVP** (including transparency, challengeability, individual benefit features per [[Core-principles]]) is out of scope for v0.1 and tracked for post-v0.1 delivery.

## Core Promise

A working end-to-end **inference engine** that captures computer activity on a Linux desktop, sends it through an AI inference pipeline, and displays inferred intent in a web dashboard. The system distinguishes learning activities from entertainment, identifies friction points in workflows, and produces confidence-scored session classifications.

## Target Audience for v0.1 Demo

**Riwi Coders and stakeholders.** The v0.1 demo demonstrates the **technical differentiator**: contextual AI classification over binary labels. It shows the same signal (YouTube, Discord, ChatGPT) classified differently depending on surrounding context — with explicit confidence scores and evidence traces.

## Inference Pipeline v0.1 in One Sentence

A Python agent captures window titles, screenshots, and input patterns on a Linux desktop; a Gemini API call infers the user's probable intent per session; a React dashboard displays the results.

## Key Differentiator Demonstrated

Unlike tools that label "YouTube = distraction," the inference pipeline classifies YouTube as "technical learning" when paired with a code editor and documentation sites, or "entertainment" when activity context suggests personal use — with explicit confidence scores and evidence traces.

## What Success Looks Like on June 29 (v0.1)

1. A simulated Riwi Coder session (VS Code → MDN docs → Discord → YouTube tutorial) is captured, inferred, and displayed correctly in the dashboard
2. The same session shown through naive classification (YouTube = bad, Discord = bad) versus Insight Monitor's contextual classification. At least one friction point is identified by the AI (e.g., "User switched between 3 tabs to find the correct API reference")
3. The demo runs on a live Ubuntu machine, not just a simulation

## What Is NOT in v0.1 (Deferred to Product MVP)

| Capability | Core Principle | Post-v0.1 Tracking |
|---|---|---|
| **Challengeability** — User can dispute/reframe inferences | Challengeability | [[Use-Case-Challenge-Inference]] |
| **Transparency View** — User sees all collected data & inferences | Transparency | [[Use-Case-Transparency-View]] |
| **Individual Benefit Dashboard** — Self-insight for monitored person | Individual Benefit | [[Use-Case-Self-Insight]] |
| **Uncertainty UI** — "We don't know" surfaced as feature | Uncertainty as Feature | [[Use-Case-Uncertainty-UI]] |
| **Proportionality Controls** — Per-inference data justification | Proportionality | [[Use-Case-Proportionality]] |
| **Observation vs Control** — Power distribution features | Observation vs Control | [[Use-Case-Power-Distribution]] |

## Relationship to Full Product Vision

| Layer | v0.1 Delivers | Product MVP Adds |
|---|---|---|
| **Capture** | Window title, screenshots, input freq | Browser extension, clipboard metadata, Wayland |
| **Inference** | Session → Intent (Riwi-aware) | Dynamic catalog, anomaly detection, NL query |
| **API** | REST endpoints for sessions/intent | Auth, multi-tenant, webhooks |
| **Dashboard** | Observer view (session list + detail) | **Self-view** (transparency, challenge, self-insight) |
| **Ethics** | Error philosophy (false positive near zero) | Challengeability, proportionality, user control |

## See Also

- [[MVP-Definition]] — Full Product MVP definition with ethical safeguards
- [[Inference-Pipeline-v0.1-vs-MVP]] — Detailed scope comparison
