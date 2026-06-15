---
title: Multi-Device Blindspots
type: concept
domain: limitations
priority: high
ai-context: medium
status: review
audience: all
version: 1.0.0
tags:
  - multi-device
  - blindspot
aliases:
  - Multi-Device Blindspots
  - Cross-Device Limitation
creation-date: 2026-06-13
last-reviewed: 2026-06-14
---
# Multi-Device Blindspots

The system monitors a single endpoint. Modern work spans multiple devices:

- Reading on one monitor, typing on another
- Phone used for 2FA, research, or messaging
- Meetings joined on a separate device
- VDI or remote desktop sessions
- Private browser profiles

## Risk
Activity fragments across devices. The system sees only part of the user's workflow, which can lead to incomplete or misleading inferences about intent and productivity.

## Mitigation
- Acknowledge the blindspot in all reports
- Never claim full activity visibility
- Consider device-agnostic signals (calendar, VPN auth logs) as auxiliary context