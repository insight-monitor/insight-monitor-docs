---
type: concept
audience: all
status: draft
domain: narrative
priority: low
creation-date: 2026-06-08
---

# WorkMemory

> **Operational intelligence platform for BPO and call centers.**
> Not just what your agents did — but *why* they did it.

---

## Table of Contents

1. [What is WorkMemory?](#1-what-is-workmemory)
2. [The Problem It Solves](#2-the-problem-it-solves)
3. [Scope](#3-scope)
4. [System Architecture](#4-system-architecture)
5. [Multi-Agent System](#5-multi-agent-system)
6. [Inference Pipeline](#6-inference-pipeline)
7. [Project Structure](#7-project-structure)
8. [Tech Stack](#8-tech-stack)
9. [Error Handling](#9-error-handling)
10. [Roadmap](#10-roadmap)
11. [Security & Privacy](#11-security--privacy)
12. [Quick Start](#12-quick-start)

---

## 1. What is WorkMemory?

WorkMemory is a **human work observability platform** built for BPO operations and call centers. While traditional monitoring tools measure *how long* an agent spends in each application, WorkMemory infers the *intent* behind every action.

The system captures discrete OS-level events — window focus changes, visited URLs, keyboard and mouse patterns, opened files, clipboard metadata — and processes them through a multi-agent AI pipeline to build a **semantic, queryable work diary** available in real time to supervisors.

Instead of:
> *"Agent spent 11 minutes on this case"*

WorkMemory delivers:
> *"Agent spent 11 minutes because the CRM didn't have the client's history and they had to search across two additional systems — 4 minutes were pure friction"*

---

## 2. The Problem It Solves

Existing monitoring tools (Teramind, Hubstaff, time-on-screen trackers) measure **quantity of time**. They don't measure **why** time is spent. Supervisors can see that a case took 11 minutes but have no visibility into whether that was caused by a slow system, a missing piece of information, an untrained agent, or a fundamentally broken process.

WorkMemory answers that question automatically — without agents filling out a single form.

---

## 3. Scope

### What the MVP includes

- Local Python capture agent running silently in the background on each agent's machine
- Continuous screen reading with AI visual interpretation (Claude Vision API)
- Keyboard and mouse pattern capture as behavioral and intent signals
- Multi-agent inference pipeline with a central orchestrator
- Automatic anomaly detection with forensic screenshot capture
- Local vector database for natural language semantic search over the full history
- Real-time supervisor dashboard with advanced animations and visualizations
- On-demand operational report generation
- Dynamic session type catalog that grows with usage — no manual configuration required
- Per-tenant data isolation — each BPO client sees only their own data

### What the MVP does not include

- Continuous video recording or screenshots (only on confirmed anomalies)
- Clipboard content storage (metadata only: source app, type, character count)
- Direct integration with external CRMs (inferred from observed activity)
- Mobile app or browser extension

---

## 4. System Architecture

WorkMemory operates in four independent, decoupled layers:

```
┌──────────────────────────────────────────────────────┐
│  Layer 1 — Capture                                   │
│  Python agent on the worker's machine                │
│  Listens to OS events → produces RawEvents           │
└─────────────────────────┬────────────────────────────┘
                          │ RawEvent stream
┌─────────────────────────▼────────────────────────────┐
│  Layer 2 — Inference                                 │
│  Multi-agent pipeline + central orchestrator         │
│  Groups sessions → builds context → calls Claude API │
│  Produces IntentRecords (structured JSON)            │
└─────────────────────────┬────────────────────────────┘
                          │ IntentRecords
┌─────────────────────────▼────────────────────────────┐
│  Layer 3 — Storage                                   │
│  SQLite (raw events + relational metadata)           │
│  LanceDB (embedded IntentRecords for vector search)  │
└─────────────────────────┬────────────────────────────┘
                          │ API + WebSocket
┌─────────────────────────▼────────────────────────────┐
│  Layer 4 — Dashboard                                 │
│  React + Framer Motion                               │
│  Real-time metrics, anomaly feed, semantic search    │
└──────────────────────────────────────────────────────┘
```

---

## 5. Multi-Agent System

The central orchestrator decides in real time which specialized agent to activate based on the current context. Three agents run continuously in parallel; others are triggered by events or schedules.

| Agent | Activation | Responsibility |
|---|---|---|
| **Vision Agent** | Continuous (2–3 fps) | Screen reading via `mss` + OCR. On anomaly: full image capture with forensic SHA-256 hash + Claude Vision analysis |
| **Input Agent** | Continuous | Keyboard and mouse pattern capture: typing speed, clicks/min, focus switches. Behavioral signals — never literal content |
| **Window Agent** | Continuous | Tracks active app, focus duration, window title. Foundation for session grouping |
| **Intent Agent** | On event (session close) | Receives enriched `SessionContext`, calls Claude API with structured prompt, returns validated `IntentRecord` JSON |
| **Anomaly Agent** | On event (business rules) | Evaluates each `RawEvent` against the tenant's whitelist. On violation: screenshot + `AnomalyRecord` + WebSocket push to supervisor |
| **Report Agent** | On demand / schedule | Generates structured reports by period, agent, or session type. Compares against the dynamic benchmark catalog |
| **Query Agent** | On demand | Converts natural language query to vector, searches LanceDB, synthesizes response with file context and timestamps |

### Orchestrator logic

```python
def evaluate(context: AgentContext):
    # Anomaly takes highest priority
    if context.anomaly_score > threshold:
        activate(AnomalyAgent, priority=HIGH)
        activate(VisionAgent, mode="capture")

    # Session close triggers inference
    if context.session_close_detected:
        activate(IntentAgent, session=context.current_session)

    # Supervisor request routes to target agent
    if context.supervisor_request:
        route_to(context.request.target_agent)

    # Always running in parallel threads
    always_on = [VisionAgent, InputAgent, WindowAgent]
```

---

## 6. Inference Pipeline

### How a session start and end are detected

WorkMemory handles both explicit and inferred session boundaries:

**Explicit close signals** (high confidence):
- Confirmation URL pattern detected in browser
- Form POST submitted
- Primary application window closed
- Softphone call ended

**Inferred close signals** (medium confidence):
- Inactivity gap greater than 8 minutes
- Total context switch (no app overlap with previous activity)

The close trigger type is passed to the model so it can calibrate its confidence score accordingly.

### What the model receives

Before calling the LLM, the `SessionBuilder` constructs a `SessionContext` object:

```python
class SessionContext(BaseModel):
    session_id: str
    agent_id: str
    timestamp_start: datetime
    timestamp_end: datetime
    duration_seconds: int
    trigger_type: Literal["explicit", "inferred"]

    app_sequence: list[AppEvent]        # App + time spent, in order
    url_sequence: list[UrlEvent]        # Domains only, never full paths
    file_sequence: list[FileEvent]      # Names and extensions only
    clipboard_events: list[ClipboardMeta]  # Metadata only, never content

    input_patterns: InputSummary        # clicks/min, typing speed, focus switches
    screen_descriptions: list[str]      # Semantic descriptions from Vision Agent
    known_process_hints: list[str]      # Recognized corporate apps from tenant whitelist

    anomalies_in_session: list[AnomalyFlag]
    recent_sessions_summary: list[str]  # Last 3 sessions for historical context
```

### What the model returns

The model returns a validated `IntentRecord` JSON:

```json
{
  "session_type": "case_registration",
  "goal": "Registered new support case in CRM after verifying client invoice in SAP",
  "subprocess_detected": [
    "Opened CRM client search",
    "Copied client ID from softphone to CRM",
    "Navigated to SAP to verify pending invoice",
    "Returned to CRM to complete case form",
    "Submitted case with resolution code"
  ],
  "friction_points": [
    {
      "type": "integration_gap",
      "detail": "Client ID had to be manually copied from softphone to CRM — no auto-fill",
      "estimated_seconds_lost": 45
    },
    {
      "type": "app_switching",
      "detail": "3 switches between CRM and SAP before finding correct invoice",
      "estimated_seconds_lost": 90
    }
  ],
  "problem": "SAP invoice search required full account number; agent only had client name",
  "outcome": "resolved",
  "outcome_signals": ["CRM form submitted", "Confirmation URL detected", "Softphone idle after call"],
  "confidence": 0.91,
  "inference_notes": "High confidence — explicit session close + commit-equivalent confirmation URL",
  "tags": ["case_registration", "crm", "sap", "invoice_verification"],
  "optimization_hint": "Auto-populate CRM client ID from softphone caller data to eliminate manual copy step"
}
```

### How it maps to the business model

Since WorkMemory doesn't know processes in advance, it builds a **dynamic tenant catalog** that grows with every inferred session:

```python
TENANT_CATALOG = {
    "tenant_id": "bpo_client_x",
    "session_types": {
        "case_registration": {
            "count": 847,
            "avg_duration_seconds": 420,
            "avg_friction_ratio": 0.23,
            "common_apps": ["Salesforce", "SAP", "Chrome"],
            "most_frequent_friction": "integration_gap between SAP and CRM",
            "benchmark_good": 300,   # p25 duration in seconds
            "benchmark_bad": 600     # p75 duration in seconds
        }
    }
}
```

This catalog becomes the business benchmark. A `case_registration` session taking 800 seconds is automatically compared against the client's own historical average — not a generic industry standard.

---

## 7. Project Structure

```
workmemory/
│
├── agents/                         # Specialized capture and inference agents
│   ├── __init__.py
│   ├── base_agent.py               # Base class — common interface for all agents
│   ├── orchestrator.py             # Central orchestrator — activates/pauses agents
│   ├── vision_agent.py             # Screen reading (mss + OCR + Claude Vision)
│   ├── input_agent.py              # Keyboard and mouse (pynput)
│   ├── window_agent.py             # Active apps and focus time
│   ├── event_agent.py              # URLs, files, clipboard, git
│   ├── intent_agent.py             # Intent inference with Claude API
│   ├── anomaly_agent.py            # Anomaly detection + forensic capture
│   ├── report_agent.py             # Structured report generation
│   └── query_agent.py              # Natural language semantic search
│
├── models/                         # Pydantic schemas — structure only, zero logic
│   ├── __init__.py
│   ├── raw_event.py
│   ├── session_context.py
│   ├── intent_record.py
│   ├── anomaly_record.py
│   ├── screen_record.py
│   ├── report_record.py
│   ├── agent_profile.py
│   └── tenant_catalog.py
│
├── controllers/                    # Business logic — never touches UI or DB directly
│   ├── __init__.py
│   ├── capture_controller.py       # Coordinates capture agents
│   ├── session_controller.py       # Detects session start/end, builds context
│   ├── intent_controller.py        # Processes IntentRecord, updates catalog
│   ├── anomaly_controller.py       # Evaluates severity, triggers captures
│   └── report_controller.py        # Orchestrates report generation
│
├── services/                       # Data access and external integrations
│   ├── __init__.py
│   ├── db_service.py               # SQLite — raw events and relational metadata
│   ├── vector_service.py           # LanceDB — embedded IntentRecords
│   ├── llm_service.py              # Claude API — inference and intent
│   ├── vision_service.py           # Claude Vision API — screen interpretation
│   ├── embedding_service.py        # nomic-embed-text via Ollama
│   ├── screenshot_service.py       # mss — forensic capture and storage
│   ├── websocket_service.py        # Real-time push to dashboard
│   └── alert_service.py            # Supervisor notifications
│
├── api/                            # FastAPI — exposes everything to the dashboard
│   ├── __init__.py
│   ├── main.py                     # API entry point
│   ├── dependencies.py             # Global dependency injection
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── agents.py               # GET /agents — real-time agent status
│   │   ├── sessions.py             # GET /sessions — session history
│   │   ├── intents.py              # GET /intents — IntentRecords by agent/period
│   │   ├── anomalies.py            # GET/PATCH /anomalies — feed and resolution
│   │   ├── reports.py              # POST /reports — on-demand generation
│   │   ├── query.py                # POST /query — semantic search
│   │   ├── agents_control.py       # POST /agents/{id}/pause|resume
│   │   └── health.py               # GET /health — full system status
│   └── middleware/
│       ├── __init__.py
│       ├── error_handler.py        # Global exception handler
│       ├── auth.py                 # JWT + roles (admin, supervisor, developer)
│       └── tenant.py               # Per-tenant data isolation
│
├── core/                           # Cross-cutting infrastructure
│   ├── __init__.py
│   ├── config.py                   # Per-tenant config + environment variables
│   ├── logger.py                   # Loguru — levels, rotation, structured format
│   ├── error_bus.py                # Central error bus — capture, classify, route
│   ├── event_queue.py              # Async RawEvent queue (asyncio + Redis)
│   ├── health.py                   # Health checks for all agents and services
│   └── constants.py                # Global enums and constants
│
├── pipeline/                       # Inference pipeline — the heart of the system
│   ├── __init__.py
│   ├── session_builder.py          # Builds SessionContext from RawEvents
│   ├── prompt_builder.py           # Builds structured prompt for the LLM
│   ├── intent_parser.py            # Parses and validates LLM JSON output
│   ├── catalog_updater.py          # Updates TenantCatalog after each session
│   └── benchmark_engine.py         # Compares sessions against tenant historical data
│
├── storage/                        # Persistence layer
│   ├── __init__.py
│   ├── migrations/                 # Alembic migrations
│   │   ├── env.py
│   │   └── versions/
│   ├── database.py                 # SQLite connection and sessions
│   ├── vector_store.py             # LanceDB connection and operations
│   └── repositories/               # One repository per model (Repository pattern)
│       ├── __init__.py
│       ├── raw_event_repo.py
│       ├── intent_record_repo.py
│       ├── anomaly_record_repo.py
│       ├── session_repo.py
│       ├── agent_profile_repo.py
│       └── tenant_catalog_repo.py
│
├── workers/                        # Celery async tasks and schedulers
│   ├── __init__.py
│   ├── celery_app.py               # Celery + Redis configuration
│   ├── inference_worker.py         # LLM inference worker (event-triggered)
│   ├── report_worker.py            # Report worker (scheduled)
│   ├── embedding_worker.py         # Embedding worker (batch)
│   └── cleanup_worker.py           # Data retention worker (scheduled)
│
├── tests/
│   ├── unit/
│   │   ├── test_session_builder.py
│   │   ├── test_prompt_builder.py
│   │   ├── test_intent_parser.py
│   │   └── test_anomaly_controller.py
│   ├── integration/
│   │   ├── test_pipeline.py        # RawEvents → IntentRecord end-to-end
│   │   ├── test_api_sessions.py
│   │   └── test_websocket.py
│   └── fixtures/
│       ├── raw_events.json         # Realistic BPO test events
│       └── intent_records.json     # Reference IntentRecords
│
├── scripts/
│   ├── seed_db.py                  # Populate DB with mock data for development
│   ├── run_agent.py                # Launch capture agent in dev mode
│   ├── simulate_session.py         # Simulate a full BPO session without hardware
│   └── export_catalog.py           # Export tenant session type catalog to JSON
│
├── docs/
│   ├── architecture.md
│   ├── agent_roles.md
│   ├── pipeline_flow.md
│   ├── api_reference.md
│   └── deployment.md
│
├── dashboard/                      # React frontend
│   └── src/
│       ├── views/                  # Main pages (MVC — View)
│       ├── components/             # Reusable UI components
│       ├── controllers/            # Hooks and state logic (MVC — Controller)
│       ├── models/                 # TypeScript types and schemas (MVC — Model)
│       ├── services/               # API calls and WebSocket
│       └── styles/                 # Design tokens and global animations
│
├── .env.example
├── .gitignore
├── alembic.ini
├── docker-compose.yml              # Redis + infrastructure services
├── pyproject.toml                  # Dependencies managed with Poetry
├── README.md
└── main.py                         # Main entry point — starts orchestrator + API
```

---

## 8. Tech Stack

| Layer | Technology | Libraries / Tools |
|---|---|---|
| Capture agent | Python 3.11+ | `mss`, `pynput`, `watchdog`, `pywinauto`, `Pydantic v2` |
| Backend / API | FastAPI + Uvicorn | `Celery`, `Redis`, `Alembic`, `Loguru`, JWT |
| LLM inference | Claude API (Sonnet) | `claude-sonnet-4-20250514`, Claude Vision API |
| Local LLM (optional) | Ollama | Llama 3 / Mistral for offline inference fallback |
| Database | SQLite + LanceDB | Relational events + local vector storage |
| Embeddings | nomic-embed-text | Via Ollama — fully local, no API key required |
| Frontend | React 18 + Vite | TypeScript, TailwindCSS, Framer Motion, Recharts |
| State management | Zustand + React Query | API sync and real-time WebSocket state |

---

## 9. Error Handling

WorkMemory implements a centralized `ErrorBus` that captures, classifies, and routes every exception in the system. There are no silent `except` blocks anywhere in the codebase.

### Severity levels

| Level | Behavior |
|---|---|
| `LOW` | Structured log entry — operation continues normally |
| `MEDIUM` | Log + push alert to supervisor dashboard |
| `HIGH` | Log + alert + automatic pause of the affected agent |
| `CRITICAL` | Log + alert + controlled system shutdown |

### Pattern enforced on every agent

```python
try:
    result = await self.run_task()
except LLMConnectionError as e:
    error_bus.capture(e, {"agent_id": self.id, "task": "intent_inference"}, ErrorSeverity.HIGH)
except ScreenCaptureError as e:
    error_bus.capture(e, {"agent_id": self.id, "task": "screenshot"}, ErrorSeverity.MEDIUM)
except Exception as e:
    error_bus.capture(e, {"agent_id": self.id}, ErrorSeverity.CRITICAL)
```

### Live terminal panel

The dashboard includes a real-time terminal panel (visible only to `admin` and `developer` roles) that streams backend logs via WebSocket. From the browser, operators can:

- Pause or resume individual agents
- Force reconnection to external services
- Clear task queues
- Inspect the orchestrator's internal state
- Run diagnostic commands without leaving the dashboard

---

## 10. Roadmap

| Period | Module                           | Deliverable                                                               |
| ------ | -------------------------------- | ------------------------------------------------------------------------- |
| Week 1 | `core/` + `models/` + `storage/` | Config, logger, error bus, Pydantic schemas, SQLite + LanceDB operational |
| Week 2 | `agents/` — capture layer        | Vision, Input, Window, Event agents producing real RawEvents              |
| Week 3 | `pipeline/` + `workers/`         | Session Builder, prompt constructor, end-to-end LLM inference             |
| Week 4 | `controllers/` + `services/`     | Session, Intent, Anomaly controllers — LLM and vector services wired      |
| Week 5 | `api/` complete                  | All routes, middleware, JWT auth, WebSocket, health checks                |
| Week 6 | Advanced agents                  | Forensic Anomaly Agent, Report Agent, Semantic Query Agent                |
| Week 7 | `tests/` + `scripts/`            | Unit + integration tests, realistic fixtures, seed DB, session simulator  |

---

## 11. Security & Privacy

- Clipboard content is **never stored** — only metadata: source app, content type, character count
- Screenshots are **only saved** when an anomaly is confirmed or a supervisor explicitly requests capture
- Agent data is **fully isolated per tenant** — no cross-contamination between BPO clients
- All LLM inference runs on **semantic metadata**, never on literal content
- Data in transit is encrypted with **AES-256**
- Screenshot capture policy (`silent` / `notify_agent` / `notify_both`) is **configurable per company**
- Terminal logs in the dashboard are **restricted to admin and developer roles**
- Data retention period is configurable per tenant — data is automatically purged when the period expires
- WorkMemory complies with local labor monitoring regulations when the BPO client properly discloses monitoring policies to their employees

---

## 12. Quick Start

### Requirements

- Python 3.11+
- Node.js 18+ (for the dashboard)
- Redis (for Celery task queue)
- Ollama installed locally (for embeddings)
- Anthropic API key

### Setup

```bash
# 1. Clone and install Python dependencies
git clone https://github.com/your-org/workmemory.git
cd workmemory
poetry install

# 2. Configure environment
cp .env.example .env
# Set ANTHROPIC_API_KEY, TENANT_ID, and JWT_SECRET in .env

# 3. Run database migrations
poetry run alembic upgrade head

# 4. Start infrastructure
docker-compose up -d  # starts Redis

# 5. Start the orchestrator and API
poetry run python main.py

# 6. Start the dashboard
cd dashboard
npm install
npm run dev

# 7. For development — simulate a BPO session without real hardware
poetry run python scripts/simulate_session.py
```

### Environment variables

```env
# Required
ANTHROPIC_API_KEY=sk-ant-...
TENANT_ID=your_bpo_client_id
JWT_SECRET=your_secret_key

# Optional — defaults shown
LLM_MODEL=claude-sonnet-4-20250514
SCREEN_CAPTURE_FPS=2
INACTIVITY_THRESHOLD_MINUTES=8
SCREENSHOT_MODE=silent               # silent | notify_agent | notify_both
AHT_DEVIATION_MULTIPLIER=2.0
FRICTION_RATIO_ALERT_THRESHOLD=0.4
SQLITE_PATH=./storage/workmemory.db
LANCEDB_PATH=./storage/vector
RETENTION_DAYS=30
REDIS_URL=redis://localhost:6379/0
API_PORT=8000
```

---

> WorkMemory does not record video, store clipboard content, or transmit any data outside the local machine without explicit tenant configuration. All captured data belongs to the tenant.
