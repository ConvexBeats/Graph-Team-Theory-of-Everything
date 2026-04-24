---
type: analysis
title: party-rearch Phase 1 — Summary & Applications-Affected
created: 2026-04-22
updated: 2026-04-22
tags: [analysis, summary, phase-1]
project: party-rearch
phase: [phase-1]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
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
**MDM + PCT co-ship to the 1 September [[high-volume]] go-live.** One phase, one gate, one feature-flag. The new MDM (DynamoDB + OpenSearch) and the new [[party-curation-tool]] ship together under a **single feature-flag coupled rollout** ([[pct-and-mdm-go-live-together]]); the legacy Neo4j Knowledge Graph is kept alive behind a proxy-event strangler during cutover ([[strangle-the-graph-via-proxy-events]]) so [[data-universe]] sees no schema change; historical migrations are deliberately bounded — MDM carries party-version history from cutover forward, with **no pre-cutover InRisk client snapshots** ([[no-historic-client-backfill-into-mdm]]) and **no Jira PCT audit history** ([[no-pct-audit-backfill]]) brought across.

**Gate**: **2026-09-01**. [[high-volume]] production requires MDM as its Party source-of-truth; everything in this phase is scoped against that date.

### Applications affected in Phase 1

| Application | Change intensity | Shape of change |
|---|---|---|
| [[party-application]] | **high** — core subject | Neo4j Knowledge Graph retires as primary datastore; DynamoDB + OpenSearch stood up; proxy-event strangler in place; bulk-migration CLI delivered by [[graph-team]] ([[bulk-migrations-owned-by-mdm-phase-1]]) |
| [[party-curation-tool]] | **high** — co-ships | Next.js UI on the new MDM; single-flag coupled rollout with the search widget; Jira workflow retired live (no dual-running); no audit backfill |
| [[inrisk]] | **medium** — interim changes | Party-ID reference table; Party-ID stamped on outgoing messaging; broker retrieval moved direct from InRisk (URD path removed) |
| [[inrisk-engine]] | **none** — out of scope | Explicitly scoped out of Phase 1: targets the **final-state** Party contract, not the interim. Phase-1 work should not design for it. |
| [[high-volume]] | **integration only** | Consumes MDM via the new versioned-reference API; integration shape still being specified ([[open-questions#OQ-013]]) |
| [[eclipse]] | **scope call** | Ongoing ingestion into [[party-application]] under review ([[open-questions#OQ-001]]) |

Every application currently tracked for the project is listed. "`none` with a reason" ([[inrisk-engine]]) is an explicit scope call, not an omission.

### Phase-1 decisions (all accepted, all `project: party-rearch · phase: [phase-1]`)

- [[strangle-the-graph-via-proxy-events]] — MDM emits Graph-shape proxy events; no dual-running.
- [[pct-and-mdm-go-live-together]] — new PCT + MDM ship together under one feature flag.
- [[no-historic-client-backfill-into-mdm]] — MDM history starts at cutover.
- [[no-pct-audit-backfill]] — Snowflake remains the cross-period audit store.
- [[bulk-migrations-owned-by-mdm-phase-1]] — [[graph-team]]-owned CLI (not self-serve in Phase 1).
- [[d-and-b-caching-and-auto-parent]] — D&B cached in Dynamo with TTL; scheduled refresh → curation revision loop.
- [[uuid-system-id-with-display-id]] — system ID = UUID, display ID = legacy Graph party ID.

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

## Confidence
**High on structure**, because this digest is a re-assembly of already-filed, already-reviewed content from Sessions 1 and 2 with user-confirmed corrections through Pass B and the lint pass. **Medium-to-low on some specifics** — notably the **HV integration shape** (OQ-013) and the **MDM squad shape** (OQ-017); these are still open questions and anything in this summary about them is provisional.

## Follow-ups
- When [[open-questions#OQ-013]] is resolved, update this summary's [[high-volume]] row.
- When [[open-questions#OQ-002]] is resolved, update the [[data-universe]] / [[inrisk]] proxy-event framing on this page and on [[party-rearch-phase-1]] in lockstep.
- Revisit **Confidence** after the Graph-API consumer audit ([[open-questions#OQ-004]]) lands — that spike may materially change the [[party-application]] row by re-scoping decommissioning.

## Related
- [[party-rearch-phase-1]] — canonical Phase-1 page (full scope table, done-state, predecessors/successors).
- [[party-rearch]] — project overview / thesis / pillars.
- [[party-rearch-phase-2]] — successor phase (picks up deferred items).
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] · [[open-questions]]
- [[contract-buckets]] · [[intercept-backfill-projection]] · [[roles-vs-views-litmus-test]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — Phase-1 scope calls (no-backfill; feature-tagging deferred; bulk-migration CLI; MDM-squad-shape tension).
- [[sources/20260422-meeting-transcript-session-2]] — strangler pattern, PCT co-delivery, InRisk interim changes, D&B and UUID decisions.
