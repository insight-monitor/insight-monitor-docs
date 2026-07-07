---
title: Inference Pipeline v0.1 Excluded Capabilities
type: reference
domain: scope
priority: critical
ai-context: high
status: accepted
audience: all
version: 2.1.0
tags:
  - inference-pipeline
  - boundaries
aliases:
  - Pipeline v0.1 Excluded
  - MVP-Excluded (legacy alias)
creation-date: 2026-06-15
last-reviewed: 2026-06-24
parent:
  - "[[500-SCOPE]]"
---

# Inference Pipeline v0.1 Excluded Capabilities

All items below are excluded from the **Inference Pipeline v0.1** (14-day sprint, June 15–29) but noted with scaling intent toward the full **Product MVP**.

> **Note**: The technical exclusion list remains unchanged from the prior MVP scope. The re-scoping only changes terminology: "MVP" → "Inference Pipeline v0.1" for this sprint's deliverable. The full Product MVP adds challengeability, transparency, and individual benefit features (see [[Inference-Pipeline-Definition]]).

## Storage & Infrastructure

| Feature | Why Excluded | Scaling Path |
|---|---|---|
| LanceDB / Vector search | SQLite + LIKE queries sufficient for MVP session count | Add LanceDB when sessions exceed 10k or semantic search is required |
| Redis / Celery | Pipeline is synchronous — no need for queues | Add when inference latency becomes a problem or report generation needs background workers |
| Alembic migrations | SQLite schema is small and hand-managed | Add Alembic when schema changes become frequent or team > 4 |
| Multi-tenant isolation | Hardcoded single tenant (Riwi) | Add tenant_id column, middleware, and isolation layer post-MVP |

## Capture Agent

| Feature | Why Excluded | Scaling Path |
|---|---|---|
| Native Wayland support (Ubuntu 22+) | `ydotool` fallback only; full Wayland needs portal APIs | Add when Wayland becomes default on target distros |
| Browser extension for full URLs | Tab title via `xdotool` provides sufficient context | Build Chrome/Firefox extension when URL precision is critical |
| Clipboard metadata capture | Not needed for MVP demo use cases | Add metadata-only capture (no content) post-MVP |
| Network connection capture | Out of scope for first demo | Add DNS/connection monitoring post-MVP |
| Multi-monitor support | Monitor only primary display | Add when multi-monitor usage data is available |

## Inference Pipeline

| Feature | Why Excluded | Scaling Path |
|---|---|---|
| Dynamic session type catalog | Hardcoded session types for MVP | Build catalog from aggregated IntentRecords when > 100 sessions exist |
| Anomaly detection | Requires baseline data (not available in MVP) | Implement rule-based + statistical anomaly after 2+ weeks of data |
| Natural language query | No vector DB in MVP | Add query endpoint when LanceDB is integrated |
| Report generation | No report schema defined yet | Add Celery-based report worker post-MVP |
| User contestability UI | No user authentication in MVP | Add "correct this" button + feedback loop post-MVP |

## Dashboard

| Feature | Why Excluded | Scaling Path |
|---|---|---|
| WebSockets / real-time push | HTTP polling every 5s is sufficient for demo | Add WebSocket when sub-second updates are needed |
| User authentication | Hardcoded demo mode | Add JWT + roles (admin, supervisor) post-MVP |
| Framer Motion / animations | Not critical for functional demo | Add when UI polish is prioritized |
| Supervisor notification system | No push infrastructure | Add email/webhook alerts post-MVP |

## Platform & Deployment

| Feature | Why Excluded | Scaling Path |
|---|---|---|
| macOS capture agent | Linux + Windows covers 95% of use cases | Add macOS when Apple Silicon BPO adoption justifies it |
| GUI installer (MSI/DEB) | CLI + `poetry run` for MVP | Package with PyInstaller + Inno Setup / fpm post-MVP |
| Cloud deployment | Everything runs locally for MVP | Add Docker Compose + cloud deployment post-MVP |
| Centralized management server | Each instance is standalone | Build admin console for fleet management post-MVP |
