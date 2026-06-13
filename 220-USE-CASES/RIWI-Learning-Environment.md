---
status: raw
---
# Use Case: RIWI Learning Environment

## Scenario
A student (Coder) at Riwi — an EdTech platform combining intensive software development training with simulated work environments. Coders study 6am-2pm and 2pm-9pm, take module exams, and deliver real projects in a factory-like setup.

## Key difference from enterprise use
Unlike a standard workplace, Riwi is a hybrid of learning and simulated work:

| Dimension | Enterprise | RIWI Coder |
|---|---|---|
| Primary goal | Productivity & output | Skill acquisition & exam passing |
| Activity type | Task execution | Learning + practice + assessment |
| Evaluation | Manager assessment | Module exams + project delivery |
| Schedule | Fixed work hours | Dual block (6-14, 14-21) |
| Tool use | Specialized per role | Broad learning tools + IDEs |
| Non-screen time | Meetings, thinking | Study groups, peer mentoring |

## How the system should classify RIWI activity

### Learning activities (work-critical)
- IDE with practice exercises → **skill development**
- Learning platform (LMS) → **module progress**
- Documentation sites → **research / self-learning**
- Peer mentoring in Discord → **collaboration (matches Riwi value)**

### Assessment activities (work-critical)
- Exam platform → **evaluation phase**
- Project work in simulated environment → **applied learning**
- Code review with peers → **team collaboration**

### Ambiguous activities (context-dependent)
- YouTube → **tutorial if technical, entertainment if personal**
- ChatGPT → **learning tool if used for understanding, dependency if used to bypass**
- Browser social media → **break if timed, distraction if during active task**

## Why RIWI needs special handling
Standard enterprise monitoring would penalize activities that are essential to the Riwi model: extended reading, peer chat, trial-and-error coding, frequent context switching between learning tools. The system must recognize that "unproductive" in a workplace can be "essential learning" in this environment.