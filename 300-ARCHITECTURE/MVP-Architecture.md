---
title: MVP Architecture
type: reference
domain: architecture
priority: critical
ai-context: high
status: draft
audience: all
version: 1.0.0
tags:
  - mvp
  - architecture
aliases:
  - MVP Architecture
creation-date: 2026-06-15
last-reviewed: 2026-06-15
---

# MVP Architecture

## High-Level Data Flow

```
┌─────────────────────────────────────────────────────┐
│  Layer 1 — Capture Agent (Python)                   │
│  Runs on user's machine (Linux/Windows)             │
│  Polls OS APIs: window title, screenshots, input    │
│  Produces RawEvents → POST to local FastAPI         │
└───────────────────────┬─────────────────────────────┘
                        │ HTTP (localhost:8000)
┌───────────────────────▼─────────────────────────────┐
│  Layer 2 — Backend API (FastAPI + SQLite)           │
│  Receives RawEvents, stores in SQLite               │
│  SessionBuilder groups events into sessions         │
│  On session close: builds SessionContext            │
│  Calls Gemini API for intent inference              │
└──────────────┬──────────────────────┬───────────────┘
               │                     │
               ▼                     ▼
        SQLite DB              Gemini 2.0 Flash
    (raw_events,               (multimodal:
     sessions,                  screenshots +
     intent_records)            text context)
               │
               │ REST API
┌──────────────▼──────────────────────────────────────┐
│  Layer 3 — Dashboard (React + Vite + TailwindCSS)   │
│  Fetches sessions and intent records via REST       │
│  Displays timeline, classifications, screenshots    │
└─────────────────────────────────────────────────────┘
```

## Technology Choices

| Layer | Technology | Justification |
|---|---|---|
| Capture agent | Python 3.11+ | Best ecosystem for OS-level hooks (`xdotool`, `pynput`, `mss`) |
| Backend API | FastAPI + Uvicorn | Fast to build, Pydantic integration, automatic OpenAPI docs |
| Database | SQLite | Zero-config, file-based, sufficient for single-user MVP |
| LLM | Gemini 2.0 Flash | Native multimodal, cheapest API, 1M token context |
| Frontend | React 18 + Vite + TypeScript | Fast dev experience, strong typing |
| Styling | TailwindCSS | Rapid UI development without CSS overhead |
| Project management | Poetry | Dependency management + virtualenv in one tool |

## Platform Support

| Platform | MVP Support | Status |
|---|---|---|
| Linux (Ubuntu 20.04/22.04) | **Primary target** | Full support with `xdotool`, `xprop`, `wmctrl` |
| Windows 10/11 | Secondary target | `pywin32` as conditional fallback |
| macOS | Not supported | Excluded from MVP scope |

## Key Design Decisions

### Why SQLite over LanceDB/PostgreSQL
SQLite requires no server process, no Docker container, zero configuration. For a single-user MVP where sessions number in the hundreds, SQLite is faster and simpler. Vector search features are unnecessary until the user needs to query historical sessions by natural language.

### Why synchronous inference instead of Celery/Redis
The inference pipeline runs once per session close, not continuously. Synchronous execution keeps the architecture simple and removes two infrastructure dependencies (Redis + Celery worker). If inference latency becomes a problem, a thread pool is sufficient before introducing a full task queue.

### Why Gemini over Claude
Gemini 2.0 Flash provides native multimodal support (screenshots + text in one API call) at significantly lower cost than Claude or GPT-4o. Google's free tier ($0.15/1M input tokens) allows extensive testing during development.

### Why tab titles over full URLs
Reading Chrome's SQLite database is unreliable on Linux (locked files, Snap/Flatpak isolation). The window title from `xdotool` provides page-level context (e.g., "React Hooks Tutorial - freeCodeCamp") which is sufficient for the LLM to infer intent. A browser extension for full URL capture can be added post-MVP.

## Directory Structure

```
insight-monitor/
├── capture/                    # Layer 1 — Capture agent
│   ├── __init__.py
│   ├── agent.py                # Main loop: poll OS APIs
│   ├── window_tracker.py       # xdotool/pywin32 wrapper
│   ├── screenshot_capture.py   # mss wrapper
│   ├── input_monitor.py        # pynput frequency capture
│   └── event_sender.py         # POST RawEvents to API
│
├── backend/                    # Layer 2 — API + Storage
│   ├── __init__.py
│   ├── main.py                 # FastAPI app entry point
│   ├── models/                 # Pydantic schemas
│   │   ├── raw_event.py
│   │   ├── session_context.py
│   │   └── intent_record.py
│   ├── storage/
│   │   ├── database.py         # SQLite connection
│   │   └── repositories.py     # CRUD for events/sessions
│   ├── pipeline/
│   │   ├── session_builder.py  # Group events into sessions
│   │   ├── prompt_builder.py   # Build Gemini prompt
│   │   └── intent_parser.py    # Parse Gemini response
│   ├── services/
│   │   └── llm_service.py      # Gemini API client
│   └── routes/
│       ├── events.py           # POST /events
│       ├── sessions.py         # GET /sessions, /sessions/{id}
│       └── health.py           # GET /health
│
├── dashboard/                  # Layer 3 — Frontend
│   └── src/
│       ├── App.tsx
│       ├── api/                # API client
│       ├── components/         # Reusable UI
│       └── views/              # Session list, detail, status
│
├── scripts/
│   ├── simulate_session.py     # Generate test data
│   └── seed_db.py              # Populate sample sessions
│
├── pyproject.toml
├── .env.example
└── README.md
```
