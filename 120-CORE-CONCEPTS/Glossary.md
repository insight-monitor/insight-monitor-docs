---
title: Glossary
type: reference
domain: core-concept
priority: critical
ai-context: high
status: review
audience: all
version: 1.0.0
tags:
  - glossary
  - definitions
aliases:
  - Glossary
  - Core Definitions
creation-date: 2026-06-14
last-reviewed: 2026-06-14
---
# Glossary

## Intent
An **estimate** of the probable task category the user is pursuing, derived from multimodal signals. Never a claim about the user's actual mental state. Expressed as a hypothesis (e.g., "research," "creation," "communication," "review") with an associated confidence score.

## Purpose
An **inferred work relevance label** assigned to an activity segment based on intent estimates, user-provided context, and historical baselines. Not a judgment of the activity's objective value.

## Efficiency
**Relative measure** of task progression compared to the user's own historical patterns for similar activities. Not an absolute benchmark. Requires sufficient historical data to establish a baseline. Explicitly undefined when no baseline exists.

## Success / Completion
**Probable completion markers** inferred from observable progression signals (document saves, navigation patterns, state changes). The system does not confirm whether an objective was met — only that the evidence is consistent with completion.

## Observation
**Raw signal capture** — the direct recording of system-level data points (window titles, process names, URLs, timestamps, screenshot pixels). Observations are facts about the system state, not interpretations.

## Inference
A **probabilistic conclusion** drawn from one or more observations. Inferences carry a confidence score and can be traced back to the specific signals that produced them. Every inference is subject to revision when new evidence arrives.

## Confidence
A **numerical score (0.0–1.0)** representing the system's certainty in a given inference. Based on signal quality, completeness, consistency with context, and model calibration. Scores below a configurable threshold are flagged as uncertain rather than presented as fact.

## Context
The **set of signals and user-provided information** that shapes inference at a given moment. Includes system state, recent activity history, user-defined role, objectives, schedule, application classifications, and sensitivity rules.

## Signal
Any **observable data point** from the monitored endpoint. Signals are the atomic unit of input to the inference pipeline. Every signal has a type (metadata, interaction, content, visual, network), a source (OS API, accessibility API, browser hook, screenshot), and a timestamp.

## Behavioral intelligence
The **structured output** of the inference pipeline: per-activity intent hypotheses with confidence scores, task chain reconstructions, efficiency estimates relative to baselines, and evidence-traceable classifications. This is the product — not raw data.
