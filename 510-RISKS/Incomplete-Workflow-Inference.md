---
title: Incomplete Workflow Inference
type: concept
domain: risks
priority: high
ai-context: medium
status: review
audience: all
version: 1.0.0
tags:
  - multi-device
  - inference
aliases:
  - Incomplete Workflow Inference
creation-date: 2026-06-15
last-reviewed: 2026-06-15
---
# Incomplete Workflow Inference

## Risk
Activity fragments across devices. The system sees only part of the user's workflow, which can lead to incomplete or misleading inferences about intent and productivity.

## Source
[[Multi-Device-Blindspots]] — The system monitors a single endpoint, but modern work spans multiple devices.

## Mitigation
- Acknowledge the blindspot in all reports
- Never claim full activity visibility
- Consider device-agnostic signals (calendar, VPN auth logs) as auxiliary context
