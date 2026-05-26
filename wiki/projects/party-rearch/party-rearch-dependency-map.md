---
type: analysis
title: Party Re-Architecture — Dependency Map
created: 2026-04-22
updated: 2026-05-26
tags: [analysis, standing, dependencies]
project: party-rearch
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement, 20260519-party-integration-timelines]
source_count: 5
status: draft
---

# Party Re-Architecture — Dependency Map

A standing analysis of inter-application dependencies for the Party Application re-architecture programme. Current-state vs target-state, called out separately. Refreshed on every ingest.

## Contract buckets (Session 1 scaffolding)

Session 1 introduced a three-bucket framing for every Party-consumer contract. This is the lens the rest of this map is read through:

- **Operational** — how parties get created / curated: the search widget, manual creation via application, manual creation via [[party-curation-tool]], CUO groupings, the PCT UI itself.
- **Data distribution** — who reads Party events: [[data-universe]] (owned by [[analytics-team]]), party-versioning in [[inrisk]] + [[artificial]], Launchpad, party reporting, EVA, broker dashboards.
- **Enrichment** — external sources flowing in: D&B, S&P, One Re, sanctions.

Every design decision in the programme maps onto one or more of these buckets. Full treatment is on the standalone concept page [[contract-buckets]].

## Legend
- **→** emits to / pushes data to
- **↔** bidirectional integration
- `(P)` payload-cached (snapshot)
- `(R)` reference-only (ID + version, resolve on-demand)
- `(E)` event stream
- `[Phase 1]` changes in this programme; `[End state]` later-phase target

---

## Current state (pre-programme)

```
[[party-application]] (Neo4j Graph DB)
      │
      ├─── (E, payload)  ─────►  [[analytics-team]] (DU → Snowflake)
      ├─── (E, payload)  ─────►  [[inrisk]]   snapshot-cached on submission/requirement
      ├─── (P, payload)  ─────►  URD broker path  ────►  [[inrisk]] (broker retrieval)
      ├─── (E)           ────►  Boomi  ──► [[inrisk]] (one-submission-at-a-time API calls; no batch endpoint)
      │                                     │
      │                                     └─►  [[ntt]] (sanctions check)  ──► back round-trip
      │                                     │
      │                                     ▼ (Boomi-side cache + idempotency layer; [[sanctions-processing]] in wrong place)
      ├─── (API)         ◄────  [[party-curation-tool]] (old, Next.js + Jira workflow)
      ├─── (ingest)      ◄────  D&B (lookup at curation time, gated by double-ticketing)
      ├─── (ingest)      ◄────  S&P  via  [[analytics-team]] (manual bulk)
      ├─── (ingest)      ◄────  Eclipse   **[retired — no live consumer, data stale]**
      └─── (E, broker variant)  ────►  common event bus  (broker-specific events for downstream dashboards)

[[inrisk]] ── (E, no party-ID on message) ──►  downstream PAS consumers
```

Notable quirks:
- Join key in DU between Party events and InRisk data is the **client ID** — same key that cascades on renewal inside InRisk.
- No `party-ID` on InRisk's outgoing messaging today.
- Broker data flows through URD rather than being owned by InRisk directly.
- **Spine-rewrite behaviour** (Session 1): every InRisk event causes Graph to rewrite the whole spine and emit one event, regardless of actual field changes. Effectively DU consumes **a single event** (plus the broker-variant on the common bus) despite the underlying schema complexity. This is the insight that makes the proxy adapter small.
- **Historical snapshots** live in [[inrisk]] (per client-ID, per submission) and in [[data-universe]] — not in the current Graph DB.
- **Sanctions orchestration lives in Boomi** (2026-05-13): Boomi consumes every party-change event, calls InRisk one submission at a time to reconstruct context (no batch endpoint), maintains its own cache + idempotency, calls [[ntt]] for the actual check. Group consensus: wrong place. See [[sanctions-processing]] · [[open-questions#OQ-032]]. Phase 1 leaves this flow unchanged.

---

## Phase 1 target state (this programme)

```
[[party-application]] (new MDM: DynamoDB + OpenSearch)
      │
      ├─── (E, proxy — same shape as current)  ────►  [[analytics-team]]   [Phase 1]
      │         └── see [[strangle-the-graph-via-proxy-events]]
      │
      ├─── (R, ID+version)  ────►  [[inrisk]] (IR2)      [Phase 1 — InRisk-side changes required, cuts over ≥ 2 weeks before HV]
      │         ├── InRisk stores Party ID (UUID v7) + version (int) on existing party + broker + party_snapshot tables (additive, 3 independent tables)
      │         ├── Outgoing InRisk messages include Party ID
      │         ├── Widget integration via Joe's design-system-agnostic component library (SDK-style; drop-in replacement / parity-not-enhancement)
      │         ├── Manual client creation on new-requirement flow moves to the widget (Story 1; foundational "demon" cleared)
      │         └── Broker retrieval moves to direct-from-InRisk (URD path removed)
      │
      ├─── (R, API via [[boomi]])  ────►  [[high-volume]]     [Phase 1 — switches on at 1 Sep gate, after InRisk cutover]
      │         └── no widget; API-only; HV is Convex's implementation of [[artificial]]
      │
      ├─── (R, API via [[boomi]])  ────►  [[artificial]]     [Phase 1 — vendor platform underpinning HV; reads MDM API]
      │         ├── Same Boomi gateway as HV (HV ≡ Convex-side delivery of Artificial)
      │         └── (E, change events via [[boomi]])  ────►  [[artificial]]  [Phase 1 later / Phase 2-style — Party emits party-change events back to Artificial so it keeps its mirror up to date; per Alex "we're going to emit events ... we're going to expect you to update the data"]
      │
      ├─── (API)   ◄──►   [[party-curation-tool]] (new, Next.js on open-API spec)  [Phase 1]
      │         ├── co-ships with MDM — [[pct-and-mdm-go-live-together]]
      │         └── consumes Joe's Chakra-3 + design-system widget (the other component library)
      │
      ├─── (ingest+cache)  ◄────  D&B (Dynamo-cached, TTL; auto-create parent)   [Phase 1]
      ├─── (ingest)         ◄────  S&P  via  [[analytics-team]] (still manual)  [Phase 1]
      │
      └─── (E, proxy)  ────►  [[boomi]]  ──► [[inrisk]] / [[ntt]]   [Phase 1 — unchanged, wrong place flagged]
                └── [[sanctions-processing]]; orchestration off-[[boomi]] is Phase 2+; [[open-questions#OQ-032]]

[[inrisk]] ── (E, now with party-ID) ──►  downstream consumers
Graph DB — read-only during stabilisation; brief dual-write from MDM for cutover-window revertability ([[strangle-the-graph-via-proxy-events]] refinement); decommissioned post-cutover.
```

Key Phase-1 changes:
- **DU sees no change** on cutover — proxy events in Graph shape.
- **InRisk gets a concrete 5-story epic** (Party MDM Integration), **ordered + gated 2026-05-14**: Stories 1 (manual-client-creation cleanup, foundational) and 2 (additive data-model change on 3 tables: party + broker + party_snapshot) ready for low level immediately; Stories 3/4/5 (client / broker / party-tagging widget integration, feature-flagged) gated on a widget-integration spike (Joe + Billy + Alex + [[daria-romanovskaia]] — slot inherited from [[andrew-turner]] on his 2026-05-29 departure). Now sized as a full deliverable in its own right that lands ≥ 2 weeks before HV ([[inrisk-cuts-over-before-high-volume]]).
- **Party-tagging in scope; feature-tagging out** — party-tagging gets a Phase-1 story; the feature-tagging Postgres table + widget stay alive past cutover unchanged so [[inrisk]] keeps pulling its static list ([[feature-tagging-moves-to-inrisk]] refined 2026-05-13; reinforced 2026-05-14).
- **Two widget component libraries** — Joe publishes a design-system-agnostic library for [[inrisk]] alongside the Chakra-3 + design-system widget for [[party-curation-tool]] / [[dataops-team]]. HV doesn't use a widget. Closes [[open-questions#OQ-005]]. **Drop-in-replacement / parity-not-enhancement posture for the InRisk widget** (refined 2026-05-14) — same auth / RBAC / session / look-and-feel / filter parameters (incl. **TOBA status**); UX improvements deferred.
- **HV integrates via [[boomi]] API** — widget question does not block HV; switches on at 1 Sep after InRisk-first cutover window.
- **HV is Convex's implementation of the [[artificial]] vendor platform** (clarified 2026-05-19) — so the Phase-1 Party-consumer footprint is effectively two arrows via the same [[boomi]] gateway: HV (Convex-side glue) and [[artificial]] (the underlying vendor). Two-stage integration: read-path first (Artificial → MDM, priority for cutover), event-push-back later (Party → Artificial, Phase-1-later or Phase-2-style).
- **Sanctions / [[boomi]] orchestration unchanged in Phase 1** — proxy events drive [[boomi]] just as Graph events did; the "wrong place" rework is Phase 2+ ([[open-questions#OQ-032]]).
- **[[boomi]] now hosts two distinct integration patterns** — sanctions (orchestration-heavy, wrong-place) and Artificial (pass-through gateway, clean). Captured on [[boomi]].
- **Cutover-window dual-write to old graph** for revertability — refinement of [[strangle-the-graph-via-proxy-events]]; not a contradiction of "no dual sources of truth".
- **No historical backfill into MDM** — per [[no-historic-client-backfill-into-mdm]], MDM carries version history from cutover forward; pre-cutover per-client-ID InRisk history stays in [[inrisk]] / [[data-universe]].
- **Eclipse ingestion removed** — no live consumer; already resolved via InRisk-client-ID path.
- **Single-flag coupled rollout** — old-search→new-search and old-PCT→new-PCT share one cutover flag (separate from the InRisk-side feature flags on the widget integration stories).
- **Bulk-migration tool** — MDM-team-owned CLI (CSV → diff preview → auto-approved revision); full self-serve UX is Phase 2+.
- **AWS estate** — MDM runs on the existing 3-account / 4-env world; not AWS 2.0.

---

## End state (later phase, out of Phase 1)

```
[[party-application]] (MDM)
      │
      ├─── (direct Dynamo stream)   ────►  [[analytics-team]]  (Snowpipe-style)
      │         └── replaces proxy events
      │
      ├─── (R, ID+version)  ────►  [[inrisk-engine]] (final Party contract)
      ├─── (R, ID+version)  ────►  [[inrisk]] (IR2) — may retire as InRisk Engine lands
      ├─── (R, API via [[boomi]])  ────►  [[high-volume]] / [[artificial]]   (read path + change-event push back)
      ├─── (feature-tagging moved to MDM once InRisk buys in)
      │
      ├─── (sanctions, simplified flow)
      │         NTT  ────►  [[party-application]]  (result lands on party)
      │         [[party-application]]  ────►  [[inrisk]]  (affected submissions)
      │
      └─── client-ID dimension removed from party-consumer path; submissions reference party-ID + version directly

      Broker UID retires — broker becomes just another role on a party.
      Feature-tagging Postgres table + widget migrate to [[prebind-team]] (InRisk ownership).
      S&P enrichment migrates from DU-stored bulk path to the S&P API (with its own entity resolution).
      Bulk-migration UX evolves to DataOps self-serve (PCT CSV upload + diff preview + revert-button-on-revision).
```

---

## Cross-cutting dependencies

| Dependency | From | To | Phase | Notes / owner |
|---|---|---|---|---|
| Data Universe schema-impact check on flattened `(party-id, submission-id)` | [[graph-team]] ([[joe-worsfold]]) | [[analytics-team]] ([[paul-rogers]]) | 1 | Blocks choice of flatten-vs-reconstruct approach |
| InRisk API for nested-shape reconstruction (fallback) | [[prebind-team]] | [[graph-team]] | 1 | Only needed if Analytics Team rejects flatter shape |
| Broker-retrieval workshop | [[tech-tooling]] · [[graph-team]] | [[prebind-team]] ([[john-trahearn]] / [[kris-mokrzycki]]) · [[architecture-team]] ([[scott-gruber]]) | 1 | [[scott-gruber]] required; target Mon/Tue after Romania |
| D&B caching alignment | [[graph-team]] | "enriched team" (name TBD) | 1 | [[joe-worsfold]] open; see [[d-and-b-caching-and-auto-parent]] |
| HV Party-integration shape | [[tech-tooling]] ([[rory-beattie]]) · [[graph-team]] | [[high-volume-team]] ([[simon-hulbert]]) | 1 | 1 Sep is fixed; HV switches on **after** InRisk-first cutover window per [[inrisk-cuts-over-before-high-volume]]. **Reframed 2026-05-19**: the actual integration is HV-as-Convex-side-glue + [[artificial]]-as-underlying-vendor, both via [[boomi]] |
| [[artificial]] integration shape | [[tech-tooling]] ([[rory-beattie]]) · [[graph-team]] ([[joe-worsfold]] for YAML spec + Boomi pairing) | [[high-volume-team]] ([[simon-hulbert]]) + [[artificial]] PMs (unnamed) | 1 | New 2026-05-19. Two-stage: (a) read path via [[boomi]] (priority — kickoff 2026-05-20; YAML spec already shared); (b) change-event push back (later). API spec subject to change until 1 Sep go-live but not massively |
| [[boomi]] connectivity for [[artificial]] | [[high-volume-team]] ([[srini]]) | [[graph-team]] ([[joe-worsfold]]) | 1 | New 2026-05-19. Unblocking dependency: dev-env DynamoDB is already stood up with fake data per the YAML spec, but Artificial can't exercise the API until Boomi connectivity + Artificial-side credentials are issued. [[party-rearch-ownership-matrix]] action #33 |
| Convex × Artificial fortnightly cadence + dedicated Slack channel | [[high-volume-team]] ([[luca]] / Boardroom-PM) | [[graph-team]] · [[tech-tooling]] · [[artificial]] | 1 | New 2026-05-19. First call 2026-05-20; tightens as 1 Sep approaches. Single Slack channel with per-application prefix convention. [[party-rearch-ownership-matrix]] actions #32, #34, #35 |
| Top-level + sequence integration diagrams (HV / Artificial side) | [[high-volume-team]] ([[simon-hulbert]]) | [[tech-tooling]] ([[rory-beattie]]) | 1 | New 2026-05-19. Raw-folder candidate when received. [[party-rearch-ownership-matrix]] action #36 |
| InRisk-cutover sequencing | [[graph-team]] · [[prebind-team]] | [[high-volume-team]] | 1 | InRisk lands on MDM ≥ 2 weeks before 1 Sep; HV consumes only after that buffer. [[inrisk-cuts-over-before-high-volume]]; concrete date [[open-questions#OQ-035]]. Sequencing restated to [[artificial]] directly on 2026-05-19 |
| ~~Chakra V2/V3 widget decision~~ | ~~[[graph-team]] · [[tech-tooling]]~~ | ~~[[prebind-team]]~~ | ~~1~~ | **Resolved 2026-05-13**: two component libraries — Chakra-3 + design-system for PCT, design-system-agnostic for InRisk. See [[inrisk-cuts-over-before-high-volume]] |
| Joe's design-system-agnostic component library for InRisk | [[graph-team]] ([[joe-worsfold]]) | [[prebind-team]] | 1 | New 2026-05-13; underpins InRisk's widget stories. Posture refined 2026-05-14: parity-not-enhancement; drop-in replacement on the widget itself |
| Widget-integration spike (pre-low-level for Stories 3/4/5) | [[graph-team]] ([[joe-worsfold]], [[billy-calladine]]) + [[graph-team]] ([[alex-sillars]]) | [[prebind-team]] ([[daria-romanovskaia]] — took over from [[andrew-turner]] 2026-05-29) | 1 | New 2026-05-14. Bottom out SDK posture, OpenSearch/Dynamo response shape, integration mechanic. Timing TBC at ad-hoc HL 2026-05-15. PreBind PO slot redirected 2026-05-26 |
| Widget-response field alignment (MDM ↔ InRisk consumer needs) | [[graph-team]] ([[joe-worsfold]]) | [[prebind-team]] ([[sergiu-postolachi]] raised) | 1 | OpenSearch indexed differently than Dynamo storage; iterate as integration starts. Spike (above) is the resolution path. [[open-questions#OQ-036]] |
| TOBA-status filter parity in new widget | [[graph-team]] ([[joe-worsfold]]) | [[prebind-team]] ([[jason-owen]] raised) | 1 | New 2026-05-14. Current widget filters by TOBA status (1984-approved / 1987-approved cohorts) for the Lloyd's-vs-retail broker split; new widget must do the same. Feeds [[inrisk-cuts-over-before-high-volume]] parity refinement |
| InRisk-side backfill of new ID columns (decide before Story 2 low-level) | [[graph-team]] ([[joe-worsfold]]) | [[prebind-team]] ([[john-trahearn]]) | 1 | New 2026-05-14. Backfill old client / broker / party-snapshot rows with MDM IDs, or null + go-forward from cutover? Sanctions is the principal impact surface. Not formalised as an OQ |
| Sanctions-domain ownership rework (off Boomi) | [[tech-tooling]] ([[rory-beattie]] · [[suzanna-whitefield]]) | [[andrea-read]] (escalation), then unassigned | 2+ | Audit pressure this year; group consensus on wrong place. [[sanctions-processing]] · [[ntt]] · [[open-questions#OQ-032]] |
| PCT rollout sponsor / business-case messaging | [[tech-tooling]] ([[will-bone]]) | [[data-quality-team]] ([[hugh-lobban]]) → [[dataops-team]] | 1 | Sponsor-level framing for the curator-facing change |
| Final-state Party contract definition | [[graph-team]] | [[devx-team]] ([[antonie-labuschagne]]) for [[inrisk-engine]] | End-state | DevX consumes final-state; not interim |
| Feature-tagging migration | [[graph-team]] | [[prebind-team]] | End-state | Direction unchanged; 2026-05-13 added party-tagging-vs-feature-tagging boundary, old-backend-stays-alive-past-cutover carve-out, and static-list-hypothesis investigation. [[feature-tagging-moves-to-inrisk]]; [[open-questions#OQ-019]] |
| Graph API consumer audit (spike) | — | — | 1 | [[joe-worsfold]] · [[billy-calladine]]; output feeds this map (expect new rows or confirms the list is complete) |
| D&B scheduled-refresh loop | [[graph-team]] | "enriched team" (TBD) | 1 | Cadence monthly/quarterly TBD; see [[d-and-b-caching-and-auto-parent]] |
| S&P enrichment path (end-state) | [[party-application]] | S&P API (direct) | End-state | Currently via DU (manual bulk); API offers built-in entity resolution |

---

## Notes
- "Proxy events" is the integration seam that makes Phase 1 possible — if it breaks, dual-running is forced and the programme gets dramatically harder.
- The Analytics Team schema-impact answer (from [[paul-rogers]]) is the **single biggest Phase-1 branch point** — one answer keeps the adapter simple, the other adds a cross-team API.
- The HV integration-shape conversation with [[simon-hulbert]] is second-highest leverage — defines what "good" looks like for the API contract MDM exposes. **Reframed 2026-05-19**: now also the [[artificial]] integration-shape conversation, since HV is Convex's implementation of Artificial.
- End-state arrows are aspirational; tracked here so they're not forgotten but explicitly **not** Phase 1 work.

## Source history
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-2]] — first substantive pass
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-1]] — added contract-buckets scaffolding; named Eclipse retirement, broker-variant event, spine-rewrite behaviour; explicit Phase-1 scope notes on backfill, feature-tagging, coupled rollout, bulk-migration CLI; end-state notes on broker-UID retirement, feature-tagging handover, S&P API; new cross-cutting rows for Graph-API audit spike, D&B scheduled refresh, S&P end-state
- 2026-04-22 — lint pass — **Knowledge Graph reclassified** as the internal Neo4j datastore inside [[party-application]]; no longer a separate data-distribution consumer on this map. Also added [[data-universe]] as the explicit platform owned by [[analytics-team]]; added [[eclipse]] as an application stub consumed by [[inrisk]].
- 2026-05-18 — [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — sanctions / Boomi / [[ntt]] expanded on the current-state diagram (orchestration flagged as wrong place); Phase-1 diagram updated to reflect InRisk-first cutover sequencing, two-component-libraries widget split, sanctions flow unchanged, additive client+broker table data model, cutover-window dual-write nuance; cross-cutting rows added for cutover sequencing, Joe's second component library, widget-response field alignment, and sanctions-domain ownership; Chakra V2/V3 row marked resolved; feature-tagging row note updated with the refined ADR framing.
- 2026-05-26 — [[sources/20260514-inrisk-high-level-refinement]] — Phase-1 diagram updated: data-model story specifies 3 tables (party + broker + party_snapshot) with UUID v7 + integer types; manual-client-creation cleanup added as a Phase-1 InRisk arrow; widget posture annotated as drop-in-replacement / parity-not-enhancement. Key Phase-1 changes list updated to reflect 5-story epic ordering + gating + parity posture. Cross-cutting rows added for: widget-integration spike (gates Stories 3/4/5); TOBA-status filter parity; InRisk-side backfill decision. Joe's-library row note refined with parity posture.
- 2026-05-26 — [[sources/20260519-party-integration-timelines]] — Phase-1 diagram: HV arrow re-labelled to clarify HV ≡ Convex-side delivery of [[artificial]]; **new Artificial arrow** for the read path + change-event push back (both via [[boomi]] gateway). [[boomi]] given consistent wiki-link treatment (was prose) across current-state and Phase-1 diagrams now that it has a first-class platform page. Key Phase-1 changes list updated. **Five new cross-cutting rows**: Artificial integration shape; Boomi connectivity for Artificial (Srini ↔ Joe pairing — the immediate unblock); Convex × Artificial fortnightly cadence + Slack channel; HV-side architecture diagrams; InRisk-cutover sequencing now-restated-to-Artificial note. End-state arrow for HV updated to HV / Artificial pair.
