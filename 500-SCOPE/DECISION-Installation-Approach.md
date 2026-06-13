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

---

## Option 1: GUI Application

### How it works
- Pre-built installer for Windows, macOS, Linux
- First-launch wizard guides user through configuration
- Settings panel for ongoing adjustments
- Visual feedback for signal collection status

### Pros
- No technical skills required
- Guided setup reduces misconfiguration
- Immediate feedback on signal collection
- Automatic updates for safety rules and model improvements
- Centralized policy management for enterprise

### Cons
- Larger distribution with bundled dependencies
- Slower iteration cycle (packaging, testing, distribution)
- Platform-specific builds required
- Less transparency into configuration internals

### Risks
- Installer supply chain security (signed builds, update integrity)
- Security updates must be pushed centrally — users on old versions are exposed
- GUI introduces UI/UX bugs independent of core logic
- Requires platform-specific testing (Windows, macOS, Linux)

### Resources and costs
- UI/UX design and development
- Installer build pipeline per OS (Inno Setup, DMG, AppImage/Flatpak)
- CI/CD infrastructure for automated releases
- User support and documentation for non-technical users
- Crash reporting and telemetry infrastructure

### Fit by use case

| Use case | Fit | Reason |
|---|---|---|
| RIWI (Coders) | **Essential** | Coders are not developers. Need 1-click install. |
| Enterprise | **Required** | No GUI = no corporate adoption. |
| Personal Productivity | **High priority** | Low friction drives user adoption. |
| Software Development | **Preferred** | Devs could clone but prefer GUI day-to-day. |
| Customer Support | **Essential** | Non-technical profile, cannot expect terminal use. |
| Knowledge Work | **Essential** | Same as support — non-technical users. |
| Compliance | **Required** | IT teams need centralized deployment. |

---

## Option 2: Clone from GitHub (Local Setup)

### How it works
- User clones the repository
- Installs dependencies manually (npm, pip, etc.)
- Edits configuration file (YAML/JSON) by hand
- Runs from terminal

### Pros
- Full transparency — every config file is visible
- Maximum flexibility — any setting can be changed
- Version-controlled configurations
- No installer dependency
- Faster for contributors

### Cons
- Requires Git, terminal, and config file knowledge
- High onboarding friction (10+ steps)
- Manual configuration errors (typos, invalid YAML, missing fields)
- No guided setup — user must read documentation
- No automatic updates — must git pull and manually maintain
- Environment-specific bugs per user machine

### Risks
- Unsustainable support burden — every environment is different
- Users abandon during setup — high drop-off before first value
- Configuration drift — different users on different versions
- Security patches not applied promptly
- Inconsistent data collection across installations

### Resources and costs
- Extensive documentation maintenance
- Issue triage for environment-specific problems
- Remote debugging per user setup
- Version compatibility matrices

### Fit by use case

| Use case | Fit | Reason |
|---|---|---|
| RIWI (Coders) | **Not viable** | 100% of Coders would abandon before starting. |
| Enterprise | **Not viable** | IT teams will not clone repos per machine. |
| Personal Productivity | **Not viable** | Only viable for technically inclined early adopters. |
| Software Development | **Possible but suboptimal** | Even devs prefer GUI for daily monitoring. |
| Customer Support | **Not viable** | Non-technical profile. |
| Knowledge Work | **Not viable** | Same as support. |
| Compliance | **Not viable** | No centralized management, no audit consistency. |

---

## Hybrid Approach (Recommended)

GUI as the primary interface for all standard users, with an option to
export and import configuration as a YAML/JSON file for advanced users.

### Benefits
- Captures both audiences with one codebase
- Advanced users can version-control their config
- GUI remains the happy path; clone is an optional power user feature
- Support team handles only GUI issues; config file issues are self-service

## Verdict
A GUI application is mandatory for any serious adoption across the documented
use cases. The clone approach is reserved for contributors who want to inspect
internals or contribute code, and should never be the primary distribution
method for end users.
