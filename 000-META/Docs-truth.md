---
type: truth
domain: documentation-architecture
priority: critical
status: review
audience: all
version: 1.0.0
aliases:
  - Docs golden rules
slug: docs-truth
---
# Docs Rules
1. **Files are small**, Files CAN'T contain more than 500 words, except for `status: draft`
2. **Files frontmatter**, All files must contain [[Metadata]] frontmatter
3. **Hybrid folder + MOCs**, All folders must have a MOC
4. **Connections create sections, not folders**, Folders CAN'T have more than 2 folder children
5. **One concern**, All files must follow [[Atomization]], except for `status: draft`
6. **Language Consistency**, All files must be in English, except for `status: draft`
7. **Proof and connection**, All `domain: legal` files must contain external links to concerning laws
8. **Capitalized Folders**, All folders names must be capitalized
9. **Obsidian links for internal references**, all internal references must use `[[reference]]`
	1. For specific sections use `[[reference#header]]`
	2. Use alias for human readability consistency `[[MOC-References#header|a comprehensive text]]` 
10. **Markdown links for external and no-markdown**, use `[something](link)` for links outside the vault's scope and for internal non-markdown links `[this file](file.yml)`
## Obvious Rules
1. **Proper metadata**, Frontmatter must be properly defined for each file
2. **Natural Navigation**, Files must appear at least once as a backlink in other files
	1. **No single circularity**: They can't be pointing ONLY at each other. 
3. **Subfolder conventions**, Subfolders must have descriptive names and be capitalized
4. **Same purpose, same name**, multiple subfolders with different parents must have the same name if they serve the same purpose
5. **No spelling mistakes**, The only valid grammars are English USA and British