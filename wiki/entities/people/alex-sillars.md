---
type: entity
title: Alex Sillars
aliases: [speaker-6]
created: 2026-04-22
updated: 2026-05-26
tags: [person, po]
team: graph-team
sources: [20260422-meeting-transcript-session-2, 20260519-party-integration-timelines, 20260519-mdm-implementation-strategy]
source_count: 3
status: draft
---

# Alex Sillars

## Summary
Product Owner of the [[graph-team]]. Leading technical voice on the MDM design in the 2026-04-22 session — despite being the PO rather than a tech-lead title, Alex is operating as the primary architect of the new Party Application and the curation flows on top of it. Coined the "strangle the graph" framing and drove the proxy-events design ([[strangle-the-graph-via-proxy-events]]).

## Key facts
- **Role**: Product Owner
- **Team**: [[graph-team]]
- **Applications owned (as PO)**: [[party-application]], [[party-curation-tool]]
- **Operating style**: technical PO; drives architectural design alongside [[joe-worsfold]]

## Relationships
- Works alongside [[joe-worsfold]] (Tech Lead / Architect) and [[ben-joseph]] (Engineer) on the [[graph-team]].
- Primary counterparty on the [[party-curation-tool]] widget / SDK conversation with InRisk.
- Reports (in programme sense) into [[tech-tooling]] oversight.

## Claims
- PO of the [[graph-team]] — user-declared.
- Designed the MDM's three-mode API contract (`by ID+version`, `by ID+timestamp`, `by ID latest`) — [[sources/20260422-meeting-transcript-session-2]].
- Proposed and argued for the strangler-via-proxy-events strategy — [[sources/20260422-meeting-transcript-session-2]]; see [[strangle-the-graph-via-proxy-events]].
- Timeline estimate: ~2 sprints for data model + proxy + adapter with focused squad; ~2–3 months overall including InRisk collaboration and change management — [[sources/20260422-meeting-transcript-session-2]].
- Raised the _"party on submission vs requirement"_ point — flagged as a [[prebind-team]] architectural choice, not a Party-side issue.
- **Stated the strangler stability promise to [[artificial]] directly** ([[sources/20260519-party-integration-timelines]]): _"Everything that's going to come out of Party after we go live on the 1st of September is exactly what it comes out today …  It might not be Party, it might be joined-up InRisk and Party data, but it will still hit sanctions, DU, in exactly the same way."_ First external statement of [[strangle-the-graph-via-proxy-events]]; recorded as a refinement on that ADR.
- **"UI, UX, I've never cared about, never will"** ([[sources/20260519-mdm-implementation-strategy]]) — agreement with [[rory-beattie]]'s in-call descope of UI/UX parity work. Reinforces the parity-not-enhancement principle on [[inrisk-cuts-over-before-high-volume]] from the PO side.
- **Identified the InRisk-side dispatch-code change** ([[sources/20260519-mdm-implementation-strategy]]): _"in-risk have something in their code that determines which of our widgets they call. Yes, they pass parameters to us, but they specifically request it, so we'd need to change that on InRisk's side."_ Surfaced the InRisk-side half of the widget remit that [[billy-calladine]] is now formalised on.
- **Wary of KG consistency** ([[sources/20260519-mdm-implementation-strategy]]) — Alex's verbatim on Option B of the proxy-event design fork: _"the question of does whom-party and knowledge graph all have consistent data is one that scares me."_ Aligns with Joe's stated preference for Option A (InRisk endpoint) on [[open-questions#OQ-041]].

## Current actions (open)
- Create and schedule tickets for data-model revisions, intercept, backfill, projections, proxy adapter.
- Involve testers early to build the go-live checklist.
- Joint with [[rory-beattie]]: InRisk widget / SDK / Chakra V2-vs-V3 discussion.

## Open questions
- Full reporting line and tenure context.
- Whether PO responsibilities extend to [[party-curation-tool]] user-side relationship management, or whether that's split with DataOps-facing roles.

## Related
- [[graph-team]] · [[joe-worsfold]] · [[ben-joseph]]
- [[party-application]] · [[party-curation-tool]]
- [[strangle-the-graph-via-proxy-events]] · [[pct-and-mdm-go-live-together]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — strangler design + PO framing
- [[sources/20260519-party-integration-timelines]] — strangler stability promise stated to [[artificial]] directly
- [[sources/20260519-mdm-implementation-strategy]] — UI/UX descope alignment; surfaced the InRisk-side dispatch-code change; wary of KG consistency on the proxy-event design fork
