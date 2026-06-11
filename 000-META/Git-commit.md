---
type: reference
domain: git
priority: low
status: accepted
audience: developer
aliases:
  - git commit
---
# Commit Message Convention: 
**Format:**
```text
<type>(<scope>): <subject>

<body> (optional)

<footer> (optional)
```
#### Types 

| Type           | When to use                                                                                       | Version Bump    |
| :------------- | :------------------------------------------------------------------------------------------------ | :-------------- |
| **`feat`**     | New section, new policy, new diagram (e.g., "Add Data Retention Policy").                         | **Minor**       |
| **`fix`**      | Correcting facts, broken links, typos in procedures, outdated legal refs.                         | **Patch**       |
| **`refactor`** | Restructuring folders, renaming files, moving content between notes *without changing meaning*.   | **Patch**       |
| **`docs`**     | **Only for meta-docs:** Updating `README`, `CONTRIBUTING`, `.gitignore`, GitHub Actions.          | **None**        |
| **`chore`**    | Dependency updates (Obsidian plugins), CI/CD, `.obsidian` config sync.                            | **None**        |
| **`security`** | **Specific type for you:** Updates to threat models, vulnerability docs, access control matrices. | **Patch/Minor** |
| **`legal`**    | **Specific type for you:** Contract updates, compliance checklists, regulatory changes.           | **Minor/Major** |
| **`breaking`** | **Rare:** Removing a mandatory process, changing the project scope/intent fundamentally.          | **Major**       |

#### Examples
```text
feat(legal): add GDPR Art. 28 DPA template
fix(security): correct encryption algorithm spec in threat-model (AES-256 -> XChaCha20)
refactor(howto): restructure deployment guides into versioned folders
breaking(intent): remove legacy monolith architecture section
chore(meta): update obsidian-lint rules for frontmatter
```
## Before committing
- Make a git status and check the current branch.
- If inside main, you're likely doing a hotfix — verify urgency.
- If inside develop, push directly (light protection) or create a topic branch for larger changes.
- If inside a topic branch, ensure it was branched from develop.
- If branch not found, create a new branch from develop: [[Git-branching]]
- If branch found, check for `git pull` before trying to commit new changes
- If more than one topic change, make multiple commits
## Unfinished changes commits
Commits that have `status: draft`:
```
<body> <file_name> Status(Draft)-> Refactor/finishing pending
```
> Only commit `status: draft` files if absolutely necessary. 
