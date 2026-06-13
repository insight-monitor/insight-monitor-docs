---
title: "Use Case: Compliance Monitoring"
type: concept
domain: use-cases
priority: medium
ai-context: medium
status: draft
audience: all
version: 0.1.0
tags:
  - use-case
  - compliance
  - audit
aliases:
  - Use Case: Compliance Monitoring
---
# Use Case: Compliance Monitoring

## Scenario
Regulated industries (finance, healthcare) require activity logging for audits, not productivity tracking.

## What the system provides
- Evidence logs of who accessed what and when
- Detection of sensitive data exposure (PII, financial data on screen)
- Audit-ready reports with timestamps and screenshot evidence
- Policy violation alerts (accessing unauthorized platforms with sensitive data)
- Automated redaction of protected content in logs

## Key difference from productivity use
Compliance monitoring prioritizes accuracy of evidence recording, not inference quality. The system acts as a witness, not a judge.