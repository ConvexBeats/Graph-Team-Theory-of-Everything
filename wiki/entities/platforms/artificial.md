---
type: entity
title: Artificial
aliases: [artificial-lab]
created: 2026-05-26
updated: 2026-05-26
tags: [platform, vendor, high-volume]
owner: high-volume-team
sources: [20260519-party-integration-timelines]
source_count: 1
status: stub
projects: [party-rearch]
---

# Artificial

## Summary
**Artificial** (sometimes referred to in-call as "Artificial Lab") is the **third-party vendor platform that underpins Convex's [[high-volume]] programme**. Convex is implementing Artificial as the engine for high-volume insurance-deal flow, and Artificial has now committed to consuming Party data — specifically the new MDM API — as part of its integrations with Convex. Artificial is the first **net-new Party consumer outside Convex's own application estate** in the [[party-rearch]] project, accessed via [[boomi]] as the integration broker. First substantive surfacing in the wiki was [[sources/20260519-party-integration-timelines]] (2026-05-19); a Convex × Artificial kick-off was scheduled for 2026-05-20.

## Key facts
- **Kind**: vendor system (third-party platform)
- **Owning team (Convex-side, programme contact)**: [[high-volume-team]] — [[simon-hulbert]] is the Architect on the Convex × Artificial integration project; [[srini]] is the Boomi Integration Architect handling Artificial-side connectivity
- **Core technology**: vendor SaaS-style platform; external — internals not in scope for this wiki. Convex integrates with it via API + Boomi
- **Role in programme**: **Party consumer in Phase 1** — Artificial reads parties via the new MDM API; eventually receives change-event pushes back for sanctions-pass/fail and other party updates

## Current shape (today)
- **Convex is mid-implementation** of Artificial for the [[high-volume]] programme — see [[high-volume-architecture]] for Convex-side specifics.
- **No live consumption of Party data yet** — Artificial has received [[joe-worsfold]]'s YAML API spec for the new Party MDM and called it _"good"_ ([[sources/20260519-party-integration-timelines]] claim #2). They have not yet exercised the API.
- **Boomi connectivity pending**: Artificial cannot call the MDM API until [[srini]] (HV-side Boomi Integration Architect) finishes the Boomi connectivity setup and Artificial-side credentials + security tokens are issued. This is the immediate unblock — see [[party-rearch-ownership-matrix]] action #33.

## Target-state exposure (from this programme)

### Phase 1 (the [[party-rearch]] cutover, ≤ 1 Sep 2026)
- Two-stage integration with [[party-application]]:
  - **(a) Read path** (priority — required for cutover): Artificial → [[boomi]] → [[party-application]] MDM API; party lookup by ID + version. Drives Artificial's deal-flow setup.
  - **(b) Change-event push** (later): [[party-application]] → [[boomi]] → Artificial; emits party-change events (incl. sanctions pass/fail, role changes) so Artificial keeps its mirror up to date. Per [[alex-sillars]]: _"we're going to emit events. We're going to tell you the party role is a cover holder, and we're going to expect you to update the data."_
- **Sequencing**: Artificial's MDM cutover follows the [[inrisk-cuts-over-before-high-volume]] rule — HV (and therefore Artificial-as-HV-consumer) switches on at the 1 Sep gate, **after** [[inrisk]] has been live on MDM for ≥ 2 weeks. Stated explicitly to Artificial via [[simon-hulbert]] in [[sources/20260519-party-integration-timelines]] claim #13.
- **Minor link to [[inrisk]]**: per [[rory-beattie]], Artificial _"just need[s] some information from InRisk. They're set up, they have endpoints that you probably can use straight away, so that's slightly different."_ Small-footprint dependency relative to the Party MDM one.

### Phase 2+ (out of [[party-rearch-phase-1]])
- _Open._ Artificial's longer-term relationship with the Convex domain model (party + sanctions + deal) is not yet captured here. Watch for material in subsequent Convex × Artificial calls (first kick-off 2026-05-20).

## Coordination shape
- **Cadence**: fortnightly Convex × Artificial catch-up agreed 2026-05-19; attendees [[alex-sillars]] · [[rory-beattie]] · [[simon-hulbert]], plus [[srini]] and Artificial-side representatives. Tightens as 1 Sep approaches. Scheduled by [[luca]] (or the Boardroom-PM — see [[open-questions#OQ-037]]).
- **Channel**: single shared Slack channel for Artificial integration coordination, with a per-application prefix convention at the start of each message.
- **Documentation flow**: [[joe-worsfold]]'s YAML API spec already shared with Artificial; [[simon-hulbert]] to send the top-level HV/Artificial integration architecture diagram + sequence diagrams to [[rory-beattie]] (action #36).
- **Artificial-side cadence**: per [[simon-hulbert]], _"they measure their development or configuration in their world in minutes and hours, or hours and days … they want to start, and they're like, well, give us access to all of your things."_ Materially faster than Convex's cadence — implication: Convex must hand them a clean, stable integration surface; Artificial will not absorb iteration cycles well.

## Relationships
- **Owned by** (Convex-side programme contact): [[high-volume-team]] ([[simon-hulbert]] Architect; [[srini]] Boomi Integration Architect)
- **Consumes from**: [[party-application]] (read + change-event push, both via [[boomi]]); [[inrisk]] (small additional surface)
- **Feeds**: nothing back to Convex internal systems beyond confirming receipt — Artificial is an external mirror of relevant Party data, not a source

## Claims
- Artificial is being implemented by Convex for the high-volume programme and has now committed to consuming Party data — _"they're keen to adopt Party as part of their integrations"_ — Boardroom-PM, [[sources/20260519-party-integration-timelines]].
- Artificial has received [[joe-worsfold]]'s YAML API spec for the new Party MDM and considers it suitable for play / test — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Artificial cannot exercise the MDM API today because Boomi-side authorisations (client ID + security token) are not yet issued; [[srini]] will run that setup with [[joe-worsfold]] — [[alex-sillars]] + [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Artificial's expected delivery cadence is materially faster than Convex's — _"they measure their development or configuration in their world in minutes and hours, or hours and days"_ — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Artificial cuts over to MDM **after** [[inrisk]] — _"InRisk … goes first … we don't want to go from one user to a million users … our intention is to move those at least a couple of weeks ahead of when we think we'll be able to land with you guys"_ — [[rory-beattie]], [[sources/20260519-party-integration-timelines]].

## Open questions
- The full Artificial-side counterparty roster (PMs, technical contacts) is not yet captured. The 2026-05-20 kick-off will surface names; add stubs once the next source lands.
- Artificial's downstream consumption pattern of Party data — does it mirror Party records fully, or selectively (sanctions-relevant fields only)? Drives the change-event payload shape ([[open-questions#OQ-007]] adjacent).
- Phase-2+ posture — once Convex has a stable feed to Artificial, what additional contracts does the relationship demand? Currently unspecified.
- See also [[open-questions#OQ-037]] (Boardroom-PM identity), [[open-questions#OQ-038]] ([[luca]] surname / role), [[open-questions#OQ-039]] (FDA acronym), [[open-questions#OQ-040]] ([[srini]] surname).

## Related
- [[high-volume-team]] · [[simon-hulbert]] · [[srini]] · [[luca]]
- [[high-volume]] — Convex's implementation programme; this is what Artificial underpins
- [[boomi]] — integration broker between Artificial and Party
- [[party-application]] · [[party-application-architecture]] — the Party-side MDM API Artificial consumes
- [[party-rearch-phase-1]] — Artificial in scope for the 1-Sep cutover (after InRisk)
- [[inrisk-cuts-over-before-high-volume]] — the sequencing rule that gates Artificial's go-live
- [[strangle-the-graph-via-proxy-events]] — the strangler-pattern stability promise restated to Artificial in this source
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]

## Sources
- [[sources/20260519-party-integration-timelines]] — first substantive treatment; established the platform role, the Boomi-gateway integration shape, the cadence + Slack channel coordination model, and the InRisk-first cutover sequencing communicated to Artificial.
