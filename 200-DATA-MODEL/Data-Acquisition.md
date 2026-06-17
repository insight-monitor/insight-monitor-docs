---
title: Data Acquisition
type: reference
domain: data-model
priority: high
ai-context: high
status: review
audience: all
tags:
  - acquisition
  - signals
aliases:
  - Data Acquisition
creation-date: 2026-06-14
last-reviewed: 2026-06-17
version: 1.0.0
---
# Data Acquisition

## Acquisition mechanisms

### OS-level accessibility APIs
Captures active window information, application metadata, and focus events. Available on Windows (UI Automation), macOS (Accessibility API), and Linux (AT-SPI/DBus). Does not require kernel-level access. Subject to OS permission prompts on first use.

### Application-level integrations
Optional plugins or extensions for specific applications (browser extensions, IDE plugins). Provide richer signal context than OS APIs alone — document titles, git actions, compiler output. Not required for core functionality.

### Screenshot capture
Periodic screen capture at configurable intervals (minimum 30 seconds). Captured frames are processed by a vision-language model on-device for text extraction, UI element classification, and region segmentation. Raw frames are not persisted.

### Process metadata
Application name, process ID, window title, foreground/background state. Collected via OS process enumeration APIs. Does not include process memory, stack traces, or file handles.

### Browser URL monitoring
Active tab domain and page title from browser extension or accessibility tree. Page content is not captured except what appears in screenshot regions. Full DOM is never collected.

### Network connection monitoring
Active connection domains and classification (API, CDN, streaming, collaboration). Inspected at the connection metadata level only. Packet contents are never captured.

## What is NOT acquired
- Keystroke content (individual key presses)
- Clipboard contents
- Microphone or camera input
- File system contents or traversal
- Private browser profiles or incognito windows
- Application memory or internal state
- Credential fields or password manager contents
- Personal communication content without explicit user opt-in

## Legal and invasiveness considerations
| Mechanism | Invasiveness | Legal consideration |
|-----------|-------------|--------------------|
| OS accessibility APIs | Medium | Requires OS-level permission grant on first use. Transparent to user. |
| Browser extension | Low to medium | Limited to tab metadata. User can disable at any time. |
| Screenshots | High | Most invasive. Requires explicit justification per capture. Never persisted raw. |
| Process metadata | Low | Application-level information only. No user content. |
| Network metadata | Low | Domain-level only. No deep packet inspection. |
