---
type: application
title: Party Application
aliases: [graph, party-mdm, mdm]
created: 2026-04-22
updated: 2026-05-26
tags: [application, in-scope, core]
owner: graph-team
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement, 20260519-party-integration-timelines]
source_count: 5
status: draft
projects: [party-rearch]
---

# Party Application

> **Architecture:** detailed current-state tech, touchpoints, and constraints live on [[party-application-architecture]]. Phase-target shapes live under [[party-rearch]] as `party-rearch-<phase-id>-party-application-architecture` pages (created when targets are authored). This identity page covers role-in-portfolio, ownership, and cross-project change items; the architecture page is the place to go for technical detail.

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
- **Key consumers (known)**: [[inrisk]] (snapshot-cached payloads), [[party-curation-tool]] (read/write), [[data-universe]] (events, consumed by [[analytics-team]]), sanctions flow via [[boomi]] → [[ntt]] (see [[sanctions-processing]]), D&B enrichment pipeline. **New Phase-1 consumers**: [[high-volume]] (Convex's implementation of the [[artificial]] vendor platform) — API-only via [[boomi]]; and [[artificial]] itself (the underlying third-party platform that HV is built on) — also via [[boomi]] as gateway. Both onboard at the 1 Sep gate, after the InRisk-first cutover window ([[inrisk-cuts-over-before-high-volume]]).
- **Key interfaces**: payload-shape party events to DU; some direct APIs for sanctions and related consumers; a URD-based broker retrieval path that InRisk uses (to be removed in target state)
- **Event-emit behaviour (Session 1)**: every InRisk event causes the Knowledge Graph (Neo4j) to **rewrite the whole spine and emit one spine-rewrite event**, regardless of whether any field actually changed. Effectively a single event shape flows to the [[data-universe]] (plus a broker-specific variant on the common bus). This is both the current flakiness _and_ the reason the strangler adapter is a small job — it's a one-event mapper, not an N-event migration.
- **Spine-rewrite cascade on sanctions** (2026-05-13 follow-up): the spine-rewrite-on-every-InRisk-event behaviour is the upstream of [[sanctions-processing]]'s false-positive problem — Boomi fires sanctions checks on every party-change event whether or not it's relevant, and InRisk feels the resulting noise. Not a Phase-1 fix; flagged as a future driver for sanctions-domain redesign ([[open-questions#OQ-032]]).
- **Eclipse ingestion**: present in current-state docs but with **no live consumers** (data has not been refreshed for months; Eclipse IDs are already resolvable via the InRisk-client-ID path). Retired in the new world — see Change items.
- **Party role support (today)**: limited — users can create *road code* parties in a complicated way; cannot create reinsured, cover holder, or claims parties without going to another team
- **Search**: Cypher queries over Neo4j nodes — acknowledged as not ideal for fuzzy / misspelling-tolerant matching

## Target state

- **Owner**: [[graph-team]] (unchanged)
- **Tech stack**: **DynamoDB** (party records, views, cache of D&B data) + **OpenSearch** (discoverability, fuzzy matching)
- **Cloud estate (Phase-1 confirmation, 2026-05-13)**: MDM runs on the **existing 3-account × 4-environment AWS estate** (not AWS 2.0 — Joe would have preferred AWS 2.0 but it's not ready). Removes a potential cross-account integration unknown for [[inrisk]]'s Phase-1 work.
- **Widget surface — two component libraries (2026-05-13; parity posture confirmed 2026-05-14)**:
  - The **Chakra-3-with-design-system widget** Joe has already published — consumed by [[party-curation-tool]] / [[dataops-team]].
  - A second, **design-system-agnostic component library** matched to [[inrisk]]'s current look — to be published by [[joe-worsfold]] for [[inrisk]] to consume. **Drop-in-replacement / parity-not-enhancement** on this library (refinement 2026-05-14): same auth / RBAC / session flow as InRisk's current widget, same filter parameters (incl. **TOBA status** for the Lloyd's-vs-retail broker workflow), same look-and-feel. UX improvements deferred to a separate change post-cutover. See [[inrisk-cuts-over-before-high-volume]] for the rationale.
  - [[high-volume]] doesn't use a widget — API-only via Boomi.
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
- **Sanctions flow simplification** (future, out of Phase 1): the long-term direction sketched in Session 2 was that NTT result lands on the party version directly; Party informs InRisk of affected submissions. **2026-05-13 follow-up** broadened the framing: the orchestration that today lives in Boomi (firing on every party change, calling [[inrisk]] one submission at a time, building its own cache + idempotency layer) is in the wrong place — it should be its own domain, not absorbed into Party, InRisk, or Boomi. Audit pressure is significant this year. See [[sanctions-processing]] for the domain framing, [[ntt]] for the vendor platform, [[open-questions#OQ-032]] for the open ownership question, and [[open-questions#OQ-008]] for the end-state-flow question. _Session 1 detail kept for reference: the new PCT's `revision.proposed` event triggers a worker that raises the sanctions check; sanction status is stamped on the version (GitHub-commit-verified analogy)._
- **Chakra V2 vs V3 widget strategy** (resolved 2026-05-13): two-library approach — Joe maintains a Chakra-3-plus-design-system widget for [[party-curation-tool]] and a design-system-agnostic library matched to InRisk's current look. Resolution lives on [[inrisk-cuts-over-before-high-volume]]; closes [[open-questions#OQ-005]].
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
- [[open-questions#OQ-032]] — sanctions-domain location (orchestration off Boomi). New 2026-05-13.
- [[open-questions#OQ-035]] — concrete InRisk MDM-cutover date (≥ 2 weeks before 1 Sep).
- [[open-questions#OQ-036]] — widget-response field alignment (Sergiu's question); widget-integration spike is the resolution path.

**Pending decision (not an OQ per user steer, flagged 2026-05-14)** — _InRisk-side backfill of MDM ID columns on existing client / broker / party-snapshot rows_. Story 2 of the InRisk Phase-1 epic adds `party_id` (UUID v7) + `version_id` (int) to three InRisk tables. For pre-cutover rows: backfill with known MDM party-IDs, or null + go-forward from cutover? Mirror of [[no-historic-client-backfill-into-mdm]] on the consumer side; sanctions is the principal impact surface. Owners: [[joe-worsfold]] · [[john-trahearn]]; to be decided ahead of Story 2 low-level (2026-05-19). Tracked also on [[inrisk]] and [[party-rearch-phase-1]].

**API-spec instability caveat (operational, not an OQ, 2026-05-19)** — the new MDM API spec is **subject to change until 1 Sep go-live**, but per [[alex-sillars]] _"won't change massively, but there may be some additions or deletions from it."_ Now that [[artificial]] is consuming the spec for kick-off (received [[joe-worsfold]]'s YAML pre-2026-05-19; first kick-off call 2026-05-20), this caveat needs to be communicated to every new consumer explicitly. Captured here so it shows up on Party-side coordination touchpoints. See [[sources/20260519-party-integration-timelines]] claims #2 + #11.

## Related decisions
- [[strangle-the-graph-via-proxy-events]] (refined 2026-05-13 with cutover-window dual-write nuance)
- [[inrisk-cuts-over-before-high-volume]] (new 2026-05-13 — cutover sequencing + two-library widget call)
- [[pct-and-mdm-go-live-together]]
- [[no-historic-client-backfill-into-mdm]]
- [[no-pct-audit-backfill]]
- [[d-and-b-caching-and-auto-parent]]
- [[uuid-system-id-with-display-id]]
- [[feature-tagging-moves-to-inrisk]] (refined 2026-05-13 — party-tagging-vs-feature-tagging boundary; static-list investigation)
- [[bulk-migrations-owned-by-mdm-phase-1]]

## Related
- [[party-application-architecture]] — detailed current-state architecture (tech, touchpoints, constraints)
- [[party-curation-tool]] — human-facing companion; co-ships
- [[inrisk]] — interim-state consumer with 3 required changes
- [[inrisk-engine]] — targets the final-state contract; not part of interim work
- [[high-volume]] — future consumer; forcing-function for the end-state timeline
- [[graph-team]] — owner
- [[analytics-team]] — event consumer
- [[party-rearch-ownership-matrix]] — workstreams and open actions
- [[party-rearch-dependency-map]] — current vs target contracts
- [[open-questions]] — open items affecting this application
- [[contract-buckets]] — the framework used to organise all of this application's consumer contracts

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; three-bucket contract framing, strangler invention, Eclipse retirement, feature-tagging scope, bulk-migration shape, team-size tension
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; consolidation and promotion of ADRs
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — cutover-window dual-write nuance; AWS estate confirmation; two-library widget call; sanctions / Boomi / NTT framing; party-tagging vs feature-tagging boundary
- [[sources/20260514-inrisk-high-level-refinement]] — InRisk widget posture refined to drop-in-replacement / parity-not-enhancement; TOBA-status filter parity surfaced; InRisk-side backfill question raised at the Party side
- [[sources/20260519-party-integration-timelines]] — [[artificial]] added as a second new Phase-1 Party consumer (alongside HV — which is itself Convex's implementation of Artificial); [[boomi]] gateway role confirmed for both; YAML spec already shared with Artificial; API-spec-subject-to-change caveat surfaced; strangler-pattern stability promise restated to a customer
