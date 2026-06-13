---
title: "Use Case: Software Development"
type: concept
domain: use-cases
priority: medium
ai-context: medium
status: review
audience: all
version: 0.1.0
aliases:
  - Use Case Software Development
tags:
  - use-case
  - software-development
creation-date: 2026-06-13
last-reviewed: 2026-06-13
---
# Use Case: Software Development

## Observable patterns
- Code editors, terminals, debuggers as primary tools
- Browser usage: documentation, Stack Overflow, PR reviews, CI/CD dashboards
- Frequent context switching between files, branches, services
- Local builds, tests, and deployments

## What the system can infer
- Active coding phases (high editor + terminal activity)
- Research phases (editor + documentation browser)
- Code review (PR tool open, diff navigation)
- Blocked state (repeated same search, low activity with frustration signals)

## Limitations specific to this domain
- Prototyping vs productive coding look similar
- Debugging a complex issue may look like spinning wheels
- Writing design docs in a browser may look like non-coding idle time