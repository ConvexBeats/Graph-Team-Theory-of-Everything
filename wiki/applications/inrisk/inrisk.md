---
type: application
title: InRisk
aliases: [ir2]
created: 2026-04-22
updated: 2026-05-18
tags: [application, in-scope, consumer]
owner: prebind-team
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up]
source_count: 3
status: draft
projects: [party-rearch]
---

# InRisk

> **Architecture:** detailed current-state tech, touchpoints, and constraints live on [[inrisk-architecture]]. Phase-target shapes live under [[party-rearch]] as `party-rearch-<phase-id>-inrisk-architecture` pages (created when targets are authored).

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

### Phase-1 epic and stories (concrete as of 2026-05-13)

[[john-trahearn]] condensed the prior architecture sessions into a concrete **"Party MDM Integration" epic** with **four to five stories** ([[sources/20260513-inrisk-integration-with-party-mdm-follow-up]]). All carry [[prebind-team]] as the delivery team:

| # | Story | Notes | Dependencies |
|---:|---|---|---|
| 1 | **Remove manual client creation on new requirement** — route manual creation via the widget rather than InRisk pages | Tech-debt clearance; widget already supports this elsewhere in InRisk. [[billy-calladine]] to confirm widget commonality at Thursday's high-level | none in scope of party-rearch |
| 2 | **Backwards-compatible data-model addition** — store party-ID + version-IDs on existing client and broker tables, with DTO updates | Snapshots and all existing fields retained; this story does **not** cover continued extraction of today's fields from the new feed (separate concern about response shape) | [[open-questions#OQ-036]] (response-shape alignment) |
| 3 | **Client-widget integration, feature-flagged** | Flag-on: render the new MDM widget + persist new IDs. Flag-off: behave as today | Story 2 (data model) · [[inrisk-cuts-over-before-high-volume]] (component-library choice) |
| 4 | **Broker-widget integration, feature-flagged** | Mirror of story 3 for brokers. May collapse with story 3 if straightforward | Story 2 · same |
| 5 | **Party-tagging integration** (raised in-call by [[joe-worsfold]], [[john-trahearn]] to add) | Party tags (obligor / loss-leader / role-like) are _legitimately_ party data and need MDM-side + widget-side support. **Not** to be conflated with feature tagging (out of scope) | Story 2 · Joe-side widget work |

**Cadence**: high-level session Thursday this week (May 14); low-level Tuesday next week (May 19); sprint starts following Wednesday (May 20). [[john-trahearn]] to brush up on InRisk's existing widget integration mechanic ahead of the high-level — current behaviour is believed to be query-parameter-driven, the new widget is SDK-style (instantiate the MDM SDK, pass methods / hooks to components). Alignment is a Thursday's session input.

### Underlying integration changes (the three from Sessions 1 & 2, now slotted under the epic)

The three changes laid out in Sessions 1 & 2 are still the right framing of the underlying work — the 13 May follow-up gave them story-shape under the epic above:

1. **Store Party ID on InRisk's party table** — _originated in the "prior call" Joe references in Session 1 with [[john-trahearn]] and [[kris-mokrzycki]], and confirmed again in Session 2._ Per Session 1: this lands as a **new reference table** holding `(client_id → party_id, version)`. Per 2026-05-13: the data-model story (story 2) implements this as additive columns on the existing **client and broker tables**, with snapshots retained — backwards-compatible and minimal schema change.
2. **Include Party ID in outgoing InRisk messaging** — so downstream systems can resolve against MDM instead of relying on embedded snapshots. (Confirmed via story 2 by [[sergiu-postolachi]]'s in-call question about MDM-side corresponding fields.)
3. **Change broker retrieval**: broker data retrieved **directly from InRisk** rather than via the current URD flow. Requires a dedicated workshop (Scott to attend) — this is the highest-effort of the three. _Session 1 phasing_: in Phase 1, a broker UID is still sent alongside the party ID so InRisk isn't forced into a simultaneous schema migration. Phase 2+: broker UID retires; broker becomes just another role on a party.

### Party tagging vs feature tagging — Phase-1 scope boundary

Confirmed in [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]]; previously conflated under "feature tagging" in earlier wiki passes:

- **Party tagging** (obligor, loss-leader, role-like tags) → _party data_ → **in scope for Phase 1** (story 5 above). Existing widget pattern + new MDM fields. Probably a different widget flavour from the main party-search widget — to be validated by [[billy-calladine]] at Thursday's high-level.
- **Feature tagging** (submission-level attributes that only piggy-backed on Party's UX / DB pattern) → _not party data_ → **out of scope for Phase 1**. [[joe-worsfold]] keeps the old feature-tagging backend, API, and widget alive past cutover so InRisk continues to pull its static list without interruption. Direction unchanged from [[feature-tagging-moves-to-inrisk]]; migration shape probably narrows toward an InRisk-classifications-style change rather than a Postgres-table handover, contingent on [[suzanna-whitefield]]'s static-list investigation ([[open-questions#OQ-019]]).

### Cutover sequencing

The party-side cutover for [[inrisk]] now lands **at least two weeks before [[high-volume]]'s 1 September go-live** (concrete InRisk-cutover date TBC at [[open-questions#OQ-035]]). HV switches on after that point. The InRisk MDM integration is therefore a **distinct deliverable** that precedes the HV cutover, not part of a single converging 1-Sep event. See [[inrisk-cuts-over-before-high-volume]] for the full rationale, including the styling-stack call (Joe maintains two component libraries; InRisk consumes the design-system-agnostic one).

### Sanctions / NTT — out of Phase 1, flagged

Today's sanctions flow runs Party events → Boomi (orchestration + cache + idempotency) → [[inrisk]] API (one-submission-at-a-time, no batch endpoint) → [[ntt]] (the actual check). [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] surfaced wide consensus that this orchestration is in the wrong place; pulling it out of Boomi into a dedicated domain is a likely Phase 2+ workstream with audit pressure this year. **Phase 1: no change** — proxy events continue to drive Boomi unchanged. See [[sanctions-processing]] for the domain framing and [[open-questions#OQ-032]] for the ownership question.

### Other items raised in earlier sessions that land on InRisk's side:
- **Client-ID / submission flattening** — either InRisk tolerates MDM emitting a flatter row in proxy events, or InRisk exposes a small API that lets Party reconstruct the nested shape on the fly. Decision depends on [[analytics-team]]'s schema-impact response.
- **Data-fixes impact** — anti-patterns in the current model mean some fixes have to be co-ordinated on both sides; [[joe-worsfold]] to run a dedicated workshop.
- **Roles vs. views feedback loop (Session 1)** — the _view_ of a party (e.g. "insured" / "reinsured") is ascribed by InRisk and must be fed back to MDM so the current DU projection shape is preserved. Roles (broker, underwriter) stay intrinsic to the party and live in MDM. See the roles-vs-views litmus test on [[party-application]].
- **Feature-tagging — stays here long term** — today's "feature-tagging" Postgres table and widget live in the [[graph-team]]'s surface because of a historical "we had a nice widget" handoff. In Phase 2+, ownership migrates to [[inrisk]] / [[prebind-team]] (their domain, their responsibility); Phase 1 leaves the old backend running so InRisk's pull continues without interruption. Formalised in [[feature-tagging-moves-to-inrisk]] (refined 2026-05-13).
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
- [[open-questions#OQ-019]] — feature-tagging migration timing / static-list investigation.
- [[open-questions#OQ-032]] — sanctions-domain location (new; [[ntt]] / Boomi orchestration ownership).
- [[open-questions#OQ-035]] — concrete InRisk MDM-cutover date (≥ 2 weeks before 1 Sep).
- [[open-questions#OQ-036]] — widget-response field alignment (Sergiu's question — does the new widget return everything InRisk needs?).

## Related decisions
- [[strangle-the-graph-via-proxy-events]] — indirectly: flattening decision shapes proxy-event emission; the cutover-window dual-write nuance (added 2026-05-13) is what makes the InRisk-first cutover reversible
- [[inrisk-cuts-over-before-high-volume]] — InRisk's MDM cutover is a deliverable in its own right preceding HV's 1-Sep go-live; rolls in the design-system-agnostic component-library decision that closes [[open-questions#OQ-005]]
- [[pct-and-mdm-go-live-together]] — scoping context
- [[no-historic-client-backfill-into-mdm]] — InRisk remains the system of record for pre-cutover client-ID-scoped history
- [[feature-tagging-moves-to-inrisk]] — Phase 2+ ownership transfer from [[graph-team]] to [[prebind-team]]; refined 2026-05-13 with party-tagging-vs-feature-tagging boundary and static-list investigation

## Related
- [[inrisk-architecture]] — detailed current-state architecture (integration-surface focus)
- [[party-application]] — upstream, primary integration
- [[inrisk-engine]] — full rewrite, targeting final Party contract; not part of this programme's interim work
- [[party-curation-tool]] — embedder of the Party widget (pending)
- [[prebind-team]] — owner · [[john-trahearn]] · [[kris-mokrzycki]]
- [[architecture-team]] · [[scott-gruber]] — broker-retrieval SME
- [[analytics-team]] · [[paul-rogers]] — flattening-decision counterpart
- [[contract-buckets]] — appears in multiple buckets (operational · data-distribution)
- [[party-rearch-ownership-matrix]] · [[party-rearch-dependency-map]] · [[open-questions]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; party-ID-table shape, broker UID retirement phasing, roles-vs-views, feature-tagging long-term ownership, manual-create edge case
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; interim-changes confirmation, broker workshop framing
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — Party MDM Integration epic + 4–5 stories; party-tagging vs feature-tagging Phase-1 boundary; InRisk-first cutover and two-component-libraries call; sanctions / NTT / Boomi flagged; SDK-style widget integration mechanic
