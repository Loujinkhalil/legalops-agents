## time log

| # | event | date | time | timezone | duration | description |
|---|-------|------|------|----------|----------|-------------|
| 1 | created | 2026-04-03 | 09:47:22 | EDT | 2m 01s | protocol-only version — 11 context engineering improvements to RPD skill note agent architecture — time log and session-specific header removed — all improvement content preserved |

---

# Context engineering improvements to RPD skill note

**Version:** v1
**Source document:** RPD skill improvement note — 15-technique analytical library
**Reference:** Context engineering discipline (context window budget, position law, conflicting context, memory strategy)
**Purpose:** Document 11 specific improvements to the RPD skill note and its agent integration architecture, derived from applying the context engineering framework to the existing 15-technique system.

---

## Background

The context engineering framework treats the LLM context window as a finite budget governed by three physical laws: (1) context rot — accuracy degrades as token count grows, even well before the context limit is reached; (2) position matters — models attend most strongly to information at the beginning and end of the context window, and information buried in the middle is systematically underattended; (3) conflicting context — contradictory information in the context window produces ambiguous or unreliable outputs.

The RPD skill improvement note (v2) is a 15-technique analytical library designed for three simultaneous uses: human legal practitioners, Claude skill execution, and Claude Code agent specification. The agent integration sections of each technique define prerequisites, inputs, call sequences, and outputs. However, gaps exist that the context engineering framework identifies and fills.

None of the improvements below touch the human-readable legal content of any technique. All improvements are to the agent integration architecture only.

---

## Improvement 1 — Context budget annotation per technique

**Problem.** Each technique's agent integration section currently specifies prerequisites and input sources, but does not define the minimum context load required for that technique to run. Without this, an agent executing all 15 techniques in sequence risks loading the full RPD decision, the full transcript, all prior technique output objects, the NDP, and the BOC simultaneously across the session. This is the core condition that produces context rot — accuracy degrading under accumulated token load.

**What is currently missing.** A context budget field in each technique's agent integration section specifying: (a) which documents are required, (b) which prior technique output objects are required, and (c) which documents and outputs are explicitly not needed and should not be loaded.

**Mapping to specific techniques.**

The following table shows the minimum context load per technique as currently analyzable from the technique definitions:

| Technique | Required documents | Required prior outputs | Explicitly not needed |
|---|---|---|---|
| 1 — Guideline commitment | RPD decision — commitment paragraphs only | None | Transcript, NDP, BOC |
| 2 — Section headings | RPD decision — headings and first sentence of each section | Technique 1 output | Transcript, NDP, BOC |
| 3 — Summary paragraphs | RPD decision — summary section; BOC narrative; BOC disclosure index | Technique 1 output | Transcript, NDP |
| 4 — Two-reading rule | RPD decision — current section; transcript — relevant exchange | Technique 1 output | NDP, BOC (unless section-specific) |
| 5 — Misunderstanding detection | Transcript — relevant exchanges | Technique 4 Reading 1 output | RPD decision body, NDP |
| 6 — Positive evidence | RPD decision — full text; BOC; disclosure index | Technique 4 Reading 2 output | NDP (unless cited in RPD) |
| 7 — Discard and weaponize | RPD decision — document criticism paragraphs; the criticized document | Technique 4 Reading 2 output | NDP, transcript (unless document-specific) |
| 8 — Information chain | Criticized document; RPD decision — criticism paragraph | Technique 7 output | NDP, BOC |
| 9 — Omissions analysis | RPD decision — adverse finding; transcript — relevant exchange | Technique 4 Reading 2 output; Technique 8 output where applicable | NDP |
| 10 — NDP engagement | RPD decision — relevant section; current NDP — relevant tabs | Technique 4 Reading 2 output; Technique 14 output | Transcript, BOC |
| 11 — Clarification version | Transcript — full exchange including pre- and post-clarification answers | Technique 4 Reading 1 output | NDP, BOC |
| 12 — Member questions | Transcript — member question exchanges | Technique 4 Reading 1 output; Technique 1 output | NDP, BOC |
| 13 — Behavioral expectations | RPD decision — expectation paragraphs; NDP Section 6 — SOGIESC tabs | Technique 4 Reading 2 output; Technique 1 output | BOC, transcript (unless exchange-specific) |
| 14 — NDP version check | RPD decision — NDP citations; disclosed NDP — cited tabs | None (runs independently) | Transcript, BOC |
| 15 — Representation failure | Transcript — full; RPD decision — adverse findings | Technique 9 category C output; Technique 5 output; Technique 11 output | NDP, BOC |

**Action required.** Add a "context budget" sub-field to the agent integration section of each of the 15 techniques, with three entries: required documents, required prior outputs, and explicitly excluded items.

---

## Improvement 2 — Position anchoring for high-dependency outputs

**Problem.** The position law states that information buried in the middle of a long context is systematically underattended by the model. In a multi-technique session, the Technique 1 output (guideline commitment profile) and the Technique 4 Reading 2 output (adverse findings inventory) are the two most foundational objects in the entire analysis. Both are produced early in the session and are referenced by many subsequent techniques. As the session grows, these objects move progressively further from the start of the context window.

**What is currently missing.** An explicit instruction in each dependent technique's agent integration section to re-anchor these high-dependency outputs at the top of the context for that technique call, not assume they are retrievable from a growing session history.

**Techniques that depend on Technique 1 output:** 2, 3, 4, 5, 12, 13. The guideline commitment profile must be the first object in the context for each of these calls.

**Techniques that depend on Technique 4 Reading 2 output:** 6, 7, 9, 10, 11, 12, 13, 15. The adverse findings inventory must be loaded at the top of the context for each of these calls — not inherited from session history.

**Action required.** Add a "position instruction" sub-field to the agent integration section of each dependent technique, specifying which output objects must be positioned at the top of the context window before that technique call begins.

---

## Improvement 3 — Memory strategy formalization

**Problem.** The handoff note system, the static extraction file, and the active analysis file constitute a functioning long-term memory architecture. This architecture already makes the right design decisions — completed outputs are written to a static file, raw transcript segments are not preserved once processed, and subsequent techniques retrieve selectively from prior output objects. However, these decisions are currently implicit in the workflow rather than specified as memory instructions in the agent integration sections.

**What is currently missing.** A "memory instruction" sub-field in the agent integration section of each technique, specifying: (a) what this technique's output object should be stored as in the session memory (full object or key fields only); (b) whether raw input material (transcript segments, RPD paragraphs) can be discarded after the technique runs; and (c) what compressed form of the output is sufficient for downstream techniques that reference it only partially.

**Example.** Technique 9 produces a cause-attribution output object. Technique 15 imports only the category C flags from that object. The memory instruction for Technique 9 would specify: store the full cause-attribution object in the session error inventory; for Technique 15 purposes, only the category C flag rows need to be retrieved, not the full object. This distinction reduces the token load on Technique 15's context without losing analytical integrity.

**Highest-priority techniques for this fix.** Techniques 4, 6, 9, and 15 — these produce the largest output objects and are the most frequently referenced by downstream techniques.

**Action required.** Add a "memory instruction" sub-field to the agent integration section of Techniques 4, 6, 9, and 15 as a first pass, then extend to the remaining techniques in a second pass.

---

## Improvement 4 — Pre-injection conflict detection

**Problem.** The conflict context law states that contradictory information loaded into the context window simultaneously produces ambiguous or unreliable outputs. The dual-reading protocol (Technique 4) and the omissions analysis (Technique 9) both function as conflict detection at the analytical level — they identify when the RPD's stated record contradicts the actual record. This is correct. However, the conflict detection problem also exists one step earlier: at the point of loading retrieved chunks into the technique's context.

When Techniques 4, 6, 9, and 10 load multiple source documents simultaneously (RPD decision + transcript + BOC + NDP tabs), those sources may contain internal contradictions that have not yet been flagged. If the contradiction is loaded into the context undetected, the technique's output may hedge or produce an ambiguous finding without identifying the source of the ambiguity.

**What is currently missing.** A pre-injection conflict check at the start of Techniques 4, 6, 9, and 10 — a brief scan of the loaded source documents for contradictions on the specific issue being analyzed before the technique's substantive steps begin.

**What this check looks like.** Before Step 1 of each of these techniques, add a preliminary step: identify whether the source documents being loaded for this technique contain conflicting accounts of the same event, fact, or document. If a conflict is detected, flag it explicitly in the output object before the technique's substantive analysis proceeds. This ensures the conflict is named and analyzed rather than absorbed silently into the output.

**Action required.** Add a "pre-injection conflict check" step as Step 0 in the how-to-apply sections of Techniques 4, 6, 9, and 10, with an instruction to flag detected conflicts before substantive analysis begins.

---

## Improvement 5 — Tiered loading architecture for the skill file itself

**Problem.** The tool budget concept applies directly to the skill file. The file is approximately 1000 lines. When loaded as a skill at session start, all 15 techniques — with full definitions, action steps, output objects, and agent integration sections — are loaded simultaneously. This consumes a large portion of the context budget before any RPD decision, transcript, or NDP tab arrives. The governing principle: give agents the minimum set needed for the current task, loaded dynamically.

**What is currently missing.** A tiered loading architecture. The file currently has one loading level — everything. It needs two: a lightweight index loaded at session start, and full technique content loaded on demand when a technique's trigger fires.

**What the lightweight index would contain.** For each of the 15 techniques: the technique name, the trigger signal (one sentence), and the prerequisite (one sentence). This is the minimum needed for the orchestrator to decide which technique to activate. Total estimated size: 15 rows of three fields — a fraction of the full file load.

**What loads on demand.** When a trigger fires for a specific technique, that technique's full content — definition, how-to-apply steps, output format, agent integration — is loaded at that point and only at that point. Once the technique's output object is recorded, the full technique content can be released from the context.

**Action required.** Extract a lightweight index from the skill file — one row per technique with name, trigger, and prerequisite — and designate it as the session-start load. Mark all full technique content as on-demand. Add a loading instruction at the top of the file specifying this architecture.

---

## Improvement 6 — Chunk redundancy check in multi-source techniques

**Problem.** Redundant retrieved chunks — chunks that repeat the same information — consume token budget without adding signal. Techniques 3, 6, 9, and 10 all load multiple source documents simultaneously. The same event, fact, or document may appear in the BOC narrative, the oral testimony, and a corroborating document — three overlapping passages loaded at the same time.

**What is currently missing.** A redundancy check step in each multi-source technique. Before injecting source passages into the technique's context, the agent should confirm that each passage adds unique content relative to the others being loaded for the same finding.

**Most acute case.** Technique 6 (positive evidence pass) loads the full BOC narrative, the full transcript, and all disclosure documents simultaneously, then checks all three against the same list of adverse findings. The same piece of positive evidence — for example, the description of physical attraction to Abiodun in MC3-60592 — may appear in nearly identical form in the narrative and was repeated at the hearing. Loading both without a redundancy check doubles the token cost for a single piece of evidence.

**Action required.** Add a redundancy check step to the how-to-apply sections of Techniques 3, 6, 9, and 10. The step instruction: before loading source passages for a specific finding, check whether the same content is present in more than one source. If yes, load the most detailed version and note the cross-source confirmation as a single flag rather than loading all versions separately.

---

## Improvement 7 — Chunk length limits for transcript loading

**Problem.** Chunks that are too long consume token budget and dilute the model's attention on the relevant content. Techniques 5, 11, and 12 all load transcript passages. Technique 11 defines a boundary of 60 seconds before and after the cited exchange. In a dense hearing, 60 seconds of testimony can be hundreds of words. No maximum chunk size is defined anywhere in the skill note, and no compression or truncation instruction exists for any transcript-loading technique.

**What is currently missing.** A maximum chunk length instruction and a compression fallback for each transcript-loading technique. When a 60-second boundary produces a chunk above the defined maximum, the technique should instruct the agent to compress the outer portions of the boundary while preserving the verbatim core exchange.

**Action required.** Add a chunk length instruction to the agent integration sections of Techniques 5, 11, and 12. Define a maximum chunk size for transcript passages (suggested: 300 words for the core exchange plus boundary context). Add a compression fallback: if the boundary produces content above the maximum, summarize the outer boundary and preserve only the verbatim core exchange — the cited answer and any clarification events.

---

## Improvement 8 — Reranking before injection in multi-source techniques

**Problem.** The position law states that the most relevant content must be positioned first in the loaded context. When Techniques 6, 9, 10, and 13 load multiple sources or multiple NDP tabs, the loading order is either sequential (narrative first, then transcript, then disclosure) or arbitrary (tabs in table-of-contents order). Neither order is a relevance ranking. The most relevant NDP tab or the most directly supportive passage may land in the middle of a large multi-source load — the position where the model systematically underattends.

**What is currently missing.** A reranking step in each multi-source technique specifying that before injection, loaded passages and tabs are ordered by direct relevance to the current finding — most directly relevant first, least relevant last or excluded.

**Most acute case.** Technique 10 (NDP engagement check). The technique loads NDP tabs to check engagement against an adverse finding. The priority categories table partially addresses this by directing the agent to the most relevant tabs first. But when multiple tabs are loaded together for context, they are not ranked — a tab peripherally related to the finding may load before the directly relevant one.

**Action required.** Add a reranking step to the agent integration sections of Techniques 6, 9, 10, and 13. The instruction: before injecting loaded source passages or NDP tabs, rank them by direct relevance to the specific finding being analyzed. Position the highest-relevance item first. Items with no direct relevance to the current finding should not be loaded.

---

## Improvement 9 — Context compression for full-document loading techniques

**Problem.** Context compression is one of the highest-return techniques in production systems — summarizing each chunk relative to the current query before injection, reducing a 500-token chunk to approximately 80 tokens while preserving the majority of the signal. No technique in the skill note includes a compression step. Techniques 3, 6, 10, and 14 are the highest-risk for context overload because they load full documents or full NDP sections rather than targeted passages.

**What is currently missing.** A compression instruction in each full-document loading technique specifying that before injection, each loaded source is filtered and summarized relative to the specific finding or question being analyzed in that technique call.

**Technique-by-technique compression opportunity:**

| Technique | Current load | Compression instruction needed |
|---|---|---|
| 3 — Summary check | Full BOC narrative; full transcript | Filter to passages addressing the same events as the summary paragraphs under review |
| 6 — Positive evidence | Full BOC narrative; full transcript; all disclosure | Filter each source to passages relevant to the current adverse finding before injection; load finding by finding, not all at once |
| 10 — NDP engagement | Full NDP table of contents; relevant tab content | Load tab content only for tabs matching the finding subject; exclude all other NDP content |
| 14 — NDP version check | Full RPD decision for footnote scan | Footnote scan only — extract footnote lines first; do not load full decision body for this technique |

**Action required.** Add a compression instruction to the agent integration sections of Techniques 3, 6, 10, and 14. The instruction format: before loading [source], filter to [specific relevance criterion] relative to the current [finding / paragraph / subject matter]. Load the filtered content, not the full document.

---

## Improvement 10 — Poisoning and distraction failure modes

**Problem.** Four conflict failure modes exist: poisoning (irrelevant information distracts the model), distraction (slightly off-topic content pulls the model away), confusion (contradictory information creates ambiguous outputs), and clash (two chunks directly contradict each other). Improvement 4 addressed confusion and clash. Poisoning and distraction were not addressed.

In the technique context, these are distinct problems from contradiction. Poisoning occurs when irrelevant content is loaded alongside relevant content — the irrelevant content consumes attention without contributing to the output. Distraction occurs when slightly off-topic content — related to the general subject but not to the specific finding — shifts the analysis away from the precise question.

**Most concrete cases in the skill note:**

Poisoning — Technique 10 NDP loading. When the agent loads the NDP table of contents to identify relevant tabs, all sections unrelated to the finding subject are loaded simultaneously. In a Nigeria NDP with 15+ sections, a finding about bisexuality does not need sections on documentation fraud, border procedures, or health conditions. Those sections are poisoning — irrelevant content consuming attention on the specific question of SOGIESC country conditions.

Distraction — Technique 6 positive evidence pass. When the agent loads the full BOC narrative to extract positive evidence on a specific adverse finding, the narrative contains extensive background on events unrelated to that finding. That background is not irrelevant to the case as a whole — it is slightly off-topic relative to the specific finding being analyzed. It is distraction in the technical sense: related content that pulls the analysis toward the general case narrative rather than the specific finding.

**What is currently missing.** A relevance filter instruction at the start of Techniques 6 and 10 — the two techniques most affected — specifying that only content directly addressing the current finding subject should be loaded. A general irrelevance exclusion instruction for any technique that loads full documents.

**Action required.** Add a relevance filter note to the agent integration sections of Techniques 6 and 10 specifically, and add a general instruction at the top of the skill file: when loading any document for a per-finding technique, load only the portions directly addressing the current finding subject. Exclude all other content — even if it is related to the case generally.

---

## Improvement 11 — Reading sequence internal inconsistency

**Problem.** This is not a context engineering deficiency — it is an internal logical error identified during the full read of the techniques file. The revision plan's "proposed revised reading sequence" and the final summary reading sequence at the end of the file place Technique 14 at different positions.

In the revision plan: Technique 14 is step 4 — immediately after identifying the summary boundary and before reading section headings.

In the final summary: Technique 14 is step 10 — near the end of the sequence, after Technique 6 (positive evidence pass).

The handoff note records this discrepancy as "reading sequence summary: pending reorder to match document workflow." The reorder was never executed. An agent following the final summary's step 10 placement would run Technique 10 (NDP engagement check) across all adverse findings before running Technique 14 (NDP version check) — but Technique 10 explicitly requires Technique 14's output as its mandatory prerequisite. The final summary sequence, as written, produces an impossible execution order.

**Action required.** Resolve the conflict by adopting the revision plan's sequence placement for Technique 14 — step 4, before any NDP-reliant analysis — and updating the final summary accordingly. The revised reading sequence summary must reflect the version where Technique 14 precedes Technique 10.

---

## Implementation sequence

All 11 improvements listed in priority order based on impact on agent reliability and severity of the deficiency:

| Priority | Improvement | Type | Techniques affected | Effort |
|---|---|---|---|---|
| 1 | Reading sequence inconsistency (Improvement 11) | Internal logical error | Final summary + revision plan | Immediate — one edit |
| 2 | Context budget annotation (Improvement 1) | Context rot | All 15 | Medium — one sub-field per technique |
| 3 | Tiered loading architecture (Improvement 5) | Tool token budget | Skill file as a whole | Medium — extract lightweight index |
| 4 | Context compression (Improvement 9) | Context rot | 3, 6, 10, 14 | Medium — compression instructions per technique |
| 5 | Reranking before injection (Improvement 8) | Position law | 6, 9, 10, 13 | Low — one reranking step per technique |
| 6 | Position anchoring for anchor outputs (Improvement 2) | Position law | 2, 3, 4, 5, 6, 7, 9, 10, 11, 12, 13, 15 | Low — one instruction line per technique |
| 7 | Poisoning and distraction filter (Improvement 10) | Conflicting context | 6, 10 + general instruction | Low — relevance filter per technique |
| 8 | Pre-injection conflict check (Improvement 4) | Conflicting context | 4, 6, 9, 10 | Low — Step 0 per technique |
| 9 | Chunk redundancy check (Improvement 6) | Context quality | 3, 6, 9, 10 | Low — one check step per technique |
| 10 | Chunk length limits (Improvement 7) | Context quality | 5, 11, 12 | Low — max size + compression fallback per technique |
| 11 | Memory strategy formalization (Improvement 3) | Memory | 4, 6, 9, 15 first pass; all 15 second pass | Medium — compressed output format definitions |

---

## Relationship to the three uses of the skill note

All improvements apply differently across the three uses of the skill note:

**For human legal practitioners.** Improvements 3 and 4 are most directly useful. Memory formalization tells the practitioner what to carry forward from each technique and what can be set aside. Pre-injection conflict detection formalizes what experienced practitioners do instinctively — noticing when the sources tell different stories before deciding which analysis to run.

**For Claude skill execution.** All improvements apply. Context budget and position anchoring are the most important for session reliability across long RPD decisions and transcripts.

**For Claude Code agent specification.** All improvements apply and are most precisely actionable at this level. Context budget, position anchoring, and memory instructions translate directly into agent orchestration logic — what to load, what to position first, what to compress, and when to discard.

---

*v1 — 11 context engineering improvements identified and mapped to the RPD skill note agent architecture*
*Source: context engineering framework applied to the 15-technique RPD analytical library*
