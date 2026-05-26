---
type: application-architecture
title: High Volume — Current-state Architecture
created: 2026-04-22
updated: 2026-05-26
tags: [architecture, current-state]
application: high-volume
state: current
sources: [20260422-meeting-transcript-session-2, 20260519-party-integration-timelines]
source_count: 2
status: stub
---

# High Volume — Current-state Architecture

_Living reference for the **as-is** technical shape of [[high-volume]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** High Volume is **in development** — there is no production "current state" yet. This page is intentionally thin; it exists to give [[high-volume]]'s forthcoming target architecture ([[party-rearch-phase-1-high-volume-architecture]], when filed) a baseline to delta from. Reframed 2026-05-26 around the underlying [[artificial]] vendor platform — most of HV's technical substance actually lives in Artificial, not in a Convex-built application. Populate the **Convex-side integration surface** here as it stabilises ahead of the **1 September** cutover.

## Summary
**HV is Convex's implementation of the [[artificial]] vendor platform** for high-volume insurance-deal flow. Its relevance to [[party-rearch]] is that HV (via Artificial) is a **net-new Party consumer** that must be designed against the **target** Party architecture, not the legacy one. The Convex-side build is the integration layer between Artificial and the Convex domain (Party, InRisk, sanctions); the integration broker for Party-side traffic is [[boomi]]. HV's go-live on 1 September is the gating milestone that drives [[party-rearch-phase-1]]'s timeline.

## Technologies & services

_HV-as-a-product is largely the [[artificial]] vendor platform's tech stack — out of scope for this wiki._

**Convex-side integration surface** (what HV's owning team actually builds and operates):

- **Integration broker**: [[boomi]] — gateway for all Party ↔ Artificial / HV traffic. Same pattern Convex uses for [[sanctions-processing]] ([[boomi]] → [[inrisk]] → [[ntt]]). Connectivity setup currently in flight ([[srini]] HV-side, paired with [[joe-worsfold]] Party-side).
- **Vendor platform**: [[artificial]] — third-party SaaS-style platform; internals not in scope for this wiki.
- **Convex-side glue / configuration / domain mappings**: TBD — pending HV team's own architecture material. _Action #36 on [[party-rearch-ownership-matrix]]: [[simon-hulbert]] to send top-level + sequence architecture diagrams; raw-folder candidate when received._
- _Other compute / infrastructure: pending ingest._

## Touchpoints

### Inbound API / reads
- **From [[party-application]] (Phase 1 onward, via [[boomi]])**: HV must resolve party-by-ID against the re-architected MDM, not snapshot payloads. Target contract shape to be documented in [[party-rearch-phase-1-high-volume-architecture]] when authored. Spec is **subject to change until 1 Sep go-live, but not massively** ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]).
- **From [[inrisk]] (small additional surface)**: per [[rory-beattie]], HV/Artificial _"just need[s] some information from InRisk. They're set up, they have endpoints that you probably can use straight away."_ Small-footprint integration; not blocked on InRisk's MDM cutover work.

### Outbound API / writes
- **To [[party-application]] (Phase 1 onward, via [[boomi]])**: TBD — whether HV creates new parties programmatically, looks up only, or both is open ([[open-questions#OQ-013]]).

### Event streams

#### Produced
- _TBD — pending HV team's own design material._

#### Consumed
- **From [[party-application]] (Phase 2-style, later in Phase 1 or post-cutover)**: party-change-event push back to Artificial so Artificial keeps its mirror up to date (incl. sanctions pass/fail, role changes). Per [[alex-sillars]]: _"we're going to emit events. We're going to tell you the party role is a cover holder, and we're going to expect you to update the data."_ Routed via [[boomi]].

### Batch / scheduled integrations
- _Pending ingest._

## Internal structure
_Pending HV team's own design material._ HV-as-Convex is a thin wrapper / configuration layer over [[artificial]]; HV-as-Artificial is out of scope. The first authoritative architecture to capture here will come from [[simon-hulbert]]'s top-level + sequence diagrams (action #36 on [[party-rearch-ownership-matrix]]).

## Observability & SLOs
_Pending ingest._

## Known constraints / rough edges
- **Must be target-compatible by 1 September** — HV cannot integrate against the legacy payload-event shape; this gates Phase 1 of [[party-rearch]].
- **Boomi connectivity is the immediate unblock** — Artificial cannot exercise the MDM API until [[srini]] finishes the Boomi connectivity setup and Artificial-side credentials + security tokens are issued. Party-side dev-env DynamoDB is already stood up with fake data per the YAML spec ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]).
- **Artificial's development cadence is materially faster than Convex's** — minutes/hours/days, not sprints. Convex needs to hand them a clean integration surface; Artificial will not absorb iteration cycles well.
- **InRisk-first cutover sequencing applies** — HV switches on at 1 Sep **after** InRisk has been on MDM for ≥ 2 weeks ([[inrisk-cuts-over-before-high-volume]]). Stated to the Artificial counterparty via [[simon-hulbert]] on 2026-05-19.
- **Integration with [[inrisk]] is open** — see [[open-questions#OQ-013]]; small surface per HV's framing.
- **Architect-level contact**: [[simon-hulbert]] (Convex side); [[srini]] for Boomi specifically.

## Relationships to other architecture pages
- [[party-application-architecture]] — HV (via Artificial, via Boomi) is the forcing consumer that motivates the Party re-architecture timeline
- [[inrisk-architecture]] — small additional integration shape; not on the critical path
- [[artificial]] · [[boomi]] · [[ntt]] · [[sanctions-processing]] — platform / domain pages that frame the Convex-side integration surface around HV

## Phase-target pages that delta from this one
- _(none yet — [[party-rearch-phase-1-high-volume-architecture]] to be created when HV's target detail is authored; primary inputs will come from [[simon-hulbert]]'s diagrams once received)_

## Superseded claims
_None — this page has never carried production-state claims to supersede._

## Related
- [[high-volume]] — identity page
- [[artificial]] — vendor platform underpinning HV
- [[boomi]] — integration broker for HV ↔ Party
- [[party-rearch-phase-1]] — the phase gated by HV's 1-Sep go-live
- [[high-volume-team]] — owning team
- [[inrisk-cuts-over-before-high-volume]] — sequencing rule

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — HV framed as gating consumer for Phase 1
- [[sources/20260519-party-integration-timelines]] — HV reframed around [[artificial]]; [[boomi]] confirmed as gateway; Convex-side coordination shape captured
