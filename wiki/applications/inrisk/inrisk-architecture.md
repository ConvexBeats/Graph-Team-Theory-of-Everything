---
type: application-architecture
title: InRisk — Current-state Architecture
created: 2026-04-22
updated: 2026-04-22
tags: [architecture, current-state]
application: inrisk
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: stub
---

# InRisk — Current-state Architecture

_Living reference for the **as-is** technical shape of [[inrisk]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** Minimal scaffolding; populated from forthcoming raw-folder ingests. InRisk is a **downstream consumer** of [[party-application]] in the [[party-rearch]] project — the page below focuses primarily on InRisk's integration surface with the party domain; general InRisk internals can be layered in later as needed.

## Summary
_Populated at first architecture ingest. From transcripts so far: InRisk (IR2) is the Policy Admin System for pre-bind policy information. In the party context, InRisk today **caches party payloads** received from [[party-application]] and resolves locally; under [[party-rearch-phase-1]] it moves to an interim state that still consumes payloads but with the **client-ID / submission / requirement nesting problem** addressed. The **final** party-side state is realised when [[inrisk-engine]] goes live (not Phase 1)._

## Technologies & services
_Populate from raw ingest. Known so far:_

- **Datastore**: _unknown — pending ingest_
- **Feature tagging**: currently lives elsewhere (originally mapped into the party domain); **moves to InRisk in Phase 2+** per [[feature-tagging-moves-to-inrisk]].
- **Integration shape with [[high-volume]]**: open — see [[open-questions#OQ-013]].
- **Other**: _unknown — pending ingest_

## Touchpoints

### Inbound API / reads
- _unknown — pending ingest_

### Outbound API / writes
- _unknown — pending ingest_

### Event streams

#### Consumed
- Party-change events ← [[party-application]] — payload-bearing today; snapshotted locally. This is the contract the [[party-rearch]] project is changing.

#### Produced
- _unknown — pending ingest_

### Batch / scheduled integrations
- Ingestion from [[eclipse]] — third-party inflow; InRisk is the application that uses Eclipse. Shape TBC.

## Internal structure
_Populate from raw ingest. Known architectural pressure point: **client-ID / submission / requirement nesting** — current data model nests these in a way that is the architectural-choice pressure point the PreBind team is working through (distinct from a regulatory constraint)._

## Observability & SLOs
_Populate from raw ingest._

## Known constraints / rough edges
- **Interim-state carrier** — InRisk must support a transitional party-payload shape for the life of Phase 1 while the final-state [[inrisk-engine]] is still being built. This is explicit scope, not debt.
- **Feature tagging ownership drift** — in Phase 1, feature tagging sits awkwardly between party and InRisk domains; Phase 2+ formalises the move to InRisk.

## Relationships to other architecture pages
- [[party-application-architecture]] — upstream source of party data; contract changes drive InRisk-side work
- [[high-volume-architecture]] — sibling downstream consumer; integration shape between the two is open ([[open-questions#OQ-013]])

## Phase-target pages that delta from this one
- _(none yet — [[party-rearch-phase-1-inrisk-architecture]] to be created if/when InRisk Phase-1-delta is authored)_

## Superseded claims
_None yet._

## Related
- [[inrisk]] — identity page
- [[inrisk-engine]] — the final-state rewrite (not yet in production; its architecture is tracked separately in [[inrisk-engine-architecture]] when created)
- [[feature-tagging-moves-to-inrisk]] — Phase-2+ ownership shift
- [[eclipse]] — upstream third-party data source
- [[party-rearch-dependency-map]] — cross-app view

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — client-ID / submission / requirement framing, interim-state carrier scope
- [[sources/20260422-meeting-transcript-session-2]] — feature-tagging ownership, payload-contract pressure
