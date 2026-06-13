---
title: "User Context Schema"
type: reference
domain: data-model
priority: high
ai-context: high
status: draft
audience: all
version: 0.1.0
tags:
  - user-context
  - schema
aliases:
  - User Context Schema
---
# User Context Schema

The user can define context that shapes how the AI interprets activity.

## Role / Job function
- Title or description of the user's role
- Example: "Software Engineer", "Data Analyst", "Student at RIWI"

## Current objectives
- List of active goals or projects
- Each objective: name, priority (high/medium/low), expected deliverables
- Example: "Implement user authentication module (high)"

## Work schedule
- Expected working hours or study hours
- Break periods
- Meeting blocks (integration with calendar optional)

## Application classification
- User can tag applications as: work-critical, work-optional, personal, or blocked
- Overrides the system's default classification for that app
- Example: "YouTube → work-optional" (system default may differ)

## Sensitive keywords / domains
- User-defined terms or domains that trigger enhanced redaction
- Example: "therapy, lawyer, bank-name"

## Task annotations
- User can mark time periods with labels: "deep work", "meeting", "research", "break"
- These labels serve as ground truth for model improvement