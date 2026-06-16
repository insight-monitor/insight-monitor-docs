---
title: MVP 14-Day Execution Plan
type: reference
domain: project-management
priority: critical
ai-context: high
status: draft
audience: all
version: 1.0.0
tags:
  - mvp
  - plan
  - schedule
aliases:
  - MVP 14-Day Plan
creation-date: 2026-06-15
last-reviewed: 2026-06-15
---

# MVP 14-Day Execution Plan

**Timeline:** June 15 → June 29, 2026
**Team:** 4 people
**Strategy:** Heavy AI Agent usage for code generation

---

## Team Assignments

| Person | Role | Primary Tech | AI Agent Focus |
|---|---|---|---|
| **A** | Capture Agent | Python, `xdotool`, `mss`, `pynput` | OS API wrappers, cross-platform abstraction |
| **B** | Backend API | FastAPI, SQLite, Pydantic | Routes, repositories, session builder |
| **C** | Inference Pipeline | Gemini API, prompt engineering, Pydantic | Prompt design, response parsing, Riwi context |
| **D** | Frontend | React, TypeScript, TailwindCSS | Components, API integration, session views |

---

## Week 1: Foundation + Capture + API

### Day 1 — June 15 (Today)

| Task | Owner | Description |
|---|---|---|
| Create `700-MVP/` docs | All (coordinated) | Write MVP-Definition, Included, Excluded, Architecture |
| Create code repository | B | `git init`, Poetry project, `pyproject.toml` |
| Define Pydantic models | C | `RawEvent`, `SessionContext`, `IntentRecord` |
| Scaffold React project | D | `npm create vite`, TailwindCSS setup, router skeleton |
| Decide Gemini API key | All | Create Google AI Studio account, test a prompt |

**AI Agent tasks:** Generate Pydantic models + FastAPI skeleton + React scaffold in parallel.

### Day 2 — June 16

| Task | Owner | Description |
|---|---|---|
| Window tracker (Linux) | A | `xdotool` + `xprop` loop, emit window title + PID on focus change |
| URL context | A | `xdotool getwindowname` for browser tab title (read from active window) |
| SQLite schema + repos | B | `raw_events` table, `sessions` table, `intent_records` table |
| POST /events route | B | Accept RawEvent, validate, store to SQLite |
| Pipeline models | C | Finalize Pydantic models, validate against simulated JSON |

**AI Agent tasks:** Generate `window_tracker.py` with Linux xdotool bindings + SQLite repository classes.

### Day 3 — June 17

| Task | Owner | Description |
|---|---|---|
| Screenshot capture | A | `mss` at configurable interval (default 30s), save to temp dir |
| Input frequency monitor | A | `pynput`: count clicks/min, keystrokes/min — no content |
| POST /events batch | B | Accept multiple RawEvents at once for efficiency |
| GET /sessions endpoint | B | List sessions with basic metadata (duration, start/end) |
| Frontend scaffold | D | API client module, basic health check display |

**AI Agent tasks:** Generate `screenshot_capture.py` with mss + `input_monitor.py` with pynput.

### Day 4 — June 18

| Task | Owner | Description |
|---|---|---|
| Session builder | B | Group RawEvents by time + inactive gap (8 min default) |
| Session close detection | B | Gap > threshold OR explicit signal → finalize session |
| GET /sessions/{id} endpoint | B | Return session detail with all RawEvents |
| Frontend session list | D | Table showing sessions with status (open/closed) |

**AI Agent tasks:** Generate `session_builder.py` with gap detection algorithm.

### Day 5 — June 19 — INTEGRATION CHECKPOINT

| Task | Owner | Description |
|---|---|---|
| End-to-end test (dummy data) | B (coordinator) | Capture → API → store → retrieve → display |
| Hardcoded IntentRecord | C | Return fake intent data for now (proves pipe works) |
| Frontend shows intent | D | Display hardcoded session_type, goal, confidence |
| Fix contract mismatches | All | Align Pydantic models ↔ API responses ↔ TypeScript types |

**Milestone:** Working E2E with dummy data. All team members see the same data on their screen.

### Day 6 — June 20

| Task | Owner | Description |
|---|---|---|
| Prompt builder (RIWI mode) | C | Gemini prompt template with Riwi context categories |
| LLM service | C | Gemini API client with retry, error handling, timeout |
| Intent parser | C | Parse validated JSON from Gemini response |
| Pipeline integration | C | Wire SessionBuilder → PromptBuilder → LLM → IntentParser |

**AI Agent tasks:** Generate `prompt_builder.py` with Riwi-specific prompt template + `llm_service.py` with Gemini client.

### Day 7 — June 21

| Task | Owner | Description |
|---|---|---|
| `simulate_session.py` | A + C | 3 scenarios: BPO (CRM → SAP), Riwi learning (VS Code → MDN → Discord → YouTube), Mixed (work + personal) |
| `seed_db.py` | B | Populate SQLite with 10-20 sample sessions |
| End-to-end with real LLM | C | Full pipeline: simulated capture → Gemini inference → IntentRecord |
| Refine prompt | C | Adjust prompt based on first inference results |

**AI Agent tasks:** Generate `simulate_session.py` with realistic event generation.

---

## Week 2: Dashboard + Integration + Demo

### Day 8 — June 22

| Task | Owner | Description |
|---|---|---|
| Session list view | D | Table with session_type, duration, confidence score, timestamp |
| Color-coded confidence | D | High (green), medium (yellow), low (red) badges |
| Agent status indicator | D | Green dot when API returns 200, red when unreachable |
| API refinements | B | Error handling, validation messages, CORS for dev |

### Day 9 — June 23

| Task | Owner | Description |
|---|---|---|
| Session detail view | D | Timeline of app switches, screenshot thumbnails |
| Intent card | D | Display goal, friction_points, tags with confidence |
| Backend refinements | B | Pagination for session list, filtering by date |
| Cross-team sync | All | Review what still doesn't work, reprioritize |

### Day 10 — June 24

| Task | Owner | Description |
|---|---|---|
| Riwi demo scenario | C | Simulate a full Coder session with correct classification |
| Naive vs. contextual comparison | C + D | Side-by-side view: simple rules vs. Insight Monitor inference |
| Friction point highlighting | D | Visual emphasis on identified friction points in timeline |
| Live capture test | A | Run capture agent on real Ubuntu machine, verify it works |

### Day 11 — June 25

| Task | Owner | Description |
|---|---|---|
| BPO demo scenario | C | Simulate a call center session with CRM + SAP + softphone |
| Edge case handling | All | Long gaps, short sessions, unknown apps, missing screenshots |
| Error bus (basic) | B | Loguru logging, structured error capture |
| `.env.example` finalization | B | Document every configuration option |

### Day 12 — June 26

| Task | Owner | Description |
|---|---|---|
| Unit tests | C | `test_session_builder.py`, `test_intent_parser.py` |
| Integration test | B | RawEvents → IntentRecord end-to-end test |
| Dashboard polish | D | Loading states, empty states, error messages |
| Demo script draft | A | Step-by-step walkthrough for June 29 |

### Day 13 — June 27

| Task | Owner | Description |
|---|---|---|
| Full dry run | All | Run through the entire demo from start to finish |
| Bug fixes | All | Prioritize demo-blocking bugs only |
| Buffer for unexpected issues | All | Whatever broke during dry run |
| Simplify if needed | All | Strip non-essential features to protect the demo flow |

### Day 14 — June 28-29

| Task | Owner | Description |
|---|---|---|
| Final dry run (June 28) | All | Complete rehearsal with real hardware |
| Stakeholder presentation (June 29) | All | Show the demo, explain what it proves, discuss next steps |

---

## Daily Ceremony

Each day at 9:00 AM:
1. **15-min sync** — What was done yesterday, what is planned today, blockers
2. **Update `610-DAILY/`** — Log the blocker report
3. **AI Agent assignments** — Each person drafts prompts for their AI agent based on today's task

## Risk Register

| Risk | Likelihood | Mitigation |
|---|---|---|
| Gemini API rate limits exceed free tier | Medium | Have GPT-4o API key ready as fallback |
| `xdotool` fails on Wayland (Ubuntu 22+) | Medium | Fall back to `ydotool` or XWayland compatibility |
| Integration reveals incompatible Pydantic/TypeScript types | High | Day 5 checkpoint catches this early |
| Team member falls behind | Medium | AI agents can take over backlog items |
| Demo script breaks during live run | Medium | Recorded video backup ready |
