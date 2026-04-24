---
type: phase
title: party-rearch — Phase 2 (post-cutover; end-state)
created: 2026-04-22
updated: 2026-04-22
tags: [phase, planned, stub]
project: party-rearch
phase_id: phase-2
status: planned
gate_date: TBD
---

# Phase 2 — post-cutover end-state

## Summary
Picks up the items explicitly deferred from [[party-rearch-phase-1]]. Phase 2 is the move from the Phase-1 interim end-state (Graph-shape proxy events, feature-tagging still on [[graph-team]], CLI-only bulk migration, sanctions flow unchanged, [[inrisk-engine]] waiting on the final contract) to the programme's stated end-state.

Phase boundaries and timeline are not yet defined — this page is a stub. Expected to crystallise after 1-Sep cutover.

## Provisional scope

| Application | Change intensity | Shape of change |
|---|---|---|
| [[party-application]] | **medium** | Direct Dynamo → [[data-universe]] stream replacing proxy events (Snowpipe-style); S&P API integration replacing manual DU bulk path |
| [[party-curation-tool]] | **medium** | Full self-serve bulk-migration UX (CSV upload, diff preview) migrating away from the Phase-1 CLI |
| [[inrisk]] | **medium** | Feature-tagging ownership (Postgres table + widget) migrates to [[prebind-team]] per [[feature-tagging-moves-to-inrisk]] |
| [[inrisk-engine]] | **high** | [[devx-team]] consumes the final-state Party contract; gates on [[open-questions#OQ-018]] being spec'd |
| [[data-universe]] | **medium** | Accepts direct Dynamo stream; possibly rewrites to remove client-ID-based joins |

## Decisions bound to this phase
All carry `project: party-rearch` · `phase: [phase-2]`:

- [[feature-tagging-moves-to-inrisk]] — formally Phase-2+; triggers and concrete date still open ([[open-questions#OQ-019]])
- Direct Dynamo → DU stream (not yet an ADR)
- End-state sanctions flow — revision-event-triggered, stamped on party version ([[open-questions#OQ-008]])
- Final-state Party contract specification ([[open-questions#OQ-018]])

## Open questions gating this phase
- [[open-questions#OQ-018]] — final-state Party contract specification (gates [[inrisk-engine]])
- [[open-questions#OQ-019]] — feature-tagging migration timing
- [[open-questions#OQ-008]] — end-state sanctions flow owner / timing
- [[open-questions#OQ-006]] — Phase-2 bulk-migration UX shape
- [[open-questions#OQ-007]] — version-change event semantics (push vs pull)

## Predecessors / successors
- **Predecessor**: [[party-rearch-phase-1]] must complete before any Phase-2 work is unblocked.
- **Successor**: TBD — likely end of the party-rearch project unless a third phase emerges.

## Done-state
TBD. To be defined once Phase 1 is clearly in-flight and Phase-2 trigger conditions are settled.

## Related
- [[party-rearch]] · [[party-rearch-phase-1]] · [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]

## Sources
- _Seeded from Phase-1 deferred items across [[sources/20260422-meeting-transcript-session-1]] and [[sources/20260422-meeting-transcript-session-2]]._
