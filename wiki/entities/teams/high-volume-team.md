---
type: entity
title: High Volume Team
aliases: [hv-team]
created: 2026-04-22
updated: 2026-05-26
tags: [team, consumer]
sources: [20260422-meeting-transcript-session-2, 20260519-party-integration-timelines]
source_count: 2
status: stub
---

# High Volume Team

## Summary
The working unit that owns the [[high-volume]] application — Convex's implementation of the [[artificial]] vendor platform for high-throughput insurance-deal flow. Main Party-integration contact is [[simon-hulbert]] (Architect on the HV/Artificial integration project). The team's **1 September** HV go-live is the forcing function for [[party-rearch-phase-1]]'s timeline. As of 2026-05-26 the team's footprint in this wiki widened materially via [[sources/20260519-party-integration-timelines]] — additional roles surfaced around Boomi connectivity, coordination, and integration FDA.

## Key facts
- **Kind**: internal product / engineering team (Convex-side delivery of the [[artificial]] vendor platform)
- **Role in programme**: owns the forcing-function consumer application; API-only integration into new MDM via [[boomi]]

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[simon-hulbert]] | Architect — main Party-integration contact on the HV/Artificial project |
| [[srini]] | Boomi Integration Architect on the HV/Artificial project (surname TBD — [[open-questions#OQ-040]]) |
| [[luca]] | Coordinator / project-management role; schedules Convex × Artificial cadence (surname + formal role TBD — [[open-questions#OQ-038]]) |

_Other members pending._

_Other names referenced in-session_:
- **Sam** — _"FDA for integrations"_ on the HV/Artificial project; accepted the 2026-05-20 Convex × Artificial kick-off call. Identification + FDA acronym still open — see [[open-questions#OQ-024]] · [[open-questions#OQ-039]]. _May sit inside HV team proper or in a separate integrations / FDA function — uncertain._
- **Convex – Orega – 6B Boardroom** speaker (2026-05-19) — HV-side PM coordinator; possibly [[luca]] or a colleague — see [[open-questions#OQ-037]].

## Current workstreams (for this programme)
- [[high-volume]] build-out to 1 September production: Convex-side integration layer atop [[artificial]].
- Stand up [[boomi]] connectivity between [[artificial]] and [[party-application]]: [[srini]] pairing with [[joe-worsfold]] (Party-side). Unblocking dependency for Artificial-side dev work. — [[party-rearch-ownership-matrix]] action #33.
- Run Convex × Artificial fortnightly catch-ups + dedicated Slack channel ([[luca]] / Boardroom-PM). — actions #34, #35.
- Run Convex × Artificial kick-off 2026-05-20 ([[rory-beattie]] · [[alex-sillars]] · [[simon-hulbert]] + Boardroom-PM). — action #32.
- [[simon-hulbert]] to send top-level + sequence architecture diagrams to [[rory-beattie]]. — action #36.

## Relationships
- **Owns**: [[high-volume]] — the Convex-side delivery of the [[artificial]] vendor platform integration.
- **Underpinned by**: [[artificial]] — third-party vendor platform.
- **Integrates with**: [[party-application]] (via [[boomi]]) — see [[party-rearch-dependency-map]]; [[inrisk]] (small additional surface).
- **Coordinates with**: [[graph-team]] on the MDM API contract; [[tech-tooling]] ([[rory-beattie]]) on timeline and integration shape; sanctions / [[boomi]] / [[ntt]] adjacent via [[sanctions-processing]].

## Claims
- High Volume Team owns [[high-volume]] — user-declared (Pass B).
- [[simon-hulbert]] is an Architect on this team and the main contact for Party integration — user-declared.
- HV is Convex's implementation of the [[artificial]] vendor platform; the Convex-side delivery is the integration layer between Artificial and the Convex domain — [[simon-hulbert]] · [[rory-beattie]], [[sources/20260519-party-integration-timelines]].
- [[srini]] is Boomi Integration Architect on the HV/Artificial project and is _"central"_ to multiple integration workstreams — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- [[luca]] is the named scheduler for the fortnightly Convex × Artificial cadence — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Sam is the _"FDA for integrations"_ on the HV/Artificial project — Boardroom-PM, [[sources/20260519-party-integration-timelines]] (advances [[open-questions#OQ-024]]).

## Open questions
- Full team composition.
- Sam — full name and role; FDA acronym expansion ([[open-questions#OQ-024]] · [[open-questions#OQ-039]]).
- [[srini]] surname ([[open-questions#OQ-040]]).
- [[luca]] surname + formal role; whether Luca is the same person as the in-room Boardroom-PM ([[open-questions#OQ-037]] · [[open-questions#OQ-038]]).
- Whether **Sam** + **[[srini]]** sit inside HV-team-proper or in a shared integrations / FDA function that supports HV alongside other vendor-integration projects.
- HV's expected Party-write volume shape (batch bursts vs steady state) — affects MDM intercept/backfill sizing ([[open-questions#OQ-013]]).
- Whether HV / Artificial creates new parties programmatically, looks up only, or both — affects auto-curation / D&B behaviour ([[open-questions#OQ-013]]).

## Related
- [[simon-hulbert]] · [[srini]] · [[luca]]
- [[high-volume]] — application owned
- [[artificial]] — vendor platform underpinning HV
- [[boomi]] — integration broker
- [[party-application]] — integration target
- [[graph-team]] · [[tech-tooling]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — original HV framing as forcing-function consumer
- [[sources/20260519-party-integration-timelines]] — team's footprint widened (Srini, Luca, Boardroom-PM, Sam-as-FDA-for-integrations); Convex × Artificial coordination shape established
