---
title: Offline Cognition Misclassification
type: concept
domain: risks
priority: high
ai-context: medium
status: review
audience: all
version: 2.0.0
tags:
  - offline
  - misclassification
aliases:
  - Offline Cognition Misclassification
creation-date: 2026-06-15
last-reviewed: 2026-06-15
parent:
  - "[[510-RISKS]]"
---
# Offline Cognition Misclassification

## Problem
Users whose work involves high cognitive load with low screen interaction (writers, strategists, researchers) may appear idle or unproductive. The system cannot distinguish deep thinking from being away from the keyboard.

### Examples
| Scenario | Observed signal | System inference | Real context | Impact |
|---|---|---|---|---|
| Writer composing a chapter, staring at screen, typing a few sentences per hour | Near-zero active input for extended periods | Idle / away from keyboard | Deep composition, mentally structuring arguments, revising internally | Creative knowledge workers appear least productive |
| Strategist reading a 50-page report, extracting insights for one slide | Document open for 2 hours, no typing | Distracted / procrastinating | Close reading, synthesis, identifying patterns | Strategic synthesis work invisible |
| Developer debugging a race condition by tracing code mentally | Long gaps between quick edits, no app switching | Low activity / ambiguous | Tracing control flow, formulating and discarding hypotheses | Debugging time classified as neutral rather than productive |
| Architect sketching system design on whiteboard | Laptop locked, screensaver active | Away / break | Resolving data-flow tradeoffs, capturing photo for ADR | Core design work entirely invisible |

## Source
[[Offline-Cognition]] — The system relies on visible digital signals, but significant knowledge work happens outside this view.

## Mitigation
- Allow users to mark "focus mode" periods
- Do not penalize inactivity without additional context
- Surface uncertainty when screen activity is low but task continuity exists
