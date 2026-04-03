## time log

| # | event | date | time | timezone | duration | description |
|---|-------|------|------|----------|----------|-------------|
| 1 | created | 2026-04-03 | 09:34:43 | EDT | 8m 26s | protocol-only version of RPD skill improvement note — 15 techniques extracted from MC3-60592 analysis — time log and session-specific handoff removed — all technique content, examples, and revision plan preserved |

---

# RPD skill improvement note — analytical protocol
**Version:** v2
**Purpose:** Document the analytical techniques, patterns, and protocols developed through RPD decision analysis. Goal is to improve RPD decision analysis for future files.

**What this file is:** A structured library of 15 analytical techniques for reviewing rejected RPD decisions in Canadian refugee law. Each technique identifies a specific category of error RPD members make, provides the trigger signal that activates it, numbered action steps for applying it, a structured output object, and an agent integration section specifying how it connects to other techniques and to the rpd-decision-analysis and rad-memorandum skills. The file is designed for three simultaneous uses: (1) as a reference guide for human legal practitioners reviewing RPD decisions; (2) as a Claude skill file governing how Claude reads and analyzes RPD decisions; and (3) as a technical specification for building RPD analysis agents in OpenClaw using Claude Code. Human-readable legal content is authoritative and unchanged. Technique structure — triggers, action steps, output objects, agent integration — is optimized for Claude and Claude Code agent execution.

---

## Revision plan

**Scope:** Type 2 (logical sequence and structure) and type 3 (Claude instruction quality) only. Human-readable legal content — definitions, examples, legal framing, jurisprudential references — untouched.

**What gets revised or added in every technique:**
- Trigger — made more specific, pattern-recognizable, and unambiguous for Claude; must be an observable signal not a category judgment
- How to apply — rewritten as numbered action steps, not descriptive prose; no embedded description in the action section
- Output format — new element added to each technique: what Claude should produce when the technique activates

**Type 2 fixes applied across all techniques:**
- Identify circular dependencies and add explicit cross-references where one technique depends on output of another
- Ensure each technique is self-contained unless dependency is explicitly stated
- Reorder reading sequence summary to match linear document-reading workflow

**Technique-by-technique issue log:**

| Technique | Type 2 issue | Type 3 issue | Type 4 issue (agent-ready) | Status |
|---|---|---|---|---|
| 1 — Chairperson guideline | Must be identified before techniques 2–15 — prerequisite not flagged | How-to-apply mixed description with action steps | Output object needed for downstream techniques | Revised |
| 2 — Section heading | Independent but output not defined | How-to-apply has embedded description; profile extraction sub-technique needs its own action steps | Profile table output needs consistent field names for agent parsing | Revised |
| 3 — RPD summary | Depends on identifying summary boundary first — not stated as step 1 | Three-step method correct but each step needs a concrete action verb | Output object needs field names consistent with agent error inventory | Revised |
| 4 — Two-reading rule | Meta-protocol applying to every technique — not flagged as such | Second reading questions are prose, need to be a checklist | Nested call architecture; Reading 1 and Reading 2 as structured objects; orchestrator note for error inventory aggregation | Revised |
| 5 — Member heard vs. understood | Depends on transcript access — not flagged | Key categories are explanatory, need to be triggers | Input: transcript exchange + RPD paragraph; output: misunderstanding object with typed fields | Revised |
| 6 — Positive evidence pass | Depends on completing error-finding pass first — not flagged | Evidence categories need observable signals, not descriptions | Input: adverse finding; output: positive evidence objects for each category | Revised |
| 7 — Discard and weaponize | Independent | Trigger clear; how-to-apply needs one additional action step | Input: discarded document reference; output: binary discard/weaponize flag with paragraph citations | Revised |
| 8 — Information chain | Depends on identifying document first — links to technique 7 | Chain-tracing questions need numbered sequence | Input: corroborating document; output: chain object with steps and precision-loss flags | Revised |
| 9 — Omissions vs. external causes | Depends on technique 8 output | Obstacle categories need observable triggers | Input: alleged omission text; output: cause-attribution object with obstacle type and source | Revised |
| 10 — NDP engagement check | Depends on identifying finding first; technique 14 should precede it | Action steps need: find finding → identify subject → check table → flag if no citation | Input: adverse finding subject; output: NDP mapping object with matched tabs and engagement status | Revised |
| 11 — Clarification sequence | Depends on transcript access | "Read the full exchange" needs specific observable steps | Input: transcript exchange block; output: clarification object — first-attempt answer, clarified answer, version RPD used | Revised |
| 12 — Member's own questions | Depends on transcript; links to technique 11 | Trigger clear; needs output action | Input: member-initiated question and answer; output: binary flag — answer referenced in decision yes/no | Revised |
| 13 — Behavioral expectation test | Depends on technique 1 output — not flagged | Test question good; needs binary output: yes/no Guideline 9 flag | Input: RPD paragraph with if-X-then-Y structure + technique 1 output; output: expectation object with source classification and Guideline flag | Revised |
| 14 — NDP version check | Independent; prerequisite for technique 10 — should precede it in reading sequence | Action steps need: find disclosed version → find every footnote → compare → flag mismatch | Input: NDP version in disclosure + every NDP footnote in decision; output: version-check object with match/mismatch per footnote | Revised |
| 15 — Representation failure | Depends on transcript; should follow techniques 5 and 11 | Indicator list good; needs binary output per indicator | Input: hearing record; output: representation failure inventory with binary flag per indicator and linked adverse finding | Revised |

**Proposed revised reading sequence (replaces current summary at end of file):**

1. Identify any named Chairperson's Guideline (Technique 1) — before reading anything else
2. Identify the summary boundary (Technique 3 prerequisite)
3. Read the summary section — three-step check (Technique 3)
4. Verify NDP version in evidence (Technique 14) — before any NDP citation is trusted
5. Read each section heading (Technique 2)
6. For each adverse finding — apply two-reading rule (Technique 4 — meta-protocol)
7. Within each reading — NDP engagement check (Technique 10)
8. Within each reading — omissions vs. external causes (Technique 9)
9. For documents discarded or criticized — discard and weaponize (Technique 7), then information chain (Technique 8)
10. For transcript exchanges used adversely — clarification sequence (Technique 11), then member heard vs. understood (Technique 5)
11. For member-inserted questions — Technique 12
12. Positive evidence pass across full section (Technique 6)
13. On SOGIESC files — behavioral expectation test after each adverse finding (Technique 13)
14. Representation failure inventory after full transcript review (Technique 15)

---

Each entry below captures one analytical technique. Every technique is:
- Named and defined
- Explained in terms of what it detects
- Illustrated with a concrete example from the source case
- Marked with a trigger — the signal in an RPD decision that activates the technique

These techniques apply to any RPD rejection. They are not case-specific.

---

## Technique 1 — chairperson guideline identification and compliance tracking

**Definition:** When the RPD expressly names a Chairperson's Guideline, that naming does two things simultaneously: it identifies the specific legal standard the decision will be measured against, and it removes any guesswork about which framework applies. A Guideline 9 violation is a ground of appeal regardless of whether the RPD ever mentioned Guideline 9. But when the RPD names the guideline itself, the compliance standard is stated in the decision's own words. You do not have to argue that the guideline should have applied — the RPD already confirmed it does.

**What it detects:** Violations of the named guideline across the full decision — every paragraph where the RPD's reasoning failed to honor the standard it expressly committed to applying. The named guideline becomes the internal benchmark the RAD uses to evaluate compliance.

**How to apply:** First, identify whether the RPD named any Chairperson's Guideline anywhere in the decision. This is usually in a preliminary paragraph — in MC3-60592 it is at para 4, where Guideline 9 is named explicitly. If a guideline is named, extract its core requirements. Then read every subsequent paragraph of the decision against those requirements. For each paragraph where the reasoning fails to honor the guideline, record: the paragraph number, the specific requirement that was violated, and the specific language in the paragraph that demonstrates the violation.

**Example — MC3-60592:** Para 4 named Guideline 9 — SOGIESC proceedings. Guideline 9 requires: avoiding stereotypical behavioral expectations about SOGIESC individuals; recognizing the particular challenges SOGIESC claimants face in presenting their cases; assessing SOGIESC corroborating evidence independently, not as compensation for prior adverse findings; not penalizing claimants for how they present their evidence. Reading the full decision against these requirements identified violations at: para 14 (stereotypical behavioral expectation — bisexual man who risked his life in Nigeria should seek relationships in safe Canada); para 35 (penalizing claimant for his lawyer's preparation failure); the other evidence section after para 46 (LGBTQ organization evidence assessed as compensation for prior findings — wrong test). All three violations were confirmed against the RPD's own stated standard.

**Trigger:** Any paragraph in an RPD decision that names a Chairperson's Guideline. Once identified, that guideline becomes a compliance checklist applied to every subsequent paragraph.

**Appeal argument structure:** The RPD named the guideline. Quote the paragraph. State what the guideline requires. Identify each violation by paragraph number. The RAD sees the gap between the named standard and the actual application on the face of the decision — no inference required.

---

## Technique 2 — the section heading is not a label, it is evidence of the analytical method

**Definition:** Section headings in RPD decisions are not neutral organizational labels. They reveal how the decision-maker organized the analysis. A heading that states a conclusion before the analysis begins is evidence of reversed analytical sequencing.

**What it detects:** Confirmation bias in the analytical structure — the RPD organized the section to justify a pre-announced finding rather than to reach a finding through open inquiry.

**Prerequisite:** Technique 1 must have been applied first. The Technique 1 output — which guideline, if any, was named — is required for step 3 below.

**Trigger:** Any text in the RPD decision that functions as a section heading — bolded, on its own line, positioned between paragraphs, and introducing a block of analysis. Apply this technique to each heading before reading the paragraphs beneath it.

**How to apply:**

Step 1 — record the exact heading text verbatim.

Step 2 — classify the heading using this binary test: does it announce a conclusion ("your evidence had serious inconsistencies") or does it open a question ("credibility")? Record the classification: conclusion-announcing or neutral.

Step 3 — check for profile language: does the heading use any of the following patterns?
- "your profile as a [identity]"
- "your evidence regarding your [identity]"
- "with regard to your [identity or characteristic]"
- "inconsistencies / contradictions / omissions" combined with an identity descriptor

If yes: flag as profile-importing and activate the profile extraction sub-technique below.

Step 4 — cross-check against the Technique 1 output: if a named guideline was identified, ask whether this heading's framing is consistent with that guideline's requirements. If the guideline prohibits profile-matching and the heading imports a profile: record as a structural violation of the named guideline.

Step 5 — record the output (see format below) before reading the paragraphs beneath the heading.

**Profile extraction sub-technique** — activate only when step 3 returns a profile-importing flag:

Step A — read all paragraphs under the heading. For each adverse finding, record in one sentence: what did the RPD find deficient in the claimant's evidence?

Step B — for each deficiency recorded in step A, state the expectation it assumes: "the RPD expected that [a person with this identity] would / should / must have [done X / produced Y / experienced Z]."

Step C — for each expectation, classify its source: (i) derived from this claimant's specific circumstances and country conditions, or (ii) a generalized assumption about how a person with this identity behaves or presents.

Step D — flag every class (ii) expectation as a potential Guideline 9 violation if the file involves a SOGIESC claim.

Step E — output a profile table: one row per deficiency.

**Example — MC3-60592:** The heading "Your evidence had serious inconsistencies, contradictions and/or omissions with regard to your profile as a bisexual man" classified as: conclusion-announcing; profile-importing (explicit use of "profile as a bisexual man"). Cross-check against Technique 1 output — Guideline 9 named at para 4 — heading uses profile-matching method that Guideline 9 prohibits: structural violation recorded. Profile extraction produced five rows, all class (ii) generalized assumptions — all flagged as Guideline 9 violations.

**Output format:**

| Heading text | Classification | Profile language | Guideline conflict |
|---|---|---|---|
| [exact text] | Conclusion-announcing / Neutral | Yes / No | Yes — [guideline + paragraph] / No / N/A |

If profile-importing: attach profile table.

| RPD deficiency | Expectation assumed | Source | Guideline 9 flag |
|---|---|---|---|
| [what RPD found deficient] | [identity] should have [done X] | Claimant-specific / Generalized | Yes / No |

---

## Technique 3 — the RPD summary is not neutral

**Definition:** Every RPD decision contains a preliminary section — called variously the "summary," "allegations," "background," "claim summary," or given no explicit title at all — that precedes the credibility analysis. This section may be two paragraphs or it may be ten or more. The number does not matter and should not be assumed. What matters is that it consists of all paragraphs before the RPD's analytical sections begin. This preliminary section is not a neutral restatement of what the claimant alleged. It is the RPD's construction of the baseline — the version of the claim against which all subsequent findings are measured. Errors, omissions, and distortions in the summary are not corrected later; they are assumed and built upon throughout the decision.

**What it detects:** Factual errors, selective omissions, added qualifiers, recharacterizations, and embedded contradictions introduced before any credibility analysis begins. These are foundational errors — they set up adverse findings before the claimant has been given a chance to respond to them.

**Trigger:** The opening line of any RPD decision. This technique activates before any section heading or analytical paragraph is read. The observable signal is: a decision text has been opened and no section has yet been read. Technique 3 runs before Technique 2 and before any adverse finding is analyzed.

**How to apply:**

Step 1 — locate the summary boundary: read from the first paragraph forward. Stop at the first paragraph that contains a section heading (bolded, on its own line) or language explicitly announcing analysis ("Your evidence gave rise to the following concerns," "I have the following credibility concerns," "I will address the following issues"). Every paragraph before that point is the summary. Record the last summary paragraph number.

Step 2 — read the summary as a single constructed baseline: read all paragraphs identified in Step 1 together, not one by one. Note every name, date, event, qualifier, and characterization the RPD uses to describe the claim. Do not cross-check against anything yet — this step only records what the RPD built.

Step 3 — check the summary against the written narrative: open the BOC narrative. For each element recorded in Step 2, verify whether it accurately reflects what the narrative says. Record every addition (element not in the narrative), omission (element in the narrative absent from the summary), qualifier (language softening or shifting a narrative fact), and recharacterization (narrative fact described differently).

Step 4 — check the summary against the oral testimony: open the transcript or hearing notes. For each element in the summary, verify whether it reflects what was said at the hearing. Record every discrepancy between the summary and the oral record.

Step 5 — check the summary against the disclosure record: identify every document in the claimant's disclosure that addresses the same events the summary describes. For each such document, ask: if the RPD had engaged this document, would the summary have been written differently? Record every document that would have changed the summary.

**Example — MC3-60592:** The summary was paras 1–3. Para 2 introduced the date October 27, 2022 — not in the narrative, not stated at the hearing, no footnote. Para 2 embedded "he punched you" as the baseline and then used the oral version as a contradiction at para 24. Para 3 summarized the Lagos relocation in one sentence, stripping: the family lawyer's named advice, the TRV timeline, the CBSA departure city of Lagos in the government's own record, the son's Lagos address in Document 1, the BOC checkbox hierarchy error, and the NDP tabs explaining why the affidavit contained no SOGIESC reference. The stripped summary became the foundation for all three credibility sections.

**Key rule:** What the RPD omits from its summary is as important as what it includes. The most serious category is selective omission that is later weaponized — stripped from the summary and then cited against the claimant in a later section. In MC3-60592: "declared wanted immediately" was stripped from the summary and then cited as a credibility problem at para 26. Spitting was stripped from the summary and then cited as an omission at para 24. The RPD used its own summary omissions as evidence against the claimant.

**Output format:**

| Summary element | Source check | Status | Reappears in decision |
|---|---|---|---|
| [element from summary] | Narrative / Oral / Disclosure | Accurate / Added / Omitted / Recharacterized / Qualified | Para [x] — yes / no |

Flag every element marked Added, Omitted, Recharacterized, or Qualified. In the last column, note whether that element reappears in a later adverse finding — this is the weaponized omission pattern. The most serious category: stripped from the summary, then cited against the claimant in a credibility section.

---

## Technique 4 — the two-reading rule for every RPD section

**Definition:** Every section of an RPD decision requires two separate readings. The first reading identifies what the RPD said and what evidence it cited. The second reading identifies what the RPD missed, did not consider, got wrong, and what it should have weighed differently.

**What it detects:** The full error inventory of any section — including errors of omission (relevant evidence not engaged), errors of reasoning (logical leaps, false premises), errors of characterization (misreading the source), and errors of framing (applying the wrong standard or expectation).

**Meta-protocol note:** This technique is not a standalone technique called once. It is a sub-routine that runs inside every other technique when any block of RPD paragraphs is being analyzed. Every technique in this skill that involves reading RPD paragraphs invokes Technique 4 automatically. An agent implementing this skill must call Technique 4 as a nested function within the execution of techniques 2, 3, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, and 15.

**Trigger:** The start of any RPD section or paragraph block being analyzed under any other technique. The observable signal is: a block of RPD paragraphs is about to be read for the first time under the current technique. When this signal appears, run Reading 1 before any other action. Do not begin Reading 2 until Reading 1 output is complete and recorded.

**How to apply:**

Reading 1 — factual inventory (run first; produce structured output before Reading 2 begins):

Step 1 — state the finding: write one sentence recording what the RPD found in this section.

Step 2 — list the cited evidence: record every piece of evidence the RPD cited to support the finding, with paragraph number for each.

Step 3 — record paragraph connections: note every other paragraph in the decision that this finding connects to or depends upon.

Step 4 — record the Reading 1 output object (see output format below) before proceeding. Do not begin Reading 2 without this output recorded.

Reading 2 — critical interrogation (runs after Reading 1 output is complete; takes Reading 1 object as input):

Step 1 — identify missed evidence: name every piece of evidence in the record that directly addresses this finding and was not cited by the RPD. For each: state the source, location, and what it shows.

Step 2 — identify unconsidered interpretations: for each piece of evidence cited in Reading 1 Step 2, state the alternative interpretation available in the record that was not mentioned.

Step 3 — identify factual and logical errors: state each factual error, logical error, or false premise in the RPD's reasoning. For each: quote the specific text containing the error and state what the record shows instead.

Step 4 — identify accumulated detail: list every small detail in the transcript, narrative, or disclosure that is inconsistent with the RPD's finding when read together.

Step 5 — assess member understanding: for each claimant answer the RPD cited, ask — is the RPD's interpretation the only reasonable one? If not: record the answer, the RPD's interpretation, the alternative interpretation, and whether the claimant was given an opportunity to clarify.

Step 6 — record the Reading 2 output object (see output format below).

**Example — MC3-60592 Part 8 second reading:** First reading of the section heading identified three structural functions. Second reading added eight additional findings: the "first opportunity" ambiguity never resolved; the member's own "thank you, thank you" acknowledgment of the clarified answer she then ignored; multiple positive details in the BOC narrative never weighed; the 17-minute emotional testimony block never referenced; the triple "No" certainty evidence misread; the member's own construction crew question producing the answer that undermined her inference, discarded; "omissions" word misattributing cause; the narrative's developmental arc never assessed as a whole.

**Rule:** Never deliver an RPD section analysis after only one reading. The first reading is orientation. The second reading is where errors are found.

**Output format:**

Reading 1 object:

| Field | Value |
|---|---|
| Section / paragraphs | [para range] |
| RPD finding | [one sentence] |
| Evidence cited | [list with para numbers] |
| Connected paragraphs | [list] |

Reading 2 object:

| Error type | Description | Record location | Strength |
|---|---|---|---|
| Missed evidence | [what was not engaged] | [source and location] | Strong / Medium / Weak |
| Unconsidered interpretation | [alternative reading available] | [source and location] | Strong / Medium / Weak |
| Factual or logical error | [what is wrong and why] | [RPD para and source] | Strong / Medium / Weak |
| Accumulated detail | [detail inconsistent with finding] | [source and location] | Strong / Medium / Weak |
| Member misunderstanding | [answer / RPD interpretation / correct interpretation] | [transcript time and RPD para] | Strong / Medium / Weak |

**Agent integration:**

- Input: paragraph block (para range and raw text); Reading 1 output object if re-entering mid-technique
- Call sequence: Technique 4 is a nested call within every technique that processes RPD paragraphs. The calling technique passes the paragraph block; Technique 4 returns the Reading 1 and Reading 2 objects; the calling technique resumes with those objects as its working evidence
- Output: Reading 1 object and Reading 2 object, both in structured table format, passed back to the calling technique and appended to the session error inventory
- Orchestrator note: Reading 2 error rows feed directly into the Phase 4 error inventory table in the rpd-decision-analysis skill. Field names are consistent for aggregation across all technique calls

---

## Technique 5 — the hearing transcript must be read for what the member heard vs. what the member understood

**Definition:** There is a difference between what the member heard (the words said at the hearing) and what the member understood (the meaning she attributed to those words). Misunderstanding is not the same as disbelieving. A claimant whose coherent answer is misunderstood is different from a claimant whose credible answer is rejected.

**What it detects:** Interpretive errors — moments where the member assigned the wrong meaning to an answer, resolved an ambiguity against the claimant without flagging it, or applied a framework that made a coherent answer look deficient.

**Prerequisite:** Technique 4 Reading 1 must be complete. The exchanges to analyze are drawn from Reading 1 Step 2 (evidence cited by the RPD) and Reading 2 Step 5 (member understanding check). Transcript access is required — if no transcript is available, record this technique as not applicable and note the gap in the session error inventory.

**Trigger:** Any transcript exchange identified in Technique 4 Reading 1 as evidence the RPD cited to support an adverse finding. The observable signal is: an RPD paragraph references something the claimant said at the hearing. When this signal appears, locate that exchange in the transcript and read it in full before assessing the RPD's characterization of it.

**How to apply:**

Step 1 — locate the exchange: find the transcript passage corresponding to the RPD paragraph. Read from the start of the question to the end of the claimant's complete answer — not just the fragment the RPD quoted.

Step 2 — record the actual answer: write the claimant's answer verbatim or as close as the transcript allows. Note the timestamp.

Step 3 — record the RPD's characterization: write how the RPD described or used that answer in the decision paragraph. Note the paragraph number.

Step 4 — apply the four category tests: run each category below as a binary yes/no test against this exchange. Record the result for each.

Step 5 — state the alternative interpretation: for each category that returns yes, state in one sentence what the answer actually means under that category's framework.

Step 6 — assess the clarification duty: ask whether the ambiguity should have been put to the claimant before an adverse finding was made. Record: yes (duty existed and was not discharged), no (no duty on these facts), or unclear.

Step 7 — record the output object (see output format below).

**Misunderstanding categories — apply as binary tests in Step 4:**

Category A — language ambiguity through interpretation. Observable signal: the interpreter flags difficulty, repeats the question back to the claimant, or the claimant re-answers unprompted after a pause. If any of these appear at or immediately before the exchange under review: flag yes. The reliability of the entire surrounding exchange is reduced.

Category B — cultural framework mismatch. Observable signal: the answer uses a place name described by relationship rather than geography; a time reference described by season or event rather than calendar; a family role described by a naming convention that differs from Western norms; a financial or property concept described by local practice. If the member's adverse inference depends on the Western reading of any of these: flag yes.

Category C — certainty misread as suspicion. Observable signal: the claimant gives a flat, unhedged, unqualified answer — a direct denial, a single unelaborated statement — and the RPD characterizes it as rehearsed, pat, or implausible. If the RPD uses words like "surprisingly certain," "too clean," or makes an adverse finding from a direct denial: flag yes.

Category D — emotional language read as legal admission. Observable signal: the claimant uses language describing an internal experience ("I couldn't understand what was happening," "I didn't know what to do," "I couldn't believe it") and the RPD treats it as a factual claim about objective reality — specifically that the event was novel, unknown to the claimant, or not believed by the claimant at the time. If this substitution appears: flag yes.

**Example — MC3-60592:** A summary table of six exchanges shows the member consistently heard the words and misread their meaning:

| What the claimant said | What the member understood | What it actually means |
|---|---|---|
| "I couldn't understand what was happening to me" | First-time novelty | Emotional overwhelm on re-encountering a suppressed feeling |
| "That was my first opportunity" | First time I felt attraction | First contextual occasion in which a suppressed attraction surfaced |
| "No. No. No." | Suspiciously clean denial | Honest description of context-dependent attraction genuinely absent |
| Three women, three men, no bonding | Not referenced | Context-dependent attraction absent in a non-bonding environment for both genders |
| "I love him" | Not referenced | Genuine emotional attachment — positive evidence of bisexual experience |
| Wife did not write | Credibility failure | Consequence of damaged marriage in a country where writing such a letter carries legal and social risk |

**Output format:**

Misunderstanding object — one per exchange:

| Field | Value |
|---|---|
| Transcript timestamp | [time] |
| RPD paragraph | [para number] |
| Claimant's actual answer | [verbatim or close paraphrase] |
| RPD's characterization | [quoted from decision] |
| Category A — interpretation flag | Yes / No |
| Category B — cultural framework flag | Yes / No |
| Category C — certainty flag | Yes / No |
| Category D — emotional language flag | Yes / No |
| Alternative interpretation | [what the answer actually means] |
| Clarification duty | Yes / No / Unclear |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: Technique 4 Reading 1 output — provides the list of RPD-cited transcript exchanges to process
- Input: transcript exchange block (timestamp range and text) and corresponding RPD paragraph text and number
- Call sequence: invoked from Technique 4 Reading 2 Step 5 for each answer the RPD cited adversely. Also invoked when Technique 11 (clarification sequence) identifies a pre-clarification exchange the RPD used as its finding
- Output: one misunderstanding object per exchange, structured table format, passed back to Technique 4 Reading 2 and appended to the session error inventory
- Orchestrator note: Category C and Category D flags map directly to Guideline 9 sub-requirements and can be used to auto-populate inputs for Technique 13 (behavioral expectation test). Flag these connections when they appear

---

## Technique 6 — positive evidence must be extracted and weighed separately

**Definition:** RPD analysis typically focuses on deficiencies — what is missing, inconsistent, or contradicted. Positive evidence — detail consistent with genuine experience — must be actively extracted and weighed against the deficiency findings.

**What it detects:** Selective presentation of the record — the RPD acknowledging only the negative side of a mixed evidentiary picture.

**Prerequisite:** Technique 4 Reading 2 must be complete for all RPD sections in the current analysis block. The positive evidence pass runs across the full record after the error-finding pass — not section by section. Running it mid-pass produces an incomplete picture. The adverse findings from Technique 4 Reading 2 objects are the direct inputs to this technique.

**Trigger:** After Technique 4 Reading 2 is complete for all RPD sections in the current analysis block. The observable signal is: a complete Reading 2 error inventory exists and no further sections remain to be analyzed in the current pass. At that point activate the positive evidence pass across the full record.

**How to apply:**

Step 1 — load the adverse findings: collect all adverse findings from the Technique 4 Reading 2 objects produced in the current pass. This list is the baseline against which positive evidence is weighed.

Step 2 — run the positive evidence pass against the written narrative: read the BOC narrative with the adverse findings list open. For each passage consistent with the claimant's account on an adverse finding, apply the five category tests below. Record every match.

Step 3 — run the positive evidence pass against the oral testimony: read the transcript or hearing notes with the adverse findings list open. For each passage consistent with the claimant's account on an adverse finding, apply the five category tests. Record every match.

Step 4 — run the positive evidence pass against the disclosure record: review the claimant's disclosure documents with the adverse findings list open. For each document or passage supporting the claimant's account on an adverse finding, apply the five category tests. Record every match.

Step 5 — check RPD engagement: for each item recorded in Steps 2–4, check whether the corresponding RPD paragraph engaged it. Record: referenced and weighed, referenced and dismissed, or not referenced.

Step 6 — record the output objects (see output format below).

**Positive evidence categories — apply as binary tests in Steps 2–4:**

Category A — spontaneous emotional or sensory detail. Observable signal: the passage contains physical sensation, emotional response, or sensory description — touch, smell, sound, visual detail — that was not prompted by a direct question. Test: was this detail volunteered without a question asking for it? If yes: flag. Fabricated accounts typically lack unprompted sensory specificity.

Category B — narrative arc coherence. Observable signal: a sequence of events across multiple time periods follows a psychologically consistent progression — cause, response, and consequence at each stage — not a series of discrete disconnected facts. Test: does the narrative move from one stage to the next in a way that requires internal consistency across all stages simultaneously? If yes: flag. A fabricated arc typically breaks down when each stage is tested against the others.

Category C — cross-source consistency. Observable signal: the same fact, detail, or description appears in two or more independent sources — narrative and transcript; narrative and disclosure document; transcript and witness letter — without appearing to have been coordinated in response to the same direct question. Test: does the same detail appear in at least two sources independently? If yes: flag.

Category D — self-defeating detail consistent with genuine experience. Observable signal: a fact in the record works against the claimant evidentially but is consistent with genuine experience of what the claimant describes. Test: would a person fabricating this claim have included this detail? If no — because it creates an evidentiary gap or credibility risk — and the detail is nonetheless consistent with genuine experience: flag.

Category E — positive relational evidence. Observable signal: the record contains evidence of genuine attachment, emotional bond, or relationship — declarations of love, descriptions of being known intimately, evidence of grief or loss after separation — with identifying specific detail rather than generic affection. Test: does the passage describe a specific relational experience with identifying detail? If yes: flag.

**Example — MC3-60592:** The RPD's heading said the evidence had "serious inconsistencies, contradictions and/or omissions" regarding the bisexual profile. The positive evidence in the record that was never weighed included: physical attraction noted in the BOC on first meeting ("a very handsome and tall man"); sensory description of the massage ("his touch was so soft and delicate"); physiological attraction response ("my heart was beating so fast"); reciprocal initiation ("I returned back the kiss"); post-coital preoccupation ("the sex was amazing, I thought about it the whole night"); spontaneous declaration of love for Abiodun ("I love him"); description of being deeply known ("he studies me very well"); and the loss after separation (tried phone and social media; never saw him again). These are not incidental details. They form a consistent pattern of genuine emotional and sexual attachment. None was referenced in the decision.

**Rule:** Every RPD analysis must include a positive evidence pass — a deliberate search for what the record shows that is consistent with the claimant's account, separate from the search for errors in the RPD's reasoning.

**Output format:**

Positive evidence object — one per item:

| Field | Value |
|---|---|
| Source | Narrative / Transcript / Disclosure — [document name and location] |
| Passage | [verbatim or close paraphrase] |
| Adverse finding addressed | [from Technique 4 Reading 2 — para number and finding] |
| Category A flag | Yes / No |
| Category B flag | Yes / No |
| Category C flag | Yes / No |
| Category D flag | Yes / No |
| Category E flag | Yes / No |
| RPD engagement | Referenced and weighed / Referenced and dismissed / Not referenced |
| Cepeda-Gutierrez flag | Yes — not referenced on core finding / No |

**Agent integration:**

- Prerequisite: complete Technique 4 Reading 2 inventory for all sections in the current pass
- Input: adverse findings list from Technique 4 Reading 2 objects; full record — narrative text, transcript text, disclosure documents
- Call sequence: single full-record pass after the error-finding pass is complete — not a per-section call. Runs once per analysis block
- Output: positive evidence objects, one per item, structured table format. Fed to: (1) the session error inventory for completeness; (2) the RAD memorandum affirmative case section as the evidentiary foundation for the claimant's profile argument
- Orchestrator note: items flagged not referenced on core findings feed directly into Cepeda-Gutierrez error arguments in Phase 4 of the rpd-decision-analysis skill

---

## Technique 7 — the document cannot be simultaneously discarded and weaponized

**Definition:** When the RPD gives a document no weight, it cannot then use the contents of that same document to damage the claimant's credibility. A document that is too unreliable to prove something happened cannot be reliable enough to prove it did not happen, or that the claimant's account is false.

**What it detects:** Internal inconsistency in the RPD's treatment of a single piece of evidence.

**Trigger:** The phrases "I give this document no weight," "I give this little weight," "I assign no weight," or "I assign little weight" anywhere in the RPD decision. The observable signal is any of these exact phrases or close variants. When found: record the paragraph number and the document named, then activate the full-decision scan in Step 2.

**How to apply:**

Step 1 — record the discard: note the paragraph number where no weight or little weight was given. Record the document name and the RPD's stated reason for discarding it.

Step 2 — scan the full decision for adverse references: read every other paragraph of the decision for any reference to that document's contents, contradictions, or details being used to damage the claimant's credibility. Record every such paragraph number and the specific use made.

Step 3 — apply the binary test: if Step 2 finds any adverse reference, state the binary choice the RPD had to make but did not: "Either this document has no weight — and its contents cannot be used for any purpose — or it has some weight — and it proves at least that [minimum content the document establishes]. It cannot be both."

Step 4 — classify the error: record whether this is a hard binary error (document given no weight and its contents used adversely in the same decision) or a soft version (document given little weight but relied upon heavily for adverse inferences — inconsistent weighting).

Step 5 — record the output object (see output format below).

**Example — MC3-60592:** The RPD gave the family lawyer's letter no weight at para 20 because of contradictions and lack of detail. It found the letter too unreliable to prove any event occurred. But at paras 18–19 it used the letter's contradictions — assault vs. crowd outside; extortion vs. wanting to extort — to damage the claimant's credibility directly. The same document was simultaneously too unreliable to prove anything and reliable enough to show the claimant's account was false. This is the binary error. The RPD had to choose: either the letter has no weight (and its contents cannot be used adversely) or it has some weight (and it proves at least that the lawyer believed events occurred). It cannot have no weight for one purpose and weight for another.

**Output format:**

Discard-and-weaponize object — one per discarded document:

| Field | Value |
|---|---|
| Document name | [name] |
| Discard paragraph | [para number] |
| Stated reason for discard | [RPD's reason] |
| Adverse use paragraphs | [list of para numbers] |
| Adverse use description | [what the RPD used the document to show] |
| Error type | Hard binary / Soft inconsistent weighting |
| Binary choice statement | Either [no weight — cannot use] or [some weight — proves minimum X] |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Input: RPD decision full text; scan trigger — phrases "no weight," "little weight," "I assign no weight," "I assign little weight"
- Call sequence: agent scans full decision text for trigger phrases. For each hit: extracts the paragraph, identifies the document named, then scans all remaining paragraphs for references to that document. Independent technique — no prerequisite from other techniques
- Output: one discard-and-weaponize object per discarded document, structured table format, appended to the session error inventory
- Orchestrator note: hard binary errors are strong grounds — flag for priority placement in Phase 5 error prioritization table of the rpd-decision-analysis skill. Soft inconsistent weighting errors are medium strength — flag separately

---

## Technique 8 — trace the information chain before attributing a document's errors to the claimant

**Definition:** When a corroborating document contains errors, omissions, or contradictions with the claimant's account, the first question is not "is the claimant lying?" It is "how did this document come to say what it says?" Trace the information chain from the event to the document and identify how many steps of transmission occurred and how much precision was lost at each step.

**What it detects:** Errors the RPD attributed to the claimant's credibility that are actually attributable to the document's drafting circumstances.

**Prerequisite:** This technique is triggered in two ways. First: Technique 7 output — when a document has been discarded or given little weight and a discard-and-weaponize object exists, apply Technique 8 to that same document to explain why it contains the errors the RPD used adversely. Second: any RPD paragraph that uses a corroborating document's inconsistency with the claimant's account as a credibility finding, regardless of whether the document was discarded.

**Trigger:** Any RPD paragraph that (a) uses a discarded document's contents adversely — link from Technique 7 output — or (b) uses any corroborating document's inconsistency with the claimant's account as a credibility finding. The observable signal is: the RPD identifies a difference between what a document says and what the claimant said, and uses that difference to damage credibility.

**How to apply:**

Step 1 — identify the document: name the document, the paragraph where it is criticized, and the specific inconsistency the RPD used adversely.

Step 2 — identify who prepared the document: record the author. Note any relevant limitations on the author's knowledge or legal drafting capacity — lawyer, family member, community member, government official.

Step 3 — identify who provided the information to the author: was it the claimant directly, a third party, or a combination? If a third party: record their name and their relationship to the events described.

Step 4 — trace the chain step by step: for each step from the event to the document, record who was at that step, what they knew directly versus what they were told, when the information passed to the next step, and what precision was lost at that step — interpretation, paraphrase, omission, or extrapolation.

Step 5 — count the total steps: record the number of transmission steps between the event and the document. The more steps, the greater the cumulative precision loss — and the weaker the basis for attributing the document's errors to the claimant's credibility.

Step 6 — state the alternative attribution: in one sentence, state what actually caused the inconsistency — the information chain — and contrast it with the RPD's attribution of the error to the claimant.

Step 7 — record the output object (see output format below).

**Example — MC3-60592:** The family lawyer's letter was prepared by a non-specialist family lawyer. The information came from the wife, not the claimant. The wife was not present during the incident — she arrived hours later. The wife called the lawyer by phone on the day of the incident. The claimant never spoke to the lawyer directly. The information chain was: incident → claimant (locked inside, not fully present to events outside) → tells wife (hours later, after calming down) → wife calls lawyer by phone (same day) → lawyer writes letter (at an unknown later point). Each step introduced imprecision. The lawyer drafted "assault" and "extortion" from second-hand information about a community mob situation — he extrapolated from what the wife told him to what probably happened. The RPD attributed the letter's imprecision to the claimant's credibility. The imprecision was caused by the information chain, not by fabrication.

**Output format:**

Information chain object — header:

| Field | Value |
|---|---|
| Document name | [name] |
| RPD paragraph | [para number] |
| Inconsistency used adversely | [what the RPD said the document shows vs. what the claimant said] |
| Total chain steps | [number] |
| Alternative attribution | [what actually caused the inconsistency] |
| Ground of appeal | Yes / No — [brief reason] |

Chain step table — one row per step:

| Step | Who | Direct knowledge or told | When | Precision loss |
|---|---|---|---|---|
| 1 — event | [who was present] | Direct | [date and time] | — |
| 2 — first transmission | [who received information] | Told by [step 1 person] | [when] | [what was lost] |
| 3 — document | [author] | Drew on [step 2] | [when] | [what was lost] |

**Agent integration:**

- Trigger sources: (1) Technique 7 output — discard-and-weaponize object identifies the document; (2) independent scan — any RPD paragraph using document inconsistency adversely against the claimant
- Input: document name and criticized paragraph from trigger source; full record for chain reconstruction — narrative, transcript, disclosure documents
- Call sequence: runs immediately after Technique 7 for discarded documents. Also runs independently for any criticized document not caught by Technique 7. One call per criticized document — not a per-section call
- Output: information chain object with header and chain step table, appended to the session error inventory
- Orchestrator note: chains of three or more steps with documented precision loss at each step are strong grounds for reversing document-based credibility findings. Flag for Phase 5 prioritization in the rpd-decision-analysis skill

---

## Technique 9 — "omissions" vs. external causes — always ask who caused the gap

**Definition:** When the RPD characterizes an evidentiary gap as an "omission," that word attributes the cause of the gap to the claimant. Before accepting this characterization, identify the actual cause of the gap.

**What it detects:** Incorrect attribution of evidentiary gaps to the claimant's credibility when those gaps are caused by country conditions, cultural barriers, legal representation failures, or structural barriers the claimant could not control.

**Prerequisite:** Technique 8 output should be consulted before this technique is applied to any document-related omission. If Technique 8 has already traced the information chain for the same document, that chain step table is direct evidence for obstacle category E below — import it rather than reconstructing the chain.

**Trigger:** The word "omission" or the phrase "I would have expected" anywhere in the RPD decision. Also triggered by: "I note the absence of," "there is no," "the claimant did not provide," or any phrase that attributes an evidentiary gap to the claimant's choice. The observable signal is: the RPD states that evidence is missing and implies or states that the claimant is responsible for the gap.

**How to apply:**

Step 1 — identify the alleged omission: record the RPD paragraph, the exact trigger phrase, and what the RPD says is missing.

Step 2 — state what would have needed to happen: in one sentence, describe the action the claimant would have needed to take, the document that would have needed to exist, or the witness who would have needed to come forward for this evidence to be present.

Step 3 — apply the five obstacle category tests: run each category below as a binary yes/no test. For each yes: record the evidence from the record that confirms the obstacle.

Step 4 — state the alternative attribution: in one sentence, state what actually caused the gap — the confirmed obstacle — and contrast it with the RPD's attribution of the gap to the claimant.

Step 5 — record the output object (see output format below).

**Obstacle categories — apply as binary tests in Step 3:**

Category A — country condition barrier. Observable signal: an NDP tab addresses the specific type of document or action the RPD expected and explains why it would not be available, certifiable, or safe to produce in the country of origin. Test: does an NDP tab directly address why this evidence could not exist or be obtained? If yes: name the tab and what it shows. Flag as Cepeda-Gutierrez error if the tab was not cited in the decision.

Category B — cultural or social barrier. Observable signal: the evidence the RPD expected would require a person in the claimant's community or family to take an action that carries legal, social, or reputational risk under the conditions documented in the NDP or record. Test: would producing this evidence have exposed a witness to harm, shame, legal risk, or community sanction? If yes: identify the specific risk and the record or NDP evidence that confirms it.

Category C — legal representation failure. Observable signal: the evidence gap corresponds to a type of evidence that counsel would have been responsible for identifying, requesting, or explaining to the claimant. Test: would adequate legal preparation have produced this evidence? If yes: record the specific preparation failure — failure to explain what was needed, failure to request documents, failure to prepare witnesses. Link to Technique 15 (representation failure inventory).

Category D — structural legal or procedural barrier. Observable signal: a rule, timeline, or procedural constraint prevented the evidence from being filed, requested, or produced. Test: was there an IRPA provision, tribunal rule, or filing deadline that made this evidence unavailable? If yes: identify the specific rule and its effect.

Category E — information chain limitation. Observable signal: the witness who produced the document was not present at the events described, or received the information second-hand. Test: does a Technique 8 chain step table exist for this document? If yes: import that finding directly as the evidence for this category. If no: invoke Technique 8 before completing this step.

**Example — MC3-60592:** The heading used the word "omissions" across all of Section 1. Mapping the actual causes of each alleged omission: wife's silence — caused by trauma, shame, and country-condition risk (not a choice to hide evidence); Lagos friend's affidavit with no SOGIESC content — caused by narrow instruction from the claimant and notarial barriers documented in NDP tabs 6.6 and 6.10; no same-sex relationships in Canada — Guideline 9 prohibits treating this as an omission; lawyer's letter errors — caused by the information chain. Not one gap was caused by the claimant choosing to withhold evidence.

**Output format:**

Cause-attribution object — one per alleged omission:

| Field | Value |
|---|---|
| RPD paragraph | [para number] |
| Trigger phrase | [exact phrase from decision] |
| Evidence alleged missing | [what the RPD said was absent] |
| What would have been needed | [action, document, or witness required] |
| Category A flag | Yes / No — [NDP tab if yes] |
| Category B flag | Yes / No — [specific risk if yes] |
| Category C flag | Yes / No — [representation failure if yes] |
| Category D flag | Yes / No — [rule or deadline if yes] |
| Category E flag | Yes / No — [chain step count if yes] |
| Alternative attribution | [what actually caused the gap] |
| Cepeda-Gutierrez flag | Yes — NDP tab not cited / No |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Trigger sources: (1) phrase scan — "omission," "I would have expected," "I note the absence of," "the claimant did not provide," "there is no [evidence / document / letter]"; (2) Technique 8 output — information chain limitation identified for a document the RPD criticized
- Input: RPD paragraph containing trigger phrase; Technique 8 output if available for the same document; NDP table of contents for category A check; record for categories B–D
- Call sequence: runs per alleged omission. For category E: check for existing Technique 8 output first — if it exists import it; if not invoke Technique 8 before completing Step 3
- Output: cause-attribution object per alleged omission, structured table format, appended to the session error inventory. Category C flags feed into Technique 15 (representation failure inventory)
- Orchestrator note: category A flags with confirmed NDP tabs not cited are Cepeda-Gutierrez errors — auto-flag for Phase 4 error category 2 in the rpd-decision-analysis skill

---

## Technique 10 — NDP country evidence engagement check

**Definition:** For every adverse credibility finding, check whether there is an NDP tab that directly addresses the subject matter of that finding. Under *Cepeda-Gutierrez* [1998 CanLII 8667 (FC)], a decision-maker who fails to engage directly relevant country evidence on a core finding before relying on that finding commits a reviewable error.

**What it detects:** Failure to engage relevant country evidence — one of the most common and most reviewable errors in RPD decisions.

**Prerequisite:** Technique 14 (NDP version check) must be run before this technique. The NDP version verified in Technique 14 is the only version to be used in this technique's mapping. Using an unverified version risks building arguments on tabs the claimant could not have responded to. Adverse findings from Technique 4 Reading 2 objects are the direct inputs — this technique runs per finding, not per section.

**Trigger:** Every adverse credibility finding recorded in the Technique 4 Reading 2 error inventory. The observable signal is: a Reading 2 object exists with a finding recorded in the RPD finding field. Apply this technique to every such finding before the analysis of that finding is considered complete.

**How to apply:**

Step 1 — extract the finding subject: from the Technique 4 Reading 2 object, identify the subject matter of the adverse finding in one or two words — the topic the RPD made an adverse finding about (for example: bisexuality in Nigeria, affidavit authenticity, internal relocation, family detention risk).

Step 2 — check the priority categories table: consult the priority categories table below. If the finding subject appears in the table: go directly to the tab identified and proceed to Step 4. If the finding subject does not appear: proceed to Step 3.

Step 3 — check the full NDP table of contents: open the NDP table of contents for the verified version from Technique 14 output. Search for any tab whose title or description directly addresses the finding subject. Record every matching tab.

Step 4 — check RPD engagement: for each matching tab identified in Steps 2 or 3, check whether the RPD decision cited that tab in connection with the adverse finding. Record: cited and engaged, cited but not applied, or not cited.

Step 5 — flag Cepeda-Gutierrez errors: for every matching tab that was not cited in connection with a core adverse finding, record a confirmed Cepeda-Gutierrez error. The strength of the error increases with the directness of the tab's relevance to the finding.

Step 6 — record the output object (see output format below).

**Priority categories for SOGIESC files:**

| Subject matter | NDP section to check |
|---|---|
| Bisexuality in Nigeria | Tab 6.7 |
| Notarial certification of SOGIESC documents | Tabs 6.6, 6.10 |
| Police procedures for same-sex conduct | Tab 6.8 |
| Family detention risk | Tab 10.1 |
| Internal relocation obstacles | Tabs 13.1, 14.6 |
| Community violence against LGBTQ persons | Section 6 generally |
| Nigerian affidavit requirements | Tab 9.2 |
| Canadian LGBTQ support letters | Tab 3.19 |
| Document fraud (general) | Tab 3.11 — note: general, not affidavit-specific |

**Example — MC3-60592:** The RPD made bisexuality credibility findings across paras 8–20 without engaging NDP tab 6.7. It criticized the Lagos affidavit for not mentioning sexual orientation without engaging NDP tabs 6.6 and 6.10, which explain why Nigerian notaries would refuse to certify such a document. It impugned the affidavit using a 2018 visa fraud tab (3.11) while ignoring the two directly applicable 2021 and 2025 affidavit tabs (9.2). It rejected the Lagos relocation without engaging the internal relocation tabs (13.1, 14.6). Every one of these is a confirmed *Cepeda-Gutierrez* error.

**Output format:**

NDP mapping object — one per adverse finding:

| Field | Value |
|---|---|
| Adverse finding | [from Technique 4 Reading 2 — para number and finding] |
| Finding subject | [one or two words] |
| NDP version | [from Technique 14 output] |
| Matching tabs | [tab numbers and titles] |
| RPD engagement per tab | Cited and engaged / Cited but not applied / Not cited |
| Cepeda-Gutierrez flag | Yes — [tab number] not cited on core finding / No |
| Error strength | Strong — directly on point / Medium — related / Weak — peripheral |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: Technique 14 output — verified NDP version. Do not run this technique without a confirmed NDP version in evidence
- Input: adverse finding subject from Technique 4 Reading 2 object; NDP table of contents for verified version; RPD decision text for citation check
- Call sequence: invoked per adverse finding after Technique 4 Reading 2 is complete for that finding. Runs in parallel with Technique 9 — not sequentially dependent on it
- Output: NDP mapping object per finding, structured table format, appended to session error inventory
- Orchestrator note: Cepeda-Gutierrez flags auto-populate Phase 4 error category 2 in the rpd-decision-analysis skill. Strong flags — tabs directly on point and not cited on a core finding — are priority grounds. Cross-reference with Technique 9 category A flags to avoid duplicate error entries

---

## Technique 11 — the clarification sequence: which version did the RPD use?

**Definition:** Claimants often give confused first-attempt answers at the hearing that become clearer on clarification, re-examination, or interpreter intervention. The operative answer is the clarified version. Check whether the RPD used the first-attempt answer or the clarified version to make its finding.

**What it detects:** RPD findings built on pre-clarification confusion rather than the claimant's actual, coherent account.

**Prerequisite:** Transcript access is required. This technique cannot run without the hearing transcript. Technique 4 Reading 1 must have been applied first — it identifies which transcript exchanges the RPD cited adversely. Those exchanges are the direct inputs to this technique. If Technique 4 Reading 1 has not been applied, apply it before running this technique.

**Trigger:** Any RPD paragraph that (a) identifies a contradiction between hearing testimony and the BOC, disclosure, or another source, or (b) uses a claimant's oral answer as the basis for an adverse credibility finding. The observable signal is: an RPD paragraph references or characterizes something the claimant said at the hearing. When this signal appears: extract the corresponding transcript exchange and apply the steps below before accepting the RPD's characterization.

**How to apply:**

Step 1 — identify the RPD's cited moment: from the RPD paragraph, record the paragraph number and the RPD's characterization of the answer in one sentence. This is the finding you are testing.

Step 2 — locate the exchange in the transcript: find the passage in the transcript corresponding to the RPD's cited moment. Record the timestamp of the start and end of the cited answer.

Step 3 — extend the exchange boundary: do not stop at the RPD's cited moment. Read the transcript from at least 60 seconds before the cited answer to at least 60 seconds after. This is the exchange boundary — the full context within which the answer was given and any clarification may have occurred.

Step 4 — scan the boundary for clarification events: within the exchange boundary identified in Step 3, look for any of the following. Record each one found, with its timestamp and type:

- Interpreter intervention — the interpreter flags difficulty, requests repetition, or corrects a translation
- Member clarification request — the member asks the claimant to repeat, explain, or clarify
- Claimant self-correction — the claimant modifies or adds to a prior answer unprompted
- Re-examination question — counsel on re-examination addresses the same subject
- Member follow-up — the member asks additional questions after re-examination has concluded

Step 5 — extract both versions: record the first-attempt answer (the answer the RPD cited, at the timestamp from Step 2) and the clarified answer (the answer given after any clarification event found in Step 4). If multiple clarification events occurred, record the final clarified answer. If no clarification events were found in Step 4: record that no clarification occurred and proceed to Step 7.

Step 6 — check for member acknowledgment: look in the transcript immediately after the clarified answer for any member acknowledgment — "thank you," "okay," "I understand," or any phrase confirming she received the clarified answer. Record the timestamp and exact phrase if found.

Step 7 — identify which version the RPD used: compare the RPD's characterization from Step 1 against the two versions from Step 5. Record one of:

- Pre-clarification version used — the RPD's finding matches the first-attempt answer and does not reference the clarified answer
- Clarified version used — the RPD's finding reflects the clarified answer
- Both versions referenced — the RPD acknowledged both and explained its choice

Step 8 — apply the binary test and record the output object: if Step 7 returns pre-clarification version used and a materially different clarified answer exists, state: "The RPD's finding at para [X] is built on the pre-clarification answer at [timestamp]. The clarified answer at [timestamp] is materially different. [If member acknowledgment was found in Step 6: the member confirmed she received the clarification at [timestamp].] The clarified answer is not referenced in the decision. This is a direct factual error in the RPD's account of the evidence." Then record the output object (see output format below).

**Example — MC3-60592:** The RPD's para 11 finding was built on the confused first-attempt answer at recording time 39:45–40:04. The interpreter then flagged difficulty at 40:06–40:25. The claimant gave a completely different and coherent clarified answer at 40:39–41:29. The member said "thank you, thank you, okay" at 41:46 — confirming she heard the clarification. The decision does not mention it. The para 11 finding is built on the pre-clarification confusion, not on the clarified answer. This is a direct factual error in the RPD's account of the evidence.

**Output format:**

Clarification object — one per exchange analyzed:

| Field | Value |
|---|---|
| RPD paragraph | [para number] |
| RPD's characterization | [one sentence — what the RPD said the answer showed] |
| Transcript timestamp — cited moment | [start–end] |
| First-attempt answer | [verbatim or close paraphrase] |
| Clarification events found | [type and timestamp for each — or: none] |
| Clarified answer | [verbatim or close paraphrase — or: n/a] |
| Member acknowledgment | [phrase and timestamp — or: none found] |
| Version RPD used | Pre-clarification / Clarified / Both referenced |
| Factual error confirmed | Yes / No |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: transcript access required — if transcript is not in the session, record technique as not applicable and note the gap in the session error inventory. Technique 4 Reading 1 output required — provides the list of transcript exchanges the RPD cited adversely
- Input: RPD paragraph text and number; transcript text for the corresponding exchange (minimum 60 seconds before and after the cited moment)
- Call sequence: invoked from Technique 4 Reading 2 Step 5 for each exchange the RPD cited adversely. Runs before Technique 5 (member heard vs. understood) when both apply to the same exchange — Technique 11 establishes which version to pass to Technique 5 as its input. Also invoked when Technique 12 (member's own questions) identifies a member-inserted exchange the decision does not reference
- Output: one clarification object per exchange, structured table format, appended to the session error inventory
- Orchestrator note: pre-clarification version used + factual error confirmed = high-priority ground. Flag for Phase 5 prioritization in the rpd-decision-analysis skill. When both Technique 11 and Technique 5 run on the same exchange, pass the clarified answer as the input exchange block to Technique 5 — not the first-attempt answer

---

## Technique 12 — the member's own questions as evidence

**Definition:** When the member inserts her own follow-up questions — especially after counsel has finished re-examination — the answers to those questions are part of the record. If the member asked a question and the answer undermined her own inference, the decision must engage that answer. Silence on the answer to the member's own question is a heightened form of evidentiary non-engagement.

**What it detects:** The member heard an answer that contradicted her finding and did not incorporate it.

**Prerequisite:** Transcript access is required. This technique cannot run without the hearing transcript. If the same exchange involves a claimant clarification, apply Technique 11 (clarification sequence) first — Technique 11 establishes whether the answer given to the member's question is itself the clarified version of an earlier answer.

**Trigger:** Any passage in the transcript where the member speaks after counsel has completed re-examination. The observable signal is: the member asks a question after the designated re-examination phase ends. When this signal appears: record the question, read the full answer, and apply the steps below before accepting any inference in the decision that depends on the subject matter of that question.

**How to apply:**

Step 1 — locate the post-re-examination boundary: identify in the transcript the point where counsel completed re-examination — the last question asked by counsel during that phase. Everything after that point that is spoken by the member is within scope for this technique.

Step 2 — identify all member-inserted questions: read from the post-re-examination boundary to the close of the hearing. Record every question the member asked on her own initiative. For each: record the question text, the timestamp, and the subject matter in one phrase.

Step 3 — read and record each answer: for each member-inserted question from Step 2, record the claimant's complete answer verbatim or as close as the transcript allows. Note the timestamp of the answer.

Step 4 — apply Technique 11 if needed: for each answer recorded in Step 3, check whether the exchange involves a first-attempt answer followed by clarification. If yes: apply Technique 11 before proceeding. Use the clarified answer, not the first-attempt answer, as the input for Step 5.

Step 5 — assess the answer against the decision: for each answer from Step 3 (or the clarified version from Step 4), check whether the answer supports the claimant on any adverse finding in the decision. Ask: if the decision had engaged this answer, could it have sustained the same finding? If the answer would have required a different finding or a different weight: record yes.

Step 6 — check whether the decision references the answer: search the full decision for any reference to the subject matter of each member-inserted question. Record: referenced and engaged, referenced and dismissed, or not referenced.

Step 7 — apply the heightened non-engagement test: if Step 6 returns not referenced on any answer that Step 5 assessed as supportive of the claimant, apply this test: "The member asked this question herself. She received this answer. The answer is not in the decision. The member either did not understand the answer or chose not to engage it. Either way, it is a heightened form of evidentiary non-engagement — the decision must address evidence the member herself elicited." Record the ground.

Step 8 — record the output object (see output format below).

**Example — MC3-60592:** After counsel completed re-examination, the member asked: "Working in construction, are there many women working with you?" The answer — three women, three men, small crew, no bonding — (1) factually destroyed the "almost exclusively surrounded by men" premise of para 11, and (2) showed that context-dependent attraction was absent for both genders in a non-bonding workplace, consistent with the claimant's account on both sides of his bisexuality. The member elicited this answer herself. The decision does not mention it. Para 11 still proceeds on the all-male construction assumption.

**Output format:**

Member question object — one per member-inserted question:

| Field | Value |
|---|---|
| Transcript timestamp — question | [time] |
| Member's question | [verbatim or close paraphrase] |
| Subject matter | [one phrase] |
| Claimant's answer | [verbatim or close paraphrase] |
| Technique 11 applied | Yes — clarified answer used / No |
| Answer supports claimant on adverse finding | Yes — [finding and para number] / No |
| Decision references answer | Referenced and engaged / Referenced and dismissed / Not referenced |
| Heightened non-engagement flag | Yes / No |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: transcript access required — if transcript is not in the session, record technique as not applicable and note the gap. Technique 11 on standby for any exchange within this technique that contains clarification events
- Input: full transcript text from the post-re-examination boundary to close of hearing; RPD decision full text for reference check
- Call sequence: invoked once per hearing after all examination phases are complete. Scans the transcript from the post-re-examination boundary. For each member-inserted question found: calls Technique 11 if clarification events are present, then applies Steps 5–7. Runs after Technique 11 in the session workflow — not before
- Output: one member question object per member-inserted question, structured table format, appended to the session error inventory
- Orchestrator note: heightened non-engagement flag + answer supports claimant on adverse finding = strong ground. The heightened weight comes from the member's own authorship of the question — this is not an omission the RPD can attribute to counsel or the record. Flag for Phase 5 prioritization in the rpd-decision-analysis skill

---

## Technique 13 — behavioral expectation test for Guideline 9 compliance

**Definition:** Guideline 9 prohibits applying stereotypical behavioral expectations to SOGIESC claimants. A behavioral expectation is any statement in the form "if X were true, you would have done Y." When you see this structure in an RPD decision on a SOGIESC claim, test it against Guideline 9.

**What it detects:** Guideline 9 violations embedded in reasoning that appears logical but is actually based on assumptions about how SOGIESC people behave.

**Prerequisite:** Technique 1 (chairperson guideline identification) must have been applied first. The Technique 1 output — confirmation that Guideline 9 was named and its core requirements extracted — is the compliance benchmark this technique applies. If the RPD did not name Guideline 9 but the file involves a SOGIESC claim, this technique still applies — Guideline 9 applies regardless of whether the RPD named it. Note that fact in the output object.

**Trigger:** Any RPD paragraph on a SOGIESC file that contains any of the following structures: "you would have," "you should have," "it is not consistent that," "it is not reasonable that," "I would have expected," "if X were true, you would have done Y," or any reasoning that implies a person with the claimant's SOGIESC identity would or should have behaved differently. The observable signal is: the RPD makes an inference about what a person with this identity would do, not about what this specific claimant did.

**How to apply:**

Step 1 — extract the expectation: record the RPD paragraph number and the exact sentence containing the behavioral expectation. State the expectation in the form: "the RPD expected that [a person with this identity] would / should have [done X]."

Step 2 — identify the source of the expectation: ask where the Y expectation comes from. Classify it using one of the following:

- Source A — claimant-specific: the expectation is derived directly from this claimant's stated circumstances, country conditions, or documented experience. The expectation would logically follow for any person in the same specific factual situation regardless of their SOGIESC identity.
- Source B — generalized assumption: the expectation applies because of the claimant's SOGIESC identity and would not apply, or would apply differently, to a person without that identity in the same country and circumstances.
- Source C — country condition expectation: the expectation is derived from a documented country condition or NDP tab — not from the identity itself. Record the NDP tab if cited; flag as not cited if no tab is referenced.

Step 3 — apply the substitution test: ask — would this expectation still hold if applied to a heterosexual claimant in the same country and circumstances? If the expectation only makes sense when applied to someone with a SOGIESC identity: it is Source B — a generalized assumption. If it would hold for any refugee claimant regardless of SOGIESC identity: it may be Source A or Source C.

Step 4 — check Guideline 9 compliance: if Step 2 returns Source B, apply this test: does Guideline 9, as extracted in the Technique 1 output, prohibit applying this type of expectation? Record: prohibited — [specific Guideline 9 requirement violated], or not prohibited — [reason].

Step 5 — check NDP engagement: if the expectation relates to how SOGIESC persons behave in the country of origin, check whether the RPD cited an NDP tab to support the expectation. If the RPD applied a behavioral expectation about SOGIESC persons in Nigeria without citing NDP tab 6.7 or the relevant section: flag as a Cepeda-Gutierrez error in addition to the Guideline 9 violation.

Step 6 — record the output object (see output format below).

**The key test:** Would the expectation still hold if applied to a heterosexual claimant in the same country and circumstances? If not — if it only makes sense as an inference about someone with a SOGIESC identity — it is a behavioral expectation the Guideline prohibits.

**Example — MC3-60592:** Para 14: "You maintained a relationship under mortal risk in Nigeria for three years, but once in safe Canada you did not seek any relationships with men." The unstated premise is: a bisexual man willing to risk his life for a relationship will seek relationships when safe. This premise applies only to a bisexual man, not to any other refugee claimant. It embeds an assumption about how bisexual people behave. The Guideline specifically addresses the inference that SOGIESC individuals "necessarily act upon their attractions" — para 14 applied a softer version of the same inference. It is prohibited.

**Output format:**

Expectation object — one per behavioral expectation found:

| Field | Value |
|---|---|
| RPD paragraph | [para number] |
| Exact sentence | [verbatim quote from decision] |
| Expectation stated | [the RPD expected that (identity) would/should have (done X)] |
| Source classification | Source A — claimant-specific / Source B — generalized assumption / Source C — country condition |
| Substitution test result | Fails — only applies to SOGIESC identity / Passes — applies to any claimant |
| Guideline 9 named by RPD | Yes — para [x] / No |
| Guideline 9 requirement violated | [specific requirement — or: not applicable] |
| NDP engagement check | Tab cited / Tab not cited — [tab number if applicable] / Not applicable |
| Cepeda-Gutierrez flag | Yes / No |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: Technique 1 output required — confirms whether Guideline 9 was named and provides extracted requirements as the compliance benchmark. If Technique 1 output shows Guideline 9 named: use those extracted requirements directly in Step 4. If Guideline 9 was not named: note that fact in the output object and proceed — the guideline applies regardless
- Input: RPD paragraph text containing the behavioral expectation trigger; Technique 1 output (guideline requirements); NDP table of contents for Step 5 check
- Call sequence: invoked per RPD paragraph containing a trigger phrase on any SOGIESC file. Runs after Technique 4 Reading 2 has been applied to the same paragraph — the Reading 2 object may already have flagged a member misunderstanding category C or D that overlaps with this technique. Cross-reference and avoid duplicate error entries
- Output: one expectation object per behavioral expectation, structured table format, appended to the session error inventory
- Orchestrator note: Source B classification + substitution test fails + Guideline 9 named by RPD = strongest form of this ground — the RPD violated its own stated standard. Source B + Guideline 9 not named = still a valid ground but requires one additional inference step at the RAD. Flag both for Phase 4 error category 1 in the rpd-decision-analysis skill. Category C or D flags from Technique 5 on the same paragraph may feed directly into this technique's Step 2 analysis — check for overlap before recording separate objects

---

## Technique 14 — the NDP version check

**Definition:** Verify that every NDP tab cited in the RPD decision matches the NDP version that was actually in evidence. If the RPD cites a tab from a version not in evidence, the claimant had no opportunity to respond to it, and the reliance on it is a procedural error.

**What it detects:** Procedural error — the RPD relied on country evidence the claimant had no opportunity to respond to.

**Prerequisite:** The claimant's disclosure record must be in the session. Specifically: the document list identifying the NDP version disclosed to the claimant, including the date of that version. This technique cannot be completed without confirmed knowledge of which NDP version was in evidence. If the disclosure document list is not available, record this technique as incomplete and flag it as a gap — do not proceed to Technique 10 until this technique has been completed.

**Trigger:** The first NDP footnote encountered in the RPD decision. The observable signal is: any footnote citing a tab from any National Documentation Package. When the first NDP footnote appears, this technique activates and runs across the full decision before any other NDP-reliant analysis proceeds. This technique must be completed before Technique 10 (NDP engagement check) is run — Technique 10 depends on knowing which NDP version is verified.

**How to apply:**

Step 1 — identify the disclosed NDP version: from the claimant's disclosure document list, record the NDP version in evidence. Capture: the country, the date of the version, and the version identifier if stated (e.g., "National Documentation Package — Nigeria — November 28, 2025").

Step 2 — locate all NDP footnotes in the decision: scan the full RPD decision for every footnote citing an NDP tab. For each footnote found, record: the footnote number, the paragraph it appears in, the NDP country stated, the date stated in the footnote, and the tab number cited.

Step 3 — compare each footnote against the disclosed version: for each footnote recorded in Step 2, compare the date stated in the footnote against the date of the disclosed version from Step 1. Record one of:

- Match — the footnote cites the same version date as the disclosed NDP
- Mismatch — the footnote cites a different date or version
- Ambiguous — the footnote does not state a date or version clearly enough to confirm

Step 4 — verify mismatches: for each footnote returning mismatch in Step 3, verify whether the cited version exists. Check the NDP changelog for the relevant country. Record: version exists and was not disclosed, version does not exist (phantom citation), or unable to verify.

Step 5 — assess the materiality of each mismatch: for each confirmed mismatch, identify what the tab was used for in the decision. Record: the finding it was applied to, the paragraph number, and whether the finding is core or peripheral to the RPD's overall credibility conclusion.

Step 6 — record the output object (see output format below).

**Example — MC3-60592:** The NDP in evidence was the November 28, 2025 version. The RPD at footnote 5 (para 38) cited "NDP on Nigeria, 19 December 2025, tab 3.11." The NDP changelog confirms there is no December 19, 2025 version. The RPD cited a document not in the evidence the claimant had an opportunity to respond to. The citation was used specifically to impugn the Lagos friend's affidavit — the most damaging document finding in Section 3. This is a confirmed procedural error.

**Output format:**

Version check object — one per NDP footnote:

| Field | Value |
|---|---|
| Footnote number | [number] |
| RPD paragraph | [para number] |
| NDP country | [country] |
| Date cited in footnote | [date as stated in the decision] |
| Tab cited | [tab number] |
| Disclosed version date | [from Step 1] |
| Comparison result | Match / Mismatch / Ambiguous |
| Verification result | Version exists and not disclosed / Phantom citation / Unable to verify / N/A |
| Finding footnote supports | [para number and one-sentence description of the finding] |
| Materiality | Core finding / Peripheral finding |
| Procedural error confirmed | Yes / No |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: disclosure document list must be in the session — specifically the disclosed NDP version date. If not available, halt and flag the gap before proceeding. This technique must be completed before Technique 10 is invoked — Technique 10 takes the verified NDP version from this technique's Step 1 output as its mandatory input
- Input: full RPD decision text for footnote scan; claimant's disclosure document list for disclosed NDP version; NDP changelog for the relevant country for mismatch verification in Step 4
- Call sequence: invoked once per decision, triggered by the first NDP footnote encountered. Full-decision footnote scan runs in a single pass. Output is produced before any per-finding NDP analysis begins. Independent of all other techniques except that it is a prerequisite for Technique 10
- Output: one version check object per NDP footnote, structured table format, appended to the session error inventory. The verified NDP version from Step 1 is also passed as a standing input to all subsequent Technique 10 calls in the session
- Orchestrator note: phantom citations and mismatch on core findings are strong procedural grounds — flag for Phase 5 prioritization in the rpd-decision-analysis skill. A mismatch on a peripheral finding is a weaker ground but still recordable. All confirmed mismatches must be noted in the RAD memorandum's procedural fairness section

---

## Technique 15 — the representation failure inventory

**Definition:** When a claimant was inadequately represented, many apparent credibility failures are actually representation failures. Building a representation failure inventory protects the claimant from being held responsible for his lawyer's inadequacy.

**What it detects:** Adverse findings the RPD treated as the claimant's failures that were caused by counsel's failures.

**Prerequisite:** The full hearing transcript and the RPD decision must both be in the session. Technique 5 (member heard vs. understood) and Technique 11 (clarification sequence) should be complete before this technique runs — their outputs identify exchanges where the claimant's answer was unclear or clarified, which may themselves be products of inadequate preparation. Technique 9 category C flags (legal representation failure) feed directly into this technique — import those flags before beginning Step 3.

**Trigger:** Any of the following observable signals in the hearing record or RPD decision: the counsel of record did not appear and a stand-in appeared; the narrative was filed within six months of the hearing after a waiting period of more than one year; no written submissions were filed; the record shows zero clarifying questions from counsel during examination-in-chief. Any one of these signals activates a full representation failure review. If none of these signals is present, this technique does not activate.

**How to apply:**

Step 1 — establish the representation profile: from the hearing record and file documents, record the following. Each item is a binary yes/no:

- Stand-in counsel appeared: yes / no — if yes, record when stand-in was assigned relative to the hearing date
- Narrative filed late: yes / no — if yes, record the filing date, the hearing date, and the waiting period between arrival and narrative filing
- Zero clarifying questions during examination-in-chief: yes / no
- Re-examination addressed fewer than half the credibility issues on the record: yes / no — if yes, record how many issues were on the record and how many were addressed
- No written submissions filed: yes / no
- No NDP tabs cited in oral arguments: yes / no
- No jurisprudence cited: yes / no
- Witnesses not prepared for their documents: yes / no — if yes, identify which witnesses and which documents
- Forms not explained to claimant before submission: yes / no — if yes, identify which forms

Step 2 — import Technique 9 category C flags: retrieve every cause-attribution object from Technique 9 where category C (legal representation failure) returned yes. For each such object, record: the alleged omission, the finding it was used to support, and the representation failure identified. These are pre-confirmed representation failures — do not re-analyze them, import them directly.

Step 3 — map adverse findings to representation indicators: for each adverse finding in the RPD decision not already covered by Step 2, apply this test: would adequate legal preparation — defined as: sufficient preparation time, counsel familiar with the file, proper explanation of forms and narrative requirements, preparation of witnesses, and written submissions with jurisprudence — have produced the missing evidence or explanation? If yes: record the finding, the specific preparation failure that caused the gap, and the indicator from Step 1 that confirms the failure.

Step 4 — apply the binary attribution test per finding: for each finding mapped in Step 3, state: "This adverse finding at para [X] is attributable to [specific representation failure], not to the claimant's credibility. A claimant with adequate representation would have [produced the missing evidence / explained the gap / corrected the form / prepared the witness]."

Step 5 — record the output object (see output format below).

**Example — MC3-60592:** The stand-in counsel was assigned in December 2025 for a January 6, 2026 hearing. The narrative was first filed November 2025 — two years after arrival. Counsel asked zero clarifying questions during two hours of examination. Re-examination consumed five minutes and addressed two of eight identified credibility issues. No written submissions were filed. No NDP tabs were cited in oral arguments. No jurisprudence was cited. The adverse findings attributable to this representation failure include: the co-tenant affidavit problem (claimant gave narrow instruction because counsel did not explain what was needed); the address history form (claimant was not told how to complete it); the BOC checkbox hierarchy (pre-counsel checkboxes used against the sworn narrative amendment without explanation); and the eight unaddressed credibility issues on the record at the close of the hearing.

**Output format:**

Representation profile — one per file:

| Indicator | Present | Detail |
|---|---|---|
| Stand-in counsel | Yes / No | [assignment date vs. hearing date] |
| Narrative filed late | Yes / No | [filing date / hearing date / waiting period] |
| Zero clarifying questions | Yes / No | — |
| Re-examination incomplete | Yes / No | [issues on record vs. addressed] |
| No written submissions | Yes / No | — |
| No NDP tabs cited | Yes / No | — |
| No jurisprudence cited | Yes / No | — |
| Witnesses unprepared | Yes / No | [which witnesses and documents] |
| Forms unexplained | Yes / No | [which forms] |

Representation failure object — one per adverse finding attributed to counsel:

| Field | Value |
|---|---|
| RPD paragraph | [para number] |
| Adverse finding | [one sentence] |
| Source | Technique 9 category C import / Step 3 mapping |
| Representation failure | [specific failure — preparation, explanation, witness prep, form, submission] |
| Indicator confirmed | [indicator from Step 1 — yes entry] |
| Adequate representation would have | [produced X / explained Y / corrected Z] |
| Attribution statement | Para [X] is attributable to [failure], not to the claimant's credibility |
| Ground of appeal | Yes / No — [brief reason] |

**Agent integration:**

- Prerequisite: full transcript and RPD decision in session. Technique 9 category C flags imported before Step 3 begins. Technique 5 and Technique 11 outputs reviewed before Step 3 — exchanges involving misunderstanding or pre-clarification answers may themselves be representation failures if preparation would have prevented the confusion
- Input: hearing record for Step 1 profile; Technique 9 cause-attribution objects for Step 2 import; full adverse findings list from Technique 4 Reading 2 objects for Step 3 mapping
- Call sequence: invoked once per file, after the full transcript has been reviewed and Techniques 5, 9, and 11 are complete. Single pass through the Step 1 indicators, then one pass through all adverse findings not covered by Step 2. Does not run per-section or per-finding — runs once at the end of the full analysis
- Output: one representation profile per file and one representation failure object per attributed finding, both in structured table format, appended to the session error inventory as a dedicated section
- Orchestrator note: representation failures are a distinct category of ground in the RAD memorandum — they do not merge into the credibility error section. They feed the procedural fairness and natural justice arguments separately. A file with five or more confirmed representation failure indicators is a candidate for a fresh hearing argument at the RAD on the basis of inadequate representation alone

---

## Summary — the reading sequence for any RPD decision

When receiving any RPD rejection, apply the following sequence before forming any view:

1. Read the commitment paragraphs — identify every guideline, legal standard, and framework the member committed to applying (Technique 1)
2. Read the summary paragraphs — three-step check against narrative, oral testimony, and disclosure (Technique 3)
3. Read each section heading — check for pre-announced conclusions, imported profiles, and consistency with commitment paragraphs (Technique 2)
4. For each adverse finding — apply the two-reading rule (Technique 4)
5. For each adverse finding — conduct the NDP engagement check (Technique 10)
6. For each adverse finding — apply the "omissions vs. external causes" analysis (Technique 9)
7. For each corroborating document the RPD discards or criticizes — check for simultaneous discard and weaponization (Technique 7) and trace the information chain (Technique 8)
8. For each critical exchange in the transcript — identify which version (first attempt or clarified) the RPD used, and whether the member understood the claimant (Techniques 11, 5)
9. Extract positive evidence across the record and weigh it separately (Technique 6)
10. Check every NDP version citation against the disclosed NDP (Technique 14)
11. On SOGIESC files — apply the behavioral expectation test to every "if X, you would have done Y" paragraph (Technique 13)
12. On SOGIESC files — check every member follow-up question and its answer (Technique 12)
13. Build the representation failure inventory (Technique 15)

---

*Techniques extracted from MC3-60592 analysis — April 2026*
*v2 — all 15 techniques revised: type 2 (logical sequence), type 3 (Claude instruction quality), type 4 (agent-ready) fixes applied; human-readable legal content unchanged*
*This file grows as new analytical techniques are identified in future files*
