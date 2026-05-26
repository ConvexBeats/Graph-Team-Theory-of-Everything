---
type: application
title: High Volume
aliases: [hv]
created: 2026-04-22
updated: 2026-05-26
tags: [application, in-scope, future-consumer]
owner: high-volume-team
state: in-development
sources: [20260422-meeting-transcript-session-2, 20260519-party-integration-timelines]
source_count: 2
projects: [party-rearch]
status: draft
---

# High Volume

> **Architecture:** detailed current-state tech, touchpoints, and constraints live on [[high-volume-architecture]] (still a thin stub — HV is greenfield, and most of HV's substance lives in the underlying [[artificial]] vendor platform). Phase-target shapes live under [[party-rearch]] as `party-rearch-<phase-id>-high-volume-architecture` pages (created when targets are authored).

## Summary
**High Volume (HV) is Convex's implementation of the [[artificial]] vendor platform** for high-throughput insurance-deal flow. HV is not a greenfield in-house build — it's Convex's adoption of Artificial as the engine, wrapped in Convex-specific integrations (Party, InRisk, and others). As a Party consumer, HV — and through it, Artificial — reads the new MDM API via **[[boomi]]** as the integration broker (the same pattern Convex uses for [[sanctions-processing]] → [[ntt]]). HV's **1 September production go-live is the forcing function for [[party-rearch-phase-1]]**. HV does **not** consume Party via the search widget — API-only, no UI dependency.

## Aliases / previous names
- HV

## Purpose
Handle deal flow at a volume and pace that the existing pre-bind path can't efficiently serve. Convex is implementing the [[artificial]] vendor platform to do this; the Convex-side delivery work — integrations, configuration, mapping Convex's domain onto Artificial — is what this page tracks. In the Party-consumer context: HV (via Artificial) needs lightweight, API-driven access to the Party contract to look up, create, or reference parties programmatically as part of high-throughput flows.

## Vendor platform underpinning HV
- **[[artificial]]** — the third-party platform that does the heavy lifting. HV's Party-consumption shape (read-then-event-back) and integration cadence are largely driven by what Artificial expects from its data sources.
- **[[boomi]]** — the integration broker between Artificial and [[party-application]]. All Party reads and event-pushes between HV/Artificial and Party flow through Boomi; no direct point-to-point integration.
- See [[high-volume-architecture]] for the technical shape of the Convex-side integration surface.

## Current state

- **Owner (Convex-side programme)**: [[high-volume-team]]
- **Vendor platform**: [[artificial]] (mid-implementation; first substantive Party-consumer kick-off scheduled 2026-05-20)
- **Integration broker**: [[boomi]] (connectivity setup in flight — [[srini]] (HV-side) pairing with [[joe-worsfold]] (Party-side); see [[party-rearch-ownership-matrix]] action #33)
- **Main Convex-side contacts**:
  - [[simon-hulbert]] — Architect on the HV/Artificial integration project; main Party-integration contact for HV
  - [[srini]] — Boomi Integration Architect on the HV/Artificial project
  - [[luca]] — scheduler / coordinator for the fortnightly Convex × Artificial cadence ([[open-questions#OQ-038]])
  - **Boardroom-PM** — in-room PM coordinator named in [[sources/20260519-party-integration-timelines]] — possibly [[luca]] or a colleague ([[open-questions#OQ-037]])
  - **Sam** — _"FDA for integrations"_ ([[open-questions#OQ-024]] advanced; FDA acronym TBD at [[open-questions#OQ-039]])
- **Status**: in development (pre-production)
- **Integration posture for Party**: consume the new MDM's API **via [[boomi]]** — no widget dependency. **Bypasses widget / styling-stack decisions entirely** (see [[inrisk-cuts-over-before-high-volume]]).
- **What Artificial has today**: [[joe-worsfold]]'s YAML interface spec for the new Party MDM (received pre-2026-05-19); spec considered _"good"_ for play / test. Cannot yet exercise the API — blocked on Boomi connectivity + Artificial-side credentials ([[srini]]'s setup work).
- **Coordination shape**: fortnightly Convex × Artificial catch-ups agreed 2026-05-19 ([[alex-sillars]] · [[rory-beattie]] · [[simon-hulbert]] + [[srini]] + Artificial-side reps). Single shared Slack channel with per-application prefix convention. Cadence may tighten as 1 Sep approaches.

## Target state

- **Party integration**: new MDM API (via [[boomi]] broker) for party lookup and, likely, draft-party creation.
- **Two-stage Party integration** (per [[sources/20260519-party-integration-timelines]]):
  - **(a) Read path** (priority — required for cutover): Artificial → [[boomi]] → [[party-application]] MDM API; party lookup by ID + version.
  - **(b) Change-event push** (later): [[party-application]] → [[boomi]] → Artificial; emits party-change events (incl. sanctions pass/fail, role changes) so Artificial keeps its mirror up to date.
- **Sequencing**: HV (and therefore Artificial-as-HV-consumer) switches on at the **1 Sep gate**, **after** [[inrisk]] has been live on MDM for ≥ 2 weeks per [[inrisk-cuts-over-before-high-volume]]. Stated explicitly to Artificial via [[simon-hulbert]] on 2026-05-19.
- **Not dependent on**: the PCT widget, Chakra V2/V3 decisions, or any UI consideration on the Party side — see [[party-curation-tool]].
- **Strangler-pattern stability**: [[strangle-the-graph-via-proxy-events]] means HV (and Artificial behind it) consume the same effective event contract from Party from 1 Sep onward as today's downstream consumers see — _"Everything that's going to come out of Party after we go live on the 1st of September is exactly what it comes out today. We're going to do that in a different way. We're going to obtain that information through a different source. It might not be Party, it might be joined-up InRisk and Party data, but it will still hit sanctions, DU, in exactly the same way."_ — [[alex-sillars]], [[sources/20260519-party-integration-timelines]].

## Timeline

- **1 September 2026** — production go-live. Fixed deadline; the programme's external clock. Outer gate of [[party-rearch-phase-1]].
- **Inner gate (≥ 2 weeks before 1 Sep)**: [[inrisk]] cuts over to MDM first, exercising the contract in production before HV depends on it. Concrete InRisk-cutover date open at [[open-questions#OQ-035]].
- **Pre-cutover**: Convex × Artificial fortnightly cadence; first substantive call 2026-05-20.
- **In-session, [[will-bone]] previously speculated** that 1 Sep might be training and real production was _"1st of January or whatever"_. **Pass B confirmed this is not the case** — 1 September is the real deadline. Do not propagate the 1 Jan framing.

## Change ownership

- **Working unit (Convex-side)**: [[high-volume-team]]
- **Main contact (Party-integration side)**: [[simon-hulbert]]
- **Boomi connectivity (HV side)**: [[srini]] (pairing with [[joe-worsfold]] on the Party side)
- **Scheduling / coordination**: [[luca]] (and/or Boardroom-PM)
- **Integration FDA**: **Sam** — role partially identified ([[open-questions#OQ-024]] · [[open-questions#OQ-039]])
- **Party-side counterpart**: [[graph-team]] for the MDM API contract; integration via [[boomi]]

## Pending / unknown

- **Sam's full name** and where the FDA-for-integrations function sits — [[open-questions#OQ-024]]; FDA acronym expansion at [[open-questions#OQ-039]].
- **[[srini]]'s surname** — [[open-questions#OQ-040]].
- **[[luca]]'s surname + formal role** and whether Luca is the same person as the in-room Boardroom-PM — [[open-questions#OQ-038]] · [[open-questions#OQ-037]].
- **Expected Party-write volume and throughput shape** (batch bursts vs steady state) — affects MDM's intercept-and-backfill sizing. [[open-questions#OQ-013]].
- **Whether HV / Artificial creates new parties programmatically, looks up existing parties only, or both** — has implications for auto-curation gating and D&B handling. [[open-questions#OQ-013]].
- **Whether HV's user population includes any humans using the PCT** — current assumption: M2M only via Boomi.
- **Other Party-side integration expectations** from HV/Artificial that haven't surfaced yet (e.g. version-change event subscription, D&B enrichment thresholds, sanctions-result event filtering).
- **Artificial's PM cohort** — pushed back on attending the 2026-05-19 prep call; will surface names at the 2026-05-20 kick-off.

## Related
- [[high-volume-architecture]] — current-state architecture (still a thin stub; substance largely lives in [[artificial]])
- [[high-volume-team]] — owning team (Convex side)
- [[artificial]] — vendor platform underpinning HV
- [[boomi]] — integration broker for HV ↔ Party
- [[simon-hulbert]] — main Party-integration contact
- [[srini]] · [[luca]] — Boomi-side and coordination-side counterparts
- [[party-application]] — target integration
- [[inrisk-cuts-over-before-high-volume]] — sequencing rule that gates HV's cutover
- [[strangle-the-graph-via-proxy-events]] — stability promise restated to Artificial on 2026-05-19
- [[party-rearch-ownership-matrix]] · [[party-rearch-dependency-map]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — original HV framing as a forcing-function consumer (Pass A + B)
- [[sources/20260519-party-integration-timelines]] — reframed HV around [[artificial]] as the underlying vendor platform; introduced [[srini]], [[luca]], the Boardroom-PM, and Sam-as-FDA-for-integrations; established the Convex × Artificial coordination cadence
