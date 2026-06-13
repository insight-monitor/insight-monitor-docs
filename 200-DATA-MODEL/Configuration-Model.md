---
status: raw
---
# Configuration Model

## Configuration layers (lowest to highest priority)

### 1. System defaults
Hardcoded baseline: application categories, signal types, inference thresholds.
Not modifiable by the user. Defined by the application source code.

### 2. Organization policies (enterprise only)
Applied by administrators: minimum required signals during working hours,
mandatory redaction rules, allowed configuration range for employees.
Stored server-side or in a policy file.

### 3. User preferences
User-defined context: role, objectives, app classifications, schedule.
Stored locally by default. Can be synced if user opts in.

## Configuration delivery methods

### Method A: GUI/Interface (recommended)
Dedicated settings panel in the application.
- Role/objectives form
- App classification drag-and-drop
- Schedule calendar
- Sensitivity keyword editor
- Export/import settings as JSON

### Method B: Configuration file (advanced)
JSON or YAML file at a known path.
- Same settings as GUI
- Useful for automation or version-controlled configs
- Validated against a schema on load

## Configuration scope
| Setting | Enterprise | Personal |
|---|---|---|
| App classification | Limited range | Full control |
| Objectives | May be required | Optional |
| Schedule | Enforced hours | Flexible |
| Custom prompts | Not allowed | Allowed within limits |
| Data retention | Defined by org | User-defined |
| Export/delete data | Limited by policy | Full rights |