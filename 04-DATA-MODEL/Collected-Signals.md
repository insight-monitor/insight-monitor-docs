---
status: raw
---
# Collected Signals Taxonomy

## Metadata signals
- Active window title and process name
- Application in focus and its category (IDE, browser, terminal, communication)
- Timestamp of every event
- Session start and end

## Interaction signals
- Focus changes: window switches, tab switches, monitor changes
- Navigation events: URL changes, document opens, file saves
- Input activity level: keyboard and mouse event frequency (not content)

## Content signals
- Screenshot capture at configurable intervals (min 30s)
- OCR text extraction from visible screen regions
- Browser URL and page title (not full DOM)

## Visual signals
- UI element classification (code editor, document viewer, video player, terminal)
- Visible entities detection (charts, tables, code blocks, forms)
- Screen region segmentation for sensitive content redaction

## Network signals
- Active connection domains and classifications
- Request categories (API, CDN, streaming, collaboration)

## Inferred signals
- Task category hypothesis with confidence score
- Work relevance score
- Intent estimate (research, creation, communication, review, idle)
- Context continuity score (related to preceding activity)

## Derived signals
- Task transition probability
- Interruption frequency and duration
- Focus depth estimate (single-tasking vs multitasking)
- Productivity pattern classification