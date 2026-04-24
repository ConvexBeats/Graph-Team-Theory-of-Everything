---
type: application
title: Party Application
aliases: [graph, party-mdm, mdm]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, core]
owner: graph-team
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Party Application

## Summary
The authoritative store and routing service for **business parties** (clients, brokers, underwriters, etc.) consumed across the insurance-deal lifecycle. Today: **Neo4j / Cypher** graph database ("Party Graph" / MDM / Graph) emitting full party payloads to downstream consumers. Target: **DynamoDB + OpenSearch** store ("new MDM") handing out a party **ID + version** reference, with consumers resolving details near-real-time. Migration is via the strangler pattern — see [[strangle-the-graph-via-proxy-events]].

## Aliases / previous names
- Graph / Party Graph
- Party MDM · MDM
- MDM (in this programme, "MDM" now specifically means the **new** architecture; "Graph" means the current Neo4j system)

## Purpose
Provide version-controlled party records to every application that needs to display, route to, or transact against a business party. A single party can take multiple **roles** (client, broker, underwriter, reinsured, cover holder, claims, etc.) across deal contexts.

## Current state

- **Owner**: [[graph-team]]
- **Production status**: in production
- **Tech stack**: **Neo4j** (graph DB) with **Cypher** query; large-payload party events emitted to the [[data-universe]] (owned by [[analytics-team]]) which indexes into Snowflake
- **Primary datastore (current state)**: the **Knowledge Graph** — a **Neo4j** instance sitting inside the Party Application infrastructure. It is not a separate application or downstream consumer; it is the graph database behind the current-state Party Application. The target-state re-architecture replaces it with DynamoDB + OpenSearch.
- **Integration shape**: downstream systems receive full party payloads and **snapshot-cache** locally
- **Key consumers (known)**: [[inrisk]] (snapshot-cached payloads), [[party-curation-tool]] (read/write), [[data-universe]] (events, consumed by [[analytics-team]]), sanctions flow via Boomi → NTT, D&B enrichment pipeline
- **Key interfaces**: payload-shape party events to DU; some direct APIs for sanctions and related consumers; a URD-based broker retrieval path that InRisk uses (to be removed in target state)
- **Event-emit behaviour (Session 1)**: every InRisk event causes the Knowledge Graph (Neo4j) to **rewrite the whole spine and emit one spine-rewrite event**, regardless of whether any field actually changed. Effectively a single event shape flows to the [[data-universe]] (plus a broker-specific variant on the common bus). This is both the current flakiness _and_ the reason the strangler adapter is a small job — it's a one-event mapper, not an N-event migration.
- **Eclipse ingestion**: present in current-state docs but with **no live consumers** (data has not been refreshed for months; Eclipse IDs are already resolvable via the InRisk-client-ID path). Retired in the new world — see Change items.
- **Party role support (today)**: limited — users can create *road code* parties in a complicated way; cannot create reinsured, cover holder, or claims parties without going to another team
- **Search**: Cypher queries over Neo4j nodes — acknowledged as not ideal for fuzzy / misspelling-tolerant matching

## Target state

- **Owner**: [[graph-team]] (unchanged)
- **Tech stack**: **DynamoDB** (party records, views, cache of D&B data) + **OpenSearch** (discoverability, fuzzy matching)
- **Integration shape**: consumers receive a **party ID + version**; they call back near-real-time with any of:
  - `get by ID + version` — pinned snapshot
  - `get by ID + timestamp` — as-of
  - `get by ID` — latest
  Deterministic per what the consumer asks — each consumer decides which mode suits its use case.
- **IDs**: **system ID is a UUID**; a **display ID** field carries the legacy human-readable Graph party ID. Display ID preserved through migration so users continue to see familiar numbers.
- **Party roles**: generic role schema; any role can be attached to any party via the standard integration surface (no more per-role gating).
- **Search**: OpenSearch-powered; improves discoverability and reduces resolution/creation workload.
- **Event emission (interim)**: proxy events in the Graph event shape, emitted to the DU — see [[strangle-the-graph-via-proxy-events]].
- **Event emission (end state)**: direct stream from Dynamo to the DU (Snowpipe-style). Requires DU buy-in — out of Phase 1.

## Transitional states

From [[sources/20260422-meeting-transcript-session-2]]:

1. **Intercept** — from a defined point, writes that would land in Graph DB are diverted to MDM (or a precursor), keeping MDM current during the backfill window. _(Owner: [[alex-sillars]], via ticketised work for [[ben-joseph]] and [[joe-worsfold]].)_
2. **100% backfill** — historical Graph DB state is migrated into MDM, with legacy Graph party IDs preserved as display IDs. Parent / ultimate-parent links reconstructed in a second pass using the legacy ID.
3. **Proxy adapter + projections** — MDM emits events to the DU in the existing Graph event shape. Acceptance criterion (per [[ben-joseph]]): bidirectional mappability — event → MDM record → event must round-trip exactly.
4. **PCT + MDM hard cutover event** — single cutover for the event stream; a **2-day crossover** period for PCT (old PCT and new PCT operating alongside briefly). See [[pct-and-mdm-go-live-together]].
5. **Post-cutover**: Graph DB retires. Feature-tagging stays on the current feature-tech side until InRisk buys in — not in Phase 1.
6. **End-state cleanup (later phase)**: proxy events replaced by direct Dynamo → DU stream; client ID removed from the party-consumer path in favour of party ID + version on submission.

## Change ownership
- **Working unit**: [[graph-team]]
- **Key contributors**: [[alex-sillars]] (PO, design lead), [[joe-worsfold]] (Tech Lead, integration surface), [[ben-joseph]] (Engineer, event shapes + backfill)
- **Oversight**: [[tech-tooling]] ([[rory-beattie]], [[will-bone]])
- **Primary cross-team dependencies**: [[prebind-team]] (InRisk interim-state changes), [[analytics-team]] (schema-impact check), [[dataops-team]] (change management for PCT), the still-unknown HV owning team

## Change items recorded in this session

- [[strangle-the-graph-via-proxy-events]] (**accepted**) — proxy events strategy; **origin** in Session 1 (morning DU-events segment), **consolidated** in Session 2
- [[pct-and-mdm-go-live-together]] (**accepted**) — co-delivery of MDM + PCT; in Session 1 reinforced as the **single feature-flag cutover** for old-search→new-search _and_ old-PCT→new-PCT
- [[no-historic-client-backfill-into-mdm]] (**accepted, new in Session 1**) — MDM history starts at cutover; pre-cutover InRisk client-ID-scoped history stays in InRisk / [[data-universe]]
- **Flatten `(party-id, submission-id)` rows in MDM** (proposed, pending DU check): fall back to an InRisk API for nested reconstruction if DU cannot tolerate the flatter shape
- **UUID system ID + Graph-ID display ID** (accepted in-session) — see [[uuid-system-id-with-display-id]]
- **D&B enrichment pattern simplified** (accepted): cache D&B in Dynamo with TTL; on a find, auto-create draft + ultimate-parent with full provenance (replaces double-gated curation). **Session 1 update**: refresh is driven by a **scheduled job** (monthly / quarterly — cadence TBD) that re-calls D&B and spawns a curation revision if the record has changed. Implementation note on [[d-and-b-caching-and-auto-parent]].
- **S&P ingestion remains manual** through DU for Phase 1 (accepted); revisit source-of-truth question in a later phase. **Session 1 note**: S&P also offers a direct API that internally performs entity resolution — a future replacement for the DU-stored path once Party is the source of truth.
- **One Re + CUO groupings** remain manual-in-PCT for Phase 1 (accepted); rules-based auto-grouping deferred
- **Eclipse enrichment — open scope call** (Session 1): no current consumer found in-room, but confirmation pending the Graph-API audit spike. Tracked at [[open-questions#OQ-001]].
- [[feature-tagging-moves-to-inrisk]] (**accepted**, promoted to ADR in Session 1 Pass B): Phase 1 no change; Phase 2+ migrate Postgres table + widget component to [[inrisk]] / [[prebind-team]] ownership.
- [[bulk-migrations-owned-by-mdm-phase-1]] (**accepted**, promoted to ADR in Session 1 Pass B): CLI-first in Phase 1; full self-serve UX is Phase 2+.
- [[no-pct-audit-backfill]] (**accepted**, promoted to ADR in Session 1 Pass B): the PCT-side analog of [[no-historic-client-backfill-into-mdm]].
- **Sanctions flow simplification** (future, out of Phase 1): NTT result lands on party; Party informs InRisk of affected submissions directly. **Session 1 detail**: the new PCT's `revision.proposed` event triggers a worker that raises the sanctions check; sanction status is stamped **on the version** of the party (GitHub-commit-verified analogy).
- **Chakra V2 vs V3 widget strategy** (deferred, pending InRisk workshop)
- **No GraphQL facade in target state** (accepted, new in Session 1): single API, single Dynamo-backed store, OpenSearch for discoverability — fewer technologies for DevOps to support. Joe: _"there's no graph file, just as a heads up — everything's in one API, it's a lot simpler."_
- **Roles vs. views litmus test** (accepted, new in Session 1): _"if every policy was deleted from this party, would it still be insured? No. Would it still be a broker? Yes."_ Roles are intrinsic to the party; views (insured/reinsured) are ascribed by consumers and fed back into MDM to maintain DU projection shape.

## Pending / unknown

All Party-Application-facing open items are tracked in [[open-questions]]:
- [[open-questions#OQ-002]] — DU tolerance for the flattened schema. Single biggest Phase-1 branch point.
- [[open-questions#OQ-003]] — whether any consumer beyond [[inrisk]] needs the nested client-ID shape.
- [[open-questions#OQ-004]] — Graph API consumer audit spike (expected to surface any hidden consumers, including Eclipse).
- [[open-questions#OQ-007]] — version-change event semantics (push vs pull).
- [[open-questions#OQ-008]] — end-state sanctions flow owner/timing.
- [[open-questions#OQ-011]] — D&B scheduled-refresh cadence.
- [[open-questions#OQ-012]] — "enriched team" identity.
- [[open-questions#OQ-013]] — HV integration-shape specifics.
- [[open-questions#OQ-017]] — MDM delivery squad shape.
- [[open-questions#OQ-018]] — final-state Party contract specification.

## Related decisions
- [[strangle-the-graph-via-proxy-events]]
- [[pct-and-mdm-go-live-together]]
- [[no-historic-client-backfill-into-mdm]]
- [[no-pct-audit-backfill]]
- [[d-and-b-caching-and-auto-parent]]
- [[uuid-system-id-with-display-id]]
- [[feature-tagging-moves-to-inrisk]]
- [[bulk-migrations-owned-by-mdm-phase-1]]

## Related
- [[party-curation-tool]] — human-facing companion; co-ships
- [[inrisk]] — interim-state consumer with 3 required changes
- [[inrisk-engine]] — targets the final-state contract; not part of interim work
- [[high-volume]] — future consumer; forcing-function for the end-state timeline
- [[graph-team]] — owner
- [[analytics-team]] — event consumer
- [[ownership-matrix]] — workstreams and open actions
- [[dependency-map]] — current vs target contracts
- [[open-questions]] — open items affecting this application
- [[contract-buckets]] — the framework used to organise all of this application's consumer contracts

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; three-bucket contract framing, strangler invention, Eclipse retirement, feature-tagging scope, bulk-migration shape, team-size tension
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; consolidation and promotion of ADRs
