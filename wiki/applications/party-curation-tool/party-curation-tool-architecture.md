---
type: application-architecture
title: Party Curation Tool — Current-state Architecture
created: 2026-04-22
updated: 2026-04-22
tags: [architecture, current-state]
application: party-curation-tool
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: stub
---

# Party Curation Tool — Current-state Architecture

_Living reference for the **as-is** technical shape of [[party-curation-tool]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** Scaffolding in place, ready to be populated from forthcoming raw-folder ingests. Everything currently on this page is either (a) a confirmed detail from Sessions 1 & 2 or (b) a placeholder awaiting authoritative source material.

## Summary
_Populated at first architecture ingest. From transcripts so far: PCT is a **Next.js frontend sitting on top of a Jira workflow**. Data curation operations (spelling fixes, D&B mapping, party merges, version updates) are driven through Jira tickets; audit history accrues in Jira and in [[data-universe]]. Under [[party-rearch-phase-1]] the Jira-backed workflow is retired live and PCT is re-backed onto the new MDM ([[pct-and-mdm-go-live-together]])._

## Technologies & services
_Populate from raw ingest. Known so far:_

- **Frontend**: **Next.js** — confirmed in transcripts.
- **Workflow / ticketing backend**: **Jira** — the curation workflow is Jira-driven today; retired live in Phase 1.
- **Search widget**: UI component with a current/future Chakra-UI-version tension (V2 vs V3) — see [[open-questions#OQ-005]].
- **Primary datastore**: reads from / writes to [[party-application]] — the PCT does not have its own authoritative datastore for party data; Jira is the workflow store, not the party store.
- **Audit / history**: Jira audit trail (for workflow events) + [[data-universe]] (Snowflake) for long-horizon analytical queries. Not folded into PCT's own model — see [[no-pct-audit-backfill]] for the explicit boundary.
- **Compute / hosting**: _unknown — pending ingest_
- **Auth**: _unknown — pending ingest_

## Touchpoints

### Inbound API / reads
- User browser sessions — Next.js app; auth mechanism TBC
- _Any programmatic integrations unknown — pending ingest_

### Outbound API / writes
- `→ Jira` — reads and writes curation workflow tickets (current-state; retires in Phase 1)
- `→ [[party-application]]` — reads / writes party data for curation operations; shape / endpoints TBC
- `→ Dun & Bradstreet` lookup — provenance-stamped mappings; see [[d-and-b-caching-and-auto-parent]]
- _Others — pending ingest_

### Event streams

#### Produced
- _unknown — pending ingest (Jira workflow events may or may not be exposed as event streams; likely internal)_

#### Consumed
- _unknown — pending ingest_

### Batch / scheduled integrations
- D&B scheduled refresh loop — refreshed D&B records trigger curation revisions on affected parties; cadence open ([[open-questions#OQ-011]])
- _Others — pending ingest_

## Internal structure
_Populate from raw ingest. The **search widget** is singled out in transcripts as a high-salience component (co-ships with the MDM cutover; Chakra-version tension) — treat it as its own module when populating._

## Observability & SLOs
_Populate from raw ingest._

## Known constraints / rough edges
_From transcripts:_

- **Jira workflow is load-bearing** — curation operations and their audit trail currently require Jira. Phase 1 retires this live (no dual-running) rather than gradually.
- **No audit backfill** — the new PCT will not carry Jira history across the cutover; Snowflake becomes the cross-period store. This is a deliberate scope call, not a debt. See [[no-pct-audit-backfill]].
- **Search-widget framework lock-in** — Chakra V2 / V3 strategy is open; settlement requires an InRisk-in-the-room conversation. Not a Phase-1 blocker but architectural in nature. See [[open-questions#OQ-005]].
- **Coupled to [[party-application]]'s data model** — PCT is a thin layer above party data; a shift in party data model (as is happening in [[party-rearch]]) forces coordinated change on PCT. Operationally managed via the single-flag coupled rollout ([[pct-and-mdm-go-live-together]]).

## Relationships to other architecture pages
- [[party-application-architecture]] — PCT is the curation frontend; the two move together in Phase 1
- [[inrisk-architecture]] — PCT is not a direct integration point for InRisk today, but PCT-driven curation changes propagate downstream via [[party-application]]'s payload events

## Phase-target pages that delta from this one
_Populate as targets are created._

- _(none yet — [[party-rearch-phase-1-party-curation-tool-architecture]] to be created when Phase-1 target detail is available)_

## Superseded claims
_None yet — this page has not yet been through a phase-completion fold-in._

## Related
- [[party-curation-tool]] — identity page (owner, primary user, role)
- [[party-rearch-dependency-map]] — cross-app view of touchpoints
- [[pct-and-mdm-go-live-together]] — the single-flag coupled rollout that governs PCT's Phase-1 transition
- [[no-pct-audit-backfill]] — the audit-history boundary
- [[d-and-b-caching-and-auto-parent]] — D&B integration decisions that PCT participates in

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — audit boundary and bulk-migration CLI scope
- [[sources/20260422-meeting-transcript-session-2]] — Next.js-on-Jira current shape, single-flag rollout, Chakra widget tension
