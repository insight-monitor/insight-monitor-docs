---
title: Decision on MVP
status: in-progress
priority: critical
ai-context: high
type: decision
domain: develop
audience: all
tags:
  - architecture
  - settings
  - software-development
  - applicability
  - decision
aliases:
  - MVP initial stages
version: 1.0.0
creation-date: 2026-06-16
last-reviewed: 2026-06-16
parent:
  - "[[610-DAILY]]"
---
# Daily 
We discussed the developing process and responsibility division on the tasks to perform.
## Questions answered
- Will we proceed with the MVP as it is?
  Yes.
- How are we going to handle documentation?
  The technical documentation will live in the code repository, inside a `docs` folder, and the non-technical documentation will live inside the docs repository
- How are we going to work?
  @SrLampi1001 will create the issues and day by day roadmap to be completed each day, each day a pull request will be created to the `develop` branch, were it would be reviewed, improved and approved. This process will be daily and the last day a functional MVP MUST be finished
## Job distribution
@ValenColm - Will be in charge of the Inference Pipeline process.
@JjuanGarcia77 - Will be in charge of Backend API, except QA, repository and documentation related tasks.
@kevincano218-eng - Will be in charge of the Capture Agent.
@SrLampi1001 - Will be in charge of reviewing, coordinating and testing. 

The fronted issues will be almost completely solved with AI agents.

## Stoppers
None

## To do
- [x] Create the issues roadmap
- [x] Finish the day 1-2 todos
- [ ] Test functionality
