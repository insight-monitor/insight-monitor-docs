---
title: Stance on Surveillance
type: concept
domain: narrative
priority: critical
ai-context: high
status: review
audience: all
version: 1.0.1
tags:
  - ethics
  - surveillance
  - philosophy
aliases:
  - How do we surveil
  - our surveillance stance
creation-date: 2026-06-12
last-reviewed: 2026-06-12
---

# Stance on Surveillance

As established in [[Founding-Story|Founding Story]]: Insight Monitor is a monitoring tool. It collects data from computer activity. It processes that data into inferences about behavior. Calling it anything else would be marketing, not philosophy.

The relevant question is not "is this surveillance?" but "what kind of monitoring is this, who does it serve, and what are its rules?"

## Observation vs. Control

Traditional workplace monitoring is fundamentally a control mechanism. Its purpose is to detect deviation, enforce compliance, and produce scores that can be used punitively. The monitored person has no say, no visibility into the conclusions, and no way to challenge them.

Insight Monitor is designed as an observation mechanism. Its purpose is to surface context, understand patterns, and produce probabilistic estimates that remain challengeable. The monitored person sees the same data. The conclusions are uncertain by design.

The difference is not in the signals collected. It is in the intent, the transparency, and the distribution of power.

## What Makes This Different

- **Conclusions are probabilistic, not absolute.** Every inference carries an uncertainty estimate. The system knows when it does not know.
- **Every conclusion can be challenged.** Evidence traces link each inference to the signals that produced it. A worker can say "I was researching, not distracted" and the system can evaluate whether the signals support that reframe.
- **The monitored person sees the same data.** There is no hidden score. No secret dashboard. What the organization sees, the individual sees.
- **Individual benefit is a requirement, not an afterthought.** If the monitored person gains nothing from the system, the system fails — regardless of what the organization gains.

## Considerations Across Deployment Contexts

The ethical stakes differ depending on who deploys the system and who is monitored.

**Corporate.** The power asymmetry between employer and employee is real. Insight Monitor cannot eliminate this asymmetry, but it can mitigate it through radical transparency: the worker sees all inferences, can flag false positives, and the tool generates evidence on their behalf, not against them. Deployment must include opt-in mechanisms appropriate to the jurisdiction and context.

**Educational.** The power asymmetry between educator and student is different but equally real. Here the purpose must be developmental, not evaluative. The student's patterns serve their own learning insight first; institutional visibility is secondary and must be justified per use case. Data about students serves the student.

**Personal.** There is no external observer. The individual has full control over collection, retention, and inference. The ethical obligation shifts to design: the tool must not manipulate, must not infer beyond what it signals, and must make its uncertainty visible.

These positions are grounded in the project's [[Core-principles|Core Principles]].
