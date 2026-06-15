---
title: "Intent vs Category: The YouTube Case"
type: concept
domain: use-cases
priority: medium
ai-context: medium
status: review
audience: all
version: 0.1.0
tags:
  - use-case
  - intent
  - classification
aliases:
  - Intent vs Category
creation-date: 2026-06-13
last-reviewed: 2026-06-13
---
# Intent vs Category: The YouTube Case

## Scenario
An employee has YouTube open during working hours.

## Naive approach (binary classification)
YouTube = distraction → negative flag in report.

## Contextual approach (Intent-vs-Category)
The system evaluates additional signals before classifying:

| Signal | Tutorial scenario | Entertainment scenario |
|---|---|---|
| Screenshot content | Code editor visible alongside tutorial video | Full-screen entertainment content |
| Active window title | "How to implement OAuth in Python" | Music video or vlog title |
| Browser history | Documentation sites before/after | Social media before/after |
| Focus continuity | Frequent alt-tab between IDE and browser | Long uninterrupted playback |
| Input activity | Typing code patterns in editor | No keyboard/mouse input during playback |

## Outcome
- Tutorial → classified as **work-relevant research**
- Entertainment → classified as **personal / probable distraction**
- Ambiguous → classified as **unclear** (not defaulted to negative)

## Why this matters
Reduces false accusations against legitimate YouTube usage. Makes classifications explainable by citing the evidence signals used.