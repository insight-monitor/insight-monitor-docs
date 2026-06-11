---
status: draft
domain: documentation-architecture
tags:
  - ai-input
type: process
priority: high
aliases:
  - raw ai response
---
This is a **complete, beginner-friendly guide** tailored exactly to your `insight-monitor-docs` repository, your Obsidian structure, and your "Non-Technical / AI-Assisted / Legal-Heavy" workflow.

---

# PART 1: The Human-Readable Guide (`000-META/WORKFLOW_GUIDE.md`)
**Create this file:** `000-META/WORKFLOW_GUIDE.md`
*This is the "README for contributors" — no YAML jargon, just the "How we work here" rules.*
![[Workflow-guide]]

### Linking Rules (Obsidian Native)
*   **Internal:** `[[400-PRIVACY/data-mapping|Data Mapping Spec]]` (Use full path from root).
*   **External:** `[GDPR Art. 5](https://gdpr.eu/article-5-how-to-process-personal-data/)`.
*   **Diagrams:** Embed in note: `![[diagrams (now stored as subdirectory in each domain folder)/architecture/c4-container.drawio]]`.
*   **No Dead Links:** CI will fail the PR if a `[[wikilink]]` target doesn't exist.

### Forbidden / Flagged Words (Enforced by Vale)
| Word                         | Severity  | Why                  | Alternative                          |
| :--------------------------- | :-------- | :------------------- | :----------------------------------- |
| `guarantee` / `guaranteed`   | **Error** | Legal Liability      | `ensure`, `commit to`, `designed to` |
| `unhackable` / `100% secure` | **Error** | False Claim          | `resistant to`, `hardened against`   |
| `gdpr` (lowercase)           | **Error** | Standard             | `GDPR`                               |
| `ai` (lowercase, standalone) | Warning   | Ambiguity            | `AI`, `LLM`, `ML Model`              |
| `etc.`                       | Warning   | Lazy Writing         | Be specific or use `and so on`       |
| `simply` / `just`            | Warning   | Dismisses Complexity | Remove                               |

---

## 4. AI Agent Instructions (For Cursor / Copilot / Custom GPTs)
*Add this context to your AI prompt or `.cursorrules` / `AGENTS.md`.*

> **CONTEXT:** You are editing the `insight-monitor-docs` Obsidian Vault.
> **RULES:**
> 1.  **Read `000-META/WORKFLOW_GUIDE.md` first.**
> 2.  **Never invent facts.** If unsure, add `[[TODO: Verify with @owner]]` link.
> 3.  **Maintain Frontmatter.** Update `last-reviewed: {{TODAY}}` on edit.
> 4.  **Use Wikilinks `[[...]]`** for all internal references.
> 5.  **Respect Folder Boundaries.** Legal text → `420-LEGAL/` or `400-PRIVACY/`. Tech specs → `300-ARCHITECTURE/`.
> 6.  **Generate Acceptance Criteria** in `220-USE-CASES/` as `Given/When/Then` blocks for QA.
> 7.  **Diagrams:** Output Mermaid (`.mmd`) code blocks for sequence/flow. Save `.drawio`/`excalidraw` files in `diagrams (now stored as subdirectory in each domain folder)/`.
> 8.  **Compliance:** Never write "We comply with X." Write "We implement controls aligned with X per `[[400-PRIVACY/compliance-matrix.md]]`."

---

## 5. Quick Commands Cheatsheet

```bash
# 1. Start new doc work
git checkout develop && git pull
git checkout -b topic/legal-update-terms

# 2. Work in Obsidian... (Save, Commit, Push)
git add .
git commit -m "feat(legal): add arbitration clause v2"
git push origin topic/legal-update-terms

# 3. Open Review Issue (via GitHub Web UI -> Issues -> New -> "Documentation Review Request")

# 4. After Approval -> Open PR
gh pr create --title "docs(legal): Arbitration Clause v2" --body "Closes #123" --draft

# 5. Fix CI locally (Vale/Links)
vale .              # Run linter locally
markdown-link-check README.md # Check links locally

# 6. Ready to Merge
gh pr ready
# -> Reviewers Approve -> Merge Button (Squash)
```

---

# PART 2: The CI/CD Engine (`.github/workflows/`)

**Concept:** YAML files are just **recipes**. They say: *"On Event X, run Jobs Y on Runner Z."*
*   **Runner:** A fresh Linux VM (Ubuntu) GitHub gives you for free.
*   **Steps:** Shell commands (`run:`) or reusable Actions (`uses:`).
*   **Caching:** Saves `node_modules`/Vale styles between runs (speed).

Create this folder structure:
```text
.github/
├── workflows/
│   ├── docs-ci.yml           # Main Gatekeeper (Runs on PR)
│   ├── docs-preview.yml      # Deploys Obsidian -> HTML (Runs on Push to docs/*)
│   └── stale-bot.yml         # Housekeeping (Optional)
├── actions/
│   └── setup-vault/          # (Advanced) Custom composite action for Obsidian setup
└── dependabot.yml            # Auto-updates Actions/Vale versions
```

---

### File 1: The Main Gatekeeper (`.github/workflows/docs-ci.yml`)
**This runs on every Pull Request. It MUST pass to merge.**

```yaml
# .github/workflows/docs-ci.yml
name: 📚 Docs CI: Validate & Lint

# 1. TRIGGERS
on:
  pull_request:
    branches: [main]
    # Only run if markdown/files changed (optimization)
    paths:
      - '**.md'
      - '**.mmd'
      - '**.drawio'
      - 'diagrams (now stored as subdirectory in each domain folder)/**'
      - '.github/workflows/docs-ci.yml'
      - '.vale/**' # Config changes trigger re-run

  # Allow manual trigger from Actions tab
  workflow_dispatch:

# 2. GLOBAL SETTINGS
permissions:
  contents: read       # Checkout code
  pull-requests: write # Post comments on PR
  checks: write        # Report status

# 3. JOBS (Run in parallel)
jobs:
  # ---------------------------------------------------------
  # JOB A: MARKDOWN STRUCTURE & FRONTMATTER
  # ---------------------------------------------------------
  validate-structure:
    name: 🏗️ Structure & Frontmatter
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Needed for git diff/history checks

      - name: 🟢 Setup Node (for tooling)
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: 📦 Install Markdown Lint & Frontmatter Checker
        run: npm ci
        working-directory: .github/tools/linter # See "Local Tooling" below

      - name: 🧹 Run Markdownlint (Style)
        run: npx markdownlint-cli2 "**/*.md" --config .markdownlint.jsonc

      - name: 📋 Validate Frontmatter Schema
        run: node .github/tools/linter/validate-frontmatter.js
        # Custom script: Checks required fields, status enum, date format, ai-context enum.

  # ---------------------------------------------------------
  # JOB B: VALE PROSE LINTING (The "Legal/Style" Gate)
  # ---------------------------------------------------------
  vale-lint:
    name: ✍️ Vale Prose Lint
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v4

      - name: 🛠️ Install Vale
        uses: jamesmortensen/vale-action@latest
        with:
          version: "latest" # Pins to specific version in dependabot

      - name: 📥 Download Project Styles (Cached)
        uses: actions/cache@v4
        id: vale-styles-cache
        with:
          path: .vale/styles
          key: vale-styles-${{ hashFiles('.vale/styles/**/*') }}

      - name: 🚀 Run Vale
        # --glob="!980-DEPRECATED/**" ignores deprecated folder
        # --output=line enables GitHub Annotations (red squigglies in PR Files tab)
        run: vale --glob="!980-DEPRECATED/**" --output=line .

  # ---------------------------------------------------------
  # JOB C: LINK CHECKING (Internal Wikilinks + External)
  # ---------------------------------------------------------
  link-check:
    name: 🔗 Link Integrity
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v4

      - name: 🟢 Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: 📦 Install Link Checker
        run: npm install -g markdown-link-check

      - name: 🔍 Check External Links (HTTPS)
        # Retries 3 times, ignores localhost
        run: |
          find . -name "*.md" -not -path "./980-DEPRECATED/*" -exec markdown-link-check {} -q -c .markdown-link-check.json \;

      - name: 🧩 Check Internal Wikilinks [[...]]
        run: node .github/tools/linter/check-wikilinks.js
        # Custom Script: Parses [[path]] or [[path|alias]], verifies file exists in vault.

  # ---------------------------------------------------------
  # JOB D: DIAGRAM VALIDATION (Mermaid/PlantUML Syntax)
  # ---------------------------------------------------------
  diagram-check:
    name: 📐 Diagram Syntax
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v4

      - name: 🟢 Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: 📦 Install Mermaid CLI
        run: npm install -g @mermaid-js/mermaid-cli

      - name: ✅ Validate Mermaid Files (.mmd)
        run: |
          find . -name "*.mmd" -not -path "./980-DEPRECATED/*" | while read file; do
            echo "Checking $file..."
            mmdc -i "$file" -o /dev/null || exit 1
          done
        # If you use PlantUML: add docker run step for plantuml jar.

  # ---------------------------------------------------------
  # JOB E: AI CONTEXT VALIDATION (Optional but Recommended)
  # ---------------------------------------------------------
  ai-context-check:
    name: 🤖 AI Context Tags
    runs-on: ubuntu-latest
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v4
      - name: 🏷️ Validate ai-context tags
        run: node .github/tools/linter/check-ai-context.js
        # Ensures high-priority folders (01, 03, 04, 06, 07, 08, 09, 13) have ai-context: high
```

---

### File 2: The Preview Deploy (`.github/workflows/docs-preview.yml`)
**Runs on push to `docs/*` branches. Gives reviewers a clickable HTML link.**

```yaml
# .github/workflows/docs-preview.yml
name: 🌐 Docs Preview Deploy

on:
  push:
    branches: [ "docs/**" ] # Only feature branches
  workflow_dispatch:

permissions:
  contents: read
  pages: write      # Required for GitHub Pages
  id-token: write   # Required for OIDC auth

jobs:
  build-and-deploy:
    name: Build Obsidian -> HTML & Deploy
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: ⬇️ Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: 🟢 Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      # OPTION A: Use Quartz (Best for Obsidian -> Static Site)
      # https://quartz.jzhao.xyz/
      - name: 📦 Setup Quartz
        uses: actions/checkout@v4
        with:
          repository: jackyzha0/quartz
          path: quartz
          ref: v4.4.0 # Pin version!

      - name: 🔗 Symlink Vault into Quartz
        run: |
          cd quartz
          rm -rf content
          ln -s ${{ github.workspace }} content
          npm ci

      - name: 🏗️ Build Quartz (Outputs to public/)
        run: |
          cd quartz
          npx quartz build --directory ${{ github.workspace }}

      - name: 📄 Upload Artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ${{ github.workspace }}/public

      - name: 🚀 Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
        # URL will be: https://<user>.github.io/insight-monitor-docs/preview/<branch-name>/
        # (Requires GitHub Pages set to "GitHub Actions" in Repo Settings)
```

> **Why Quartz?** It natively renders `[[Wikilinks]]`, `![[Embeds]]`, Mermaid, Math, Graph View, and Backlinks — **exactly like Obsidian**. It turns your vault into a shareable website automatically.

---

### File 3: Dependabot (`.github/dependabot.yml`)
**Keeps your Actions & Tools secure/updated automatically.**

```yaml
# .github/dependabot.yml
version: 2
updates:
  # GitHub Actions Updates
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
      time: "09:00"
    labels: ["dependencies", "github-actions"]
    commit-message:
      prefix: "chore(ci)"

  # Node.js Tooling (Markdownlint, Mermaid, etc.)
  - package-ecosystem: "npm"
    directory: "/.github/tools/linter" # Where your package.json lives
    schedule:
      interval: "weekly"
    labels: ["dependencies", "node"]
```

---

# PART 3: The Vale Configuration (`.vale/`)

**This is your "Legal/Style Robot".** It lives in repo so it versions with docs.

## Folder Structure
```text
.vale/
├── .vale.ini           # Main Config (Entry Point)
├── styles/
│   ├── InsightMonitor/ # YOUR Custom Style (Highest Priority)
│   │   ├── legal.yml          # Legal Capitalization / Forbidden Words
│   │   ├── formatting.yml     # Heading order, List styles
│   │   ├── links.yml          # Wikilink checks (Custom)
│   │   └── ai-context.yml     # Custom rule for frontmatter tags
│   └── vocab/
│       ├── accept.txt      # Words to ALLOW (Project specific: "InsightMonitor", "RAG", "DPA")
│       └── reject.txt      # Words to FLAG (Typos, "teh", "recieve")
└── templates/            # (Advanced) Templates for new files
```

---

### 1. The Config (`.vale/.vale.ini`)
```ini
# .vale/.vale.ini
StylesPath = styles
MinAlertLevel = suggestion # suggestion | warning | error

# File Formats to Check
[formats]
md = markdown

# Global Ignores
Ignore = "980-DEPRECATED/**"
Ignore = "990-REFERENCES/**" # Often messy PDFs/exports
Ignore = ".obsidian/**"

# Rule Settings
[*]
# Enable Custom Styles
BasedOnStyles = InsightMonitor
# Enable Standard English (Oxford Comma, etc.)
BasedOnStyles = write-good
BasedOnStyles = Joblint # Good for "We are a fast-paced startup" -> Flag

# Vocabulary
Vocab = vocab/accept
Vocab = vocab/reject

# Specific Overrides
[*.{mmd,drawio,plantuml}]
# Don't lint diagram code as prose
BasedOnStyles = 

[README.md]
# Allow H1 in README only
heading-level = false
```

---

### 2. Your Custom Style: Legal/Compliance (`.vale/styles/InsightMonitor/legal.yml`)
**This implements the "Forbidden Words" table from Part 1.**

```yaml
# .vale/styles/InsightMonitor/legal.yml
extends: substitution
message: "Legal Risk: '%s' implies absolute guarantee/liability. Use '%s'."
level: error
ignorecase: true
swap:
  # Absolute Guarantees -> Legal Death Spiral
  "guarantee": "ensure"
  "guarantees": "ensures"
  "guaranteed": "ensured"
  "guaranteeing": "ensuring"
  "100% secure": "hardened"
  "unhackable": "resistant to attack"
  "zero risk": "mitigated risk"
  "fully compliant": "aligned with"
  "completely anonymous": "pseudonymized"
  "immutable": "append-only" # Unless truly blockchain/merkle

# Standard Capitalization (Proper Nouns)
extends: capitalization
message: "Standard Capitalization: '%s' should be '%s'."
level: error
scope: heading # Check headings strictly
match: '(?i)\b(gdpr|dpa|dpia|iso|soc|hipaa|ccpa|pii|pi|ai|ml|llm|rag|api|sdk|ui|ux|devops|cicd|yaml|json|markdown|mermaid|plantuml|excalidraw|obsidian|quartz|github|gitlab|vscode|cursor)\b'
```

---

### 3. Your Custom Style: Formatting (`.vale/styles/InsightMonitor/formatting.yml`)
```yaml
# .vale/styles/InsightMonitor/formatting.yml

# 1. Heading Hierarchy (No skipping H2 -> H4)
extends: heading-level
level: error

# 2. Sentence Spacing (One space after period)
extends: spacing
level: warning

# 3. List Style Consistency (Use '-' not '*')
extends: list-style
level: warning
style: dash

# 4. Max Line Length (Soft wrap in Obsidian, but hard limit for diffs)
extends: line-length
level: warning
length: 120

# 5. Frontmatter Presence (Custom Script handles schema, this checks existence)
# We use a custom script in CI for schema validation (see validate-frontmatter.js)
```

---

### 4. Vocabulary (`.vale/styles/vocab/accept.txt` & `reject.txt`)
**Plain text files. One word per line. Case sensitive unless `ignorecase: true` in rule.**

**`accept.txt` (Project Dictionary - Add yours!)**
```text
InsightMonitor
RAG
DPA
DPIA
Mermaid
Excalidraw
Wikilink
Frontmatter
Obsidian
Quartz
Vale
CI/CD
GitHub
Pseudonymization
DataController
DataProcessor
DataSubject
```

**`reject.txt` (Common Typos / "Stop Words")**
```text
teh
recieve
accomodate
occured
seperate
definately
```

---

# PART 4: Local Tooling (The "Secret Sauce" for Devs)

Don't make devs wait for CI. Give them `npm run lint` locally.

### 1. Create `package.json` at `.github/tools/linter/package.json`
```json
{
  "name": "insight-monitor-docs-linter",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "lint": "npm-run-all --parallel lint:*",
    "lint:md": "markdownlint-cli2 \"../../**/*.md\" --config ../../.markdownlint.jsonc",
    "lint:vale": "vale --glob=\"!../../980-DEPRECATED/**\" --output=cli ../../",
    "lint:links": "node check-wikilinks.js && node check-external-links.js",
    "lint:frontmatter": "node validate-frontmatter.js",
    "fix:md": "markdownlint-cli2 \"../../**/*.md\" --config ../../.markdownlint.jsonc --fix"
  },
  "devDependencies": {
    "markdownlint-cli2": "^0.12.0",
    "markdownlint": "^0.33.0",
    "npm-run-all": "^4.1.5",
    "gray-matter": "^4.0.3",
    "glob": "^10.3.0"
  }
}
```

### 2. Markdownlint Config (`.markdownlint.jsonc` at Root)
```jsonc
{
  "default": true,
  "MD013": { "line_length": 120, "code_blocks": false, "tables": false }, // Line length
  "MD024": false, // Allow duplicate headings (common in docs)
  "MD033": false, // Allow HTML (needed for Obsidian embeds/details)
  "MD041": false, // Allow missing H1 (some notes are fragments)
  "MD050": { "style": "atx_closed" } // Consistent heading style
}
```

### 3. Custom Node Scripts (Place in `.github/tools/linter/`)

#### A. `validate-frontmatter.js` (The Schema Enforcer)
```javascript
// .github/tools/linter/validate-frontmatter.js
import fs from 'fs';
import path from 'path';
import matter from 'gray-matter';
import { globSync } from 'glob';

const REQUIRED_FIELDS = ['title', 'slug', 'status', 'tags', 'ai-context', 'owner', 'last-reviewed'];
const VALID_STATUSES = ['draft', 'review', 'approved', 'deprecated'];
const VALID_AI_CONTEXT = ['high', 'medium', 'low'];
const ROOT = '../../';

const files = globSync('**/*.md', { 
  cwd: ROOT, 
  ignore: ['980-DEPRECATED/**', '990-REFERENCES/**', 'node_modules/**', '.obsidian/**'] 
});

let hasErrors = false;

console.log(`🔍 Validating Frontmatter for ${files.length} files...`);

for (const file of files) {
  const fullPath = path.join(ROOT, file);
  const content = fs.readFileSync(fullPath, 'utf8');
  const { data, content: body } = matter(content);

  const errors = [];

  // 1. Required Fields
  for (const field of REQUIRED_FIELDS) {
    if (!data[field]) errors.push(`Missing required field: "${field}"`);
  }

  // 2. Status Enum
  if (data.status && !VALID_STATUSES.includes(data.status)) {
    errors.push(`Invalid status: "${data.status}". Must be one of: ${VALID_STATUSES.join(', ')}`);
  }

  // 3. AI Context Enum
  if (data['ai-context'] && !VALID_AI_CONTEXT.includes(data['ai-context'])) {
    errors.push(`Invalid ai-context: "${data['ai-context']}". Must be: ${VALID_AI_CONTEXT.join(', ')}`);
  }

  // 4. Date Format (YYYY-MM-DD)
  if (data['last-reviewed'] && !/^\d{4}-\d{2}-\d{2}$/.test(data['last-reviewed'])) {
    errors.push(`Invalid last-reviewed format: "${data['last-reviewed']}". Use YYYY-MM-DD.`);
  }

  // 5. Slug Format (kebab-case)
  if (data.slug && !/^[a-z0-9-]+$/.test(data.slug)) {
    errors.push(`Invalid slug: "${data.slug}". Use lowercase kebab-case only.`);
  }

  // 6. Tags Array
  if (data.tags && !Array.isArray(data.tags)) {
    errors.push(`Tags must be an array (YAML list), not a string.`);
  }

  if (errors.length > 0) {
    hasErrors = true;
    console.error(`\n❌ ${file}`);
    errors.forEach(e => console.error(`   - ${e}`));
  }
}

if (hasErrors) {
  console.error('\n💥 Frontmatter validation failed.');
  process.exit(1);
} else {
  console.log('✅ All frontmatter valid.');
}
```

#### B. `check-wikilinks.js` (The Obsidian Link Checker)
```javascript
// .github/tools/linter/check-wikilinks.js
import fs from 'fs';
import path from 'path';
import { globSync } from 'glob';

const ROOT = '../../';
const WIKILINK_REGEX = /\[\[([^\]|#]+)(?:#[^\]]+)?(?:\|([^\]]+))?\]\]/g;

const mdFiles = globSync('**/*.md', { 
  cwd: ROOT, 
  ignore: ['980-DEPRECATED/**', 'node_modules/**', '.obsidian/**'] 
});

// Build Index of all valid targets (files + headers)
const validTargets = new Set();
mdFiles.forEach(f => {
  // Add file path (relative to root, no .md)
  validTargets.add(f.replace('.md', ''));
  // Add file name only (Obsidian allows [[filename]] if unique)
  validTargets.add(path.basename(f, '.md'));
  
  // TODO: Parse headers for #header-links (Advanced)
});

let hasErrors = false;

console.log(`🔗 Checking Wikilinks in ${mdFiles.length} files...`);

for (const file of mdFiles) {
  const fullPath = path.join(ROOT, file);
  const content = fs.readFileSync(fullPath, 'utf8');
  let match;
  const fileErrors = [];

  while ((match = WIKILINK_REGEX.exec(content)) !== null) {
    const target = match[1].trim(); // The path inside [[...]]
    
    // Check if target exists in our index
    // Logic: Check exact path match OR basename match
    if (!validTargets.has(target) && !validTargets.has(path.basename(target))) {
      // Heuristic: Check if adding .md helps
      if (!validTargets.has(target.replace('.md', ''))) {
        fileErrors.push(`Broken Wikilink: "[[${target}]]" (Line ~${content.substring(0, match.index).split('\n').length})`);
      }
    }
  }

  if (fileErrors.length > 0) {
    hasErrors = true;
    console.error(`\n❌ ${file}`);
    fileErrors.forEach(e => console.error(`   - ${e}`));
  }
}

if (hasErrors) {
  console.error('\n💥 Broken Wikilinks found.');
  process.exit(1);
} else {
  console.log('✅ All Wikilinks resolve.');
}
```

---

# PART 5: Setup Checklist (Do this once)

1.  **Enable GitHub Pages:**
    *   Repo Settings → Pages → Source: **"GitHub Actions"**.
2.  **Enable Branch Protection (Settings → Branches → Add Rule):**
    *   Branch pattern: `main`
    *   ✅ **Require a pull request before merging**
    *   ✅ **Require approvals** (1)
    *   ✅ **Require status checks to pass** → Select: `🏗️ Structure & Frontmatter`, `✍️ Vale Prose Lint`, `🔗 Link Integrity`, `📐 Diagram Syntax`, `🤖 AI Context Tags`.
    *   ✅ **Require conversation resolution before merging**.
    *   ✅ **Do not allow bypassing the above settings**.
3.  **Install Vale Locally (Devs):**
    *   `brew install vale` (Mac) / `scoop install vale` (Win) / `sudo apt install vale` (Linux).
    *   Run `vale sync` in repo root (downloads `write-good`, `Joblint` styles referenced in `.vale.ini`).
    *   Test: `vale .`
4.  **Add `package.json` to Root (Optional but nice):**
    ```json
    // package.json (Repo Root)
    {
      "scripts": {
        "docs:lint": "cd .github/tools/linter && npm ci && npm run lint",
        "docs:fix": "cd .github/tools/linter && npm run fix:md"
      }
    }
    ```
    *   Devs run: `npm run docs:lint`.
5.  **Create the Issue Template:**
    *   Save the YAML from **Part 1 of my previous answer** as `.github/ISSUE_TEMPLATE/docs-review-request.yml`.

---

# Summary: How it feels to work here

| Actor | Action | Tool | Time |
| :--- | :--- | :--- | :--- |
| **You (PO/Legal)** | "I need new Privacy Policy" | GitHub Web UI → **New Issue** → `Documentation Review Request` | 2 min |
| **AI Agent (You/Copilot)** | Drafts policy in `400-PRIVACY/` | Obsidian / Cursor (Reads `WORKFLOW_GUIDE.md`) | 5 min |
| **Dev** | Reviews AI draft, fixes links | Obsidian → `git commit -m "feat: privacy policy v1"` | 10 min |
| **Dev** | Pushes branch | `git push origin topic/privacy-v1` | 5 sec |
| **Bot (CI)** | **Blocks PR if:** "guarantee" used, `[[wikilink]]` broken, `ai-context` missing, Mermaid syntax error | GitHub Actions (`.github/workflows/docs-ci.yml`) | 30 sec |
| **Legal** | Clicks **Preview Link** (from `docs-preview.yml`), reads rendered HTML, comments on **Issue**: "Approved, change retention to 3 years" | GitHub Issue Page | 10 min |
| **Dev** | Updates file, pushes. CI turns Green. | Terminal / Obsidian Git | 2 min |
| **Dev** | Marks PR **Ready** → **Merge** | GitHub PR Page | 1 min |
| **System** | Deploys to `insight-monitor-docs` Pages Site. Closes Issue. | Automatic | 1 min |

**You now have:** Governance (Issue), Quality Gates (CI/YAML), Legal Safety (Vale), AI Context (Structure), and a live Preview (Quartz). All version controlled.