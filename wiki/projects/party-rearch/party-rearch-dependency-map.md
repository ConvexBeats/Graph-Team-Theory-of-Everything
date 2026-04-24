---
type: analysis
title: Party Re-Architecture — Dependency Map
created: 2026-04-22
updated: 2026-04-22
tags: [analysis, standing, dependencies]
project: party-rearch
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Party Re-Architecture — Dependency Map

A standing analysis of inter-application dependencies for the Party Application re-architecture programme. Current-state vs target-state, called out separately. Refreshed on every ingest.

## Contract buckets (Session 1 scaffolding)

Session 1 introduced a three-bucket framing for every Party-consumer contract. This is the lens the rest of this map is read through:

- **Operational** — how parties get created / curated: the search widget, manual creation via application, manual creation via [[party-curation-tool]], CUO groupings, the PCT UI itself.
- **Data distribution** — who reads Party events: [[data-universe]] (owned by [[analytics-team]]), party-versioning in [[inrisk]] + Artificial, Launchpad, party reporting, EVA, broker dashboards.
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
      ├─── (R)           ────►  sanctions via Boomi → NTT → back round-trip
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

---

## Phase 1 target state (this programme)

```
[[party-application]] (new MDM: DynamoDB + OpenSearch)
      │
      ├─── (E, proxy — same shape as current)  ────►  [[analytics-team]]   [Phase 1]
      │         └── see [[strangle-the-graph-via-proxy-events]]
      │
      ├─── (R, ID+version)  ────►  [[inrisk]] (IR2)      [Phase 1 — InRisk-side changes required]
      │         ├── InRisk stores Party ID on its party table
      │         ├── Outgoing InRisk messages include Party ID
      │         └── Broker retrieval moves to direct-from-InRisk (URD path removed)
      │
      ├─── (R, API via Boomi)  ────►  [[high-volume]]     [Phase 1]
      │         └── no widget; API-only
      │
      ├─── (API)   ◄──►   [[party-curation-tool]] (new, Next.js on open-API spec)  [Phase 1]
      │         └── co-ships with MDM — [[pct-and-mdm-go-live-together]]
      │
      ├─── (ingest+cache)  ◄────  D&B (Dynamo-cached, TTL; auto-create parent)   [Phase 1]
      └─── (ingest)         ◄────  S&P  via  [[analytics-team]] (still manual)  [Phase 1]

[[inrisk]] ── (E, now with party-ID) ──►  downstream consumers
Graph DB — read-only during stabilisation, decommissioned post-cutover.
```

Key Phase-1 changes:
- **DU sees no change** on cutover — proxy events in Graph shape.
- **InRisk** gets 3 discrete code changes (party-ID storage, messaging, broker retrieval).
- **PCT widget** potentially embeds in [[inrisk]] (Chakra V2/V3 question deferred).
- **HV** integrates via Boomi API — widget question does not block HV.
- **No historical backfill into MDM** — per [[no-historic-client-backfill-into-mdm]], MDM carries version history from cutover forward; pre-cutover per-client-ID InRisk history stays in [[inrisk]] / [[data-universe]].
- **Eclipse ingestion removed** — no live consumer; already resolved via InRisk-client-ID path.
- **Feature-tagging table** — stays in current Postgres + widget shape (unchanged); in-session scope call defers its migration to Phase 2.
- **Single-flag coupled rollout** — old-search→new-search and old-PCT→new-PCT share one cutover flag.
- **Bulk-migration tool** — MDM-team-owned CLI (CSV → diff preview → auto-approved revision); full self-serve UX is Phase 2+.

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
      ├─── (R, API via Boomi)  ────►  [[high-volume]]
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
| HV Party-integration shape | [[tech-tooling]] ([[rory-beattie]]) · [[graph-team]] | [[high-volume-team]] ([[simon-hulbert]]) | 1 | 1 Sep is fixed; integration shape still being specified |
| Chakra V2/V3 widget decision | [[graph-team]] · [[tech-tooling]] | [[prebind-team]] | 1 | Workshop with InRisk in PCT context |
| PCT rollout sponsor / business-case messaging | [[tech-tooling]] ([[will-bone]]) | [[data-quality-team]] ([[hugh-lobban]]) → [[dataops-team]] | 1 | Sponsor-level framing for the curator-facing change |
| Final-state Party contract definition | [[graph-team]] | [[devx-team]] ([[antonie-labuschagne]]) for [[inrisk-engine]] | End-state | DevX consumes final-state; not interim |
| Feature-tagging migration | [[graph-team]] | [[prebind-team]] | End-state | Gated on InRisk buy-in; formal handover of both Postgres table + widget component |
| Graph API consumer audit (spike) | — | — | 1 | [[joe-worsfold]] · [[billy-calladine]]; output feeds this map (expect new rows or confirms the list is complete) |
| D&B scheduled-refresh loop | [[graph-team]] | "enriched team" (TBD) | 1 | Cadence monthly/quarterly TBD; see [[d-and-b-caching-and-auto-parent]] |
| S&P enrichment path (end-state) | [[party-application]] | S&P API (direct) | End-state | Currently via DU (manual bulk); API offers built-in entity resolution |

---

## Notes
- "Proxy events" is the integration seam that makes Phase 1 possible — if it breaks, dual-running is forced and the programme gets dramatically harder.
- The Analytics Team schema-impact answer (from [[paul-rogers]]) is the **single biggest Phase-1 branch point** — one answer keeps the adapter simple, the other adds a cross-team API.
- The HV integration-shape conversation with [[simon-hulbert]] is second-highest leverage — defines what "good" looks like for the API contract MDM exposes.
- End-state arrows are aspirational; tracked here so they're not forgotten but explicitly **not** Phase 1 work.

## Source history
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-2]] — first substantive pass
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-1]] — added contract-buckets scaffolding; named Eclipse retirement, broker-variant event, spine-rewrite behaviour; explicit Phase-1 scope notes on backfill, feature-tagging, coupled rollout, bulk-migration CLI; end-state notes on broker-UID retirement, feature-tagging handover, S&P API; new cross-cutting rows for Graph-API audit spike, D&B scheduled refresh, S&P end-state
- 2026-04-22 — lint pass — **Knowledge Graph reclassified** as the internal Neo4j datastore inside [[party-application]]; no longer a separate data-distribution consumer on this map. Also added [[data-universe]] as the explicit platform owned by [[analytics-team]]; added [[eclipse]] as an application stub consumed by [[inrisk]].
