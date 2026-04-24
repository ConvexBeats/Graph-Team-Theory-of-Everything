---
type: application
title: High Volume
aliases: [hv]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, future-consumer]
owner: high-volume-team
state: in-development
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: draft
projects: [party-rearch]
---

# High Volume

> **Architecture:** detailed current-state tech, touchpoints, and constraints live on [[high-volume-architecture]] (stub — HV is greenfield). Phase-target shapes live under [[party-rearch]] as `party-rearch-<phase-id>-high-volume-architecture` pages (created when targets are authored).

## Summary
An application in active development for processing large volumes of insurance deals at higher pace. Designed as a future consumer of the [[party-application]]. Does **not** consume via the UI widget — will integrate against the new MDM API via **Boomi**. Its production go-live on **1 September** is the forcing function for this programme's Phase 1 timeline.

## Aliases / previous names
- HV

## Purpose
Handle deal flow at a volume and pace that the existing pre-bind path can't efficiently serve. In the Party-consumer context: needs lightweight, API-driven access to the Party contract to look up, create, or reference parties programmatically as part of batch / high-throughput flows.

## Current state

- **Owner**: [[high-volume-team]]
- **Main contact**: [[simon-hulbert]] (Architect)
- **Status**: in development (pre-production)
- **Integration posture for Party**: consume the new MDM's API via **Boomi** — no UI widget dependency

## Target state

- **Party integration**: MDM API (via Boomi broker) for party lookup and, likely, draft-party creation.
- **Not dependent on** the PCT widget, Chakra V2/V3 decisions, or any UI consideration on the Party side — see [[party-curation-tool]].

## Timeline

- **1 September** — production go-live. Fixed deadline; the programme's external clock.
- In-session, [[will-bone]] speculated that 1 Sep might be training and real production was "1st of January or whatever" (raw transcript line 2993). **Pass B has confirmed this is not the case** — 1 September is the real deadline. Do not propagate the 1 Jan framing.

## Change ownership
- **Working unit**: [[high-volume-team]]
- **Main contact (Party-integration side)**: [[simon-hulbert]]
- **Also mentioned (role pending)**: Sam — owner of the HV go-live date per in-session references; not yet mapped to a full name / team
- **Party-side counterpart**: [[graph-team]] for the MDM API contract; integration via Boomi

## Pending / unknown

- **Sam**'s full name and role — in-session reference only; possibly [[simon-hulbert]]'s counterpart on delivery-date ownership, possibly a separate role.
- **Expected Party-write volume and throughput shape** (batch bursts vs steady state) — affects MDM's intercept-and-backfill sizing.
- Whether HV creates new parties programmatically, looks up existing parties only, or both — has implications for auto-curation gating and D&B handling.
- Whether HV's user population includes any humans using the PCT, or purely machine-to-machine (current assumption: M2M only via Boomi).
- Other Party-side integration expectations from HV that haven't surfaced yet (e.g. version-change event subscription, D&B enrichment thresholds).

## Related
- [[high-volume-architecture]] — current-state architecture stub (greenfield)
- [[high-volume-team]] — owning team
- [[simon-hulbert]] — main contact
- [[party-application]] — target integration
- [[party-rearch-ownership-matrix]] · [[party-rearch-dependency-map]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
