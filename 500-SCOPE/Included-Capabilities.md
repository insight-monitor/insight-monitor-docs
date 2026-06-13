---
status: raw
---
# Included Capabilities

## Signal collection
The system collects endpoint activity signals: active window titles, application names, process metadata, browser URLs/domains, periodic screenshots, and focus/navigation events.

## Multimodal inference
A vision-language model processes screenshots and contextual signals to produce probable task categories, work relevance scores, and intent hypotheses.

## Confidence scoring
Every inference carries a confidence score (0.0–1.0) reflecting the system's certainty. Low-confidence inferences are flagged as uncertain rather than presented as fact.

## Contextual classification
Activity is classified along contextual axes (work-relevant, research, distraction, personal, ambiguous) instead of binary good/bad labels.

## User transparency
The user can view collected signals, see what was inferred, and contest or correct misclassifications. The system logs evidence sources for each inference.

## Sensitive content redaction
Before persistence, the system detects and redacts credentials, financial data, government IDs, health information, and other sensitive categories. Redacted content is never stored or transmitted.

## Audit trail
All inferences, classifications, and user corrections are logged with timestamps and evidence references for accountability.