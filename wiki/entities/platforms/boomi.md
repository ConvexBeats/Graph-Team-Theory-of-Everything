---
type: entity
title: Boomi
aliases: []
created: 2026-05-26
updated: 2026-05-27
tags: [platform, integration, gateway]
owner: _unknown_
sources: [20260513-inrisk-integration-with-party-mdm-follow-up, 20260519-party-integration-timelines]
source_count: 2
status: stub
projects: [party-rearch]
---

# Boomi

## Summary
**Boomi** is Convex's **vendor integration broker** — a third-party iPaaS-style platform sitting between [[party-application]] and several downstream consumers / vendor systems. It is the gateway through which **sanctions screening** ([[sanctions-processing]] → [[ntt]]) flows today, and from the [[party-rearch-phase-1]] cutover onward will also be the gateway through which **[[artificial]] consumes Party MDM data**. Promoted to a first-class platform entity 2026-05-26 because two distinct vendor-integration patterns now hang off it; previously referenced as inline prose in several places. **Owning team inside Convex is not yet confirmed in this wiki**; the sanctions-side orchestration that today lives in Boomi is by group consensus _in the wrong place_ — see [[open-questions#OQ-032]].

## Key facts
- **Kind**: vendor system (integration platform / iPaaS broker)
- **Owning team (Convex-side)**: **TBD** — no team has been identified as Boomi's product owner inside Convex in this wiki. The sanctions-related orchestration that today lives on Boomi is the subject of [[open-questions#OQ-032]] (which will surface ownership as a side-effect); a separate question is who owns Boomi as a platform end-to-end.
- **Core technology**: vendor iPaaS; orchestration logic + cache + idempotency layer + pass-through credentials management
- **Role in programme**: **integration gateway** — sits between [[party-application]] and external counterparties; today routes sanctions checks, from Phase 1 cutover onward also routes Artificial's MDM consumption

## Current shape (today)

Boomi currently hosts **two distinct integration patterns**:

### 1. Sanctions orchestration (in-flight today; called out as wrong-place 2026-05-13)
The orchestration that decides _when_ to fire a sanctions check, _what context_ to send, and _what to do_ with the response. Detail lives on [[sanctions-processing]]; in short:

- [[party-application]] emits party-change events; Boomi consumes every event.
- For each event, Boomi calls the [[inrisk]] API **one submission at a time** (InRisk has no batch endpoint) to reconstruct submission + party context.
- Boomi maintains its own **cache + idempotency layer** on top of that reconstruction.
- Boomi then calls [[ntt]] for the actual sanctions check; the result is fed back into the deal lifecycle.
- **Write-back into Party**: [[alex-sillars]] describes the leg as _"the result is retrieved back or not sanctioned, and then we update our party with that"_ ([[sources/20260422-meeting-transcript-session-2]]) — Boomi writes the NTT decision onto the Party record and then separately fires an event to [[inrisk]] tagged with the affected submission IDs. The **specific API endpoint / event shape** Boomi uses for this Party-side write, and **which identifier** it uses as the lookup key (legacy Graph party ID vs UUID system ID vs other), are **not documented in any source we hold** — see [[open-questions#OQ-045]]. Bears on Phase-1 cutover transparency under [[strangle-the-graph-via-proxy-events]] + [[uuid-system-id-with-display-id]].

**Problems flagged 2026-05-13** ([[sources/20260513-inrisk-integration-with-party-mdm-follow-up]]):
- Fires on every party event, including irrelevant ones — cascading false positives ([[kris-mokrzycki]]).
- Significant orchestration logic absorbed into Boomi that the rest of the system can't see ([[joe-worsfold]]: _"I don't love the idea of having all this, what is probably very important logic on the Boomi side that none of us can see"_).
- Audit visibility this year is on sanctions ([[suzanna-whitefield]]: _"Audit's eyes are going to be on sanctions this year"_).
- Group consensus: this orchestration **should not live in Boomi**, nor inside [[party-application]] or [[inrisk]]. Phase 1 leaves the flow as-is; Phase 2+ rework. See [[open-questions#OQ-032]].

### 2. Artificial → Party MDM gateway (Phase 1 setup in progress; live at cutover)
Per [[sources/20260519-party-integration-timelines]] (2026-05-19), Boomi will be the integration broker through which **[[artificial]] consumes the new Party MDM API**.

- Same pattern as Convex has used with other third-party providers — pass-through credentials, link to Party, hand out tokens to the vendor side.
- Owned on the HV/Artificial-project side by **[[srini]]** (Boomi Integration Architect). Srini's counterpart on the Party side is [[joe-worsfold]].
- **Unblocking dependency**: until Boomi connectivity is stood up and Artificial-side credentials issued, Artificial can't exercise the MDM API even though the dev-env DynamoDB is ready with fake data. Action #33 on [[party-rearch-ownership-matrix]].
- [[high-volume]] (the Convex-side application that consumes Party for the Artificial-underpinned HV programme) is **API-only via Boomi** — no widget dependency (see [[high-volume-architecture]] and [[inrisk-cuts-over-before-high-volume]]).

## Target-state exposure (from this programme)

### Phase 1 ([[party-rearch-phase-1]])
- **Sanctions side**: Boomi orchestration **unchanged**. Proxy events from MDM ([[strangle-the-graph-via-proxy-events]]) drive Boomi exactly as Graph events do today; Boomi → InRisk → NTT chain untouched. Phase 1 makes no Boomi-side change for sanctions.
- **Artificial side**: Boomi connectivity stood up; credentials issued; Artificial begins reading the MDM API. Live at cutover (after [[inrisk]] cutover window).
- **[[high-volume]] side**: HV consumes MDM API via Boomi from the 1 Sep gate.

### Phase 2+ (open)
- **Sanctions orchestration moves off Boomi** — the room's consensus direction. Destination is a dedicated sanctions service / domain; ownership is open. See [[open-questions#OQ-032]], [[open-questions#OQ-008]].
- **Artificial / HV consumption pattern** — likely remains via Boomi; not confirmed.

## Relationships
- **Sits between** [[party-application]] and external vendors / consumers
- **Sanctions chain (today)**: [[party-application]] → Boomi → [[inrisk]] (one submission at a time) → [[ntt]]; result → back into deal flow
- **Artificial chain (Phase 1 onward)**: [[artificial]] → Boomi → [[party-application]] MDM API
- **HV chain (Phase 1 onward)**: [[high-volume]] → Boomi → [[party-application]] MDM API
- **People**: [[srini]] (Boomi Integration Architect on HV/Artificial project); [[joe-worsfold]] (Party-side counterpart on Boomi setup); [[simon-hulbert]] (Artificial integration architect)

## Claims
- Boomi sits between [[party-application]] events and [[ntt]] sanctions checks, with [[inrisk]] one-submission-at-a-time API calls to reconstruct context — [[joe-worsfold]] · [[kris-mokrzycki]], [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]].
- Boomi maintains a cache + idempotency layer on top of the sanctions reconstruction it performs — [[joe-worsfold]], [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]].
- Group consensus: the sanctions orchestration currently in Boomi is **in the wrong place** — [[joe-worsfold]] · [[rory-beattie]] · [[suzanna-whitefield]] · [[kris-mokrzycki]], [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]].
- Boomi will be the integration broker between [[artificial]] and the new Party MDM API — [[alex-sillars]] (_"I think Boomi is going to act as the interface between artificial and party"_), [[sources/20260519-party-integration-timelines]].
- [[srini]] is the Boomi Integration Architect on the HV/Artificial project and will run the Artificial-side Boomi setup with [[joe-worsfold]] on the Party side — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Boomi access for third parties (credentials + security tokens) is the unblocking dependency for Artificial's MDM consumption — [[alex-sillars]], [[sources/20260519-party-integration-timelines]].

## Open questions
- **Boomi's owning team inside Convex** — not surfaced in any source so far. Distinct from but related to [[open-questions#OQ-032]] (sanctions-domain location).
- [[open-questions#OQ-045]] (new 2026-05-27) — **Boomi ↔ Party write-back contract + identifier usage**: what API/event does Boomi use to write the NTT result back into Party today, and which ID is the lookup key? Phase-1 cutover transparency depends on this.
- See also [[open-questions#OQ-032]] (sanctions-domain location — Boomi-side orchestration's eventual home), [[open-questions#OQ-008]] (end-state sanctions flow), [[open-questions#OQ-041]] (proxy-event-with-InRisk-data design fork — Boomi's **inbound** proxy-event shape, paired with OQ-045's **outbound** write-back), [[open-questions#OQ-040]] ([[srini]]'s surname).

## Related
- [[sanctions-processing]] — the domain page; explains the sanctions orchestration that Boomi hosts today
- [[ntt]] — the underlying sanctions-check vendor that Boomi calls
- [[artificial]] — the third-party vendor platform that will consume Party MDM via Boomi from Phase 1 cutover
- [[party-application]] · [[party-application-architecture]] — the integration target on the Party side
- [[high-volume]] · [[high-volume-architecture]] — the Convex-side application that integrates with Party via Boomi
- [[inrisk]] — provides submission context to Boomi's sanctions orchestration
- [[srini]] · [[joe-worsfold]] · [[simon-hulbert]] — key people
- [[party-rearch-dependency-map]] — cross-app integration view; Boomi appears in current-state, Phase-1, and end-state diagrams
- [[party-rearch-ownership-matrix]] — Boomi-related workstreams and actions
- [[strangle-the-graph-via-proxy-events]] — the proxy events that drive Boomi from Phase 1 cutover are shape-identical to today's Graph events; Boomi consumes transparently

## Sources
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — first treatment of the sanctions-orchestration role; wrong-place framing; cache + idempotency detail
- [[sources/20260519-party-integration-timelines]] — second integration pattern surfaced (Artificial → Party MDM gateway); [[srini]] introduced; promotion to first-class platform entity triggered by this ingest
