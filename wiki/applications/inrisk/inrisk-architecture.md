---
type: application-architecture
title: InRisk — Current-state Architecture
created: 2026-04-22
updated: 2026-05-26
tags: [architecture, current-state]
application: inrisk
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement]
source_count: 4
status: stub
---

# InRisk — Current-state Architecture

_Living reference for the **as-is** technical shape of [[inrisk]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: stub.** Minimal scaffolding; populated from forthcoming raw-folder ingests. InRisk is a **downstream consumer** of [[party-application]] in the [[party-rearch]] project — the page below focuses primarily on InRisk's integration surface with the party domain; general InRisk internals can be layered in later as needed.

## Summary
_Populated at first architecture ingest. From transcripts so far: InRisk (IR2) is the Policy Admin System for pre-bind policy information. In the party context, InRisk today **caches party payloads** received from [[party-application]] and resolves locally; under [[party-rearch-phase-1]] it moves to an interim state that still consumes payloads but with the **client-ID / submission / requirement nesting problem** addressed. The **final** party-side state is realised when [[inrisk-engine]] goes live (not Phase 1)._

## Technologies & services
_Populate from raw ingest. Known so far:_

- **Datastore**: relational; three independent tables receive the Phase-1 party-ID + version-ID additive columns: the **party table** (keyed by `client_id` — legacy naming), the **broker table**, and the **party_snapshot table** ([[sources/20260514-inrisk-high-level-refinement]]). All three are mechanical schema additions plus DTO updates; snapshots and existing fields are retained. Column types: `party_id` is **UUID v7**, `version_id` is an **integer**. Specific RDBMS engine pending ingest.
- **Feature tagging**: currently lives elsewhere (originally mapped into the party domain because [[party-application]] had a usable widget at the time); **moves to InRisk in Phase 2+** per [[feature-tagging-moves-to-inrisk]]. Per [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]], the likely target shape is an InRisk-classifications-style surface rather than a Postgres-table handover, contingent on [[suzanna-whitefield]]'s static-list investigation ([[open-questions#OQ-019]]). Phase 1: no change — [[joe-worsfold]] keeps the old backend, API, and widget alive past cutover so InRisk's pull is not interrupted.
- **Party-tagging surface**: _existing widget pattern_ (likely the same Party search widget used elsewhere, to be confirmed by [[billy-calladine]] at Thursday's high-level). Phase-1 work adds the new MDM ID/version fields and feature-flagged routing through Joe's new widget.
- **Integration shape with [[high-volume]]**: open — see [[open-questions#OQ-013]].
- **Sanctions integration (today)**: events from [[party-application]] are consumed by Boomi, which calls InRisk's API **one submission at a time** (no batch endpoint exists) to reconstruct submission context, then calls [[ntt]] for the actual sanctions check. See [[sanctions-processing]]. **Wrong place** by group consensus — not a Phase-1 deliverable but flagged with audit pressure ([[open-questions#OQ-032]]).
- **Other**: _unknown — pending ingest_

## Touchpoints

### Inbound API / reads
- **Single-submission lookup API** consumed by Boomi for sanctions-context reconstruction — no batch endpoint exists, which is a known cost driver on [[sanctions-processing]].
- Other inbound endpoints _unknown — pending ingest_.

### Outbound API / writes
- **Submission events** emitted to downstream PAS consumers — _no party-ID on the message today_ (one of the three interim changes is to stamp it).
- Other outbound APIs _unknown — pending ingest_.

### Event streams

#### Consumed
- **Party-change events** ← [[party-application]] — payload-bearing today; snapshotted locally. This is the contract the [[party-rearch]] project is changing.

#### Produced
- **Submission events** → downstream PAS consumers (no party-ID stamped today; Phase-1 story 2 of the [[inrisk#Phase-1 epic and stories (concrete as of 2026-05-13)|Party MDM Integration epic]] adds party-ID + version to outgoing messages).

### Widget integration (today, and changing in Phase 1)

- **Today**: party-search widget embedded in [[inrisk]]; integration is **query-parameter-driven**. The widget passes filter parameters (e.g. **TOBA status** — Terms of Business Agreement, with cohorts like `1984-approved` / `1987-approved` for brokers — and the Lloyd's-vs-retail broker workflow split) and receives party records. The new widget must support the same filter parameters at parity (raised by [[jason-owen]], [[sources/20260514-inrisk-high-level-refinement]]).
- **Phase-1 target**: bespoke, parity-only widget on Joe's design-system-agnostic library. **SDK-style** integration — instantiate the MDM SDK, pass methods / hooks to the widget components — but matched to InRisk's current auth / RBAC / session flow and look-and-feel. **Drop-in replacement now, enhancement later** (rolled back from Joe's original "modern and different" vision at the 2026-05-14 HL): _"my original scope was don't affect in-risk too much, so I just went crazy and was like, no, I want it to be really modern and different, but, yeah, we can roll it back a bit"_ ([[joe-worsfold]]). The widget is already published in its Chakra-3-with-design-system form; Joe to re-build into the design-system-agnostic library before InRisk's spike. See [[inrisk-cuts-over-before-high-volume]].
- **Manual party creation flow change (Phase-1 Story 1)**: today, the requirement-creation flow falls back to **InRisk-side manual creation** if both Party Search and the D&B fallback miss. After Story 1, the widget's existing manual-create path is enabled (already supported, used in the renewals flow — [[billy-calladine]] confirmed at the 2026-05-14 HL) and the InRisk-side legacy logic is removed. Joe: this is the foundational story that clears the decks before MDM cutover can proceed.

### Batch / scheduled integrations
- Ingestion from [[eclipse]] — third-party inflow; InRisk is the application that uses Eclipse. Shape TBC.

## Internal structure
_Populate from raw ingest. Known architectural pressure point: **client-ID / submission / requirement nesting** — current data model nests these in a way that is the architectural-choice pressure point the PreBind team is working through (distinct from a regulatory constraint)._

## Observability & SLOs
_Populate from raw ingest._

## Known constraints / rough edges
- **Interim-state carrier** — InRisk must support a transitional party-payload shape for the life of Phase 1 while the final-state [[inrisk-engine]] is still being built. This is explicit scope, not debt.
- **Feature tagging ownership drift** — in Phase 1, feature tagging sits awkwardly between party and InRisk domains; Phase 2+ formalises the move to InRisk. Suzy's static-list investigation ([[open-questions#OQ-019]]) may simplify the eventual migration.
- **No batch endpoint for submission lookup** — Boomi's sanctions orchestration today calls InRisk one submission at a time because no batch API exists. Known pain point on [[sanctions-processing]]; not a Phase-1 fix.
- **No design-system surface** — [[inrisk]] uses Chakra-2 components but not as a design system. Per [[inrisk-cuts-over-before-high-volume]], InRisk does **not** absorb Chakra 3 + design-system migration under the September gate; the new widget integration uses Joe's design-system-agnostic library.

## Relationships to other architecture pages
- [[party-application-architecture]] — upstream source of party data; contract changes drive InRisk-side work
- [[high-volume-architecture]] — sibling downstream consumer; integration shape between the two is open ([[open-questions#OQ-013]]). Cutover sequencing: [[inrisk]] lands on MDM ≥ 2 weeks before HV per [[inrisk-cuts-over-before-high-volume]]
- [[ntt]] (platform) — downstream sanctions-check vendor reached via Boomi's orchestration; see [[sanctions-processing]]

## Phase-target pages that delta from this one
- _(none yet — [[party-rearch-phase-1-inrisk-architecture]] to be created if/when InRisk Phase-1-delta is authored)_

## Superseded claims
_None yet._

## Related
- [[inrisk]] — identity page
- [[inrisk-engine]] — the final-state rewrite (not yet in production; its architecture is tracked separately in [[inrisk-engine-architecture]] when created)
- [[feature-tagging-moves-to-inrisk]] — Phase-2+ ownership shift; refined 2026-05-13
- [[inrisk-cuts-over-before-high-volume]] — InRisk-first cutover sequence; design-system-agnostic widget library
- [[sanctions-processing]] · [[ntt]] — sanctions touchpoint domain
- [[eclipse]] — upstream third-party data source
- [[party-rearch-dependency-map]] — cross-app view

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — client-ID / submission / requirement framing, interim-state carrier scope
- [[sources/20260422-meeting-transcript-session-2]] — feature-tagging ownership, payload-contract pressure
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — concrete data-model story (client + broker tables); SDK-style widget integration; design-system-agnostic library; sanctions-orchestration pain points (one-at-a-time submission lookup, no batch endpoint)
- [[sources/20260514-inrisk-high-level-refinement]] — data-model expanded to three tables (party + broker + party_snapshot); UUID-v7 + integer column types confirmed; party-table-keyed-by-`client_id` legacy naming captured; manual-client-creation cleanup as Story 1 (foundational); TOBA-status filter parity requirement; drop-in-replacement / parity-not-enhancement widget posture
