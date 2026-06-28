---
title: Commercial Pitch
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
  - Commercial Pitch
  - Sales Pitch
creation-date: 2026-06-28
last-reviewed: 2026-06-28
version: 3.0.0
---

# Commercial Pitch — The Context Layer

> **Format:** Live presentation, 4 speakers, 2 minutes each (8 min total)
> **Audience:** Developers evaluating the project — may act as or report to decision-makers
> **Tone:** Direct, technically honest, no corporate language. Peer-to-peer.
> See [[Voice-Tone]] for language rules. See [[Market-Position]] for differentiation.

---

## Speaker 1 — The Core Technical Problem
*2:00 | The data deficit*

As developers, we operate in two parallel worlds. The world of the terminal — code, compilers, debuggers, local environments. And the world of the ticketing system — Jira, GitHub Issues, PR descriptions, Slack threads. These worlds should inform each other. They never do.

Traditional monitoring tries to bridge them using surface signals. Application names. Process names. Window titles. If you are in Chrome you are browsing. If you are in the IDE you are coding. It is a binary guess that has not evolved in twenty years.

The problem is not bad managers or toxic culture. The problem is a data deficit. The system measures time but misses the intent that gives the time meaning. Chrome is where you read documentation, debug via DevTools, review pull requests, and sometimes watch conference talks. The same application. Radically different intents. The system cannot tell the difference because it was never designed to.

Existing tools collapse all of this into a single category because they lack the technical capacity to distinguish context. They optimized for administrative simplicity — a green check or a red flag — instead of signal fidelity. When a developer spends two hours researching a solution and the dashboard shows "two hours unproductive browser time," the data is worse than useless. It is misleading.

We think there is a better trade-off. Estimate intent from available signals instead of guessing from application names. That is the gap we set out to close.

---

## Speaker 2 — Architecture and Local Pipeline
*2:00 | How it works without melting your machine*

Here is how we do it without tanking your performance.

Insight Monitor is a local-first, modular pipeline. It does not stream screenshots to a cloud API. It does not run a vision model on every frame. That would be irresponsible engineering and we are not going to hand-wave it.

Instead we use a layered approach with three tiers. Tier one: lightweight event hooks that constantly sample text signals — window titles, OS focus events, active processes, browser URLs. These cost almost nothing. Think polling a few hundred bytes every second.

Tier two: when text signals produce ambiguous classification — for example a browser tab titled "localhost:3000" — we check against known work patterns. Recent git branch. Open editor tabs. Running docker containers. If the context fits an active task, we classify without ever touching visual data.

Tier three activates only when the first two tiers cannot reach a confidence threshold. A small vision-language model runs locally and analyzes the current frame. It triggers heuristically — not every N seconds, only when needed. The frame is processed in memory and instantly discarded. No persistent screenshots. No video stream.

The output is a probabilistic data structure: seventy percent confidence this interval was active coding, thirty percent technical research. The inference uses your GPU when available, falls back to CPU, and always yields priority to user processes.

We optimized for performance because the system runs while you work. If it slows down your compile, your docker build, or your terminal, it fails its purpose.

---

## Speaker 3 — What Exists Now
*2:00 | The first wrapper: developer self-insight*

The core engine needs a wrapper to be useful. Our first implementation is a local developer self-insight dashboard.

This is not an enterprise surveillance portal. It is a local tool that gives you telemetry on your own cognitive patterns. When does your deep focus peak? How much time do you lose bouncing between code, documentation, and Slack? What does a blocked state actually look like in your workflow — and how often does it happen?

You own every bit of data. The dashboard shows each inference with its supporting evidence: the window titles, the URLs, the confidence scores that produced the estimate. If the system misclassifies your intent — says you were distracted when you were researching a bug — you correct it with one click. That feedback improves the model locally. The system learns from you, not about you.

For a team lead, this replaces guesswork with signal. Instead of "four hours in IDE, one hour in browser" the aggregate view shows estimated engineering states: active development, code review, technical research, blocked, context switching. Leads see where friction lives, not who is or is not working.

The data serves you first. You control what gets shared. The team sees only what you authorize. This is not a tool that reports you. It is a tool that gives you evidence of your own work — evidence you can use to protect your focus time, justify your research hours, and push back on bad metrics.

This is what we have running. It is local. It respects your machine. And it treats you as the user, not the target.

---

## Speaker 4 — The Vision
*2:00 | Killing the administrative overhead*

Here is why this project matters and where we are taking it next.

Right now, monitoring and productivity live in separate universes. You finish a task. Then you manually log hours, update Jira tickets, write stand-up notes, comment on PRs explaining what you did. That overhead is pure waste — and it exists because your tools have no idea what you actually did.

Our inference engine maps your intent to engineering states in real time. The next step is to connect that context directly to the tools you already use. Not a separate dashboard — a context layer that feeds Jira, GitHub, Linear.

Imagine this: the system detects you just resolved a pull request. It maps the documentation you referenced, the files you touched, the research detour you took to unblock a dependency. It generates a stand-up summary and updates the ticket — automatically. No timesheet. No "what did you do yesterday" Slack message. No administrative drag.

Why does this need AI? That is a fair question. Text signals alone — a tab title that says "localhost:3000", a process name that says "chrome" — cannot tell you whether someone is actively debugging or passively scrolling. A vision-language model adds the visual context needed to disambiguate. That is the technical justification. Not buzzwords. A concrete signal gap that only multimodal inference can close.

We have the core engine running on local hardware. We need early adopters to help us build the wrappers that make this useful — the integrations, the dashboards, the automation triggers. If this sounds like something you would use, if you want to break it and tell us what to build next, talk to us after.

We are selling the infrastructure to make developer intent visible — to you first, and to your tools second. Thank you.
