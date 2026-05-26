---
type: analysis
title: party-rearch Phase 1 — Summary & Applications-Affected
created: 2026-04-22
updated: 2026-05-26
tags: [analysis, summary, phase-1]
project: party-rearch
phase: [phase-1]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement]
source_count: 4
status: draft
---

# party-rearch Phase 1 — Summary & Applications-Affected

_Filed from a query on 2026-04-22: "summary of Phase 1 of the MDM party rearchitecture project and which applications are affected." Focused digest; the fully-detailed phase page is [[party-rearch-phase-1]]._

## Question asked
> Could you give me a summary of Phase 1 of the MDM party rearchitecture project and which applications are affected?

## Method
Synthesised from the [[party-rearch]] project overview, the [[party-rearch-phase-1]] phase page, the eight Phase-1 ADRs listed under **Decisions** below, and the two source transcripts ([[sources/20260422-meeting-transcript-session-1]], [[sources/20260422-meeting-transcript-session-2]]). No new evidence — a digest of existing wiki content with an applications-first framing.

## Findings

### What Phase 1 is
**MDM + PCT co-ship, then [[inrisk]] cuts over to MDM, then [[high-volume]] goes live at the 1 September gate.** One phase, one outer gate, with an inner sequencing constraint added by [[inrisk-cuts-over-before-high-volume]] (2026-05-13): InRisk's MDM integration lands **≥ 2 weeks before 1 Sep**, so HV consumes MDM only after real production traffic has exercised the new contract. The new MDM (DynamoDB + OpenSearch) and the new [[party-curation-tool]] ship together under a **single feature-flag coupled rollout** ([[pct-and-mdm-go-live-together]]); the legacy Neo4j Knowledge Graph is kept alive behind a proxy-event strangler during cutover ([[strangle-the-graph-via-proxy-events]] — refined 2026-05-13 with a brief dual-write to the old graph for revertability during the cutover window) so [[data-universe]] sees no schema change; historical migrations are deliberately bounded — MDM carries party-version history from cutover forward, with **no pre-cutover InRisk client snapshots** ([[no-historic-client-backfill-into-mdm]]) and **no Jira PCT audit history** ([[no-pct-audit-backfill]]) brought across.

**Outer gate**: **2026-09-01**. [[high-volume]] production requires MDM as its Party source-of-truth.
**Inner gate (new 2026-05-13)**: InRisk's MDM-integration cutover, **≥ 2 weeks before 1 Sep**, concrete date TBC at [[open-questions#OQ-035]]. Slipping this compresses HV's buffer.

### Applications affected in Phase 1

| Application | Change intensity | Shape of change |
|---|---|---|
| [[party-application]] | **high** — core subject | Neo4j Knowledge Graph retires as primary datastore; DynamoDB + OpenSearch stood up on existing 3-account / 4-env AWS estate; proxy-event strangler in place; bulk-migration CLI delivered by [[graph-team]] ([[bulk-migrations-owned-by-mdm-phase-1]]); **two widget component libraries** published — Chakra-3-with-design-system for [[party-curation-tool]] and design-system-agnostic for [[inrisk]] ([[inrisk-cuts-over-before-high-volume]]) |
| [[party-curation-tool]] | **high** — co-ships | Next.js UI on the new MDM; single-flag coupled rollout with the search widget; Jira workflow retired live (no dual-running); no audit backfill; consumes the Chakra-3 + design-system widget |
| [[inrisk]] | **high** _(raised from "medium" 2026-05-13; story-ordered + gated 2026-05-14)_ — Party MDM Integration epic | Concrete epic of 5 stories; **Stories 1 & 2 ready for low level (2026-05-19); Stories 3/4/5 gated on widget-integration spike**: (1) remove manual client-creation on new requirement → widget — foundational "demon" cleared; (2) data-model migration — `party_id` (UUID v7) + `version_id` (int) added to **party + broker + party_snapshot** tables (three tables, not two — third confirmed 2026-05-14); (3/4/5) client / broker / party-tagging widget integration, feature-flagged. **Party tagging in scope; feature tagging out** (old backend stays alive past cutover). SDK-style widget integration via Joe's design-system-agnostic library, on **parity-not-enhancement** posture (drop-in replacement now, enhancement later). InRisk's cutover lands ≥ 2 weeks before HV. |
| [[inrisk-engine]] | **none** — out of scope | Explicitly scoped out of Phase 1: targets the **final-state** Party contract, not the interim. Phase-1 work should not design for it. |
| [[high-volume]] | **integration only** | Consumes MDM via the new versioned-reference API at the 1-Sep gate, **after** the InRisk-first cutover window. API-only via Boomi; no widget. Integration shape still being specified ([[open-questions#OQ-013]]) |
| [[eclipse]] | **scope call** | Ongoing ingestion into [[party-application]] under review ([[open-questions#OQ-001]]) |

Every application currently tracked for the project is listed. "`none` with a reason" ([[inrisk-engine]]) is an explicit scope call, not an omission.

**Flagged but out of Phase 1**: sanctions / [[ntt]] / Boomi orchestration. By group consensus the current Boomi-hosted orchestration is in the wrong place; audit pressure is significant this year; not a Phase-1 deliverable. See [[sanctions-processing]] · [[open-questions#OQ-032]].

### Phase-1 decisions (all accepted, all `project: party-rearch · phase: [phase-1]`)

- [[strangle-the-graph-via-proxy-events]] — MDM emits Graph-shape proxy events; sources-of-truth stay single (refined 2026-05-13: brief dual-write to old graph during cutover window for revertability, not a contradiction).
- [[pct-and-mdm-go-live-together]] — new PCT + MDM ship together under one feature flag.
- [[inrisk-cuts-over-before-high-volume]] **(new 2026-05-13)** — InRisk's MDM cutover lands ≥ 2 weeks before 1 Sep; HV switches on at the gate. Rolls in the design-system call: Joe maintains two component libraries (Chakra-3 + design-system for PCT; design-system-agnostic for InRisk). Resolves [[open-questions#OQ-005]].
- [[no-historic-client-backfill-into-mdm]] — MDM history starts at cutover.
- [[no-pct-audit-backfill]] — Snowflake remains the cross-period audit store.
- [[bulk-migrations-owned-by-mdm-phase-1]] — [[graph-team]]-owned CLI (not self-serve in Phase 1).
- [[d-and-b-caching-and-auto-parent]] — D&B cached in Dynamo with TTL; scheduled refresh → curation revision loop.
- [[uuid-system-id-with-display-id]] — system ID = UUID, display ID = legacy Graph party ID.

**Refined for Phase 1 (Phase-2-target ADR, scope clarification)**: [[feature-tagging-moves-to-inrisk]] — direction unchanged (Phase-2+ migration to InRisk); 2026-05-13 added the party-tagging-vs-feature-tagging boundary, the old-backend-stays-alive-past-cutover carve-out, and the static-list-hypothesis investigation ([[open-questions#OQ-019]]).

### Delivery shape
[[intercept-backfill-projection]] — the three-step delivery pattern that implements [[strangle-the-graph-via-proxy-events]]:
1. **Intercept** — writes divert to MDM from a defined cut-in point.
2. **Backfill** — 100% of historical Knowledge Graph state replayed into MDM (display IDs preserved).
3. **Projection** — MDM emits in the existing Graph event shape via a proxy adapter (bidirectional round-trip must hold).

All three must be verified before the single-feature-flag cutover.

### Highest-leverage Phase-1 open questions
Drawn from [[open-questions]] (filter `project: party-rearch`, `phase: phase-1`):

- [[open-questions#OQ-002]] — Data Universe tolerance for flattened `(party-id, submission-id)` shape. **Single biggest branch point.**
- [[open-questions#OQ-004]] — Graph API consumer audit spike.
- [[open-questions#OQ-013]] — HV integration-shape specifics.
- [[open-questions#OQ-017]] — MDM delivery squad shape (blocks sprint-planning).
- [[open-questions#OQ-014]] — DataOps change-management lead time.
- [[open-questions#OQ-035]] **(new 2026-05-13)** — Concrete InRisk MDM-cutover date. Sizes HV's buffer.
- [[open-questions#OQ-036]] **(new 2026-05-13)** — Widget-response field alignment between MDM and InRisk's needs.

## Confidence
**High on structure**, because this digest is a re-assembly of already-filed, already-reviewed content from Sessions 1, 2, and the 2026-05-13 follow-up. **Medium-to-low on some specifics** — notably the **HV integration shape** (OQ-013), the **MDM squad shape** (OQ-017), and the **concrete InRisk cutover date** (OQ-035); these are still open questions and anything in this summary about them is provisional.

## Follow-ups
- When [[open-questions#OQ-013]] is resolved, update this summary's [[high-volume]] row.
- When [[open-questions#OQ-002]] is resolved, update the [[data-universe]] / [[inrisk]] proxy-event framing on this page and on [[party-rearch-phase-1]] in lockstep.
- When [[open-questions#OQ-035]] is resolved (concrete InRisk cutover date set), update both the Outer/Inner gate framing on this page and on [[party-rearch-phase-1]].
- Revisit **Confidence** after the Graph-API consumer audit ([[open-questions#OQ-004]]) lands — that spike may materially change the [[party-application]] row by re-scoping decommissioning.
- ~~After Thursday's high-level (2026-05-14) and Tuesday's low-level (2026-05-19), revisit the InRisk row to reflect any story-shape changes.~~ **Done 2026-05-14**: epic ordered and gated; updated above. Tuesday 2026-05-19 low-level only covers Stories 1 & 2 (Stories 3/4/5 wait on widget-integration spike).
- When the widget-integration spike completes (timing TBC at ad-hoc HL on 2026-05-15), revisit the InRisk row + [[inrisk-cuts-over-before-high-volume]]'s Open Risks section to reflect resolved widget mechanics + response-shape.
- When the **InRisk-side backfill** decision lands (backfill old client / broker / party-snapshot rows with MDM IDs vs null + go-forward) — note that decision on [[inrisk]], [[party-application]], and here. Sanctions impact is the main consideration.

## Related
- [[party-rearch-phase-1]] — canonical Phase-1 page (full scope table, done-state, predecessors/successors).
- [[party-rearch]] — project overview / thesis / pillars.
- [[party-rearch-phase-2]] — successor phase (picks up deferred items).
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] · [[open-questions]]
- [[contract-buckets]] · [[intercept-backfill-projection]] · [[roles-vs-views-litmus-test]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — Phase-1 scope calls (no-backfill; feature-tagging deferred; bulk-migration CLI; MDM-squad-shape tension).
- [[sources/20260422-meeting-transcript-session-2]] — strangler pattern, PCT co-delivery, InRisk interim changes, D&B and UUID decisions.
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — InRisk-first cutover sequence; concrete Party MDM Integration epic (4–5 stories); two component-libraries call; party-tagging vs feature-tagging boundary; sanctions / NTT / Boomi flagged.
- [[sources/20260514-inrisk-high-level-refinement]] — InRisk epic ordered + gated (Stories 1 & 2 ready; 3/4/5 spike-gated); data-model story expanded to three tables (+ party_snapshot); parity-not-enhancement widget posture; TOBA-status filter parity; InRisk-side backfill question raised.
