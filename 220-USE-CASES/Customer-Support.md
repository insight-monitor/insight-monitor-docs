---
title: "Use Case: Customer Support"
type: concept
domain: use-cases
priority: medium
ai-context: medium
status: draft
audience: all
version: 0.1.0
tags:
  - use-case
  - customer-support
aliases:
  - Use Case: Customer Support
---
# Use Case: Customer Support

## Observable patterns
- CRM/ticketing system as primary interface
- Email and chat tools for client communication
- Knowledge base and documentation lookup
- Call handling (phone/softphone) with screen idle periods

## What the system can infer
- Active ticket resolution (switching between ticket, KB, email reply)
- Research phase (searching KB for answers during a call)
- After-call work (documenting the interaction)
- Idle between tickets (waiting for queue)

## Key consideration
Calls produce significant offline activity. The system must not penalize the agent for being on a call with low screen interaction. Integration with telephony or calendar data improves accuracy here.