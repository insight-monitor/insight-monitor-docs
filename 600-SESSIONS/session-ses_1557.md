---
status: draft
type: session
domain: documentation-architecture
audience: all
tags:
  - narrative
  - ai-input
creation-date: 2026-06-08
aliases:
  - docs startup
---
# Documentation startup plan from critique

**Session ID:** ses_1557957a3ffec0cXubI78eYuSD
**Created:** 8/6/2026, 22:56:52
**Updated:** 8/6/2026, 23:40:22

---

## User

Can you please see the @900-CRITIQUE-ITERATION\AI_analisis.md and the @README.md? Please make sure to follow @0000-META\How-to-review.md . I want to start populating the folders and the information. So give me an start up plan to have a step by step on what to start defining, what is priority, etc, so that we can start the developing process with a strongly structured non-technical documentation

---

## Assistant

_Thinking:_

The user wants me to:
1. Read the critique analysis (900-CRITIQUE-ITERATION/AI_analisis.md)
2. Read the README.md
3. Follow the How-to-review.md guidelines
4. Help them create a startup plan with step-by-step guidance on what to start defining, priorities, etc., so they can start the development process with strongly structured non-technical documentation

Let me also look at the existing folder structure to understand what's already there.

**Tool: read**

**Tool: grep**

---

## Assistant

_Thinking:_

Let me also look at the Workflow-guide.md and INDEX.md to understand the full picture.

**Tool: read**

**Tool: read**

---

## Assistant

_Thinking:_

Now I have a clear picture. Let me analyze:

1. **What was critiqued**: The README.md (the core concept) was severely critiqued in 900-CRITIQUE-ITERATION/AI_analisis.md. The critique says:
   - The concept is vague, internally inconsistent, legally dangerous, technically underdefined
   - Overclaims everywhere (anthropomorphic "AI understands intent" language)
   - No formal data model
   - No confidence model
   - No distinction between observation and inference
   - No sensitivity classification framework
   - No explanation layer
   - No boundary of use cases
   - No error philosophy
   - Privacy section is weak
   - Security section is superficial
   - No structured data taxonomy
   - No scope boundaries

2. **The folder structure already exists** with numbered folders from 000-META to 99-REFERENCES. But they're likely empty or mostly empty (except 000-META, 900-CRITIQUE-ITERATION).

3. **The Workflow-guide.md** defines the documentation architecture:
   - Folders represent dependency order
   - Each folder has an ownership
   - Atomization (single file per concern)
   - Frontmatter required
   - The workflow: Issue → PR → Merge

4. **The user wants**: A startup plan - step by step priority guide on what to define first, so they can start populating the folders with strongly structured non-technical documentation.

Let me check which folders actually have content already.

**Tool: task**

---

## Assistant

_Thinking:_

Now I have the full picture. Let me create a comprehensive startup plan based on:

1. The critique - which identified all the weak spots
2. The folder structure - which defines the dependency order
3. The Workflow-guide - which defines how to write docs
4. The vault rules (Docs-truth.md) - atomization, frontmatter, etc.

The key insight is: the critique tore apart the README concept because it lacked foundational definitions. The folders already encode the right order. The user needs to populate them in order.

Let me create the plan. The user wants me to give a step-by-step on what to start defining, what is priority, etc. This should be actionable and follow the How-to-review.md spirit (critical, concrete, point at faults/solutions).

Given the empty folders (01-13 are all empty), the user has a solid meta-layer (000-META) but zero actual content. The critique essentially says "your idea is vague because you haven't defined the fundamentals." 

The plan should focus on populating in dependency order:
1. First: 01-NARRATIVE (vision, story, why) — because everything else flows from this
2. Then: 02-BRAND-POSITIONING (voice, tone, market)
3. Then: 03-CORE-CONCEPT (first principles, definitions, glossary)
4. Then: 04-DATA-MODEL (data types, taxonomy)
5. Then: 05-INFERENCE-FRAMEWORK (confidence, inference rules)
6. Then: 06-PRIVACY (privacy model)
7. Then: 07-SECURITY (security model)
8. Then: 08-LEGAL (legal)
9. Then: 09-ARCHITECTURE (system design)
10. Then: 10-RISKS, 11-LIMITATIONS, 12-SCOPE
11. Then: 13-USE-CASES-EXAMPLES
12. Then: 15-DIAGRAMS to visualize

But within each folder, what specifically to address? The critique highlights the key missing pieces. Let me map critique points to folders:

- No formal data model → 04-DATA-MODEL
- No confidence model → 05-INFERENCE-FRAMEWORK
- No distinction between observation and inference → 03-CORE-CONCEPT + 05-INFERENCE-FRAMEWORK
- No sensitivity classification framework → 06-PRIVACY
- No explanation layer → 09-ARCHITECTURE or 05-INFERENCE-FRAMEWORK
- No boundary of use cases → 12-SCOPE + 11-LIMITATIONS
- No treatment of offline cognition → 11-LIMITATIONS
- No handling of multi-monitor/multi-device → 11-LIMITATIONS
- No false positive / false negative philosophy → 10-RISKS or 05-INFERENCE-FRAMEWORK
- Privacy section weak → 06-PRIVACY
- Security section superficial → 07-SECURITY
- No explanation of data acquisition mechanisms → 04-DATA-MODEL
- Anthropomorphic language → 03-CORE-CONCEPT (define glossary precisely)
- Overclaims about intent, purpose, efficiency → 03-CORE-CONCEPT (define terms honestly)

Let me write a concrete, structured plan.

The user asked for "a start up plan to have a step by step on what to start defining, what is priority, etc, so that we can start the developing process with a strongly structured non-technical documentation"

I should give them a clear actionable plan.

Here is the full picture: **you have a robust meta-layer (000-META with 14 files defining how to write) but the entire content layer (01 through 13) is empty.** The critique essentially validated your direction but shredded the execution because there are zero foundational definitions. Your folder structure already encodes the right dependency order — you just need to populate it with honest, precise content.

Here is the startup plan:

---

## Phase 1 — Ground Truth (01, 02, 03)

These define *what the product actually is* before any technical work. The critique's biggest accusations ("overclaims," "undefined terms," "anthropomorphic fluff") get fixed here.

1. **01-NARRATIVE/MOC-Narrative.md + Vision.md** — State the "why we exist" in deflated, honest language. Drop "AI understands like a human." Say "we estimate probable task intent from multimodal signals with confidence scores." (This directly answers critique §1, "Brutal rewrite of your actual idea in honest terms" at line 698.)

2. **03-CORE-CONCEPT/Glossary.md** — Define every loaded term operationally:
   - *Intent* = estimated probable task category (not true mental state)
   - *Purpose* = inferred work relevance label
   - *Efficiency* = relative to what reference frame? (time? output? historical baseline?)
   - *Success* = probable completion markers, not truth
   - *Observation* vs *Inference* — hard separation (critique §3, lines 385-392)

3. **02-BRAND-POSITIONING/Voice-Tone.md** — Document the precise language rules: no "Eyes," no "sees," no "understands." Use "estimates," "infers with confidence," "classifies probabilistically."

**Priority: CRITICAL.** Without this, everything below will inherit the vagueness the critique flagged.

---

## Phase 2 — Data Honesty (04, 05)

The critique says your data model is "mush" and you have no confidence model.

4. **04-DATA-MODEL/Data-Taxonomy.md** — Specify every signal category (metadata, interaction data, content data, visual data, network data, inferred data, derived performance data — from critique §1 lines 364-370). For each, state acquisition mechanism (OS API? OCR? DOM? screenshot?) and whether it's raw or inferred.

5. **04-DATA-MODEL/Data-Acquisition.md** — Answer the critique's question at line 115: keystroke logging? text field capture? clipboard? OCR? accessibility API? Each has different legality and invasiveness.

6. **05-INFERENCE-FRAMEWORK/Confidence-Model.md** — Define the confidence representation. Every inference must carry: score, evidence sources, alternative hypotheses, uncertainty reason. The critique calls this a "giant omission" (line 375).

**Priority: HIGH.** Without these, your architecture and privacy docs have nothing concrete to reference.

---

## Phase 3 — Safeguards (06, 07, 08)

The critique was clearest here: your privacy model is "weak," security is "superficial," and legal logic is "naive."

7. **06-PRIVACY/Sensitivity-Taxonomy.md** — Define every sensitivity class (credentials, financial, health, legal, union, political, therapy, minors, etc.) with rules per class: *collection, processing, local-only, redaction, retention, model access, human access, deletion* (critique lines 397-420).

8. **06-PRIVACY/Data-Retention.md** — By data type, how long? Where? Deletion guarantees?

9. **06-PRIVACY/Device-Ownership-Model.md** — Resolve the core contradiction the critique identified (line 508-526): employee-owned vs company-owned device rules, mandatory vs optional telemetry.

10. **07-SECURITY/Encryption-Model.md** — Not "encryption should be the norm." Specify: in-transit, at-rest, key management, tenant isolation, access control, audit logging, redaction before upload.

11. **07-SECURITY/Threat-Model.md** — Answer: endpoint compromise, insider access, forensic persistence, malware exposure, admin extraction (critique lines 598-606).

12. **08-LEGAL/Consent-Model.md** — Address why "user accepted" is often fiction in employment contexts. Define meaningful consent boundaries.

**Priority: HIGH.** These are compliance gates — you cannot build without them.

---

## Phase 4 — Boundaries (11, 12, 10)

The critique called out "no boundary of use cases," "no error philosophy," "no treatment of offline cognition."

13. **11-LIMITATIONS/Offline-Cognition.md** — Explicitly state what the system misses: reading, thinking, comparing, verbal discussions, stepping away to reason (critique lines 464-475).

14. **11-LIMITATIONS/Multi-Device-Gaps.md** — Multi-monitor, phone use, VDI, private profiles — partial visibility means partial truth.

15. **12-SCOPE/Workflow-Suitability.md** — Which job types is this suitable for? Dev, design, legal research, sales, support? Each has different observability (critique lines 447-461).

16. **10-RISKS/Error-Philosophy.md** — Which error is worse? Wrongly accusing waste vs missing actual waste vs wrongly storing sensitive content vs failing to classify sensitive content (critique lines 493-501). Define the product's error bias.

**Priority: MEDIUM** but important before marketing claims.

---

## Phase 5 — Concrete Outputs (13, 09, 15)

Now you can design architecture and examples because the definitions exist.

17. **13-USE-CASES-EXAMPLES/ — Write 3-5 concrete scenarios** (the YouTube example from the critique is good, but add: software dev context-switching, legal research, customer support). Each scenario must reference the defined terms from Phase 1.

18. **09-ARCHITECTURE/Explanation-Layer.md** — If the system says "this was likely distraction," it must explain *why* in evidence terms (critique lines 423-440).

19. **15-DIAGRAMS/ — Mermaid or Drawio**: Data flow, inference pipeline, privacy boundaries.

---

## The One Rule That Fixes Most of the Critique

Before writing any file, ask: *"Is this sentence something I could trace back to a specific signal, a confidence score, and a documented limitation?"* If not, it's the kind of "decorative fog" the critique called out. Write it honestly or don't write it.

---

Ready to start. Recommend beginning with **01-NARRATIVE/Vision.md** — a single 400-word file that states the honest version of the idea (line 698 of the critique is your template). Once that's approved, the rest flows cleanly.

---

## User

Okay, I've filled a @01-NARRATIVE\Vision.md.md file, but it's raw and lacking robustness. It uses a very informal speech and isn't really clear. I would like you to help me refactor and fix the file to be professional, modularized, and fit to be presented. Ask me anything that isn't clear before making an assumption yourself

---

## Assistant

_Thinking:_

The user has created a Vision.md.md file (note the double .md extension) that is in raw status. They want me to help refactor it to be professional, modularized, and fit to be presented. But they say "Ask me anything that isn't clear before making an assumption yourself."

Let me analyze the file against the critique and the meta-rules:

**Critique points that apply here:**
1. "Intent," "efficiency," "success" are undefined terms — file uses them loosely
2. "Hijack the input" — sounds aggressive/invasive
3. No confidence model mentioned
4. No separation between observation and inference
5. "Catch as much data as possible" — maximalist approach, opposite of data minimization
6. "Blank spaces" for cognitive processes is vague
7. The file mixes vision with architecture with data model — violates atomization
8. Informal language

**Meta rules:**
- Atomization: single concern per file
- 500 words max (this has ~250 words, okay)
- Frontmatter required (done, but could be improved)
- Language consistency: English (ok)

**Issues I see but shouldn't assume:**
1. The double .md.md extension — probably a mistake, should be .md
2. "Hijack the input" — is this meant literally? Keystroke logging? API hooking? OS-level input capture? The critique pointed out this ambiguity.
3. "Blank spaces" — does this mean periods of no input? Or literal blank screens? Or cognitive downtime?
4. "External tools that can't be monitored from within the device" — what kind of external tools? Physical notebooks? Phones? Other computers?
5. "Scrum or Agile methodology applications" — is this a primary use case or just an example?
6. "Company Monitoring dashboard" — is the primary customer the employer or the individual user? The critique flagged this as a key ambiguity.
7. "Data would also be formatted and stored" — stored where? Locally? Cloud? Both?
8. The relationship between the raw data collection layer and the AI inference layer — are these two separate products or one?
9. The final output is described as a "summary" — is the summary the product, or is the raw+inferred data also a product?
10. "Like an API to be called" — is this an API-first product, or a dashboard-first product with an API layer?

I should ask clarifying questions before making assumptions. Let me identify the key ambiguities.

Actually, the user said "Ask me anything that isn't clear before making an assumption yourself." So I need to ask questions. But I should be strategic about which questions to ask — not everything, just the things that would fundamentally change how I restructure the document.

Key questions:

1. **Double extension**: Is the `.md.md` intentional?
2. **"Hijack the input"**: What specific input capture mechanisms are intended? Keystroke logging? OS-level hooks? Accessibility APIs? The term "hijack" is concerning.
3. **Primary customer**: Is this B2B (employers monitoring employees) or B2C (individuals tracking their own productivity)? Or both? The answer affects everything.
4. **Data storage model**: Is data processed locally, in cloud, or hybrid? The Vision should pick a lane or acknowledge the spectrum.
5. **Scope of "external tools"**: What specifically can't be monitored? Physical tools? Other devices? Analog work?
6. **Is the core product the data pipeline (collect → infer → output structured data) or the applications built on top (dashboards, Scrum integration)?** This is the most critical architectural question.

Let me ask focused questions.

Before I refactor, I need clarity on several points. The current draft mixes vision, architecture, data model, and use cases in one file — and uses language the critique flagged as vague or risky. Let me resolve the ambiguities:

**Tool: question**

---

## Assistant

_Thinking:_

Now I have clear answers:
1. File name: typo, should be Vision.md
2. Capture: OS-level accessibility APIs + Mixed approach (TBD)
3. Customer: Both, but primarily B2B
4. Data: Hybrid - local processing + cloud
5. Core product: Both equally (pipeline + apps)
6. External tools: Both (physical + digital)

Now let me refactor the Vision.md file. Key principles:

1. **Atomization**: The vision file should be ONLY the vision — the "why we exist" and the high-level what. Remove architectural details, data model specifics, and implementation details that belong in other folders.

2. **Honest language** (per critique): 
   - No "hijack" 
   - No "understands intent" — say "estimates probable intent with confidence scoring"
   - No "catch as much data as possible" — phrase as "comprehensive signal collection within defined boundaries"
   - Define terms or acknowledge they need definition

3. **Structure**: Should clearly state:
   - Problem (why current solutions fail)
   - Solution direction (not implementation)
   - Value proposition (the differentiated output)
   - Target customers (B2B primary, B2C secondary)
   - Data philosophy (hybrid, local-first where possible)
   - Output concept (pipeline + applications)

4. **Frontmatter**: keep the raw status, add tags, ai-context, owner, etc.

5. **Modularization**: This file should be ~300-400 words max, and should reference other files that will be created later (e.g., [[03-CORE-CONCEPT/Glossary]] for term definitions).

Let me write it. I need to:
- Remove: "hijack the input" → replace with "capture via OS-level accessibility APIs and application-level integrations"
- Remove: "catch as much data as possible" → "comprehensive but governed signal collection"
- Add: explicit acknowledgment that "intent," "efficiency," "success" are estimated probabilities, not truths
- Add: customer clarity (B2B primary)
- Add: data processing philosophy (hybrid, local-first)
- Add: product definition (pipeline + applications, both first-class)
- Keep: the YouTube-like example intuition (context > category)
- Remove: implementation details (VS Code, git actions — those belong in architecture)
- Add: forward references to other files that will define the specifics

Let me draft it.

Good. Now I have clear answers. Let me also quickly check the metadata template for proper frontmatter fields.

**Tool: read**

**Tool: read**

---

## Assistant

_Thinking:_

Good. Now I have everything I need. Let me also check the File-lifecycle.md to understand the process for changing status from raw to in-review.

**Tool: read**

---

## Assistant

_Thinking:_

Now I have all the info. Let me craft the refactored Vision.md file.

Key points from the answers:
1. File name: Should be Vision.md (remove double .md.md)
2. Capture: OS-level accessibility APIs + application-level integrations (mixed/TBD)
3. Customer: B2B primary, B2C secondary
4. Data: Hybrid — local processing + cloud
5. Core product: Both equally (pipeline + applications)
6. External tools: Both physical and digital

My refactoring approach:

**Structure:**
1. Short preamble: What Insight Monitor is (one sentence)
2. Problem: Why current monitoring is broken (context-blind category matching)
3. Solution: Context-aware behavioral inference (not "understanding")
4. Value proposition: The differentiated output
5. Data philosophy: Comprehensive but governed collection, hybrid processing
6. Product model: Pipeline + applications, API-first
7. Boundaries: What we do and don't claim

**Language fixes:**
- "hijack" → "capture via OS-level accessibility APIs and application-level integrations"
- "catch as much data as possible" → "systematic collection across observable signal layers"
- "understand intent" → "estimate probable intent with confidence scoring"
- Remove "blank spaces" as vague → replace with "detect periods of low digital interaction, which may indicate offline cognitive work, meetings, or idle time — classified with explicit uncertainty"

**Atomization notes:**
- Detailed data acquisition mechanisms → 04-DATA-MODEL/
- Confidence scoring details → 05-INFERENCE-FRAMEWORK/
- Specific use cases (Scrum, dashboards) → 13-USE-CASES-EXAMPLES/
- Privacy specifics → 06-PRIVACY/
- Architecture specifics → 09-ARCHITECTURE/

I need to keep the Vision file focused. It should reference other files with `[[wikilinks]]` but not contain their content.

Let me draft the new file now. I'll use frontmatter with:
- type: concept
- domain: narrative
- priority: critical
- status: draft (to match its current state)
- audience: all
- version: 1.0.0 (or bump to 2.0.0 since it's a significant rewrite)

Wait, actually the user said "refactor and fix the file to be professional, modularized, and fit to be presented." This implies making it presentable, so maybe status should change to in-progress or in-review? But the user didn't say they want to change status. Let me keep it as raw since they're still workshopping it, but I'll flag that it should be changed when they're ready. Actually, I'll set it to in-progress since they're actively developing it and want it to be presentable. Or keep it raw — let me think. The File-lifecycle says: "Change status to in-progress once structured." So since I'm structuring it, in-progress makes sense. But the user might want to review first. I'll keep it raw and let the user decide.

Actually, I'll just keep it as status: draft and note in my response that they should change it when ready.

Let me write the file now. I'll aim for ~350-400 words, modular, professional, no vague claims.

Let me also check: should I fix the double extension issue? The file is currently at `01-NARRATIVE\Vision.md.md`. I should probably rename it by creating a new file at `01-NARRATIVE\Vision.md` and the old one should be... well, the user said it's a typo. I could either:
1. Create the new Vision.md and delete Vision.md.md
2. Or just write the new Vision.md and note the old one should be removed

Actually, looking at the Windows file system, I can use write to create the new file and then remove the old one. Let me do that.

Wait, I need to think about this more carefully. The tool `write` overwrites existing files. I could write to `01-NARRATIVE\Vision.md` (new correct name) and then delete `01-NARRATIVE\Vision.md.md`. But actually, maybe I should just write to the existing file `Vision.md.md` for now and suggest they rename it? No, the user said it's a typo. Let me create the correct file and delete the old one.

Actually, looking at the tools available, I don't have a "rename" or "move" tool. I can:
1. Write to the new correct path `01-NARRATIVE\Vision.md`
2. Use bash to remove the old file

That's the cleanest approach. Let me do that.

Now let me compile the refactored content:

```markdown
---
type: concept
domain: narrative
priority: critical
status: draft
audience: all
version: 2.0.0
tags:
  - vision
  - b2b
  - hybrid-processing
  - intent-estimation
---

# Vision

Insight Monitor is a behavioral intelligence platform that collects computer activity signals and applies multi-modal AI to estimate probable user intent, task relevance, and efficiency — producing structured data that powers both directly integrated applications and third-party pipelines.

## Problem

Existing monitoring tools rely on rigid category matching: an application name or domain is classified as "productive" or "unproductive" with no regard for context. This produces high false-positive rates, trivial bypass opportunities, and mistrust. The user's actual purpose — research, focused work, passive consumption, or distraction — cannot be determined from surface signals alone.

## Solution

We replace static classification with probabilistic, context-aware inference. Instead of asking "what app is open," the system asks "what is the user likely trying to achieve." This inference draws on multiple signal types:

- **System signals**: active windows, application metadata, focus transitions
- **Content signals**: visible text (via OCR and accessibility APIs), document titles, URLs, page context
- **Interaction signals**: input patterns, navigation events, application-specific actions (e.g., git commits, file edits)
- **Temporal signals**: session duration, task continuity, interruption frequency

The AI estimates — it does not "know." Every inference carries a confidence score and can be traced back to the evidence that produced it.

## Value Proposition

Unlike conventional monitoring, Insight Monitor outputs structured behavioral intelligence:

- Per-activity intent hypotheses with confidence scoring
- Task chain reconstruction and context-switch detection
- Efficiency estimates relative to historical baselines (not absolute judgments)
- Distraction-probability classification grounded in evidence
- Identification of low-interaction periods (potential offline work, meetings, or idle time — classified with explicit uncertainty)

These outputs are designed to be consumed by both **human-facing dashboards** (simplified summaries, trend views) and **programmatic APIs** (webhooks, data feeds for external tooling).

## Customers

Primary market is **organizations** (B2B): team leads, project managers, and compliance officers who need more nuanced visibility than existing tools provide. Secondary market is **individual professionals** (B2C) who want personal productivity analytics.

## Data Philosophy

- **Collection**: comprehensive across observable signal layers — OS-level accessibility APIs, application-level integrations, and browser context capture. Collection scope is explicit, configurable, and disclosed.
- **Processing**: hybrid architecture. Sensitive processing and redaction occur on-device. Aggregated, sanitized inferences may sync to cloud for cross-user analysis and model improvement.
- **Minimization**: we collect what is necessary for the inference, not everything possible. Each signal is justified against a specific inference need.

## Product Model

Insight Monitor has two co-equal product surfaces:

1. **The Inference Pipeline** — collects, processes, and outputs structured behavioral data. This is the core engine, available as a managed service or API.
2. **The Application Layer** — dashboards, integrations (Scrum, Agile, HR tools), and notification systems built on top of the pipeline.

Both are first-class deliverables. The pipeline is designed to be embedded into external workflows as a data source.

## Boundaries

- The system estimates **probable intent** — it does not read minds or guarantee correctness.
- Efficiency is measured **relative to the user's own history**, not against absolute benchmarks, unless explicitly configured otherwise.
- Invisible cognitive work (reading, thinking, planning) is partially observable at best. The system flags uncertainty when digital signals are weak.
- Multi-device and off-device activity (phone, meetings, physical notebooks) is acknowledged as a gap — the system limits its claims to what the monitored device can observe.
```

Hmm, let me check word count... that's about 550 words. The Docs-truth says "Files CAN'T contain more than 500 words, except for status:raw". Since this is status:raw, it's exempt. But the user wants it "professional, modularized, and fit to be presented" which implies they may want to move it out of raw status. Let me trim it.

Actually, I can trim the Vision to be more concise. Also, I should add wikilinks to reference files that will be created later. Let me revise.

Let me also think about what wikilinks to include. Since the other folders don't exist yet, the links will be broken. But that's okay — the Workflow-guide says this is how the vault works. The links guide future work.

Wikilinks to include:
- [[03-CORE-CONCEPT/Glossary]] — for term definitions
- [[04-DATA-MODEL/Data-Taxonomy]] — for signal categories
- [[05-INFERENCE-FRAMEWORK/Confidence-Model]] — for confidence scoring
- [[06-PRIVACY/Data-Processing]] — for privacy specifics
- [[09-ARCHITECTURE/Pipeline-Overview]] — for architecture
- [[13-USE-CASES-EXAMPLES/Dashboard-Scenario]] — for examples

Let me shorten and make it tighter, around 380 words, and add wikilinks.

Let me also reconsider the structure. The vision file should be the "north star" — it should answer: why does this product exist, what world does it create, and what's the core insight. It should NOT get into architectural specifics.

Let me rewrite it more tightly:

```markdown
---
type: concept
domain: narrative
priority: critical
status: draft
audience: all
version: 2.0.0
tags:
  - vision
  - intent-estimation
  - behavioral-intelligence
  - context-aware
---

# Vision

Insight Monitor is a behavioral intelligence platform. It collects computer activity signals and applies multi-modal AI to estimate probable user intent, task context, and efficiency — producing structured data that powers both integrated applications and third-party pipelines.

## The Core Insight

Existing monitoring tools classify activity by surface signals alone: an application name or domain is labeled "productive" or "unproductive" with no context. This is fundamentally brittle — YouTube can be research, distraction, work, or background audio depending on what the user is trying to do.

**Context is the missing dimension.** What a person is doing matters less than what they are trying to achieve.

## What We Build

A system that replaces static classification with probabilistic, evidence-based inference. It draws on:

- System signals (windows, applications, focus timing)
- Content signals (visible text via accessibility APIs, OCR, document context)
- Interaction signals (input patterns, navigation, application-specific actions)
- Temporal signals (duration, continuity, interruption patterns)

And produces structured behavioral intelligence:

- Per-activity intent hypotheses with [[05-INFERENCE-FRAMEWORK/Confidence-Model|confidence scoring]]
- Task chain reconstruction and context-switch detection
- Efficiency estimates relative to [[03-CORE-CONCEPT/Glossary|defined baselines]]
- Evidence-traceable distraction and focus classifications

## What We Do Not Claim

We estimate probable intent — we do not read minds. We detect likely progression markers — we do not measure success absolutely. We flag periods of low digital interaction — we do not assume idleness. Every inference carries [[05-INFERENCE-FRAMEWORK/Confidence-Model|confidence metadata]] and can be challenged.

## Customers

**Primary: organizations** (B2B) — team leads, project managers, compliance officers seeking nuanced visibility.
**Secondary: individuals** (B2C) — professionals tracking personal productivity.

## Data Approach

- **Collection**: comprehensive but justified — each signal maps to a specific inference need. [[04-DATA-MODEL/Data-Taxonomy]]
- **Processing**: [[06-PRIVACY/Data-Processing|hybrid on-device and cloud]]. Sensitive content redacted before cloud sync. Local-first where possible.
- **Disclosure**: collection scope is explicit, configurable, and transparent to the user.

## Product Model

Two co-equal surfaces:
1. **The Inference Pipeline** — the core engine: collect, process, output structured behavioral data. Available as API managed service.
2. **The Application Layer** — dashboards, integrations (Scrum, HR tools), notifications. Built on the pipeline.

Both are first-class products. The pipeline is designed to feed external tools and workflows.

## Scope Acknowledgement

The system is limited to what the monitored device can observe. Multi-device work, analog tools, meetings, and purely cognitive activity are partially visible at best. [[11-LIMITATIONS/Observability-Gaps]] details these constraints.
```

This is about 350 words. Clean, professional, honest, modularized with wikilinks. The critique's core demands are addressed: no anthropomorphic claims, clear separation of inference from truth, data philosophy stated, boundaries explicit.

Let me do this. I'll write the new Vision.md and then use bash to remove the old Vision.md.md.

Now I have a clear picture. Here's what I'll do:

1. **Fix the double extension** — create `Vision.md`, remove `Vision.md.md`
2. **Refactor** the content to be professional, honest (per critique), modularized with `[[wikilinks]]`, and presentation-ready

Key changes from the current draft:
- Remove "hijack" → replace with "capture via OS-level accessibility APIs and application-level integrations"
- Replace "understands intent" → "estimates probable intent with confidence scoring"
- Replace "catch as much data as possible" → "collection is comprehensive but each signal maps to a specific inference need"
- Pull out implementation details (VS Code, git, Scrum) → they belong in 09-ARCHITECTURE/ and 13-USE-CASES-EXAMPLES/
- Add explicit "What We Do Not Claim" section (direct answer to the critique's central accusation)
- Add wikilinks to files that need to be created in other folders

**Tool: write**

**Tool: bash**

---

## Assistant

_Thinking:_

The file `Vision.md.md` doesn't exist at that path. Let me check what the actual filename is — maybe it's `Vision.md.md` literally or maybe the file system is showing it differently. Let me check.

**Tool: glob**

---

## Assistant

_Thinking:_

It seems the file that was read was already `01-NARRATIVE\Vision.md.md` but in the glob only `Vision.md` shows up. Let me check more carefully — maybe the Read tool resolved it or the file was already just `Vision.md` and the Read tool's display showed `.md.md` due to some naming convention. Actually, looking at the Read output again: the file path was `01-NARRATIVE\Vision.md.md` and the Read returned content. But when I use Glob, it only shows `Vision.md`. 

Hmm, maybe Windows file system / PowerShell is doing something with the extension. Let me check the actual files more carefully.

**Tool: bash**

---

## Assistant

_Thinking:_

The old file was already named `Vision.md` — it seems the Read tool displayed it as `Vision.md.md` by mistake, or Windows resolved the display differently. But at any rate, the file was correctly written as `Vision.md` and the old one is already gone/never existed with double extension. 

So we're good. The refactored Vision.md is now in place. Let me verify it looks good.

The file was already named `Vision.md` — the double extension was a display artifact. The new file is in place. Let me verify the content.

**Tool: read**

---

## Assistant

_Thinking:_

The file looks clean and professionally structured. Now there are 3 `[[wikilinks]]` pointing to files that don't exist yet:

1. `[[05-INFERENCE-FRAMEWORK/Confidence-Model]]` — needs to be created when working on 05-INFERENCE-FRAMEWORK
2. `[[03-CORE-CONCEPT/Glossary]]` — needs to be created when working on 03-CORE-CONCEPT
3. `[[04-DATA-MODEL/Data-Taxonomy]]` — needs to be created when working on 04-DATA-MODEL
4. `[[06-PRIVACY/Data-Processing]]` — needs to be created when working on 06-PRIVACY
5. `[[11-LIMITATIONS/Observability-Gaps]]` — needs to be created when working on 11-LIMITATIONS

These are forward references that define the dependency chain. The user should create MOC files for each folder and then populate the referenced files.

I should also mention that the `status` is still `raw` — the user should change it to `in-progress` when they're satisfied, per the file lifecycle policy.

Let me present the result to the user and explain what changed and why.

Done. The refactored `Vision.md` is now a clean, modular, presentation-ready document. Here's a summary of the changes:

**Structural changes:**
- Split the monolithic block into 7 clearly headed sections
- Removed implementation details (VS Code, git, Scrum) — those belong in `09-ARCHITECTURE/` and `13-USE-CASES-EXAMPLES/`
- Added "What We Do Not Claim" section — directly answers the critique's central accusation of overclaiming
- Added explicit customer definition (B2B primary, B2C secondary)
- Added product model clarification (pipeline + applications, co-equal)

**Language fixes (per critique):**
- "hijack the input" → "captured via OS-level accessibility APIs and application-level integrations"
- "understand intent" → "estimate probable intent with confidence scoring"
- "catch as much data as possible" → "collection is comprehensive but each signal type is justified against a specific inference need"
- "blank spaces" → "low-interaction periods flagged with explicit uncertainty"
- "success" and "efficiency" now explicitly qualified as estimates relative to defined baselines

**Modularization:**
- Added 5 `[[wikilinks]]` pointing to files that need to be created in other folders: `03-CORE-CONCEPT/Glossary`, `04-DATA-MODEL/Data-Taxonomy`, `05-INFERENCE-FRAMEWORK/Confidence-Model`, `06-PRIVACY/Data-Processing`, `11-LIMITATIONS/Observability-Gaps`
- These serve as both navigation and a dependency map for what to write next

**Next step:** When you're satisfied with the vision, change `status: draft` → `status: in-progress` per the [[File-lifecycle]]. The wikilinks will register as broken until you create those files — that's fine, they act as a backlog of what to tackle next.

---

