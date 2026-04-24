---
type: entity
title: High Volume Team
aliases: [hv-team]
created: 2026-04-22
updated: 2026-04-22
tags: [team, consumer]
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: stub
---

# High Volume Team

## Summary
The working unit that owns the [[high-volume]] application. Main Party-integration contact is [[simon-hulbert]] (Architect). The team's **1 September** HV go-live is the forcing function for this programme's Phase 1 timeline.

## Key facts
- **Kind**: internal product / engineering team
- **Role in programme**: owns the forcing-function consumer application; API-only integration into new MDM via Boomi

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[simon-hulbert]] | Architect — main Party-integration contact |

_Other members pending._

_Other name referenced in-session:_ "Sam" — possibly an HV-side delivery role, not yet mapped to a full name or role.

## Current workstreams (for this programme)
- [[high-volume]] build-out to 1 September production.
- Integration with the new MDM's API via Boomi (no widget dependency).

## Relationships
- **Integrates with**: [[party-application]] (via Boomi) — see [[dependency-map]].
- **Coordinates with**: [[graph-team]] on the MDM API contract; [[tech-tooling]] ([[rory-beattie]]) on timeline and integration shape.

## Claims
- High Volume Team owns [[high-volume]] — user-declared (Pass B).
- [[simon-hulbert]] is an Architect on this team and the main contact for Party integration — user-declared.
- "Sam" is a first-name-only in-session reference to someone associated with the HV go-live — identity and role pending.

## Open questions
- Full team composition.
- Sam — full name and role.
- HV's expected Party-write volume shape (batch bursts vs steady state) — affects MDM intercept/backfill sizing.
- Whether HV creates new parties programmatically, looks up only, or both — affects auto-curation / D&B behaviour.

## Related
- [[simon-hulbert]]
- [[high-volume]] — application owned
- [[party-application]] — integration target
- [[graph-team]] · [[tech-tooling]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
