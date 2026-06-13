---
type: decision
domain: architecture
priority: high
status: raw
audience: product
---

# Decision: Installation and Configuration Approach

## Context
The application requires users to configure context (role, objectives, schedule,
app classifications, sensitivity rules) before meaningful monitoring can begin.
Two approaches are available: a GUI-based application with built-in configuration,
or cloning the repository and running locally with manual configuration.

## Option 1: GUI Application (recommended)

### How it works
- Pre-built installer for Windows, macOS, Linux
- First-launch wizard guides user through configuration
- Settings panel for ongoing adjustments
- Visual feedback for signal collection status

### Pros
- **No technical skills required** — anyone can install and configure
- **Guided setup** — wizard reduces misconfiguration
- **Immediate feedback** — user sees signals being collected in real time
- **Automatic updates** — safety rules, model improvements delivered seamlessly
- **Support for non-technical users** — essential for enterprise adoption
- **Centralized policy management** (enterprise) — admins push baseline configs

### Cons
- **Larger distribution** — installer size, dependencies bundled
- **Slower iteration** — releases need packaging, testing, distribution
- **Less transparency** — user cannot inspect the exact configuration files
- **Platform-specific builds** — separate builds for each OS

## Option 2: Clone from GitHub + Local Setup (not recommended)

### How it works
- User clones the repository
- Installs dependencies manually (npm, pip, etc.)
- Edits configuration file (YAML/JSON) by hand
- Runs from terminal

### Pros
- **Full transparency** — user sees every configuration file
- **Maximum flexibility** — any setting can be changed
- **Faster for developers** — no installer overhead
- **Version-controlled configs** — config can be committed and tracked
- **Lightweight** — no bundled dependencies

### Cons
- **Requires technical knowledge** — Git, terminal, editing config files
- **High onboarding friction** — 10+ steps to get running
- **Manual configuration errors** — typos, invalid YAML, missing fields
- **No guided setup** — user must RTFM to configure correctly
- **No automatic updates** — user must git pull and manually update
- **Unviable for enterprise** — support burden is unsustainable
- **Difficult troubleshooting** — environment-specific issues per user

## Recommendation
**Option 1 (GUI Application)** is the recommended approach for all users.
Option 2 may serve as a developer convenience for contributors who want to
inspect internals, but should not be the primary distribution method.

A hybrid approach is also possible: GUI as the primary interface, with an
option to export/import configuration as a file for advanced users.
