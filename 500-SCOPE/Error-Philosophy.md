---
title: Error Philosophy
type: concept
domain: scope
priority: high
ai-context: medium
status: review
audience: all
version: 1.0.0
tags:
  - errors
  - philosophy
aliases:
  - Error Philosophy
creation-date: 2026-06-13
last-reviewed: 2026-06-14
parent:
  - "[[500-SCOPE]]"
---
# Error Philosophy

## Guiding principle
The cost of a false accusation exceeds the cost of a missed detection. The system is tuned to minimize harm to the user, not to maximize detection.

## False positives (productivity)
Classifying productive work as distraction is the worst error. It erodes trust and creates adversarial dynamics. Tolerance: near zero.

## False negatives (productivity)
Missing actual distraction is acceptable. Better to classify nothing than to misclassify work as waste.

## False positives (sensitive data)
Persisting sensitive content (credentials, health data, private communications) is unacceptable. Redaction must be aggressive even at the cost of losing legitimate context.

## False negatives (sensitive data)
Acceptable within reason. If some borderline content passes through, logging and audit can catch it. Never risk user privacy to maximize capture.

## Default stance
When uncertain, the system defaults to the least harmful interpretation: activity is ambiguous, not classified.