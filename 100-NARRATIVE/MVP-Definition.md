---
title: MVP Definition
type: concept
domain: narrative
priority: critical
ai-context: high
status: draft
audience: all
version: 1.0.0
tags:
  - mvp
  - definition
  - index
aliases:
  - MVP Definition
creation-date: 2026-06-15
last-reviewed: 2026-06-15
---

# MVP Definition

## Core Promise

A working end-to-end system that captures computer activity on a Linux desktop, sends it through an AI inference pipeline, and displays inferred intent in a web dashboard. The system can distinguish learning activities from entertainment, identify friction points in workflows, and produce confidence-scored session classifications.

## Target Audience for MVP Demo

**Riwi Coders and stakeholders.** The MVP is built to demonstrate the system's value in a hybrid learning/work environment — where the same signal (YouTube, Discord, ChatGPT) can mean radically different things depending on context. A successful demo shows the system classifying activity more accurately than naive category-based monitoring.

## MVP in One Sentence

A Python agent captures window titles, screenshots, and input patterns on a Linux desktop; a Gemini API call infers the user's probable intent per session; a React dashboard displays the results.

## Key Differentiator Demonstrated

Unlike tools that label "YouTube = distraction," the MVP classifies YouTube as "technical learning" when paired with a code editor and documentation sites, or "entertainment" when activity context suggests personal use — with explicit confidence scores.

## What Success Looks Like on June 29

1. A simulated Riwi Coder session (VS Code -> MDN docs -> Discord -> YouTube tutorial) is captured, inferred, and displayed correctly in the dashboard
2. The same session shown through naive classification (YouTube = bad, Discord = bad) versus Insight Monitor's contextual classification
3. At least one friction point is identified by the AI (e.g., "User switched between 3 tabs to find the correct API reference")
4. The demo runs on a live Ubuntu machine, not just a simulation
