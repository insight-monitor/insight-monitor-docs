---
title: "Prompt Architecture"
type: reference
domain: inference
priority: high
ai-context: high
status: draft
audience: all
version: 0.1.0
tags:
  - prompt-architecture
  - system-prompt
aliases:
  - Prompt Architecture
---
# Prompt Architecture

## Structure
The system prompt has four layers, assembled at inference time:

### 1. System instruction (immutable)
Defines the model's role: impartial activity analyst. Cannot be modified. Contains safety constraints, output format, and redaction rules.

### 2. Environmental context (auto-populated)
Injected automatically from collected signals: active window, visible content, recent activity history, time of day, current objective if defined. The user has no direct control over this layer.

### 3. User context (configurable)
Injected from the user's defined context: role, objectives, app classifications, schedule, sensitivity keywords. The user controls this layer via the settings interface or config file.

### 4. Custom instructions (restricted)
In enterprise mode: disabled or limited to approved templates. In personal mode: user can append additional instructions within a character limit and subject to safety filtering. Cannot override system instruction layer.

## Output format
All inferences must follow a structured JSON schema:
```json
{
  "task_hypothesis": "string",
  "confidence": 0.0-1.0,
  "evidence": ["signal1", "signal2"],
  "alternatives": ["alt1", "alt2"],
  "sensitivity_flag": "none|low|medium|high"
}