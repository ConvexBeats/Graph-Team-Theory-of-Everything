---
type: application-architecture
title: Party Application — Current-state Architecture
created: 2026-04-22
updated: 2026-05-27
tags: [architecture, current-state]
application: party-application
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement, 20260519-party-integration-timelines, 20260519-mdm-implementation-strategy]
source_count: 6
status: draft
---

# Party Application — Current-state Architecture

_Living reference for the **as-is** technical shape of [[party-application]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: draft.** Scaffolding in place; substantive current-state detail added from [[joe-worsfold]]'s 2026-05-19 implementation walkthrough. Architecture is split into two coexisting realities: the **current production Party Application** (Neo4j-backed; the system the strangler is replacing) and the **new MDM under build** (Dynamo + OpenSearch; what's running in dev / integration today). Both are described below; the strangler keeps the production system stable while MDM displaces it under [[party-rearch-phase-1]].

> ⚠ **Contradiction recorded 2026-05-26** — earlier wiki passes described the current-state primary datastore as _"the Knowledge Graph (a Neo4j instance internal to party-application)"_. Per user clarification 2026-05-26 (during the [[sources/20260519-mdm-implementation-strategy]] ingest), this is incorrect: the current primary datastore is a **separate Neo4j instance internal to this application** (referred to in-call as "Party Graph" / "ParseDB"), and **[[knowledge-graph]]** is a different graph DB — also owned by [[graph-team]], holding InRisk + Party data as a read-replica, powering [[inrisk]]'s search functionality. See [[open-questions#OQ-010-R]] for the corrected identity question. References below have been updated.

## Summary
The Party Application is the authoritative store for business parties. **Current production state**: a Neo4j datastore inside this application's own infrastructure (informally "Party Graph" / "ParseDB"), emitting payload-bearing party events to downstream consumers ([[data-universe]], [[boomi]] for sanctions, [[inrisk]]). **MDM under build (target Phase-1 state)**: a Dynamo + OpenSearch backend, structured as a monorepo with domain-driven layering, exposing an **auto-generated REST API**, with a revision-based versioning system baked into the model. As of 2026-05-19, the backend core, REST API, curation UI, audit, and three iterations of backfill are working in dev; the integration environment is stood up but access-blocked on JumpCloud SAML Cognito ([[open-questions#OQ-042]]). The remaining backend work is the proxy-event design ([[open-questions#OQ-041]]) and the InRisk-parity widget rebuild (raw React + close-to-raw CSS, [[billy-calladine]]).

## Technologies & services

### Current production system (Neo4j-backed; what the strangler replaces)
- **Primary datastore**: **Neo4j** instance internal to this application (informally "Party Graph" / "ParseDB"), with Cypher query. **Distinct from [[knowledge-graph]]** (which is a sibling Graph-team-owned graph DB). In-scope for retirement as primary store under [[party-rearch-phase-1]]; see [[strangle-the-graph-via-proxy-events]].
- **Compute**: AWS Lambdas (per [[joe-worsfold]], 2026-05-13: _"basically just Lambdas running everything, including the UI"_).

### New MDM (Dynamo + OpenSearch — what's actually being built)
Snapshot of implementation state from [[sources/20260519-mdm-implementation-strategy]] (Joe's end-to-end walkthrough on 2026-05-19):

- **Codebase shape**: **monorepo, domain-driven** (DDD). Old graph's separate party-backend / curation-UI / party-search packages are unified, with a **domain area** that holds most of the core model. Joe: _"it's domain-driven development, you've got a domain area which actually has most of the core model."_
- **Primary datastore (target)**: **DynamoDB**.
- **Search index (target)**: **OpenSearch** — indexed differently from underlying Dynamo storage; field-alignment with InRisk's needs tracked at [[open-questions#OQ-036]].
- **API**: single **REST API**, auto-generated from code (Swagger-style document endpoint, currently RBAC-locked because the rest of the API is RBAC-locked, but hand-shareable). Joe can share the spec in **Swagger** or **Scala** flavour; a **YAML** version is with [[artificial]] (per [[sources/20260519-party-integration-timelines]]). **No GraphQL** — single REST API by design, reducing total surface for DevOps to support.
- **Backfill mechanism**: event-flow path writes **directly to the Dynamo repo, bypassing API validation**, by design — the API is strict on validation + schema; backfill needs to tolerate messy Graph-world data and fix it iteratively. Joe: _"I'd rather take in the data as is in GraphWorld, which isn't always perfect, and then fix it as we go."_ Acknowledged as a choice; could be hard-gated multi-step later. As of 2026-05-19: three iterations done; stable alpha expected Fri 2026-05-22.
- **Versioning**: revision-based — every party change is a revision (not just `created_at` / `updated_at`); full changelog over the party data. Built-in; no opt-out.
- **Compute**: lighter than ECS — _"the actual deployments and services themselves are way more lightweight because I'm not using ECS — should give us a lot more flexibility and more control."_ Specific runtime pending; Joe's slug suggests serverless / container-on-Fargate-or-Lambda.
- **AWS estate**: existing **3 accounts × 4 environments** (the shared current AWS world; **not** AWS 2.0). Confirmed 2026-05-13; reduces a cross-account integration unknown for [[inrisk]]'s Phase-1 work.
- **Environments**: dev (working; backfill iterating to stable alpha); integration (DynamoDB stood up but **access broken on JumpCloud SAML Cognito** — [[open-questions#OQ-042]]); higher envs follow.
- **Migration tooling (Dynamo)**: not Postgres-like — Dynamo doesn't accept big migration scripts. Joe wants to build a **self-service tool** eventually (shareable with [[dataops-team]]); Phase-1-vs-fast-follow scope open at [[open-questions#OQ-043]].
- **Widget surface (new MDM)**: two distinct component libraries, refined 2026-05-19:
  - **Chakra 3 + design-system widget** — consumed by [[party-curation-tool]] / [[dataops-team]]. PCT is standalone (no package-dependency clashes) so the modern stack stays.
  - **Bespoke widget for [[inrisk]] — raw React + close-to-raw CSS** (refined 2026-05-19 from "design-system-agnostic library"). Reuses existing InRisk components where possible. **Single component, three parameterised variants** — _parties_, _brokers_, and _other_ — each hitting a different OpenSearch endpoint and rendering different card / column shapes. Rendered inside a **shell container** ("the drawer that manifests in the in-risk world") so the variant is parameter-passed. Owned by [[billy-calladine]] (formalised 2026-05-19). **Parity-not-enhancement** posture per [[inrisk-cuts-over-before-high-volume]] — same auth / RBAC / session, same filter parameters (incl. **TOBA status**), same look-and-feel.
- **Event platform / message bus**: _Dynamo streams_ are the primary internal mechanism (every Dynamo change is streamed; Joe sits on top of those streams and classifies into events); shape/transport for the outbound to [[data-universe]] / [[boomi]] preserves the existing Graph event shape per [[strangle-the-graph-via-proxy-events]].
- **Observability**: **Datadog** (set up); alerts ticket pending. **Access logs**: streamed from **API Gateway → S3**, Athena-searchable. **Audit bucket**: Dynamo streams also chuck into an audit S3 bucket — _"searchable and shows what's changed every day."_ **Recovery**: Dynamo native **point-in-time recovery** (per-second rebuilds possible — _"bit like how Postgres does snapshots, but probably slightly faster because the document stores are lighter weight"_); plus re-stream from audit bucket as a fallback. **Tracking by email** for who-did-what (same as today).

## Touchpoints

### Inbound API / reads
- _unknown — pending Graph-API consumer audit spike ([[open-questions#OQ-004]])_

### Outbound API / writes
- _Mostly unknown — pending ingest._
- **Inbound write from [[boomi]] (sanctions result)**: per [[alex-sillars]] in [[sources/20260422-meeting-transcript-session-2]], when the sanctions check returns, Boomi _"updates our party with that"_ — i.e. writes the [[ntt]] sanctions decision onto the Party record. The **specific endpoint / event shape** Boomi uses for this write, and **which ID** it uses as the lookup key (legacy Graph party ID via the upcoming `display_id`, the UUID system ID, or other), are **not documented in any source we hold**. Tracked at [[open-questions#OQ-045]]. Phase-1 cutover transparency under [[strangle-the-graph-via-proxy-events]] + [[uuid-system-id-with-display-id]] depends on knowing this — the proxy can only keep Boomi working if MDM still accepts whatever Boomi sends.

### Event streams

#### Produced
**Three known event consumers** (per [[joe-worsfold]], [[sources/20260519-mdm-implementation-strategy]]):

- Party-change events → consumed by [[data-universe]] — payload-bearing today. **Note**: DU doesn't care about submission / requirement IDs in the event (it already has them from InRisk).
- Party-change events → consumed by [[high-volume]] (Phase-1 onward) — via [[boomi]] gateway.
- Party-change events → consumed by [[boomi]] for sanctions orchestration → [[inrisk]] → [[ntt]] — current call-chain on [[sanctions-processing]]. Boomi consumes the existing payload (with submission/requirement context) and builds its own composite of party + InRisk data before firing sanctions checks. Joe's framing: _"this weird triangle of data going backwards and forwards"_ — reinforces the "wrong place" group consensus on [[sanctions-processing]] · [[open-questions#OQ-032]].

Plus (also produced today; not in Joe's "three" because they're not the load-bearing critical-path consumers):
- Party-change events → consumed by [[inrisk]] — payload-bearing today; drives IR2-side synchronisation. _Note: this overlaps the Boomi/sanctions consumption — InRisk receives both direct party-change events and the Boomi-orchestrated sanctions context._
- _(Phase 1 onward)_ Party-change events → routed via [[boomi]] → [[artificial]] (mirrored party-state for the HV-underpinning vendor platform) — see [[high-volume-architecture]]. Phase-1 priority is the **read path** from Artificial; the event-push back is a later piece.

#### Consumed
- _unknown — pending ingest._ KG receives party data (it holds a read-replica of party + InRisk data per [[knowledge-graph]]) but the upstream feed mechanism (event-driven, batch, CDC) is not yet captured.

### Batch / scheduled integrations
- **Backfill jobs (MDM-side)**: event-flow-based; write directly to Dynamo, bypassing API validation. Iterative — three runs done by 2026-05-19; stable alpha targeted for Fri 2026-05-22. See Backfill mechanism above.
- D&B scheduled-refresh loop — refreshed records trigger curation revisions; cadence open ([[open-questions#OQ-011]]).

## Internal structure

Inherited from Joe's 2026-05-19 walkthrough — partial; pending deeper dive:

- **Monorepo with a domain area** holding most of the core model (DDD-structured). Old graph's separate party-backend / curation-UI / party-search packages unified under this layout.
- **Roles-vs-views model** in the domain layer (Joe: _"the same sort of shape where you've got roles and views"_). Roles = intrinsic to a party (broker, underwriter); views = ascribed by consumers (insured / reinsured) and fed back in. See [[roles-vs-views-litmus-test]].
- **Backend modules** (mapped from claims; not all explicitly named):
  - **Change classification** — turns Dynamo stream events into typed party-change events for the outbound integration surface.
  - **REST API surface** — auto-generated from code; carries the public contract for [[party-curation-tool]], [[inrisk]] (Phase-1 widget integration), [[high-volume]] / [[artificial]] (via [[boomi]]), and future direct consumers. Strict validation + schema enforcement (bypassed by the backfill path on purpose).
  - **Versioning** — every party update is a revision; the changelog is part of the read model.
  - **Proxy-event adapter** — the Phase-1 strangler seam that emits in the legacy Graph event shape. Open design fork at [[open-questions#OQ-041]] (whether to call an InRisk endpoint or pull from [[knowledge-graph]] for the InRisk-side context the proxy event needs to carry).
- **Curation UI** — fully functional in dev (cosmetic changes pending). See [[party-curation-tool-architecture]] for shape detail (revisions + filters, mark-for-review / approve / request-changes baked in).
- **Search widget** — one component with three parameterised variants (parties / brokers / other) inside a shell container. See Widget surface above.

## Observability & SLOs

Snapshot from [[sources/20260519-mdm-implementation-strategy]]:

- **Monitoring**: **Datadog** (set up); alerts ticket pending — Joe has the work item but hasn't built the alert pack yet.
- **Access logs**: streamed from **API Gateway → S3** (full access-log trail; **Athena-searchable**).
- **Change audit**: Dynamo streams → **audit S3 bucket** (searchable, day-by-day change view) — independent of the event emission to consumers. Joe: _"every single DynamoDB change gets streamed … I sit on top of that and classify those changes into events, but I also take those streams and chuck those into an audit bucket that's searchable."_
- **Tracking attribution**: by email (same pattern as the current Party Application).
- **Versioning**: revision-based — every party change has a full changelog on the data; not just `created_at` / `updated_at`.
- **Recovery**:
  - **Dynamo native point-in-time recovery** — per-second rebuilds possible. Joe: _"bit like how Postgres does snapshots, but probably slightly faster because the document stores are lighter weight."_
  - **Re-stream from audit bucket** — Joe: _"because I'm streaming all of my database updates into an audit file, I can just rerun those — there might be instructions to do that, actually, to re-stream."_ Backup fallback if Dynamo PITR is insufficient.
- **SLOs**: not captured; pending later ingest.

## Known constraints / rough edges
_From transcripts:_

- **Payload events, not reference events** — downstream consumers snapshot-cache party payloads rather than resolving by ID + version. This is the constraint the [[party-rearch]] project exists to remove. See [[contract-buckets]] "data distribution" bucket.
- **Graph API surface** — current consumers of the Knowledge-Graph API are incompletely mapped; a spike is open to audit endpoint-by-consumer ([[open-questions#OQ-004]]).
- **[[eclipse]] ingestion** — third-party data flows from [[eclipse]] into the Party Application are under scope review ([[open-questions#OQ-001]]).
- **Spine-rewrite-on-every-event drives sanctions noise** — every InRisk event causes Graph to rewrite the whole spine and emit one event; [[boomi]] consumes those events to drive [[sanctions-processing]] checks, which produces noise on irrelevant changes. Not a Phase-1 fix; [[open-questions#OQ-032]].
- **API spec is subject to change until 1 Sep go-live** ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]: _"won't change massively, but there may be some additions or deletions from it"_). Now that the YAML spec is in [[artificial]]'s hands ahead of the 2026-05-20 kick-off, every new consumer needs to be onboarded with this caveat explicit. Mirrors the InRisk-side iteration expected from the widget-integration spike.
- **Curation feature extension, Neo4j → DynamoDB data migration, and DU/sanctions event-emission completeness are still in flight** ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]). The front-of-house MDM API is the most mature surface today; backend completeness is the remaining critical-path work for Phase 1.
- **Integration environment access is broken** ([[joe-worsfold]], [[sources/20260519-mdm-implementation-strategy]]) — DynamoDB is stood up but JumpCloud SAML Cognito access is failing, so no one can actually do anything with it. Open: [[open-questions#OQ-042]]. Blocks the natural integration-env smoke-test path that [[artificial]] would want to use first.
- **Dynamo migration tooling is non-trivial** ([[joe-worsfold]], [[sources/20260519-mdm-implementation-strategy]]) — Dynamo doesn't support Postgres-style script-based migrations. Joe wants a self-service tool eventually (shareable with [[dataops-team]]); Phase-1 scope open at [[open-questions#OQ-043]]. Known forward-looking constraint on how easy subsequent schema changes will be.
- **Backfill bypasses API validation by design** — the event-flow path writes directly to the Dynamo repo, bypassing the API's strict validation + schema enforcement, to tolerate messy Graph-world data and fix iteratively. Explicit design choice (not an oversight); could be hard-gated multi-step later if a future requirement demands it.
- **Search widget — three variants in one parameterised component** — the widget shell hosts three card-rendering variants (parties / brokers / other), each hitting a different OpenSearch endpoint. Today's InRisk widget is a table+columns; new widget defaults to cards (Joe's POC choice via Cursor). Parity-rebuild (raw React + close-to-raw CSS) is owned by [[billy-calladine]] (formalised 2026-05-19). InRisk-side dispatch code (the "which widget to call" logic) also needs the corresponding update.
- **Proxy-event design has an open InRisk-data-source fork** — for the Phase-1 proxy event to carry the submission/requirement context Boomi/DU expect, MDM either calls an **InRisk endpoint** (Joe's preference; real-time consistency) or pulls from **[[knowledge-graph]]** (eventually consistent; Joe + Alex parked over consistency concerns). Decision deferred to the sanctions session with [[simon-hulbert]]; gates the proxy adapter. See [[open-questions#OQ-041]] and [[strangle-the-graph-via-proxy-events]].
- **Boomi sanctions write-back contract is undocumented** — [[boomi]] writes the [[ntt]] result back into Party (_"we update our party with that"_, [[alex-sillars]]), but the endpoint / event shape and ID Boomi uses aren't captured anywhere we hold. Symmetric to OQ-041 (which covers Boomi's _inbound_ proxy-event shape) — this covers Boomi's _outbound_ write into Party. Tracked at [[open-questions#OQ-045]]. Phase-1 cutover transparency depends on this.

## Relationships to other architecture pages
- [[party-curation-tool-architecture]] — PCT is the primary curation frontend for parties; tightly coupled data model
- [[inrisk-architecture]] — downstream consumer; currently receives party payloads; widget consumer via new InRisk-bespoke library (raw React)
- [[knowledge-graph]] — sibling Graph-team-owned graph DB (read-replica of InRisk + Party data; powers InRisk search). **Not** this application's internal datastore (correction recorded 2026-05-26)
- _([[data-universe]] is a platform rather than an in-scope application; its consumption model is captured in [[party-rearch-dependency-map]])_

## Phase-target pages that delta from this one
_Populate as targets are created._

- _(none yet — [[party-rearch-phase-1-party-application-architecture]] to be created when Phase-1 target detail is available)_

## Superseded claims
_None yet — this page has not yet been through a phase-completion fold-in._

## Related
- [[party-application]] — identity page (owner, role, aliases)
- [[party-rearch-dependency-map]] — cross-app view of touchpoints
- [[strangle-the-graph-via-proxy-events]] — the migration strategy for moving off Neo4j; refined 2026-05-13 with cutover-window dual-write
- [[inrisk-cuts-over-before-high-volume]] — InRisk-first cutover; two component-library widget call
- [[uuid-system-id-with-display-id]] — identifier scheme being adopted in the re-architecture
- [[sanctions-processing]] · [[ntt]] — sanctions touchpoint

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — partial current-state framing in the three-bucket contract discussion
- [[sources/20260422-meeting-transcript-session-2]] — mentions of Neo4j-as-primary, payload events, downstream consumer list
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — AWS estate (existing 3-account / 4-env world; not AWS 2.0); Lambdas confirmed; two component libraries; cutover-window dual-write
- [[sources/20260514-inrisk-high-level-refinement]] — second-library widget posture confirmed as parity-not-enhancement; TOBA-status filter parity requirement
- [[sources/20260519-party-integration-timelines]] — [[artificial]] confirmed as a second new Phase-1 outbound consumer (via [[boomi]] gateway); API-spec instability surfaced as an operational constraint; backend-completeness call-out (curation, Neo4j → Dynamo migration, DU/sanctions event emission)
- [[sources/20260519-mdm-implementation-strategy]] — load-bearing current-state snapshot: monorepo / DDD; auto-generated REST API; revision-based versioning; curation UI dev-functional; widget = single component with three parameterised variants (raw React + close-to-raw CSS, owned by [[billy-calladine]]); Dynamo + OpenSearch in dev + integration (integration broken on JumpCloud SAML Cognito); backfill bypass-the-API-by-design; full audit/access-log/recovery stack (Datadog + API Gateway → S3 + Dynamo streams → audit bucket + PITR); Dynamo migration tooling Phase-1 scope open. **Knowledge Graph correction** — [[knowledge-graph]] is a sibling Graph-team-owned application, **not** the internal Neo4j of this application
