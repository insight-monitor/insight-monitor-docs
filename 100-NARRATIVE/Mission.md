---
title: Mission
type: concept
domain: narrative
priority: critical
ai-context: high
status: review
audience: all
version: 1.0.1
tags:
  - mission
aliases:
  - Product's Mission
  - Insight Monitor Mission
creation-date: 2026-06-09
last-reviewed: 2026-06-12
---
# Mission

Build the infrastructure that estimates probable task intent from multimodal computer activity signals.

To achieve this, Insight Monitor collects and processes:

- **System signals** — active windows, application metadata, focus transitions
- **Content signals** — visible text (OCR, accessibility APIs), document context, URLs
- **Interaction signals** — input patterns, navigation, application-specific actions
- **Temporal signals** — session duration, task continuity, interruption patterns

And produces structured behavioral intelligence:

- Per-activity intent hypotheses with [[confidence scoring]]
- Task chain reconstruction and context-switch detection
- Efficiency estimates relative to [[defined baselines]]
- Evidence-traceable distraction and focus classification
- Detection of low-interaction periods flagged with explicit uncertainty
