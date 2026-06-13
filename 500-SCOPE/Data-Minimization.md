---
status: raw
---
## Minimum viable signals
The system can deliver value with only these sources:

- Active window title and application name
- Browser domain/URL (not full page content)
- Periodic screenshots (configurable interval, minimum 30s)
- Focus change events (window switches, tab switches)
- Process metadata (not memory contents)

## Excluded by default in minimum set
- Keystroke logging
- Clipboard contents
- Accessibility API reads
- File system traversal
- Microphone or camera input

## Sensitivity rule
No signal is collected if a less invasive signal can produce equivalent inference value. Each additional signal must be justified by measurable improvement in inference accuracy.