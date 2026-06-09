---
domain: git
type: reference
status: completed
priority: low
audience: developer
version: 1.0.0
aliases:
  - git issue
---
# git issue template
# Documentation Review Request
```markdown
# Documentation Review Request

## Context
This review occurs **before a Pull Request** and is intended for
high-level alignment (Legal, Product, Architecture).

## Branch
docs/legal-gdpr-update

## Files / Notes Reviewed
- 02-Legal/Contracts/DPA-v4.2.md
- 02-Legal/Compliance/GDPR-Mapping.md

## Review Focus
Legal/Compliance

## Summary
Added DPA v4.2 and updated GDPR mappings.

## Decisions Needed
1. Is liability cap language approved?
2. Are subprocessor obligations sufficient?
3. Any concerns before PR creation?

## Reading Guide
### Entry Point
- DPA-v4.2.md

### Supporting Docs
- GDPR-Mapping.md

## Author Checklist
- [x] Branch pushed
- [x] Wikilinks valid
- [x] Frontmatter complete
- [x] No sensitive data
- [x] Related tickets linked

## Reviewer Outcome
- [ ] Approved for PR
- [ ] Changes Requested
- [ ] Needs Legal Review
- [ ] Needs Product Review
- [ ] Needs Architecture Review
```
see the [review request](./.github/ISSUE-TEMPLATE/docs-review-request.yml) YAML file.

## example
```markdown
# [DOCS REVIEW] GDPR Compliance Update: DPA v4.2

> **Workflow Context:** This issue represents the **Alignment Phase**.
> 
> 1. Write docs in branch `docs/<slug>`
>     
> 2. Open this Issue for Legal / Product / Architecture review
>     
> 3. Incorporate feedback
>     
> 4. Open Pull Request for technical review and merge
>     

---

## Source Branch Name

`docs/legal-gdpr-update`

---

## Obsidian Vault Relative Path(s)

```text
02-Legal/Contracts/DPA-v4.2.md
02-Legal/Compliance/GDPR-Mapping.md
\```

---

## Primary Review Focus

☑ Legal/Compliance (Contracts, Policies, GDPR, Terms)

---

## 📝 Summary of Changes (The "What" & "Why")

Added new Data Processing Addendum (DPA) v4.2 per Legal request (LEG-450).

Changes include:

- Updated processor/subprocessor obligations
    
- Clarified breach notification timelines
    
- Added EU representative language
    
- Updated references to current GDPR articles
    

Related:

- Jira: LEG-450
    
- Notion: Compliance Refresh Q2
    

---

## Specific Questions for Reviewers (Blockers)

1. **@legal-team**  
    Is the liability cap language in Section 4.2 approved for Enterprise customers?
    
2. **@compliance-lead**  
    Are the new subprocessor notification requirements sufficient?
    
3. **@product-lead**  
    Should the DPA reference the upcoming onboarding flow changes or remain product-agnostic?
    

---

## 🗺️ Reading Guide / Navigation (Obsidian Context)

### Start Here

`02-Legal/Contracts/DPA-v4.2.md`

### Supporting Documents

- `02-Legal/Compliance/GDPR-Mapping.md`
    
- `01-Vision/Data-Governance-Principles.md`
    

### Graph Context

Check backlinks from:

- `Glossary/Data-Processor.md`
    
- `Glossary/Data-Controller.md`
    

These notes have the highest impact radius.
```