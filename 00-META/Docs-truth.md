---
type: truth
domain: documentation-architecture
priority: critical
status: completed
audience: all
version: 1.0.0
---
# Docs Rules
1. **Files are small**, Files CAN'T contain more than 500 words, except for `status:raw`
2. **Files frontmatter**, All files must contain [[Metadata]] frontmatter
3. **Hybrid folder + MOCs**, All folders must have a MOC
4. **Connections create sections, not folders**, Folders CAN'T have more than 2 folder children
5. **One concern**, All files must follow [[Atomization]], except for `status:raw`
6. **Language Consistency**, All files must be in English, except for `status: raw`
7. **Proof and connection**, All `domain: legal` files must contain external links to concerning laws
