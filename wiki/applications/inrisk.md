---
type: application
title: InRisk
aliases: [ir2]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, consumer]
owner: prebind-team
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# InRisk

## Summary
Policy Administration System (PAS) capturing all pre-bind policy information. Primary consumer of the [[party-application]]. Currently stores large party-payload snapshots against submissions and requirements; under this programme, InRisk takes on **three interim-state modifications** so that it can operate against the new MDM contract during Phase 1. InRisk is **not** evolving to the final-state Party contract in this programme — that is [[inrisk-engine]]'s job.

## Aliases / previous names
- IR2

## Purpose
Capture all pre-bind policy information — submissions, requirements, insureds, brokers, underwriters, pricing, terms — and drive the pre-bind workflow through to bind.

## Current state

- **Owner**: [[prebind-team]] ("The PreBind Team" / "Strikers")
- **Production status**: in production, fully operational
- **Party-integration pattern**:
  - Receives full party payloads from the current Party Graph and **snapshots them locally** against submissions and requirements
  - Joins to party on a **client ID** (with renewal cascades overwriting historical submissions onto the latest client ID — see Anti-patterns)
  - Retrieves brokers via an intermediary URD path rather than direct from InRisk's own store
- **Messaging contract**: emits submission events downstream without a party-ID reference (party information is embedded in the snapshot)

## Target state (interim, this programme)

Three required changes to support the new MDM:

1. **Store Party ID on InRisk's party table** — _originated in the "prior call" Joe references in Session 1 with [[john-trahearn]] and [[kris-mokrzycki]], and confirmed again in Session 2._ Per Session 1: this lands as a **new reference table** holding `(client_id → party_id, version)`. No other InRisk schema change is required for Phase 1.
2. **Include Party ID in outgoing InRisk messaging** — so downstream systems can resolve against MDM instead of relying on embedded snapshots.
3. **Change broker retrieval**: broker data retrieved **directly from InRisk** rather than via the current URD flow. Requires a dedicated workshop (Scott to attend) — this is the highest-effort of the three. _Session 1 phasing_: in Phase 1, a broker UID is still sent alongside the party ID so InRisk isn't forced into a simultaneous schema migration. Phase 2+: broker UID retires; broker becomes just another role on a party.

Additional items raised in-session that land on InRisk's side:
- **Client-ID / submission flattening** — either InRisk tolerates MDM emitting a flatter row in proxy events, or InRisk exposes a small API that lets Party reconstruct the nested shape on the fly. Decision depends on [[analytics-team]]'s schema-impact response.
- **Data-fixes impact** — anti-patterns in the current model mean some fixes have to be co-ordinated on both sides; [[joe-worsfold]] to run a dedicated workshop.
- **Roles vs. views feedback loop (Session 1)** — the _view_ of a party (e.g. "insured" / "reinsured") is ascribed by InRisk and must be fed back to MDM so the current DU projection shape is preserved. Roles (broker, underwriter) stay intrinsic to the party and live in MDM. See the roles-vs-views litmus test on [[party-application]].
- **Feature-tagging — stays here long term (Session 1)** — today's "feature-tagging" Postgres table and widget live in the [[graph-team]]'s surface because of a historical "we had a nice widget" handoff. In Phase 2+, both the Postgres table and the widget component **migrate to InRisk ownership** (their domain, their responsibility). Phase 1: no change. Formalised as [[feature-tagging-moves-to-inrisk]].
- **Manual-create edge case** — if InRisk creates a party via the widget and then no submission is raised, DataOps has no audit trail. Long-term fix (Joe's preference): InRisk sends `party.create` at **submit** time, not **widget-open** time, via an SDK-mediated flow. Deferred. Captured here and on [[party-curation-tool]].

## Target state (end, out of this programme)

The full rewrite / final-state behaviour is owned by [[inrisk-engine]], which targets the final Party Application architecture. InRisk (IR2) is **not** migrating to the final party contract in this programme.

## Anti-patterns (current, flagged in-session)

- **Client-ID cascade on renewal**: when a renewal creates a new client-ID version, all historical submissions under the old client-ID are moved to be under the new one. This loses the "what party was this submission actually tied to" linkage in-band and makes DU-side historical analysis incorrect.
  - **Root cause**: client ID being the joining key between submission history and party. Replacing it with `(party-id, version)` on submission removes the cascade.
  - **Phase 1 treatment**: do not fix the cascade; tolerate it. Preserve the current shape in proxy events, using the InRisk API fallback if needed.
  - **Long-term**: remove client ID from the party-consumer path entirely — see [[party-application]] end-state.

## Historical audit scope (Session 1)

- **Pre-cutover historic client-ID snapshots stay in InRisk.** MDM will _not_ be backfilled with historic per-client-ID views of parties — see [[no-historic-client-backfill-into-mdm]]. Consumers with historical-as-of-date questions against InRisk's client-ID flavour continue to query InRisk (or [[data-universe]] for the aggregated view). MDM carries full party-version history from cutover forward.

## Party on requirement vs submission — deferred

Whether "party" should hang off **requirement** or **submission** in InRisk's data model is explicitly a **[[prebind-team]] architectural choice** about their Requirement → Submission data-flow spine. It is **not** a Party-side decision and is not being made in this programme. Flagged here so it's captured as a known future PreBind decision.

## Change ownership

- **Working unit**: [[prebind-team]] (owns the IR2 application and its data-model changes)
- **PreBind PO**: [[daria-romanovskaia]]
- **PreBind Tech Leads**: [[john-trahearn]], [[kris-mokrzycki]]
- **Integration support**: [[graph-team]] ([[joe-worsfold]] primarily), [[rory-beattie]] ([[tech-tooling]])
- **Architectural SME for broker-retrieval workshop**: [[scott-gruber]] ([[architecture-team]]) — required attendee
- **Client-ID flattening / DU schema counterpart**: [[paul-rogers]] ([[analytics-team]])

## Pending / unknown

All InRisk-facing open items are tracked in [[open-questions]]:
- [[open-questions#OQ-002]] — flattened `(party-id, submission-id)` shape vs. InRisk nesting-reconstruction API.
- [[open-questions#OQ-015]] — broker-retrieval workshop date / participants.
- [[open-questions#OQ-016]] — anti-patterns / data-fixes workshop.
- [[open-questions#OQ-009]] — party-merge event handling in InRisk.
- [[open-questions#OQ-005]] — InRisk's Chakra upgrade posture (drives V2/V3 widget decision on PCT side).
- [[open-questions#OQ-019]] — feature-tagging migration timing.

## Related decisions
- [[strangle-the-graph-via-proxy-events]] — indirectly: flattening decision shapes proxy-event emission
- [[pct-and-mdm-go-live-together]] — scoping context
- [[no-historic-client-backfill-into-mdm]] — InRisk remains the system of record for pre-cutover client-ID-scoped history
- [[feature-tagging-moves-to-inrisk]] — Phase 2+ ownership transfer from [[graph-team]] to [[prebind-team]]

## Related
- [[party-application]] — upstream, primary integration
- [[inrisk-engine]] — full rewrite, targeting final Party contract; not part of this programme's interim work
- [[party-curation-tool]] — embedder of the Party widget (pending)
- [[prebind-team]] — owner · [[john-trahearn]] · [[kris-mokrzycki]]
- [[architecture-team]] · [[scott-gruber]] — broker-retrieval SME
- [[analytics-team]] · [[paul-rogers]] — flattening-decision counterpart
- [[contract-buckets]] — appears in multiple buckets (operational · data-distribution)
- [[ownership-matrix]] · [[dependency-map]] · [[open-questions]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; party-ID-table shape, broker UID retirement phasing, roles-vs-views, feature-tagging long-term ownership, manual-create edge case
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; interim-changes confirmation, broker workshop framing
