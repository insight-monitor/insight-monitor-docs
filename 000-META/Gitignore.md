---
type: reference
domain: git
priority: low
status: accepted
audience: developer
aliases:
  - gitignore
---
### Obsidian-Specific `.gitignore` 
**Commit this file to root.** It prevents merge conflicts on local UI state and plugin caches.
```gitignore
# === Obsidian Core ===
.obsidian/workspace*          # Open tabs, pane layout (HIGH CONFLICT RISK)
.obsidian/graph*              # Graph view cache
.obsidian/cache*              # Metadata cache
.obsidian/plugins/*/data.json # Plugin local settings (often machine-specific)
.obsidian/themes/             # Theme preferences
.obsidian/appearance*         # Theme preferences
# === OS / Editor ===
.DS_Store
Thumbs.db
*.swp
*.swo
*~
.vscode/                      

# === Large Media (Use Git LFS instead) ===
# *.mp4, *.mov, *.avi, *.mkv
# *.psd, *.ai, *.raw
# *.zip, *.tar.gz

# === Explicitly TRACK these .obsidian files (Add to repo) ===
# !.obsidian/app.json         # Core settings (vim mode, theme)
# !.obsidian/community-plugins.json # Enabled plugin list
# !.obsidian/core-plugins.json      # Enabled core plugins
# !.obsidian/hotkeys.json         # Shared hotkeys
# !.obsidian/snippets/            # Custom CSS snippets
```
> **Strategy:** **Track plugin *lists* (so the team has the same tools), ignore plugin *data/workspace* (so you don't fight over open tabs).**
