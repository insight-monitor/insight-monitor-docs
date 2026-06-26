---
status: draft
type: session
audience: all
domain: product-value
priority: high
ai-context: medium
creation-date: 2026-06-11
aliases:
  - Review for use cases, limitations and scope drafts
tags:
  - ai-input
  - context-awareness
  - review
---
# Review use cases against narrative with How-to-review guidelines

**Session ID:** ses_14835735affe2dA7dz5BMIDfZ7
**Created:** 6/11/2026, 12:46:05 PM
**Updated:** 6/11/2026, 1:15:22 PM

---

## User

I want you to review the @220-USE-CASES/ based on @100-NARRATIVE/, remember @000-META/How-to-review.md . They are currently drafts, I want to know how far are they from the idea on the project, what can be improved, etc. 

---

## Assistant

 Here is my critical review of the 220-USE-CASES/ drafts against the project's narrative and mission.

### Verdict: Superficial and Structurally Weak

The use-case drafts read like a feature list from a generic workforce monitoring tool, not like a derivation from Insight Monitor’s core thesis. They describe *what could be monitored* rather than *how the system’s probabilistic intent-inference changes the value of monitoring* in each domain. They are too short, too shallow, and too safe.

---

### Structural & Metadata Faults

| Issue                                                                                                                                                                                                 | Why it matters                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **No YAML headers.** Every file uses `status: raw` and nothing else. Compared to `100-NARRATIVE/`, which has `type`, `domain`, `priority`, `audience`, and `version`, these look like unloved scraps. | Inconsistent metadata breaks automated indexing and signals low maturity.                                                                                                            |
| **No links back to narrative concepts.** Words like “probabilistic,” “confidence,” “uncertainty,” or “context” appear in the narrative but are absent from all use cases except `Intent-vs-Category`. | If these use cases are intended to demonstrate the product vision/concept, then they must be able to translate the abstract ideas in the narrative into concrete, tangible examples. |
| **`MOC-Use-Cases.md` links to `[[MOC-Meta]]` which does not exist in the repo.**                                                                                                                      | Broken wikilink on day one.                                                                                                                                                          |
| **No prioritization or status differentiation.** All use cases are treated as equal, which is unlikely to be true.                                                                                    | Someone needs to decide which use case justifies the engineering effort first.                                                                                                       |

---

### Content-Level Weaknesses

#### 1. They describe *monitoring*, not *inference*
The Mission says the product “estimates probable task intent from multimodal computer activity signals.” Yet every use case except `Intent-vs-Category` simply lists **observable patterns** and **inference conclusions** as if the second naturally follows the first, with no discussion of:
- What signals are combined
- How confidence is scored
- What happens when inference is weak
- How the system distinguishes edge cases

**Example:** `Software-Development.md` says the system can infer “Blocked state (repeated same search, low activity with frustration signals).” What is a “frustration signal”? Keystroke cadence? Mouse jiggle? It is hand-waved.

#### 2. Critical narrative tension is missing
The Narrative’s whole point is that *the same observable behaviour can correspond to radically different goals.* The use cases should stress-test this. They do not.

- `Knowledge-Work.md` raises the exact problem—*Cannot distinguish deep analysis from skimming*—but offers no solution, no probabilistic handling, no fallback. It just shrugs.
- `Personal-Productivity.md` says “All data stays local by default,” but `Vision.md` already declared a hybrid on-device/cloud model. Contradiction or at least unaddressed tension.

#### 3. Some use cases contradict the Vision directly
`Compliance-Monitoring.md` frames the system as an “evidence log” that “acts as a witness, not a judge.” This is not a failure per se, but it sits uncomfortably with the Vision, which emphasizes *probabilistic behavioural intelligence* and *context-aware interpretation*. Compliance here is presented as old-school audit logging with screenshots. If that is a valid product direction, the narrative must justify it; if it is a side capability, it should not be elevated to a primary use case without explanation.

#### 4. Missing depth on the *why* and *how*
Take `Customer-Support.md`. It identifies a real problem (calls produce offline screen activity), then says: “Integration with telephony or calendar data improves accuracy here.” That is the end of the thought. There is no discussion of:
- What happens without that integration
- How the system flags low-confidence periods
- Whether the manager dashboard treats idle calls as productive, unknown, or distracting

The use cases read like they were written to check a box, not to explore the product concept.

---

### What Is Good (And Why)

| File | What works |
|---|---|
| `Intent-vs-Category.md` | This is the only use case that actually executes the narrative thesis. It contrasts naive vs. contextual classification, cites specific signals, and produces an explainable outcome. It is still too short, but it is structurally correct. |
| `Meeting-Context.md` | Identifies a genuine observability gap (`Observability-Gaps` from Vision) and explicitly states what the system **cannot** do. Admitting limits is aligned with the mission’s emphasis on uncertainty. |

---

### Specific Recommendations

1. **Elevate `Intent-vs-Category` as the template.** Every other use case should follow its structure: scenario → naive approach → contextual approach → outcome → why it matters. If a use case cannot be written this way, it may not belong.

2. **Add a “Confidence and Uncertainty” section to every use case.** The mission explicitly mentions “evidence-traceable distraction and focus classification” and “detection flagged with explicit uncertainty.” Show how that looks in practice.

3. **Distinguish “primary customer”, “data approach”, and “boundaries” explicitly, per Vision.md.** This forces each use case to answer: who pays, what gets collected, and where does the system stop?

4. **Drop or deeply revise `Compliance-Monitoring.md`.** It is the weakest fit with the narrative. If retained, it must explain how probabilistic intent inference improves compliance (e.g., detecting anomalous access *patterns*, not just logging screenshots).

5. **Add a “signal→inference→action” table to each case.** Show the reader the pipeline: what is collected, what is inferred, and what the downstream system (dashboard, API, alert) does with it.

6. **Fix the wikilink to `[[MOC-Meta]]`.**

7. **Add YAML front matter consistent with `100-NARRATIVE/`.** At minimum: `type`, `domain`, `priority`, `audience`, `version`. Consider adding `confidence-model`, `data-sources`, and `primary-signal` for use-case-specific metadata.

---

### Overall Assessment

The use cases are currently at the level of a brainstorming session. They identify plausible domains but do not yet prove that the product’s *probabilistic, context-aware, uncertainty-explicit* approach is valuable in each one. They need more rigor, more signal-to-inference detail, and explicit alignment with the narrative’s central argument. **[The project needs 1–2 strong, detailed use cases more than it needs 7 shallow ones.]**

---

## User

Good, now do the same with @500-SCOPE/, please review them

---

## Assistant

 Here is my critical review of the `500-SCOPE/` drafts against `100-NARRATIVE/` and `000-META/How-to-review.md`.

---

## Verdict: Promising but incomplete.
The scope documents are conceptually closer to the narrative than the use cases, but they are inconsistent, underpowered, and contain unresolved contradictions with the mission and vision. Several files are exposed as placeholders rather than specifications.

---

## Structural & Metadata Faults

**Same problem as 220-USE-CASES: YAML headers are minimal.** Every file uses only `status: raw`. There is no `type`, `domain`, `priority`, `audience`, or `version`—even though `Included-Capabilities.md` and `Error-Philosophy.md` conceptually deserve `priority: critical` and `type: policy`. This metadata gap makes the directory feel like an afterthought compared to the maturity of `100-NARRATIVE/`.

**`MOC-Scope.md` links to `[[MOC-Meta]]`, which does not exist in the repo.** The exact same broken wikilink pattern as `MOC-Use-Cases.md`. If the project uses these as living maps-of-content, a dead link on the landing page is a credibility hit.

---

## File-by-File Critique

### `Included-Capabilities.md` — Strong concept, shallow execution

**What works:** This is the closest any of the drafts come to operationalizing the mission. It translates "multimodal computer activity signals" and "structured behavioral intelligence" into concrete system actions: signal collection, VLM inference, confidence scoring, contextual classification, user transparency, redaction, and audit trails. It correctly treats the inference pipeline and the transparency/accountability layer as first-class capabilities.

**Where it fails:**

| Fault                                                                                     | Why it undermines the project                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **"A vision-language model processes screenshots" is presented as settled architecture.** | The Mission says the system collects *four signal types* (system, content, interaction, temporal). The Included Capabilities reduce this to "endpoint activity signals" plus screenshots run through a VLM. Where are the interaction and temporal signals in the capability list? Where is the signal fusion layer? The VLM is one component, not the whole system. |
| **Confidence scoring is described but not defined.**                                      | "0.0–1.0 reflecting the system's certainty" is tautological. What does 0.7 mean? What threshold triggers the "flagged as uncertain" behavior? The Mission references `[[confidence scoring]]` and `[[defined baselines]]` as missing links—this file is where they should be introduced, not deferred.                                                               |
| **No mention of task chain reconstruction or context-switch detection.**                  | These are explicitly listed as outputs in the Mission. Omitting them from Included Capabilities creates a gap: either the system does not do them (contradicting the mission) or they are not yet considered core capabilities.                                                                                                                                      |
| **"Contextual classification" uses undefined axes.**                                      | The axes listed are `(work-relevant, research, distraction, personal, ambiguous)`. Why these five? The Narrative's whole point is that activity only has meaning relative to intent, yet "research" and "work-relevant" are treated as peers rather than nested or probabilistic. This is a regression from the mission's "per-activity intent hypotheses."          |
| **No link to Error Philosophy or Data Minimization.**                                     | The included capabilities contain obvious touchpoints with other scope files (redaction, user transparency, audit trail), but there is no cross-referencing.                                                                                                                                                                                                         |

**Verdict:** This file should be the backbone of the project's functional specification. Instead, it reads like a marketing bullet list. It needs internal architecture detail, cross-references to other scope documents, and explicit alignment with the four signal types and five output types declared in the Mission.

---

### `Excluded-Capabilities.md` — The strongest file in the directory

**What works:** This file is disciplined, well-structured, and directly derived from the narrative's core tensions. Every exclusion is a specific negation of a common monitoring-tool temptation, and each is justified.

- "No intent certainty" ↔ Narrative: "Every conclusion remains uncertain."
- "No efficiency measurement" ↔ Mission: "Efficiency estimates relative to defined baselines" (noting the system *tracks* but does not *measure* absolutely).
- "No covert surveillance" ↔ Narrative: "The goal is not surveillance."
- "No replacement of human judgement" ↔ Vision: "Organizations understand work patterns... using evidence-based probabilistic models" (models inform, humans decide).
- "No cross-device coverage" and "No offline/thinking detection" ↔ Vision: "Boundaries — limited to monitored-device signals."

**Where it needs improvement:**

| Fault                                                                                              | Why it matters |
| -------------------------------------------------------------------------------------------------- | -------------- |
| **The distinction between "No intent certainty" and the system's actual outputs is not explored.** | The Mission says the system produces "per-activity intent hypotheses with confidence scoring." An intent hypothesis sounds like a claim about intent, even if probabilistic. This file should explicitly reconcile how a system that "estimates probable task intent" can simultaneously declare "no intent certainty." Language like "produces hypotheses with calibrated uncertainty, never claims to know intent" would close the tension. |
| **"No efficiency measurement" contradicts the Mission's "efficiency estimates."**                  | The Mission explicitly lists "efficiency estimates relative to defined baselines" as an output. If the system produces efficiency estimates, how can "no efficiency measurement" be an excluded capability? The Scope needs to distinguish between *relative, baseline-anchored estimates* (included) and *absolute, universal efficiency measurement* (excluded). Currently, this reads as a direct contradiction.                           |

**Verdict:** The best-executed file. Fix the tension with the Mission on efficiency, and clarify the "hypotheses vs. certainty" language, and this becomes a model for the rest of the project.

---

### `Error-Philosophy.md` — Critically important, dangerously thin

**What works:** The principle that "the cost of a false accusation exceeds the cost of a missed detection" is precisely the ethical stance the Narrative demands. The asymmetric treatment of false positives and false negatives in productivity classification is correct. The stance on sensitive data—"redaction must be aggressive even at the cost of losing legitimate context"—is appropriately conservative.

**Fatal flaws:**

| Fault | Why it breaks the project |
|---|---|
| **It is 22 lines long for a file about error tolerance.** | There is no mention of *how* the system implements these tradeoffs. Is there a tunable threshold? Is the "default to ambiguous" enforced at the inference layer, the classification layer, or the dashboard layer? Without implementation detail, this is a wish, not a specification. |
| **No connection to confidence scoring.** | The Included Capabilities mention "0.0–1.0" confidence. This file should map confidence thresholds to actions: e.g., `>0.8`: classify; `0.5–0.8`: flag for review; `<0.5`: mark ambiguous, do not show to manager. That mapping does not exist. |
| **No discussion of calibration.** | A model that is overconfident (predicts 0.9 but is wrong 40% of the time) is worse than a model that is underconfident. The philosophy demands "minimize harm" but does not specify whether the system must be calibrated, and to what standard. |
| **"Tolerance: near zero" for false positives is inoperable.** | "Near zero" is not a specification. What is the target false positive rate? 1%? 0.1%? Without a number, engineering cannot optimize for it. |
| **"Acceptable within reason" for false negatives in sensitive data is a liability.** | If PII leaks "within reason," the product is unviable in regulated environments. The redaction system should be held to a higher standard than "acceptable leakage." This language contradicts Included Capabilities' promise of aggressive redaction. |

**Verdict:** This file is too short for its responsibility. Error philosophy is where the product's ethics become engineering constraints. It needs threshold definitions, calibration requirements, a decision matrix, and explicit cross-references to confidence scoring and the compliance use case.

---

### `Data-Minimization.md` — Good structure, weak enforcement

**What works:** The minimum viable signal set is well-defined and appropriately constrained. The exclusion of keystroke logging, clipboard, accessibility API reads, filesystem traversal, and microphone/camera is correct for the stated privacy posture. The sensitivity rule—"no signal is collected if a less invasive signal can produce equivalent inference value"—is the right principle.

**Where it fails:**

| Fault | Why it matters |
|---|---|
| **"Minimum viable signals" are not connected to the four signal types from the Mission.** |The Mission lists system, content, interaction, and temporal signals. The minimum set includes system signals and one content signal (screenshots), but where are the interaction and temporal signals? If the minimum viable product lacks them, the mission must change, or this file is incomplete. |
| **"Periodic screenshots (configurable interval, minimum 30s)" is an arbitrary number.** | Why 30 seconds? If the interval is a key privacy knob, it needs justification: bandwidth, storage, inference cost, or user tolerance. Without it, the number looks pulled from thin air. |
| **No mention of local-only mode.** | The Vision states a "hybrid on-device and cloud processing" approach with "sensitive redaction before sync." This file should specify what happens in the on-device-only path: what is stored locally, what is synced, and under what user control. |
| **The sensitivity rule is unenforceable as stated.** | "Each additional signal must be justified by measurable improvement in inference accuracy" requires a benchmark, a process for evaluating signals, and a governance mechanism. None of these exist in the document. This is another wish. |

**Verdict:** A reasonable start, but it needs to be grounded in the signal architecture and the hybrid data strategy. The sensitivity rule is policy without process.

---

### `Target-User-Profile.md` — Safe, boring, and incomplete

**What works:** The primary/secondary distinction matches the Vision. The company-owned vs. BYOD split is a real consideration in enterprise sales. The "out of scope" exclusions are appropriate.

**Critical gaps:**

| Fault | Why it matters |
|---|---|
| **"Employer can mandate minimum telemetry during working hours" contradicts the entire Narrative.** |The Narrative says "the goal is not surveillance" and "replacing simplistic activity labels with evidence-based behavioral understanding." Mandated telemetry, even with transparency, is the same dynamic that creates adversarial employee-employer relationships. The file does not address this tension. If the product is sold to employers who mandate it, the "voluntary, transparent, contestable" thesis is undermined. |
| **No user personas beyond "enterprise teams" and "individual users."** | These are markets, not personas. What does a day in the life of a target user look like? What problem do they need solved urgently enough to buy this? The Narrative's examples (developer watching YouTube, student reading Reddit) are richer than this file. |
| **Roles requiring "high privacy guarantees" are excluded, but Compliance-Monitoring.md exists in the use cases.** | If therapy, journalism, and legal defense are out of scope, how does the company reconcile that with the compliance use case, which is explicitly aimed at regulated industries? Healthcare and finance also require high privacy guarantees. Either the compliance use case is wrong, or this exclusion is too broad. |
| **No mention of IT administrators, compliance officers, or HR managers as distinct stakeholders.** | These are the people who actually buy and operate workforce monitoring tools. A "target user profile" that only names "enterprise teams" and "individual users" misses the economic reality. |

**Verdict:** This file needs personas, stakeholder analysis, and an honest confrontation with the surveillance tension. It is currently too generic to guide product decisions.

---

## Meta-Level Issues

### Contradictions between 500-SCOPE files and 100-NARRATIVE

| Narrative/Vision/Mission Claim | 500-SCOPE Contradiction |
|---|---|
| "Hybrid on-device and cloud processing" (Vision) | `Data-Minimization.md` does not mention on-device-only mode or sync boundaries. |
| "Efficiency estimates relative to defined baselines" (Mission) | `Excluded-Capabilities.md` says "No efficiency measurement," creating a direct conflict. |
| "The goal is not surveillance" (Narrative) | `Target-User-Profile.md` allows employer-mandated telemetry without addressing the surveillance dynamic. |
| System produces "task chain reconstruction and context-switch detection" (Mission) | Not mentioned in `Included-Capabilities.md`. |
| Four signal types: system, content, interaction, temporal (Mission) | Only system and one content signal appear in `Included-Capabilities.md` and `Data-Minimization.md`. |

### What is missing entirely

- **No `Product-Architecture.md` or `Data-Strategy.md` referenced in Vision.** These are marked as `[[wikilinks]]` in the narrative but do not exist in the repo. The scope directory is where they should live.
- **No `Observability-Gaps.md` referenced in Vision.** This is a critical boundary document and should be in scope.
- **No `Market.md` referenced in Vision.** Who buys this? The scope should contain market positioning.
- **No confidence-scoring specification.** The Mission and Included Capabilities both gesture at it, but there is no document defining how it works.
- **No decision-making framework for edge cases.** The Error Philosophy says "default to ambiguous," but there is no documented exception process for when a manager overrules the system, or when a user contests a classification.

---

## Summary & Recommendations

| Priority | Action |
|---|---|
| **Critical** | Resolve the direct contradiction on "efficiency measurement" between `Excluded-Capabilities.md` and `Mission.md`. |
| **Critical** | Add missing scope files for `Product-Architecture`, `Data-Strategy`, `Observability-Gaps`, and `Market`; or remove the wikilinks from `Vision.md`. |
| **High** | Expand `Error-Philosophy.md` to include confidence thresholds, calibration requirements, and a decision matrix. It should reference `Included-Capabilities.md` and `Data-Minimization.md`. |
| **High** | Connect every file in `500-SCOPE` to the four signal types and five output types from `Mission.md`. If a signal or output is not included, justify its exclusion. |
| **High** | Add `type`, `domain`, `priority`, `audience`, and `version` to all YAML headers. |
| **Medium** | Revise `Target-User-Profile.md` to include personas for IT administrators and compliance officers, and to address the surveillance tension honestly. |
| **Medium** | Fix all broken `[[MOC-Meta]]` links. |
| **Medium** | Ground `Data-Minimization.md` in the hybrid data strategy; define local-only vs. sync paths. |
| **Low** | Add a "how capabilities are validated" section to `Included-Capabilities.md`—how does the team know a capability is truly included and working? |

**Overall assessment:** `500-SCOPE/` is heading in the right direction but needs significantly more rigor. The files identify the right topics but do not yet resolve the tensions within the project's own narrative, mission, and vision. It is a sketch, not a specification.

---

## User

Good, finally do the same for @520-LIMITATIONS/ . Review it.

---

## Assistant

  Here is my critical review of the `520-LIMITATIONS/` drafts against `100-NARRATIVE/` and `000-META/How-to-review.md`.

---

## Verdict: Conceptually coherent but structurally underdeveloped and internally repetitive. 
The limitations directory is the most honest part of the project so far—it correctly identifies what the system cannot do—but it is padded with placeholders, lacks actionable depth, and repeats content already present elsewhere in the project without adding new analysis.

---

## Structural & Metadata Faults

**Same persistent issues as 220-USE-CASES and 500-SCOPE:**

- **Minimal YAML headers:** Every file is just `status: raw`. No `type`, `domain`, `priority`, `audience`, or `version`. Compared to the maturity of `100-NARRATIVE/`, this looks lazy.
- **Broken wikilink on `MOC-Limitations.md`:** `[[MOC-Meta]]` does not exist in the repo. Same exact bug as the other directories. If this is systemic, the entire wikilink ecosystem is currently non-functional.
- **No cross-referencing between limitation files and scope/use-case files.** These are not isolated criticisms—they are actively connected to `Excluded-Capabilities.md`, `Error-Philosophy.md`, and multiple use cases, but there are no links.

---

## File-by-File Critique

### `Confidence-Uncertainty.md` — Correct thesis, no implementation

**What works:** The core principle is exactly right: "Every output below a confidence threshold must be explicitly flagged as uncertain." This directly operationalizes the Narrative's "Every conclusion remains uncertain" and the Mission's "Per-activity intent hypotheses with confidence scoring."

**Fatal flaw: There is no confidence threshold.** The file says "below a confidence threshold" without ever defining what that threshold is. This makes the entire document inoperative. Compare with `Error-Philosophy.md` (500-SCOPE), which at least gestures at "near zero" tolerance for false positives—still vague, but this file does not even try. `Included-Capabilities.md` defines a "0.0–1.0" scale without any mapping to behavior. All three documents together still fail to produce a usable confidence framework.

**Missing: What does "flagged as uncertain" mean in practice?** Is the flag shown to the end user? The manager? Only in the audit log? Does it stop downstream classification, or just annotate it? Without knowing the audience for the uncertainty flag, this is a vacuous requirement.

---

### `Domain-Applicability.md` — The most useful file, with one hidden contradiction

**What works:** This is the only file in the directory that adds genuinely new analysis. The well-suited/poorly-suited split is grounded in the system's actual constraints (digital visibility, task traceability). The principle at the end—"The system must disclose its suitability for a given domain"—is a strong, actionable rule.

**Hidden contradiction with 220-USE-CASES:** `Knowledge-Work.md` and `Compliance-Monitoring.md` both target domains that `Domain-Applicability.md` flags as "poorly suited" (creative work/research and legal work, respectively). Either the use cases are wrong, or this file is wrong, or there is a missing piece of nuance. If "research" is poorly suited because of "long reading periods" and "multi-tab information gathering that looks like distraction," then the `Knowledge-Work.md` use case that claims the system can infer "reading comprehension phase" is at least partially false.

**The "poorly suited" list is too broad.** "Creative work (design, writing, strategy — high offline cognition)" is almost every knowledge worker. If the system is poorly suited to design and writing, it is poorly suited to a massive chunk of the target market defined in `Target-User-Profile.md`. This limitation list, if accurate, invalidates the enterprise value proposition.

**Verdict:** A genuinely useful file, but it exposes a tension with the use case directory that the project must resolve.

---

### `Gaming-Adversarial.md` — Accurate but shallow

**What works:** Identifies real manipulation vectors: second-device tricks, visible-workflow shaping, context-cue exploitation, and screenshot interval exploitation. These are all credible threats.

**Why it fails:**

| Fault | Why it undermines the project |
|---|---|
| **"Known gaming vectors" are not connected to specific system capabilities.** | If `Included-Capabilities.md` lists signal collection and inference, this file should explain which specific signals are vulnerable to which gaming vectors. For example, does the system only use screenshots (vulnerable to interval exploitation), or does it also use focus events and input patterns (harder to fake)? |
| **"Surface evidence so that patterns of gaming are detectable" is hand-waved.** | This sounds good but is not a mitigation. What evidence? How is it surfaced? Who reviews it? The file says the system should make gaming "detectable rather than hidden" but does not explain what that looks like. |
| **No reference to `Error-Philosophy.md`.** | The Error Philosophy already defines a "default to ambiguous" stance. This file should show how that stance interacts with detected gaming: does the system become more conservative, or flag suspicious patterns for review? |

**Verdict:** A list of threats without a threat model. It reads like a checklist from a security audit, not a principled analysis of adversarial resilience.

---

### `Multi-Device-Blindspots.md` — Duplicate content, broken formatting

**Structural fault:** The first line of the file has an unexpected tab character (`\t# Multi-Device Blindspots`). This is sloppy.

**Substantive fault:** This file repeats content already present in `Excluded-Capabilities.md` (500-SCOPE) and `Vision.md` (100-NARRATIVE), but adds nothing new. `Excluded-Capabilities.md` already states: "Activity on unmonitored devices (personal phones, physical whiteboards, paper) is invisible." `Vision.md` already mentions: "Multi-device, analog, and purely cognitive work is partially visible at best."

What should this file add that those do not? Specific examples: yes, but all already present. Mitigations: "Acknowledge the blindspot in all reports" and "Never claim full activity visibility" are restatements of the Narrative's core principle. "Consider device-agnostic signals (calendar, VPN auth logs) as auxiliary context" is new but underexplored—how would calendar or VPN logs resolve multi-device fragmentation? A calendar event does not tell you what the user is doing on their phone.

**Verdict:** Redundant. Either merge this back into `Excluded-Capabilities.md` or expand it with genuinely novel analysis (e.g., how calendar integration fails to solve the multi-device problem, which specific inferences are most corrupted by device gaps).

---

### `Offline-Cognition.md` — Honest but recycles narrative without advancing it

**What works:** The list of offline cognition activities is thorough (reading, thinking, comparing, discussing, sketching, reflecting). The risk statement is clear. The mitigations are sensible defaults.

**Why it is weak:**

- **"Allow users to mark 'focus mode' periods" is the oldest workaround in monitoring software.** Self-reported focus time is notoriously unreliable, especially in adversarial or performance-review contexts. If the system is supposed to be "evidence-based," relying on user self-attestation is a regression.
- **"Surface uncertainty when screen activity is low but task continuity exists" contradicts itself.** Task continuity is inferred from screen activity (switches, saves). If screen activity is low, how does task continuity exist? This needs explicit modeling, not a hand-wave.
- **Reuse without reference:** The Narrative says: "An employee sitting in a document editor may be distracted." This file restates the same idea with more examples, but does not add new analytical depth.

**Verdict:** Acceptable as a boundary marker, but the proposed mitigations are either naive or already exist in every competing product. It does not advance the project's differentiation.

---

### `Task-Ambiguity.md` — The most important limitation, the least developed

**What works:** The examples are concrete and directly illustrate the Narrative's central thesis: "The same observable behavior can correspond to radically different goals."

**Fatal flaw: This is only 15 lines long for the *core problem of the entire product.*** The Narrative builds to this idea in nine paragraphs. The Mission is built around solving it. And this file—the dedicated exploration of the problem—spends exactly four bullet points on it and then declares "classification accuracy is bounded by the ambiguity inherent in surface signals."

**Missing entirely:**

| What should be here | Why it matters |
|---|---|
| **A taxonomy of ambiguity types.** | Is the ambiguity structural (same app, different tasks), contextual (same task, different intent), or temporal (same action, different phase)? | The system needs different inference strategies for different ambiguity types. |
| **How the system handles each type of ambiguity.** | The Mission says "per-activity intent hypotheses with confidence scoring." What does a high-ambiguity hypothesis look like? Is it multi-class ("could be A or B"), or low-confidence single-class? | Without this, the file is just a complaint, not a specification. |
| **Integration with project management tools is mentioned as a solution, but no PM tools are referenced in the rest of the project.** | Is Jira/Linear/Asana integration in scope? | This is a major feature request disguised as a limitation. |
| **How does this limitation interact with `Error-Philosophy.md`?** | If task ambiguity is high, does the system default to "ambiguous"? Does it require more signals? | The error philosophy demands a default stance; this file should show when that stance is triggered. |

**Verdict:** A critical file that reads like a stub. This should be the longest and most analytically dense file in the directory, not the shortest.

---

## Meta-Level Issues

### Severe internal contradiction between directories

| `520-LIMITATIONS/` Claim                                                                                 | Contradiction in Other Directories                                                                                                  |
| -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| "Research" and "creative work" are poorly suited (`Domain-Applicability.md`)                             | `Knowledge-Work.md` (220-USE-CASES) presents research and writing as core use cases.                                                |
| "The system cannot distinguish deep thinking from being away from the keyboard" (`Offline-Cognition.md`) | `Included-Capabilities.md` (500-SCOPE) does not list any capability for handling this, despite it being a known limitation.         |
| "Classification accuracy is bounded by the ambiguity inherent in surface signals" (`Task-Ambiguity.md`)  | `Included-Capabilities.md` claims "contextual classification" as a capability, suggesting it partially solves this. Which is right? |
| "No monitoring system can fully prevent gaming" (`Gaming-Adversarial.md`)                                | None of the use cases or scope documents address what happens when gaming is detected. No alert, no escalation, no fallback.        |

### What is completely missing from 520-LIMITATIONS

- **No `Observability-Gaps.md` file.** This is referenced in `Vision.md` as a critical boundary document: "Multi-device, analog, and purely cognitive work is partially visible at best. `[[Observability-Gaps]]`". It does not exist here or anywhere else in the repo.
- **No `Confidence-Scoring.md` specification.** The Mission, Included Capabilities, and Error Philosophy all reference confidence scoring, but there is no dedicated document that defines the scale, calibration, thresholds, or UI representation.
- **No performance or resource limitations.** What happens when the VLM inference is too slow for real-time classification? What is the latency budget? What is the accuracy/performance tradeoff? None of the engineering constraints are documented.
- **No legal or regulatory limitations.** The system handles sensitive data, provides audit trails, and targets regulated industries, but there is no file addressing GDPR, CCPA, HIPAA, or workplace surveillance laws.

---

## Summary & Recommendations

| Priority | Action |
|---|---|
| **Critical** | Expand `Task-Ambiguity.md` to become the flagship limitation document. It should contain a taxonomy of ambiguity, per-type handling strategies, and explicit connections to error philosophy and confidence scoring. |
| **Critical** | Resolve the contradiction: `Domain-Applicability.md` says research/creative work is "poorly suited," but `Knowledge-Work.md` treats it as a core use case. Either merge them with nuance or drop one. |
| **High** | Add missing `Observability-Gaps.md` file, as it is promised in `Vision.md`. |
| **High** | Add `Confidence-Scoring.md` to define thresholds, calibration, and UI behavior. Currently referenced everywhere, defined nowhere. |
| **High** | Merge `Multi-Device-Blindspots.md` back into `Observability-Gaps.md` or expand it with novel analysis (e.g., specific inference corruption patterns). As written, it is redundant with `Excluded-Capabilities.md`. |
| **Medium** | Connect every limitation file to `Error-Philosophy.md` and `Excluded-Capabilities.md`. Show, for each limitation, what the system does when it hits that boundary. |
| **Medium** | Add legal/regulatory limitation file (GDPR, CCPA, workplace surveillance law). This is a product that monitors employees; legal limitations are not optional. |
| **Medium** | Add engineering/resource limitation file (latency, inference cost, accuracy tradeoffs). |
| **Low** | Fix the tab character in `Multi-Device-Blindspots.md`. |
| **Low** | Add `type`, `domain`, `priority`, `audience`, and `version` to all YAML headers. |

**Overall assessment:** `520-LIMITATIONS/` is the most honest section of the project—it correctly admits what the system cannot do—but it is also the most repetitive and underdeveloped. The files identify genuine boundaries but do not explore their implications deeply enough to guide design or reassure users. The `Task-Ambiguity.md` file, in particular, is a missed opportunity: it should be the heart of this directory, and it is currently a stub. The directory as it stands is more useful as a checklist of concerns than as a specification of boundaries.

---

