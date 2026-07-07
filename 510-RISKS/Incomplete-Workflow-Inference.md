---
title: Incomplete Workflow Inference
type: concept
domain: risks
priority: high
ai-context: medium
status: review
audience: all
version: 2.0.0
tags:
  - multi-device
  - inference
aliases:
  - Incomplete Workflow Inference
creation-date: 2026-06-15
last-reviewed: 2026-06-15
parent:
  - "[[510-RISKS]]"
---
# Incomplete Workflow Inference

## Problem
Activity fragments across devices. The system sees only part of the user's workflow, which can lead to incomplete or misleading inferences about intent and productivity.

### Examples
| Scenario | Observed signal | System inference | Real context | Impact |
|---|---|---|---|---|
| Senior developer reviewing PRs on tablet while laptop shows blank IDE | Laptop idle for 45 min | Low productivity / away from desk | Reading diff on tablet, evaluating changes | Core review work penalized as non-productive |
| Developer blocked by build error, researching fix on phone | Stagnant terminal, no keystrokes | Idle / distraction | Debugging via phone, testing solutions | Active problem-solving invisible to the system |
| Engineering manager watching AI tutorial on monitored laptop | Video player open, categorized as learning | Productive / skill development | Video plays in background, real work done via phone/Slack | Non-work inflates productivity metrics |
| Researcher reading PDF on laptop, taking notes on paper | Long periods without mouse/keyboard | Ambiguous / idle | Active reading and annotation, synthesis offline | Research-heavy workflows systematically undercounted |

## Source
[[Multi-Device-Blindspots]] — The system monitors a single endpoint, but modern work spans multiple devices.

## Mitigation
- Acknowledge the blindspot in all reports
- Never claim full activity visibility
- Consider device-agnostic signals (calendar, VPN auth logs) as auxiliary context
