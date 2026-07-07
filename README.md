---
title: Walk through the project
type: concept
domain: narrative
priority: critical
ai-context: high
status: in-progress
audience: all
tags:
  - context
  - narrative
  - index
creation-date: 2026-06-06
last-reviewed: 2026-06-17
version: 2.1.0
parent:
  - "[[insight-monitor-docs]]"
---

# Insight Monitor 
>Read what [[How-to-use-this-vault|Tools to read this vault comfortably]]

Most productivity monitoring treats activity as if it had a fixed meaning. A website is "productive" or "unproductive." A developer watching a conference talk is flagged as distracted — even when they are unblocking a problem that would have taken hours. The system measured time. It missed the intent that gives the time meaning.
## Our Foundation

Before diving into architecture or data models, understand why this project exists and what it stands for.

![[Founding-Story]]

From here, the narrative section builds your understanding in order:

1. ![[Stance-on-Surveillance]]
2. ![[Core-principles]]
3. ![[Why-This-Matters-to-You]] 
4. ![[Vision]]
5. ![[Mission]] 

## Core Concepts

Once you understand the why, ground yourself in the language and axioms of the system:

- ![[Glossary]]
- ![[First-Principles]] 

## What We Measure

The system collects lightweight, multimodal signals — window titles, application names, browser domains, periodic snapshots — and builds a structured picture of user activity.

Start with [[Collected-Signals]] to understand the raw material, then [[User-Context-Schema]] to see how user-provided context shapes inference. For technical signal acquisition details and the database schema, see the [code repository docs](https://github.com/insight-monitor/insight-monitor-code/tree/develop/docs/data-model/).

See [[MOC-Data-Model]] for the full section.

## How We Make Sense of the Data

Raw signals mean nothing without inference. The inference framework defines how we turn screen captures and event logs into probabilistic estimates of intent, purpose, and efficiency.

Technical inference documentation (prompt architecture, confidence model, context injection, customization boundaries) has been migrated to the [code repository](https://github.com/insight-monitor/insight-monitor-code/tree/develop/docs/inference/).

## What This Looks Like in Practice

Use cases make the abstract concrete. They show how the system behaves across different domains and why contextual classification matters.
![[MOC-Use-Cases]]

## How We Build It

The architecture section covers the system design, tech stack, and evolution plan.

> **Note:** Architecture documentation has migrated to the [code repository](https://github.com/insight-monitor/insight-monitor-code/tree/develop/docs/architecture/).

## What We Are Building Right Now

Scope defines the boundaries of the system — what is included, what is excluded, and who it is for.

![[MOC-Scope]]

## What Could Go Wrong

Every design decision carries risk. These are documented honestly so they can be monitored and mitigated.

![[MOC-Risks]]

## What We Do Not Do (Yet)

Limitations are not failures — they are known boundaries. Understanding them prevents misapplication.

![[MOC-Limitations]]

## Navigating the Vault

For the complete map of content, see [[INDEX.md]]. Each folder has an MOC (Map of Content) that lists every file in that section.

To understand how this vault itself is organized and maintained, start with [[MOC-Meta]].
