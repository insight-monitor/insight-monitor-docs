---
title: Commercial Pitch — Reality-Grounded
type: concept
domain: brand
priority: high
ai-context: low
status: draft
audience: developers, technical evaluators, potential early adopters
tags:
  - pitch
  - commercial
  - presentation
aliases:
  - Reality-Grounded Pitch
creation-date: 2026-06-28
last-reviewed: 2026-06-28
version: 1.0.0
---

# Commercial Pitch — The Context Layer (Reality-Grounded)

> **Format:** Live presentation, 4 speakers, 2 minutes each (8 min total)
> **Audience:** Developers evaluating the project — may act as or report to decision-makers
> **Tone:** Direct, technically honest, no corporate language. Peer-to-peer.
> See [[Voice-Tone]] for language rules. See [[Market-Position]] for differentiation.
>
> **What this pitch describes:** [[Inference-Pipeline-v0.1]] (delivered June 29). This is a technical inference engine, not a full product. The [[Product-MVP]] with ethical safeguards (transparency, challengeability, self-insight) is tracked for post-v0.1 delivery. See [[Inference-Pipeline-v0.1-vs-MVP]] for the scope comparison.

---

## Speaker 1 — The Core Technical Problem
*2:00 | The data deficit*

As developers, we operate in two parallel worlds. The world of the terminal — code, compilers, debuggers, local environments. And the world of the ticketing system — Jira, GitHub Issues, PR descriptions, Slack threads. These worlds should inform each other. They never do.

Traditional monitoring tries to bridge them using surface signals. Application names. Process names. Window titles. If you are in Chrome you are browsing. If you are in the IDE you are coding. It is a binary guess that has not evolved in twenty years.

The problem is not bad managers or toxic culture. The problem is a data deficit. The system measures time but misses the intent that gives the time meaning. Chrome is where you read documentation, debug via DevTools, review pull requests, and sometimes watch conference talks. The same application. Radically different intents. The system cannot tell the difference because it was never designed to.

Existing tools collapse all of this into a single category because they lack the technical capacity to distinguish context. They optimized for administrative simplicity — a green check or a red flag — instead of signal fidelity. When a developer spends two hours researching a solution and the dashboard shows "two hours unproductive browser time," the data is worse than useless. It is misleading.

We think there is a better trade-off. Estimate intent from available signals instead of guessing from application names. That is the gap we set out to close.

---

## Speaker 2 — Architecture and Current Pipeline
*2:00 | How the inference engine works today*

Here is what the current system actually does.

**Capture agent.** A lightweight Python process runs on the monitored machine. It tracks the active window title and process name on every focus change, records input frequency (clicks and keystrokes per minute — no content), and captures a screenshot at a configurable interval (default 30 seconds). All raw events are persisted immediately to a local SQLite store. Browser context is limited to the tab title — we read what is on the title bar, not the full URL path. See [[Inference-Pipeline-Included#Capture-Agent]] for details.

**Session detection.** Raw events are grouped into sessions using a time-based gap detector (default 8 minutes of inactivity ends a session). No machine learning here — just a sliding window that works reliably for screen-based work.

**Inference.** Each assembled session is sent to the Gemini 2.0 Flash API (cloud-hosted, multimodal) alongside the screenshot grid and text context. The model returns a structured JSON record: estimated session type, inferred goal, friction points, and a confidence score. The current classification taxonomy is learning-oriented: skill development, applied learning, peer collaboration, ambiguous, or personal. See [[Inference-Pipeline-Included#Inference-Pipeline]].

**Output.** The result is a probabilistic statement, not a verdict. "Seventy percent confidence this interval was applied learning, thirty percent skill development." Every inference carries its confidence score. Low confidence means we surface that we do not know — in v0.1 this is recorded in the data structure; a dedicated Uncertainty UI is planned for the Product MVP.

**What this is not.** This is not a local VLM. The inference runs in the cloud. Screenshots leave the machine. We do not persist raw pixel data after inference, but the API call is remote. We do not track git branches, editor buffer state, or docker containers — those signals are scoped for future capture layers. See [[Excluded-Capabilities]].

We optimized for correctness of the inference pipeline as a technical proof. Performance optimization for running under load is ongoing.

---

## Speaker 3 — What Exists Now
*2:00 | The technical demo and what it proves*

The core engine is delivered. Here is what it does today and what it does not do yet.

**What exists.** A working end-to-end inference pipeline. A local capture agent sends events to a FastAPI backend. The pipeline assembles sessions, calls the Gemini API, and stores structured intent records. A React dashboard displays session lists and details with timeline, inferred intent, evidence screenshots, and confidence. We have simulation tools (`simulate_session.py`, `seed_db.py`) that generate realistic sessions without requiring live hardware — useful for evaluation and development. See [[Inference-Pipeline-Included#Dashboard]].

**What the demo proves.** The technical differentiator works: the same signal (YouTube, Discord, ChatGPT) is classified differently depending on surrounding context. A YouTube tab paired with VS Code and MDN docs produces "applied learning." A YouTube tab with no code context and long idle gaps produces "personal." Explicit confidence scores and evidence traces back each inference. See [[Inference-Pipeline-Definition#Key-Differentiator-Demonstrated]].

**What does not exist yet.** The ethical product layer. There is no challengeability — you cannot yet correct a misclassification and have the model learn from it. There is no transparency view — the monitored person does not yet see what the observer sees. There is no self-insight dashboard — the current dashboard is observer-only. There is no authentication, no multi-tenant data isolation, and no sharing controls. These are all tracked for the Product MVP, which is the next delivery after v0.1. See [[Inference-Pipeline-Definition#What-Is-NOT-in-v01-Deferred-to-Product-MVP]].

**Why this is still valuable to see.** The inference engine is the hard part. If the technical thesis is wrong — if contextual AI classification over binary labels does not produce better signal — then no amount of ethical wrapping fixes it. v0.1 lets us test that thesis. The Product MVP adds transparency, challengeability, and individual benefit on top of the same engine.

---

## Speaker 4 — The Vision and What We Need Next
*2:00 | From inference engine to ethical product*

The pipeline works. The product does not exist yet. Here is what comes next.

**Product MVP.** The ethical safeguards from [[Core-principles]] will be implemented as software features, not policy documents. Challengeability — dispute any inference with one click; the feedback retrains the prompt. Transparency view — the monitored person sees all raw events, derived sessions, and evidence traces. Self-insight dashboard — focus patterns, task energy, friction trends over time. Proportionality controls — each data point justifies its inference need. Uncertainty UI — low confidence is surfaced as a feature, not hidden. See [[MVP-Definition]].

**Integrations.** Once the ethical layer exists, the pipeline can feed context to Jira, GitHub, and Linear. A task detected as complete could trigger a draft stand-up summary. A blocked state could prompt context-aware ticket updates. This is the long-term vision, not the current roadmap — integrations are pre-product-market-fit and depend on early adopter feedback.

**Why the cloud API.** We chose Gemini 2.0 Flash for speed of iteration. A local VLM would give full privacy but slower development and higher hardware requirements. The architecture is designed so the inference backend can be swapped — the pipeline treats the model as a pluggable component. If local models reach sufficient quality, routing is a configuration change, not a rewrite.

**What we need.** Early adopters who understand this is an early-stage technical engine. We need people willing to run the capture agent, evaluate the classification quality, and tell us which Product MVP features matter most. The inference engine is real. The product is what we build together next.

We are selling the infrastructure to make developer intent visible — estimated, probabilistic, and transparent. The product around it is still being shaped. Talk to us if you want to help shape it.
