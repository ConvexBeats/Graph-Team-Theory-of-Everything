---
type: entity
title: Graph Team
aliases: [party-team]
created: 2026-04-22
updated: 2026-04-22
tags: [team]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Graph Team

## Summary
The working unit that owns both the [[party-application]] and the [[party-curation-tool]]. Also referred to informally as "The Party Team" when talking about PCT. The team driving the Party Application re-architecture programme from the build side.

## Key facts
- **Kind**: internal engineering team
- **Aliases**: informally "The Party Team" when discussing [[party-curation-tool]]
- **Role in programme**: builder — owns MDM, PCT, and the migration proxy-event adapter

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[alex-sillars]] | Product Owner |
| [[joe-worsfold]] | Tech Lead / Architect |
| [[sergiu-postolachi]] | Scrum Master |
| [[ben-joseph]] | Engineer / Developer |
| [[billy-calladine]] | Developer / Engineer |
| [[tomas-sivo]] | Business Analyst — PCT functional requirements |

_Team is ~13 people per Session 1; 6 are identified in this wiki. See [[open-questions#OQ-017]] for the MDM-delivery squad-shape conversation (focused ~3 vs. whole squad)._

## Applications owned
- [[party-application]]
- [[party-curation-tool]]

## Current workstreams
- **Core MDM build** — data model revisions, intercept, 100% backfill, projections, proxy adapter. Estimated ~2 focused sprints for the core pieces.
- **New PCT** — Next.js UI generated from the MDM open-API spec. Ships with MDM per [[pct-and-mdm-go-live-together]].
- **InRisk integration** — joint work with [[prebind-team]] to land the 3 interim dependencies (party ID storage, messaging, broker retrieval).
- **DU schema conversation** — establish whether the flattened `(party-id, submission-id)` shape works; [[joe-worsfold]] owning.
- **Widget/SDK Chakra conversation** — pending InRisk workshop, [[alex-sillars]] + [[rory-beattie]] to run.

## Relationships
- Builds for [[prebind-team]] (InRisk), [[dataops-team]] (PCT users), the still-unknown HV owning team, and indirectly for [[devx-team]] (whose [[inrisk-engine]] targets the final state Party contract).
- Data consumer: [[analytics-team]] (DU).
- Oversight: [[tech-tooling]].

## Claims
- Graph Team owns both [[party-application]] and [[party-curation-tool]] — user-declared; reinforced by [[sources/20260422-meeting-transcript-session-2]].
- [[alex-sillars]] is PO; [[joe-worsfold]] is Tech Lead / Architect; [[sergiu-postolachi]] is Scrum Master; [[ben-joseph]] and [[billy-calladine]] are Engineers — user-declared.
- Team has committed estimate of ~2 sprints for core MDM build with a focused subset; ~2–3 months for the full programme — [[sources/20260422-meeting-transcript-session-2]], see [[alex-sillars]].

## Open questions
- Full team composition beyond the six identified members (team size is ~13 per Session 1).
- **MDM-delivery squad shape** — see [[open-questions#OQ-017]]. Focused ~3-person MDM squad vs. whole ~13-person squad involvement. Rory's call; merits further discussion.

## Related
- [[party-application]] · [[party-curation-tool]]
- [[alex-sillars]] · [[joe-worsfold]] · [[sergiu-postolachi]] · [[ben-joseph]] · [[billy-calladine]] · [[tomas-sivo]]
- [[tech-tooling]] · [[architecture-team]] · [[prebind-team]] · [[devx-team]] · [[dataops-team]] · [[analytics-team]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]]
- [[sources/20260422-meeting-transcript-session-2]]
