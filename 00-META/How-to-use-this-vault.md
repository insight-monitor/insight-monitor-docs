---
type: architecture
tags:
  - data
  - schema
  - example
---
# How to use
This file discusses how to create, modify and remove documentation files on this vault
## What to use
If the git repository was cloned, know:
- Install obsidian
- Use community plugins 
	- continuous mode
## How to read
This documentation is highly modular, to read use:
- Folders naming to see specific scope themes
- MOC (Map of Content) to know the relationships between files and topics
## How to create
### When creating a file
- Only first letter must be Capital, different words are separated by "-"
- Update or create a MOC (Folder Scoped)
- use [[Metadata]] inside every markdown file, this allows to classify and create natural relationships between files.
- back-link relationships using `[[ LINk ]]` (the obsidian way)
### Files life cycle
- To start, add the `raw` tag in the file, write all the ideas without focusing on the correct way.
- Use the templates [[MOC-templates]].
- Read the [[Atomization]] file and follow the directives
- Read the [[Folder-structure]] file to understand folder isolation
- Refactor the created file and separate in as many files as needed
- Commit and push the changes as per [[Git-norms]]
