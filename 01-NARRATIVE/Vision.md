---
type: concept
domain: narrative
priority: critical
status: raw
audience: all
version: 2.0.0
tags:
  - vision
  - intent-estimation
  - behavioral-intelligence
  - context-aware
  - b2b
---

# Vision

Insight Monitor is a behavioral intelligence platform. It collects computer activity signals and applies multi-modal AI to estimate probable user intent, task context, and efficiency — producing structured data that powers both integrated applications and third-party pipelines.

## The Core Insight

Existing monitoring tools classify activity by surface signals alone: an application name or domain is labeled "productive" or "unproductive" with no context. This is fundamentally brittle — YouTube can be research, distraction, work, or background audio depending on what the user is trying to do.

**Context is the missing dimension.** The value is not in what application is open, but in what the user is trying to achieve.

## What We Build

A system that replaces static classification with probabilistic, evidence-based inference. It draws on multimodal signals captured via OS-level accessibility APIs and application-level integrations:

- **System signals** — active windows, application metadata, focus transitions
- **Content signals** — visible text (OCR, accessibility APIs), document context, URLs
- **Interaction signals** — input patterns, navigation, application-specific actions
- **Temporal signals** — session duration, task continuity, interruption patterns

And produces structured behavioral intelligence:

- Per-activity intent hypotheses with [[05-INFERENCE-FRAMEWORK/Confidence-Model|confidence scoring]]
- Task chain reconstruction and context-switch detection
- Efficiency estimates relative to [[03-CORE-CONCEPT/Glossary|defined baselines]]
- Evidence-traceable distraction and focus classification
- Detection of low-interaction periods that may indicate offline cognitive work, meetings, or idle time — flagged with explicit uncertainty

## What We Do Not Claim

We estimate probable intent — we do not read minds. We detect likely progression markers — we do not measure success absolutely. We flag low-interaction periods — we do not assume idleness. Every inference carries confidence metadata and can be challenged.

## Customers

**Primary: organizations** (B2B) — team leads, project managers, compliance officers who need nuanced visibility beyond binary productivity scores.
**Secondary: individuals** (B2C) — professionals tracking their own productivity with detailed behavioral analytics.

## Data Approach

- **Collection**: comprehensive across observable signal layers. Each signal type is justified against a specific inference need — not collected indiscriminately. [[04-DATA-MODEL/Data-Taxonomy]]
- **Processing**: [[06-PRIVACY/Data-Processing|hybrid architecture]]. Sensitive redaction and local inference run on-device. Aggregated, sanitized outputs may sync to cloud for cross-user analysis and model improvement.
- **Transparency**: collection scope is explicit, configurable, and disclosed to the user. The system does not capture what it cannot justify.

## Product Model

Two co-equal surfaces:

1. **The Inference Pipeline** — the core engine: collect, process, output structured behavioral data. Available as a managed API service.
2. **The Application Layer** — dashboards, team views, integrations (project management, HR tools), alerting. Built on the pipeline.

Both are first-class products. The pipeline is designed to be embedded into external workflows as a data source.

## Scope Acknowledgement

The system is limited to what the monitored device can observe. Multi-device work, analog tools (notebooks, whiteboards), meetings, conversations, and purely cognitive activity are partially visible at best. [[11-LIMITATIONS/Observability-Gaps]] details these constraints.
