---
title: Confidence and Uncertainty Limitations
type: concept
domain: limitations
priority: high
ai-context: medium
status: review
audience: all
version: 1.0.0
tags:
  - confidence
  - uncertainty
aliases:
  - Confidence and Uncertainty Limitations
  - Probabilistic Boundaries
creation-date: 2026-06-13
last-reviewed: 2026-06-14
parent:
  - "[[520-LIMITATIONS]]"
---
# Confidence and Uncertainty Limitations

All inferences are probabilistic estimates, not facts.

## Sources of uncertainty
- Partial visibility (screenshots at intervals, not continuous video)
- Ambiguous signals (same app used for work and personal tasks)
- Model limitations (vision-language models can hallucinate or misclassify)
- Context gaps (no access to user's internal goals or priorities)

## Constraint
The system must never present inferences as certain. Every output below a confidence threshold must be explicitly flagged as uncertain. Users and managers must understand that "the system says" is always "the system estimates with N% confidence."