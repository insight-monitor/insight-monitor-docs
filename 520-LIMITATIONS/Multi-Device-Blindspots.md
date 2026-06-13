---
status: raw
---
# Multi-Device Blindspots

The system monitors a single endpoint. Modern work spans multiple devices:

- Reading on one monitor, typing on another
- Phone used for 2FA, research, or messaging
- Meetings joined on a separate device
- VDI or remote desktop sessions
- Private browser profiles

## Risk
Activity fragments across devices. The system sees only part of the user's workflow, which can lead to incomplete or misleading inferences about intent and productivity.

## Mitigation
- Acknowledge the blindspot in all reports
- Never claim full activity visibility
- Consider device-agnostic signals (calendar, VPN auth logs) as auxiliary context