---
type: analysis
title: Open Questions Register
created: 2026-04-22
updated: 2026-05-18
tags: [analysis, standing, open-questions]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up]
source_count: 3
status: draft
---

# Open Questions Register

A standing register of unresolved questions raised across the programme. One row per question. Questions get a stable ID (`OQ-NNN`) so individual wiki pages can reference them.

**Refreshed every ingest.** When a question is answered, move it to the **Resolved** section with the answer, resolution date, and source.

## How to read
- **ID** — `OQ-NNN`, stable across the life of the register. IDs are never reused.
- **Project** — project slug the question is scoped to (e.g. `party-rearch`, `secrets-management`), or `portfolio` for cross-project questions. See [[overview]] for the current project list.
- **Question** — the thing we don't know yet.
- **Category** — scope | people | timeline | design | adr-candidate (table-section headings below).
- **Raised in** — source that surfaced the question (or `wiki` if it emerged during synthesis).
- **Owner** — person/team accountable for resolving it, if any. Blank if unassigned.
- **Status** — `open` | `parked` (deliberately deferred) | `answered` (answered ones move to Resolved).

---

## Open

### Scope / architecture calls

| ID | Project | Question | Raised in | Owner | Status |
|---|---|---|---|---|---|
| OQ-001 | party-rearch | **Drop Eclipse ingestion entirely?** No live consumer currently, but the Graph-API audit spike may surface one. Keep as scope call, not yet ADR. | [[sources/20260422-meeting-transcript-session-1]] | [[alex-sillars]] | open |
| OQ-002 | party-rearch | **Does the Analytics Team (Data Universe) tolerate a flattened `(party-id, submission-id)` shape in proxy events, or does InRisk need to expose a nesting-reconstruction API?** Single biggest Phase-1 branch point. | [[sources/20260422-meeting-transcript-session-2]] | [[paul-rogers]] (resolver) · [[joe-worsfold]] (requester) | open |
| OQ-003 | party-rearch | **Does any consumer beyond [[inrisk]] need the nested client-ID shape in DU events?** Input into OQ-002. | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |
| OQ-004 | party-rearch | **Graph API consumer map** — which endpoints are hit, by which consumers, at what volume? Spike will feed [[party-rearch-dependency-map]] and decommissioning scope. May resolve OQ-001. | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] · [[billy-calladine]] | open |
| OQ-006 | party-rearch | **Bulk-migration tool UX** — CLI-first (Phase 1 per [[bulk-migrations-owned-by-mdm-phase-1]]) is agreed. Phase 2+: CSV upload + diff preview in PCT, or separate MDM-team tool with a broader self-serve UX? | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] | open |
| OQ-007 | party-rearch | **Version-change event semantics** — do we push version-change events, or do consumers pull on demand? Per-consumer PO decision. | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |
| OQ-008 | party-rearch | **End-state sanctions flow** — owner and timing for the Phase 2+ simplification ([[ntt]] result lands on party; Party informs [[inrisk]] of affected submissions directly). Sharpened 2026-05-13: this is the _flow_ question that pairs with [[open-questions#OQ-032]]'s _location_ question — once the orchestration leaves Boomi, what's the end-state contract? See [[sanctions-processing]]. | [[sources/20260422-meeting-transcript-session-2]] · [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | _unassigned_ — escalation target [[andrea-read]] | open |
| OQ-009 | party-rearch | **Party-merge event handling in InRisk** — explicit handling needed, or can it be absorbed via the `by ID` follow-the-merge lookup pattern? | [[sources/20260422-meeting-transcript-session-2]] | [[john-trahearn]] · [[kris-mokrzycki]] | open |
| OQ-032 | party-rearch | **Sanctions-domain location** — the orchestration logic that today lives in Boomi (firing on every party-change event, calling [[inrisk]] one submission at a time, building its own cache + idempotency layer, calling [[ntt]] for the actual check) is in the wrong place by group consensus. Where should it live? Own service / domain? Owned by which team? Audit pressure this year is the forcing function. See [[sanctions-processing]] · [[ntt]]. Out of Phase 1. | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | _unassigned_ — escalation: [[rory-beattie]] · [[suzanna-whitefield]] → [[andrea-read]] | open |
### Timeline / cadence

| ID | Project | Question | Raised in | Owner | Status |
|---|---|---|---|---|---|
| OQ-011 | party-rearch | **D&B scheduled-refresh cadence** — monthly or quarterly? Implementation note on [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-1]] | [[joe-worsfold]] (via "enriched team" — OQ-012) | open |
| OQ-012 | party-rearch | **"Enriched team" identity and charter** — named counterpart for D&B caching and S&P future-path work. | [[sources/20260422-meeting-transcript-session-2]] | _unassigned_ | open |
| OQ-013 | party-rearch | **HV integration-shape specifics** — write volume, batch vs steady-state, whether HV creates parties programmatically, auto-curation expectations. Sizes the MDM intercept + backfill and the D&B auto-parent behaviour. | [[sources/20260422-meeting-transcript-session-2]] | [[simon-hulbert]] (HV side) · [[rory-beattie]] (programme side) | open |
| OQ-014 | party-rearch | **DataOps change-management lead time** — must overlap with the development timeline; cannot be done post-hoc. | [[sources/20260422-meeting-transcript-session-2]] | [[hugh-lobban]] · [[will-bone]] | open |
| OQ-015 | party-rearch | **Broker-retrieval workshop date + participants** — target Mon/Tue after Romania trip. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-016 | party-rearch | **Anti-patterns / data-fixes workshop scheduling.** | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |
| OQ-035 | party-rearch | **Concrete InRisk MDM-cutover date.** Per [[inrisk-cuts-over-before-high-volume]], InRisk cuts over to MDM ≥ 2 weeks before 1 Sep — i.e. mid-August at the latest. A concrete date is not yet set; sizes how much buffer HV's 1 Sep go-live actually gets, and the InRisk-side sprint plan. | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | [[rory-beattie]] · [[john-trahearn]] · [[joe-worsfold]] | open |

### Design open calls

| ID | Project | Question | Raised in | Owner | Status |
|---|---|---|---|---|---|
| OQ-017 | party-rearch | **MDM-delivery squad shape** — focused ~3-person squad (deep context, knowledge-bottleneck risk) vs. whole ~13-person [[graph-team]] (slower context-sharing, more resilience). Blocks sprint-planning for intercept / backfill / adapter work. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] (primary); consulted: [[alex-sillars]] | open — Rory's call; will merit further discussion |
| OQ-018 | party-rearch | **Final-state Party contract specification** — [[inrisk-engine]]'s go-live gating. Publish the spec ahead of building it so [[antonie-labuschagne]] / [[devx-team]] can plan against it. | [[sources/20260422-meeting-transcript-session-2]] | [[graph-team]] ([[joe-worsfold]], [[alex-sillars]]) | open |
| OQ-019 | party-rearch | **Feature-tagging migration timing and target shape** — [[feature-tagging-moves-to-inrisk]] is accepted for Phase 2+ but the trigger condition and concrete date remain unset. **Advanced 2026-05-13**: [[suzanna-whitefield]] to investigate whether feature tagging has become a fully static list (no new options added in two years). If yes, the migration likely becomes an **InRisk-classifications-style change** rather than a Postgres-table-plus-widget handover, materially simpler. Phase-1 carve-out is explicit: old backend (Postgres table + API + widget) stays alive past cutover so [[inrisk]] continues to pull its static list without interruption. | [[sources/20260422-meeting-transcript-session-1]] · [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | [[suzanna-whitefield]] (static-list investigation) · [[prebind-team]] + [[graph-team]] (migration execution) | open |
| OQ-020 | party-rearch | **Auto-created parent UX on PCT** — making D&B auto-created parents visible, editable, and clearly provenance-marked. Referenced from [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |
| OQ-021 | party-rearch | **Cache invalidation policy on explicit re-lookup or record merge** — for [[d-and-b-caching-and-auto-parent]]. | [[sources/20260422-meeting-transcript-session-2]] | [[joe-worsfold]] | open |
| OQ-022 | party-rearch | **"Tech users" model in new PCT** — dedicated UX for HV-created draft parties, or reuse of existing role permissions? | [[sources/20260422-meeting-transcript-session-2]] | [[alex-sillars]] | open |
| OQ-036 | party-rearch | **Widget-response field alignment** — will Joe's new MDM widget return everything [[inrisk]] needs in its response, given OpenSearch is indexed slightly differently than the underlying Dynamo storage? Sergiu raised in-call. Joe expects to adjust the widget's response shape as integration starts; iterate during stories 3, 4 (and 5) of the Party MDM Integration epic. | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | [[joe-worsfold]] · [[john-trahearn]] | open |

### People identification

| ID | Project | Question | Raised in | Owner | Status |
|---|---|---|---|---|---|
| OQ-023 | party-rearch | **Speakers 7 and 15** from [[sources/20260422-meeting-transcript-session-2]]. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-024 | party-rearch | **Sam** — first-name-only reference in Session 2; appears in HV / programme neighbourhood. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-025 | party-rearch | **Bob** — first-name-only reference in Session 2; appears in the InRisk neighbourhood. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-026 | party-rearch | **Josie** — first-name-only reference in Session 2. | [[sources/20260422-meeting-transcript-session-2]] | [[rory-beattie]] | open |
| OQ-027 | party-rearch | **Michael Hay** — stakeholder in feature-tagging functionality. Team and scope of stakeholder role still TBD. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-028 | party-rearch | **Kartik** — name-only mention in Session 1; context thin. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-029 | party-rearch | **Jenny / Ginny** — name-only mention in Session 1; GraphQL-endpoint context. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-030 | party-rearch | **Andrew Tennant** — name-only mention in Session 1; acting head for MRS team, on leave. | [[sources/20260422-meeting-transcript-session-1]] | [[rory-beattie]] | open |
| OQ-033 | party-rearch | **Anna** — first-name-only reference in 2026-05-13 follow-up; UX / product lead working on an alternative party UX. [[rory-beattie]] to speak to Anna ahead of cutover to warm users to the UX change. Team and surname TBD. | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | [[rory-beattie]] | open |
| OQ-034 | party-rearch | **Marty** — first-name-only reference in 2026-05-13 follow-up; sanctions / [[inrisk]] / Boomi-adjacent figure referenced by [[joe-worsfold]] (_"Marty has so much information about in-risk stuff"_). Likely a stakeholder in the [[sanctions-processing]] context. Team and surname TBD. | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] | [[rory-beattie]] | open |
### ADR candidates (deferred)

| ID | Project | Question | Raised in | Owner | Status |
|---|---|---|---|---|---|
| _none_ | — | _All Session 1 ADR candidates have been either promoted ([[no-pct-audit-backfill]], [[feature-tagging-moves-to-inrisk]], [[bulk-migrations-owned-by-mdm-phase-1]]) or kept as annotations ([[pct-and-mdm-go-live-together]] single-flag rollout; Eclipse retirement tracked as OQ-001)._ | — | — | — |

---

## Resolved

When a question is answered, move it here with the answer, resolution date, and resolving source. Keep the original ID.

### From pre-register triage (2026-04-22)

Historical items — resolved before the register formally existed, captured here for completeness.

| ID | Project | Question | Answer | Resolved | Source |
|---|---|---|---|---|---|
| OQ-R01 | party-rearch | HV production date (1 Jan speculation vs. 1 Sep) | **1 September** — 1 Jan speculation was a Pass A inference that the user rejected; there is only the 1 Sep deadline. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R02 | party-rearch | Sergiu's team affiliation | [[sergiu-postolachi]] is **Scrum Master on [[graph-team]]**, not [[tech-tooling]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R03 | party-rearch | "Data Universe Team" canonical name | Formally **[[analytics-team]]**; "Data Universe" is the Snowflake platform they own. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R04 | party-rearch | "Anton" — team and role | [[antonie-labuschagne]], **Tech Lead on [[devx-team]]** (not an InRisk PO). | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R05 | party-rearch | "Hugh" — role and team | [[hugh-lobban]], **Head of Data Quality on [[data-quality-team]]**. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R06 | party-rearch | John + Chris team attribution | [[john-trahearn]] and [[kris-mokrzycki]] (corrected spelling from phonetic "Chris") are **Tech Leads on [[prebind-team]]**, not DevX. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R07 | party-rearch | Andrea's team | [[andrea-read]], **Head of Technology Engineering on [[tech-tooling]]**. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R08 | party-rearch | HV owning team | [[high-volume-team]], main Party-integration contact [[simon-hulbert]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-2]] |
| OQ-R09 | party-rearch | ELA / FAS acronym expansions | ELA = Expense and Loss Adjustment; FAS = Free Alongside Ship. | 2026-04-22 | user correction |
| OQ-R10 | party-rearch | Session 1 Speaker 6 | [[billy-calladine]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R11 | party-rearch | Session 1 Speaker 7 | [[joe-worsfold]]. Transcription engine assigned a second speaker ID to Joe within the same session. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R12 | party-rearch | "Tom Ash / Tomas" identity | **Tomas Sivo**, Business Analyst on [[graph-team]]. Transcription of "Tomas" drifted to "Tom Ash". See [[tomas-sivo]]. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R13 | party-rearch | "Darius" identity | **Not a person** — mispronunciation of "InRisk" by the transcription engine. Remove from people references. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-R14 | party-rearch | "Boyle" identity and the afternoon external-provider session | **Not a person** — mispronunciation of "Will" ([[will-bone]]) in reference to him joining the afternoon session ([[sources/20260422-meeting-transcript-session-2]]). No third recording exists. Remove from people references. | 2026-04-22 | user correction, Pass B of [[sources/20260422-meeting-transcript-session-1]] |
| OQ-010 | party-rearch | Knowledge Graph owner and future | **Not a separate application** — the "Knowledge Graph" is a **Neo4j instance internal to [[party-application]]**, acting as the primary datastore in the current state. It is replaced by DynamoDB + OpenSearch in the target state. Removed from the data-distribution bucket on [[party-rearch-dependency-map]] and [[contract-buckets]]. | 2026-04-22 | user correction, lint pass |
| OQ-031 | party-rearch | PreBind Product Owner identity | **Daria Romanovskaia** — Product Owner on [[prebind-team]]. See [[daria-romanovskaia]]. | 2026-04-22 | user correction, lint pass |
| OQ-005 | party-rearch | Chakra V2 vs V3 widget strategy — V2, minimal-render, or V3-only-when-InRisk-upgrades? Settled by an InRisk-in-the-room workshop. | **Two component libraries from [[joe-worsfold]]**: a Chakra-3-with-design-system widget for [[party-curation-tool]] / [[dataops-team]], and a **design-system-agnostic component library** for [[inrisk]] matched to InRisk's current look. InRisk does **not** absorb Chakra 3 + design-system migration under the September gate. [[high-volume]] is unaffected (API-only). Joe explicitly accepted maintaining the two libraries in parallel as the easier of the available trade-offs. Captured in [[inrisk-cuts-over-before-high-volume]]. | 2026-05-13 | [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] |

---

## Conventions

1. **New questions** get appended to the appropriate **Open** table with the next unused `OQ-NNN`.
2. **Resolution** moves the row verbatim to the **Resolved** table, plus an `Answer`, `Resolved` date, and `Source` column; the original **Open** row is deleted.
3. **Reopening** is allowed (re-surfacing evidence contradicts the prior answer) — create a new ID `OQ-NNN-R` and cross-link.
4. **Referencing from wiki pages**: `see [[open-questions#OQ-014]]`. Pages shouldn't duplicate the full question text — link and summarise.
5. **No silent resolutions.** A question moves only when there's a named source for the answer (or an explicit user confirmation).

## Related
- [[party-rearch-ownership-matrix]] — open **actions** (things to do); complements this register of open **questions** (things to find out).
- [[party-rearch-dependency-map]] — many questions here become edges in the map once resolved.
- [[party-rearch]] — the project-level "What I'd want to know next" section summarises the highest-leverage open questions for party-rearch.
- [[overview]] — portfolio-level overview (cross-project).

## Source history
- 2026-04-22 — Session 1 Pass B: register created; seeded with 31 open questions (Session 1 + Session 2 carryover) and 14 resolved entries from the pre-register triage.
- 2026-04-22 — lint pass: **OQ-010** (Knowledge Graph) resolved — reclassified as internal Neo4j datastore inside [[party-application]]. **OQ-031** (PreBind PO) resolved — [[daria-romanovskaia]]. Open-question count now 29; resolved count now 16.
- 2026-05-18 — [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] ingest: **OQ-005** (Chakra V2/V3) resolved → two component libraries from Joe; closed by [[inrisk-cuts-over-before-high-volume]]. **OQ-008** (end-state sanctions flow) sharpened with the "wrong place" framing and pairing to the new OQ-032 location question. **OQ-019** (feature-tagging migration timing) advanced with Suzy's static-list investigation and the InRisk-classifications-style target shape. **5 new OQs added**: OQ-032 (sanctions-domain location), OQ-035 (concrete InRisk MDM-cutover date), OQ-036 (widget-response field alignment), OQ-033 (Anna identification), OQ-034 (Marty identification). Open count: 29 − 1 (OQ-005 resolved) + 5 (new) = **33 open**; resolved count now **17**.
