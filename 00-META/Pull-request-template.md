---
type: reference
domain: git
priority: low
status: completed
audience: developer
version: 1.0.0
aliases:
  - PR
---
# Pull Request Template
```markdown
## Summary
<!-- Link to Linear/Jira/Ticket or describe the "Why" -->
Related to: [Ticket-123]

## Type of Change
- [ ] `feat` / `legal` / `security` (New content)
- [ ] `fix` (Correction)
- [ ] `refactor` (Structure/Move)
- [ ] `breaking` (Major restructure/Removal)

## Review Checklist
- [ ] **Links Valid:** All `[[wikilinks]]` and `[markdown](links)` resolve (CI checks this).
- [ ] **Frontmatter:** `tags`, `status`, `version`, `owner` fields updated.
- [ ] **No Binary Bloat:** No large images/videos committed directly (use Git LFS or external CDN).
- [ ] **Obsidian Config:** `.obsidian/` changes are intentional and reviewed (see Section 6).

## Audit Trail (Legal/Security)
- [ ] Does this change compliance posture? (If yes, tag @legal-reviewer)
- [ ] Does this change threat model? (If yes, tag @security-reviewer)
```
