---
title: "Use Case: Meeting Context"
type: concept
domain: use-cases
priority: medium
ai-context: medium
status: review
audience: all
version: 0.1.0
tags:
  - use-case
  - meetings
aliases:
  - Use Case Meeting Context
creation-date: 2026-06-13
last-reviewed: 2026-06-13
---
# Use Case: Meeting Context

## Problem
Meetings produce little to no screen activity. The user may be presenting, listening, or taking notes — all indistinguishable from idle from a signal perspective.

## What the system can do
- Detect meeting tools (Zoom, Teams, Meet) as active window
- Use calendar integration to know if the user is in a scheduled meeting
- Distinguish between presenting (low own-input) and participating (chat activity, note-taking)
- Avoid classifying meeting time as low-productivity

## What the system cannot do
- Determine meeting value or relevance
- Know if the user is actively engaged or multitasking
- Access meeting audio or video content