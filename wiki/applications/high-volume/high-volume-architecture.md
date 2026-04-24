---
type: application-architecture
title: High Volume — Current-state Architecture
created: 2026-04-22
updated: 2026-04-22
tags: [architecture, current-state]
application: high-volume
state: current
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: stub
---

# High Volume — Current-state Architecture

_Living reference for the **as-is** technical shape of [[high-volume]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** High Volume is **in development** — it does not yet have a meaningful "current-state" in production. This page is intentionally thin; it exists to give [[high-volume]]'s forthcoming target architecture ([[party-rearch-phase-1-high-volume-architecture]], when filed) a baseline to delta from. Populate as HV's design stabilises ahead of the **1 September** cutover.

## Summary
_High Volume (HV) is a new application being built to process large volumes of deals at higher pace. Its relevance to [[party-rearch]] is that HV is a **net-new consumer** of party data and must therefore be designed against the **target** party architecture, not the legacy one. HV's go-live on 1 September is the gating milestone that drives [[party-rearch-phase-1]]'s timeline._

## Technologies & services
_In-flight. Populate from raw ingest / HV team documentation._

## Touchpoints

### Inbound API / reads
- From [[party-application]] — HV must resolve party-by-ID against the re-architected MDM, not snapshot payloads. Target contract shape to be documented in [[party-rearch-phase-1-high-volume-architecture]].

### Outbound API / writes
- _pending ingest_

### Event streams
- _pending ingest_

### Batch / scheduled integrations
- _pending ingest_

## Internal structure
_Pending ingest. HV is a greenfield build — the first authoritative architecture to capture here will come from the HV team's own design material._

## Observability & SLOs
_Pending ingest._

## Known constraints / rough edges
- **Must be target-compatible by 1 September** — HV cannot integrate against the legacy payload-event shape; this gates Phase 1 of [[party-rearch]].
- **Integration with [[inrisk]] is open** — see [[open-questions#OQ-013]].
- **Architect-level contact**: Simon Hulbert (see [[simon-hulbert]]).

## Relationships to other architecture pages
- [[party-application-architecture]] — HV is the forcing consumer that motivates the party re-architecture timeline
- [[inrisk-architecture]] — integration shape open

## Phase-target pages that delta from this one
- _(none yet — [[party-rearch-phase-1-high-volume-architecture]] to be created when HV's target detail is authored)_

## Superseded claims
_None — this page has never carried production-state claims to supersede._

## Related
- [[high-volume]] — identity page
- [[party-rearch-phase-1]] — the phase gated by HV's 1-Sep go-live
- [[high-volume-team]] — owning team

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — HV framed as gating consumer for Phase 1
