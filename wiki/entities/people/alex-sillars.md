---
type: entity
title: Alex Sillars
aliases: [speaker-6]
created: 2026-04-22
updated: 2026-04-22
tags: [person, po]
team: graph-team
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: stub
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
- [[sources/20260422-meeting-transcript-session-2]]
