---
type: analysis
title: Open Questions Register
created: 2026-04-22
updated: 2026-04-22
tags: [analysis, standing, open-questions]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Open Questions Register

A standing register of unresolved questions raised across the programme. One row per question. Questions get a stable ID (`OQ-NNN`) so individual wiki pages can reference them.

**Refreshed every ingest.** When a question is answered, move it to the **Resolved** section with the answer, resolution date, and source.

## How to read
- **ID** — `OQ-NNN`, stable across the life of the register. IDs are never reused.
- **Question** — the thing we don't know yet.
- **Category** — scope | people | timeline | design | adr-candidate.
- **Raised in** — source that surfaced the question (or `wiki` if it emerged during synthesis).
- **Owner** — person/team accountable for resolving it, if any. Blank if unassigned.
- **Status** — `open` | `parked` (deliberately deferred) | `answered` (answered ones move to Resolved).

---

## Open

### Scope / architecture calls

| ID | Question | Raised in | Owner | Status |
|---|---|---|---|---|
| OQ-001 | **Drop Eclipse ingestion entirely?** No live consumer currently, but the Graph-API audit spike may surface one. Keep as scope call, not yet ADR. | [[sources/20260422-meeting-transcript-session-1]] | [[alex-sillars]] | open |
| OQ-002 | **Does the Analytics Team (Data Universe) tolerate a flattened `(party-id, submission-id)` shape in proxy events, or does InRisk need to expose a nesting-reconstruction API?** Single biggest Phase-1 branch point. | [[sources/20260422-meeting-transcript-session-2]] | [[paul-rogers]] (resolver) · [[joe-worsfold]] (requester) | open |
| OQ-003 | **Does any consumer beyond [[inrisk]] need the nested client-ID shape in DU events?** Input into OQ-002. | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |
| OQ-004 | **Graph API consumer map** — which endpoints are hit, by which consumers, at what volume? Spike will feed [[dependency-map]] and decommissioning scope. May resolve OQ-001. | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] · [[billy-calladine]] | open |
| OQ-005 | **Chakra V2 vs V3 widget strategy** — V2, minimal-render, or V3-only-when-InRisk-upgrades? Settled by an InRisk-in-the-room workshop. Not a Phase-1 blocker. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] · [[alex-sillars]] | open |
| OQ-006 | **Bulk-migration tool UX** — CLI-first (Phase 1 per [[bulk-migrations-owned-by-mdm-phase-1]]) is agreed. Phase 2+: CSV upload + diff preview in PCT, or separate MDM-team tool with a broader self-serve UX? | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] | open |
| OQ-007 | **Version-change event semantics** — do we push version-change events, or do consumers pull on demand? Per-consumer PO decision. | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |
| OQ-008 | **End-state sanctions flow** — owner and timing for the Phase 2+ simplification (NTT result lands on party; Party informs InRisk of affected submissions directly). | [[sources/20260422-meeting-transcript-session-2]] | _unassigned_ | open |
| OQ-009 | **Party-merge event handling in InRisk** — explicit handling needed, or can it be absorbed via the `by ID` follow-the-merge lookup pattern? | [[sources/20260422-meeting-transcript-session-2]] | [[john-trahearn]] · [[kris-mokrzycki]] | open |
### Timeline / cadence

| ID | Question | Raised in | Owner | Status |
|---|---|---|---|---|
| OQ-011 | **D&B scheduled-refresh cadence** — monthly or quarterly? Implementation note on [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] (via "enriched team" — OQ-012) | open |
| OQ-012 | **"Enriched team" identity and charter** — named counterpart for D&B caching and S&P future-path work. | [[sources/20260422-meeting-transcript-session-2]] | _unassigned_ | open |
| OQ-013 | **HV integration-shape specifics** — write volume, batch vs steady-state, whether HV creates parties programmatically, auto-curation expectations. Sizes the MDM intercept + backfill and the D&B auto-parent behaviour. | [[sources/20260422-meeting-transcript-session-2]] | [[simon-hulbert]] (HV side) · [[rory-beattie]] (programme side) | open |
| OQ-014 | **DataOps change-management lead time** — must overlap with the development timeline; cannot be done post-hoc. | [[sources/20260422-meeting-transcript-session-2]] | [[hugh-lobban]] · [[will-bone]] | open |
| OQ-015 | **Broker-retrieval workshop date + participants** — target Mon/Tue after Romania trip. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-016 | **Anti-patterns / data-fixes workshop scheduling.** | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |

### Design open calls

| ID | Question | Raised in | Owner | Status |
|---|---|---|---|---|
| OQ-017 | **MDM-delivery squad shape** — focused ~3-person squad (deep context, knowledge-bottleneck risk) vs. whole ~13-person [[graph-team]] (slower context-sharing, more resilience). Blocks sprint-planning for intercept / backfill / adapter work. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] (primary); consulted: [[alex-sillars]] | open — Rory's call; will merit further discussion |
| OQ-018 | **Final-state Party contract specification** — [[inrisk-engine]]'s go-live gating. Publish the spec ahead of building it so [[antonie-labuschagne]] / [[devx-team]] can plan against it. | [[sources/20260422-meeting-transcript-session-2]] | [[graph-team]] ([[joe-worsfold]], [[alex-sillars]]) | open |
| OQ-019 | **Feature-tagging migration timing** — [[feature-tagging-moves-to-inrisk]] is accepted for Phase 2+ but the trigger condition and concrete date remain unset. | [[sources/20260422-meeting-transcript-session-1]] | [[prebind-team]] + [[graph-team]] | open |
| OQ-020 | **Auto-created parent UX on PCT** — making D&B auto-created parents visible, editable, and clearly provenance-marked. Referenced from [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |
| OQ-021 | **Cache invalidation policy on explicit re-lookup or record merge** — for [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |
| OQ-022 | **"Tech users" model in new PCT** — dedicated UX for HV-created draft parties, or reuse of existing role permissions? | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |

### People identification

| ID | Question | Raised in | Owner | Status |
|---|---|---|---|---|
| OQ-023 | **Speakers 7 and 15** from [[sources/20260422-meeting-transcript-session-2]]. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-024 | **Sam** — first-name-only reference in Session 2; appears in HV / programme neighbourhood. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-025 | **Bob** — first-name-only reference in Session 2; appears in the InRisk neighbourhood. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-026 | **Josie** — first-name-only reference in Session 2. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-027 | **Michael Hay** — stakeholder in feature-tagging functionality. Team and scope of stakeholder role still TBD. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-028 | **Kartik** — name-only mention in Session 1; context thin. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-029 | **Jenny / Ginny** — name-only mention in Session 1; GraphQL-endpoint context. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-030 | **Andrew Tennant** — name-only mention in Session 1; acting head for MRS team, on leave. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
### ADR candidates (deferred)

| ID | Question | Raised in | Owner | Status |
|---|---|---|---|---|
| _none_ | _All Session 1 ADR candidates have been either promoted ([[no-pct-audit-backfill]], [[feature-tagging-moves-to-inrisk]], [[bulk-migrations-owned-by-mdm-phase-1]]) or kept as annotations ([[pct-and-mdm-go-live-together]] single-flag rollout; Eclipse retirement tracked as OQ-001)._ | — | — | — |

---

## Resolved

When a question is answered, move it here with the answer, resolution date, and resolving source. Keep the original ID.

### From pre-register triage (2026-04-22)

Historical items — resolved before the register formally existed, captured here for completeness.

| ID | Question | Answer | Resolved | Source |
|---|---|---|---|---|
| OQ-R01 | HV production date (1 Jan speculation vs. 1 Sep) | **1 September** — 1 Jan speculation was a Pass A inference that the user rejected; there is only the 1 Sep deadline. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R02 | Sergiu's team affiliation | [[sergiu-postolachi]] is **Scrum Master on [[graph-team]]**, not [[tech-tooling]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R03 | "Data Universe Team" canonical name | Formally **[[analytics-team]]**; "Data Universe" is the Snowflake platform they own. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R04 | "Anton" — team and role | [[antonie-labuschagne]], **Tech Lead on [[devx-team]]** (not an InRisk PO). | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R05 | "Hugh" — role and team | [[hugh-lobban]], **Head of Data Quality on [[data-quality-team]]**. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R06 | John + Chris team attribution | [[john-trahearn]] and [[kris-mokrzycki]] (corrected spelling from phonetic "Chris") are **Tech Leads on [[prebind-team]]**, not DevX. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R07 | Andrea's team | [[andrea-read]], **Head of Technology Engineering on [[tech-tooling]]**. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R08 | HV owning team | [[high-volume-team]], main Party-integration contact [[simon-hulbert]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R09 | ELA / FAS acronym expansions | ELA = Expense and Loss Adjustment; FAS = Free Alongside Ship. | 2026-04-22 | user correction |
| OQ-R10 | Session 1 Speaker 6 | [[billy-calladine]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R11 | Session 1 Speaker 7 | [[joe-worsfold]]. Transcription engine assigned a second speaker ID to Joe within the same session. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R12 | "Tom Ash / Tomas" identity | **Tomas Sivo**, Business Analyst on [[graph-team]]. Transcription of "Tomas" drifted to "Tom Ash". See [[tomas-sivo]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R13 | "Darius" identity | **Not a person** — mispronunciation of "InRisk" by the transcription engine. Remove from people references. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R14 | "Boyle" identity and the afternoon external-provider session | **Not a person** — mispronunciation of "Will" ([[will-bone]]) in reference to him joining the afternoon session ([[sources/20260422-meeting-transcript-session-2]]). No third recording exists. Remove from people references. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-010 | Knowledge Graph owner and future | **Not a separate application** — the "Knowledge Graph" is a **Neo4j instance internal to [[party-application]]**, acting as the primary datastore in the current state. It is replaced by DynamoDB + OpenSearch in the target state. Removed from the data-distribution bucket on [[dependency-map]] and [[contract-buckets]]. | 2026-04-22 | user correction, lint pass |
| OQ-031 | PreBind Product Owner identity | **Daria Romanovskaia** — Product Owner on [[prebind-team]]. See [[daria-romanovskaia]]. | 2026-04-22 | user correction, lint pass |

---

## Conventions

1. **New questions** get appended to the appropriate **Open** table with the next unused `OQ-NNN`.
2. **Resolution** moves the row verbatim to the **Resolved** table, plus an `Answer`, `Resolved` date, and `Source` column; the original **Open** row is deleted.
3. **Reopening** is allowed (re-surfacing evidence contradicts the prior answer) — create a new ID `OQ-NNN-R` and cross-link.
4. **Referencing from wiki pages**: `see [[open-questions#OQ-014]]`. Pages shouldn't duplicate the full question text — link and summarise.
5. **No silent resolutions.** A question moves only when there's a named source for the answer (or an explicit user confirmation).

## Related
- [[ownership-matrix]] — open **actions** (things to do); complements this register of open **questions** (things to find out).
- [[dependency-map]] — many questions here become edges in the map once resolved.
- [[overview]] — the "What I'd want to know next" section summarises the highest-leverage open questions.

## Source history
- 2026-04-22 — Session 1 Pass B: register created; seeded with 31 open questions (Session 1 + Session 2 carryover) and 14 resolved entries from the pre-register triage.
- 2026-04-22 — lint pass: **OQ-010** (Knowledge Graph) resolved — reclassified as internal Neo4j datastore inside [[party-application]]. **OQ-031** (PreBind PO) resolved — [[daria-romanovskaia]]. Open-question count now 29; resolved count now 16.
