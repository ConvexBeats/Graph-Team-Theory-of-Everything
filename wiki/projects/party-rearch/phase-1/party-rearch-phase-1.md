---
type: phase
title: party-rearch — Phase 1 (MDM + PCT co-ship, 1 Sep HV gate)
created: 2026-04-22
updated: 2026-04-22
tags: [phase, in-flight]
project: party-rearch
phase_id: phase-1
status: in-flight
gate_date: 2026-09-01
---

# Phase 1 — MDM + PCT co-ship to 1 September HV go-live

## Summary
First phase of the [[party-rearch]] project. Ships the new MDM (DynamoDB + OpenSearch) and the new [[party-curation-tool]] together, under a **single feature-flag coupled rollout**, in time for [[high-volume]] going to production on 1 September. Legacy Graph DB is kept alive behind a proxy-event strangler during the cutover; [[data-universe]] sees no schema change. Historical migrations are deliberately bounded: MDM carries party-version history from cutover forward, no pre-cutover InRisk client snapshots and no Jira PCT audit history are backfilled.

## Gate
**2026-09-01** — [[high-volume]] in production requires MDM as its Party source-of-truth. Everything in this phase is scoped against that date.

## Scope — applications

| Application | Change intensity | Shape of change |
|---|---|---|
| [[party-application]] | **high** — core subject | Neo4j Knowledge Graph retires as primary datastore; DynamoDB + OpenSearch stood up; proxy-event strangler in place; bulk-migration CLI delivered by [[graph-team]] |
| [[party-curation-tool]] | **high** — co-ships | Next.js UI on the new MDM; single-flag coupled rollout with the search widget; Jira workflow retired live (no dual-running); no audit backfill |
| [[inrisk]] | **medium** — interim changes | Party-ID reference table; Party-ID on outgoing messaging; broker retrieval moved direct from InRisk (URD path removed) |
| [[inrisk-engine]] | **none** — out of scope | Explicitly scoped out: targets final-state Party contract, not the interim. Phase-1 work should not design for [[inrisk-engine]]. |
| [[high-volume]] | **integration only** | HV consumes MDM via the new versioned-reference API; integration shape still being specified ([[open-questions#OQ-013]]) |
| [[eclipse]] | **scope call** | Ongoing ingestion into [[party-application]] under review ([[open-questions#OQ-001]]) — confirmed dropped pending the Graph-API audit spike |

### Architecture pages per application

Per-application architecture is tracked on dedicated pages. Current-state pages live in `wiki/applications/`; phase-end target pages live in this project folder and delta from the current-state baseline.

| Application | Current state | Phase-1 target |
|---|---|---|
| [[party-application]] | [[party-application-architecture]] | `[[party-rearch-phase-1-party-application-architecture]]` _(to be created)_ |
| [[party-curation-tool]] | [[party-curation-tool-architecture]] | `[[party-rearch-phase-1-party-curation-tool-architecture]]` _(to be created)_ |
| [[inrisk]] | [[inrisk-architecture]] | `[[party-rearch-phase-1-inrisk-architecture]]` _(to be created if change-intensity justifies)_ |
| [[high-volume]] | [[high-volume-architecture]] | `[[party-rearch-phase-1-high-volume-architecture]]` _(to be created)_ |
| [[inrisk-engine]] | _not tracked (out of scope for Phase 1)_ | — |
| [[eclipse]] | _not tracked_ | — |

When Phase 1 ships, each target page gets folded into its current-state baseline per AGENTS.md §5.4.

## Decisions in this phase
All carry `project: party-rearch` · `phase: [phase-1]`:

- [[strangle-the-graph-via-proxy-events]] — MDM emits Graph-shape proxy events; no dual-running
- [[pct-and-mdm-go-live-together]] — new PCT and MDM ship together under a single feature flag
- [[no-historic-client-backfill-into-mdm]] — MDM history starts at cutover
- [[no-pct-audit-backfill]] — new PCT does not carry Jira audit history; Snowflake is the cross-period audit store
- [[bulk-migrations-owned-by-mdm-phase-1]] — [[graph-team]]-owned CLI in Phase 1
- [[d-and-b-caching-and-auto-parent]] — D&B cached in Dynamo with TTL; scheduled refresh → curation revision loop
- [[uuid-system-id-with-display-id]] — system ID = UUID, display ID = legacy Graph party ID

## Delivery shape
The Phase-1 delivery trio is [[intercept-backfill-projection]]:
1. **Intercept** — writes divert to MDM from a defined cut-in point
2. **Backfill** — 100% of historical Knowledge Graph state replayed into MDM (display IDs preserved)
3. **Projection** — MDM emits in the existing Graph event shape via a proxy adapter (bidirectional round-trip must hold)

All three must be verified before the single-feature-flag cutover.

## Open questions gating this phase
See [[open-questions]] (filter by `project: party-rearch`, `phase: phase-1`). Highest-leverage:

- [[open-questions#OQ-002]] — Data Universe tolerance for flattened `(party-id, submission-id)` schema. **Single biggest Phase-1 branch point.**
- [[open-questions#OQ-004]] — Graph API consumer audit spike.
- [[open-questions#OQ-013]] — HV integration-shape specifics.
- [[open-questions#OQ-017]] — MDM delivery squad shape. Blocks sprint-planning.
- [[open-questions#OQ-014]] — DataOps change-management lead time.

## Predecessors / successors
- **No predecessor** — first phase.
- **Successor**: [[party-rearch-phase-2]]. Phase 2 picks up the items explicitly deferred here (feature-tagging handover to [[inrisk]]; direct Dynamo→DU stream replacing proxy events; full self-serve bulk-migration UX; end-state sanctions flow; [[inrisk-engine]] buy-in on the final-state Party contract).

## Done-state
- MDM serving HV traffic in production on or before 2026-09-01.
- Legacy Knowledge Graph decommissioned (or on a clear decommission-only schedule post-cutover).
- New PCT live; old PCT switched off at cutover; Jira retained read-only.
- No schema change required on [[data-universe]]'s side.
- Phase-1 decisions above all shipped or formally deferred.

## Related
- [[party-rearch]] — project overview
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]
- [[contract-buckets]] · [[intercept-backfill-projection]] · [[roles-vs-views-litmus-test]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — Phase-1 scope calls (no-backfill; feature-tagging deferred; bulk-migration CLI)
- [[sources/20260422-meeting-transcript-session-2]] — strangler pattern, PCT co-delivery, InRisk interim changes, D&B and UUID decisions
