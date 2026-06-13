---
status: raw
---
# Context Quality Dependency

## Problem
Inference accuracy depends heavily on the quality and completeness of the
user-provided context. If the user defines vague or incorrect context, the
model's classifications will be unreliable.

## Examples of degraded inference

| User context | Actual situation | System misclassification |
|---|---|---|
| Role: "Developer" | User is a student in training | May flag learning activity as unproductive |
| Schedule: 9-5 | User works 7-3 in a different timezone | Early productive hours classified as overtime or idle |
| No objectives set | User has clear weekly goals | All activity treated as general, no task-specific weighting |

## Implication
The system is only as good as the context it receives. This is not a model
limitation — it is a data quality limitation. The system should detect stale
or missing context and warn the user.