---
type: application-architecture
title: Party Application — Current-state Architecture
created: 2026-04-22
updated: 2026-05-26
tags: [architecture, current-state]
application: party-application
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement, 20260519-party-integration-timelines]
source_count: 5
status: stub
---

# Party Application — Current-state Architecture

_Living reference for the **as-is** technical shape of [[party-application]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** Scaffolding in place, ready to be populated from forthcoming raw-folder ingests. Everything currently on this page is either (a) a confirmed detail from Sessions 1 & 2 or (b) a placeholder awaiting authoritative source material.

## Summary
_Populated at first architecture ingest. From transcripts so far: the Party Application is the authoritative store for business parties; currently **Neo4j Knowledge Graph is the primary datastore**, with payloads (not IDs) pushed to downstream consumers. The system is being re-architected to a **DynamoDB + OpenSearch** backend under the [[party-rearch]] project (see [[party-rearch-phase-1-party-application-architecture]] once filed)._

## Technologies & services
_Populate from raw ingest. Known so far:_

- **Primary datastore**: **Neo4j (Knowledge Graph)** — in-scope for retirement as primary store under [[party-rearch-phase-1]]; see [[strangle-the-graph-via-proxy-events]].
- **Secondary stores / caches**: _unknown — pending ingest_
- **Compute**: AWS Lambdas (per [[joe-worsfold]], 2026-05-13: _"basically just Lambdas running everything, including the UI"_). Other compute pending ingest.
- **AWS estate**: existing **3 accounts × 4 environments** (the shared current AWS world; **not** AWS 2.0 yet). Confirmed 2026-05-13; reduces a cross-account integration unknown for [[inrisk]]'s Phase-1 work.
- **Widget surface (new MDM)**: two component libraries published from the [[graph-team]] side:
  - **Chakra 3 + design-system widget** — consumed by [[party-curation-tool]] / [[dataops-team]].
  - **Design-system-agnostic component library** (to be published) — consumed by [[inrisk]], matched to InRisk's current look. **Parity-not-enhancement** posture on this library (refined 2026-05-14): same auth / RBAC / session, same filter parameters (incl. **TOBA status**), same look-and-feel. Per [[inrisk-cuts-over-before-high-volume]].
- **Event platform / message bus**: _unknown — payload events currently emitted to [[data-universe]] and [[inrisk]]; mechanism TBC_
- **Observability**: _unknown — pending ingest_
- **Other infrastructure**: _unknown — pending ingest_

## Touchpoints

### Inbound API / reads
- _unknown — pending Graph-API consumer audit spike ([[open-questions#OQ-004]])_

### Outbound API / writes
- _unknown — pending ingest_

### Event streams

#### Produced
- Party-change events → consumed by [[data-universe]] — payload-bearing; shape to be characterised; feeds downstream analytics
- Party-change events → consumed by [[inrisk]] — payload-bearing; drives IR2-side synchronisation
- Party-change events → routed via [[boomi]] → [[ntt]] for sanctions screening — current call-chain on [[sanctions-processing]]
- _(Phase 1 onward)_ Party-change events → routed via [[boomi]] → [[artificial]] (mirrored party-state for the HV-underpinning vendor platform) — see [[high-volume-architecture]]. Phase-1 priority is the **read path** from Artificial; the event-push back is a later piece.

#### Consumed
- _unknown — pending ingest_

### Batch / scheduled integrations
- _unknown — pending ingest_

## Internal structure
_Populate from raw ingest. No internal structure details captured in transcripts._

## Observability & SLOs
_Populate from raw ingest._

## Known constraints / rough edges
_From transcripts:_

- **Payload events, not reference events** — downstream consumers snapshot-cache party payloads rather than resolving by ID + version. This is the constraint the [[party-rearch]] project exists to remove. See [[contract-buckets]] "data distribution" bucket.
- **Graph API surface** — current consumers of the Knowledge-Graph API are incompletely mapped; a spike is open to audit endpoint-by-consumer ([[open-questions#OQ-004]]).
- **[[eclipse]] ingestion** — third-party data flows from [[eclipse]] into the Party Application are under scope review ([[open-questions#OQ-001]]).
- **Spine-rewrite-on-every-event drives sanctions noise** — every InRisk event causes Graph to rewrite the whole spine and emit one event; [[boomi]] consumes those events to drive [[sanctions-processing]] checks, which produces noise on irrelevant changes. Not a Phase-1 fix; [[open-questions#OQ-032]].
- **API spec is subject to change until 1 Sep go-live** ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]: _"won't change massively, but there may be some additions or deletions from it"_). Now that the YAML spec is in [[artificial]]'s hands ahead of the 2026-05-20 kick-off, every new consumer needs to be onboarded with this caveat explicit. Mirrors the InRisk-side iteration expected from the widget-integration spike.
- **Curation feature extension, Neo4j → DynamoDB data migration, and DU/sanctions event-emission completeness are still in flight** ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]). The front-of-house MDM API is the most mature surface today; backend completeness is the remaining critical-path work for Phase 1.

## Relationships to other architecture pages
- [[party-curation-tool-architecture]] — PCT is the primary curation frontend for parties; tightly coupled data model
- [[inrisk-architecture]] — downstream consumer; currently receives party payloads
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
