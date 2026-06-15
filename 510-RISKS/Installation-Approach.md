---
title: Installation Approach Risks
type: concept
domain: risks
priority: medium
ai-context: medium
status: review
audience: all
version: 1.0.0
tags:
  - installation
  - supply-chain
  - support
aliases:
  - Installation Approach Risks
creation-date: 2026-06-15
last-reviewed: 2026-06-15
---
# Installation Approach Risks

The installation and distribution approach introduces several risks, detailed in the [[DECISION-Installation-Approach]] analysis.

## GUI application risks
- **Supply chain security** — Installer must be signed and updates must maintain integrity
- **Centralized update dependency** — Users on old versions remain exposed to known issues; security patches require central push
- **UI/UX bugs** — Bugs in the interface can affect data collection or user experience independently of core logic
- **Platform-specific testing burden** — Windows, macOS, and Linux each require separate build and test pipelines

## Clone/local setup risks
- **Support burden** — Every environment differs, leading to unsustainable debugging and triage load
- **Setup abandonment** — High onboarding friction causes users to drop off before reaching first value
- **Configuration drift** — Different users run different versions with inconsistent configurations
- **Delayed security patches** — Without central update mechanism, patches are applied at the user's discretion (or not at all)
- **Data inconsistency** — Collection behavior varies per installation, compromising cross-user comparability
