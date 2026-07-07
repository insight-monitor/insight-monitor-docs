---
title: Inference Pipeline v0.1 Included Capabilities
type: reference
domain: scope
priority: critical
ai-context: high
status: accepted
audience: all
version: 3.0.0
tags:
  - inference-pipeline
  - capabilities
aliases:
  - Pipeline v0.1 Included
  - MVP-Included (legacy alias)
creation-date: 2026-06-15
last-reviewed: 2026-06-24
parent:
  - "[[500-SCOPE]]"
---

# Inference Pipeline v0.1 Included Capabilities

> **Note**: The technical inclusion list remains unchanged. The re-scoping only changes terminology: the 14-day sprint delivers **Inference Pipeline v0.1** — the contextual inference engine. The full Product MVP adds challengeability, transparency views, self-insight dashboard, and proportionality controls (see [[Inference-Pipeline-Definition]]).

## Capture Agent (Python 3.11+)

| Capability | Implementation | Notes |
|---|---|---|
| Active window tracking | `xdotool` / `xprop` (Linux), `pywin32` (Windows) | Window title + process name captured on focus change |
| Periodic screenshots | `mss` library | Configurable interval, default 30s |
| Input frequency | `pynput` | Clicks/min, keystrokes/min — no content |
| URL context | `xdotool getwindowname` (tab title fallback) | Tab title only, not full URL path |
| Raw event storage | SQLite via `sqlite3` | Every event persisted immediately |

## Inference Pipeline

| Capability | Implementation | Notes |
|---|---|---|
| Session detection | Time-based + gap detection (default 8 min inactivity) | Simple, no ML required |
| Context building | `SessionContext` aggregates RawEvents into session | App sequence, duration, screenshot grid |
| LLM inference | Gemini 2.0 Flash API | Multimodal: screenshots + text context |
| Intent record | Structured JSON: session_type, goal, friction_points, confidence | Validated with Pydantic |
| Riwi-aware classification | Custom prompt mode for learning context | Categories: skill_development, applied_learning, peer_collaboration, ambiguous, personal |

## API (FastAPI)

| Endpoint | Purpose |
|---|---|
| `POST /events` | Ingest RawEvents from capture agent |
| `GET /sessions` | List sessions with inferred intent |
| `GET /sessions/{id}` | Session detail with timeline and screenshots |
| `GET /health` | Agent and API status |

## Dashboard (React + Vite + TailwindCSS)

| View | Purpose |
|---|---|
| Session list | Table of sessions with type, duration, confidence |
| Session detail | Timeline view, inferred intent, friction points, screenshot thumbnails |
| Agent status | Indicator showing capture agent online/offline |

## Dev Tools

| Tool | Purpose |
|---|---|
| `simulate_session.py` | Generate realistic BPO and Riwi sessions without live hardware |
| `seed_db.py` | Populate SQLite with sample sessions for dashboard development |
| `.env.example` | All configuration with defaults documented |
