---
title: First Principles
type: truth
domain: core-concept
priority: critical
ai-context: high
status: review
audience: all
version: 1.0.0
tags:
  - first-principles
  - axioms
aliases:
  - First Principles
creation-date: 2026-06-14
last-reviewed: 2026-06-14
---
# First Principles

These are the non-negotiable conceptual axioms that govern all system design decisions. They describe what we believe about the problem domain, not how we behave (see [[Core-principles]] for behavioral rules).

## 1. Observation and inference are distinct
Raw signals are facts about system state. Inferences are probabilistic estimates drawn from those facts. The system must never conflate what it observes with what it concludes. Every inference must be decomposable into the observations that produced it.

## 2. All behavioral classification is probabilistic
There is no ground truth for human intent. The system produces estimates, not verdicts. Every output must carry a confidence score that reflects the system's certainty, and that score must be communicable to both humans and downstream systems.

## 3. No single signal determines meaning
Context is compositional. A browser URL, a screenshot, a window title, and an interaction pattern — each is weak in isolation. Meaning emerges from the combination and temporal sequence of signals. The system must not make inferences from impoverished signal sets.

## 4. Uncertainty is intrinsic, not a bug
Partial observability is a property of the problem. Cognitive work happening offline, on other devices, or in analog form is invisible by nature. The system must surface what it does not know with equal clarity to what it claims.

## 5. Every inference has an evidence trace
All conclusions must be traceable to the specific signals, user context entries, and model parameters that produced them. This trace is required for challengeability, audit, and model improvement. An inference without a trace is not a valid output.

## 6. Human judgment is the reference class
The system augments human assessment with structured evidence. It does not replace managers, reviewers, or auditors. Inferences are inputs to decisions, not decisions themselves. The system's value is providing evidence that humans would not have access to otherwise.
