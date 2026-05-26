---
type: phase
title: party-rearch — Phase 1 (MDM + PCT co-ship, 1 Sep HV gate)
created: 2026-04-22
updated: 2026-05-26
tags: [phase, in-flight]
project: party-rearch
phase_id: phase-1
status: in-flight
gate_date: 2026-09-01
---

# Phase 1 — MDM + PCT co-ship to 1 September HV go-live

## Summary
First phase of the [[party-rearch]] project. Ships the new MDM (DynamoDB + OpenSearch) and the new [[party-curation-tool]] together, under a **single feature-flag coupled rollout**. **[[inrisk]] cuts over to MDM at least two weeks before [[high-volume]]'s 1 September go-live** ([[inrisk-cuts-over-before-high-volume]]), so HV consumes MDM only after real production traffic from InRisk has exercised the new contract. Legacy Graph DB is kept alive behind a proxy-event strangler during the cutover; [[data-universe]] sees no schema change. Historical migrations are deliberately bounded: MDM carries party-version history from cutover forward, no pre-cutover InRisk client snapshots and no Jira PCT audit history are backfilled.

## Gate
**2026-09-01** — [[high-volume]] in production requires MDM as its Party source-of-truth. This is the **outer** gate. Phase 1 has an **inner gate** ahead of it: [[inrisk]]'s MDM-integration cutover, targeted for **≥ 2 weeks before 1 Sep** (concrete date open at [[open-questions#OQ-035]]). InRisk slipping past mid-August compresses the buffer that HV depends on.

## Scope — applications

| Application | Change intensity | Shape of change |
|---|---|---|
| [[party-application]] | **high** — core subject | This application's own Neo4j primary datastore (informally "Party Graph" / "ParseDB" — distinct from sibling [[knowledge-graph]]) retires as primary; DynamoDB + OpenSearch stood up on existing 3-account / 4-env AWS estate (dev working, integration env blocked on JumpCloud SAML Cognito — [[open-questions#OQ-042]]); monorepo + DDD; auto-generated REST API; revision-based versioning baked in; full audit / access-log / recovery stack live (Datadog + API Gateway → S3 + Dynamo streams → audit bucket + point-in-time recovery); three iterations of backfill done (stable alpha targeted for end of week 2026-05-22). Proxy-event strangler in place but the **InRisk-data lookup design fork** ([[open-questions#OQ-041]]) gates the adapter. Bulk-migration CLI delivered by [[graph-team]]; Dynamo migration tooling scope open ([[open-questions#OQ-043]]). **Two widget component libraries** published: Chakra-3 + design-system for PCT, **raw React + close-to-raw CSS bespoke widget for InRisk** (single component, three parameterised variants in a shell — refined 2026-05-19) per [[inrisk-cuts-over-before-high-volume]] |
| [[party-curation-tool]] | **high** — co-ships | Next.js UI on the new MDM (monorepo); **revisions + filters** in place of Jira's tabs; **mark-for-review / approve / request-changes** baked into the ticket UI. Single-flag coupled rollout with the search widget; Jira workflow retired live (no dual-running); no audit backfill. Fully functional in dev as of 2026-05-19; curation gap-analysis ticket authoring with [[tomas-sivo]]. DataOps parity confirmation open ([[open-questions#OQ-044]]) |
| [[inrisk]] | **high** — concrete epic of 5 stories, **ordered + gated as of 2026-05-14** | "Party MDM Integration" epic: **Stories 1 & 2 ready for low level immediately**: (1) remove manual client-creation on new requirement → widget — the foundational "demon" that has to clear before MDM cutover; (2) backwards-compatible data-model addition — `party_id` (UUID v7) + `version_id` (int) on **party + broker + party_snapshot** tables. **Stories 3/4/5 gated on a widget-integration spike** (Joe + Billy + Alex + [[daria-romanovskaia]] — slot inherited from [[andrew-turner]] on his 2026-05-29 departure): (3) client-widget integration, feature-flagged (renewals folds in here); (4) broker-widget integration; (5) party-tagging integration. **Drop-in-replacement / parity-not-enhancement** on the widget per [[inrisk-cuts-over-before-high-volume]] refinement; current widget filters (incl. **TOBA status**) must continue. **Feature tagging is out of Phase 1** — old backend stays alive past cutover; party tagging _is_ in scope |
| [[inrisk-engine]] | **none** — out of scope | Explicitly scoped out: targets final-state Party contract, not the interim. Phase-1 work should not design for [[inrisk-engine]]. |
| [[high-volume]] | **integration only** | HV consumes MDM via the new versioned-reference API at the 1-Sep gate, **after** the InRisk-first cutover window. API-only via [[boomi]]; no widget. **HV is Convex's implementation of the [[artificial]] vendor platform** — so the consumer is effectively (a) [[high-volume]] (Convex-side glue) and (b) [[artificial]] (the underlying third-party platform), both via [[boomi]]. Convex × Artificial kick-off scheduled 2026-05-20; YAML API spec already shared with Artificial; Boomi connectivity setup ([[srini]] paired with [[joe-worsfold]]) is the immediate unblock. Integration shape still being specified ([[open-questions#OQ-013]]) |
| [[eclipse]] | **scope call** | Ongoing ingestion into [[party-application]] under review ([[open-questions#OQ-001]]) — confirmed dropped pending the Graph-API audit spike |

### Architecture pages per application

Per-application architecture is tracked on dedicated pages. Current-state pages live in `wiki/applications/`; phase-end target pages live in this project folder and delta from the current-state baseline.

| Application | Current state | Phase-1 target |
|---|---|---|
| [[party-application]] | [[party-application-architecture]] | `[[party-rearch-phase-1-party-application-architecture]]` _(to be created)_ |
| [[party-curation-tool]] | [[party-curation-tool-architecture]] | `[[party-rearch-phase-1-party-curation-tool-architecture]]` _(to be created)_ |
| [[inrisk]] | [[inrisk-architecture]] | `[[party-rearch-phase-1-inrisk-architecture]]` _(to be created if change-intensity justifies)_ |
| [[high-volume]] | [[high-volume-architecture]] | `[[party-rearch-phase-1-high-volume-architecture]]` _(to be created)_ |
| [[inrisk-engine]] | _not tracked (out of scope for Phase 1)_ | — |
| [[eclipse]] | _not tracked_ | — |

When Phase 1 ships, each target page gets folded into its current-state baseline per AGENTS.md §5.4.

## Decisions in this phase
All carry `project: party-rearch` · `phase: [phase-1]`:

- [[strangle-the-graph-via-proxy-events]] — MDM emits Graph-shape proxy events; no dual-running of sources of truth (refined 2026-05-13: brief dual-write to the old graph during the cutover window for revertability, not a contradiction of "no dual-running")
- [[pct-and-mdm-go-live-together]] — new PCT and MDM ship together under a single feature flag
- [[inrisk-cuts-over-before-high-volume]] — InRisk's MDM integration cuts over ≥ 2 weeks before 1 Sep; HV switches on at the gate. Includes the design-system call: two widget component libraries (PCT keeps Chakra-3 + design-system; InRisk consumes a design-system-agnostic library). Resolves [[open-questions#OQ-005]]
- [[no-historic-client-backfill-into-mdm]] — MDM history starts at cutover
- [[no-pct-audit-backfill]] — new PCT does not carry Jira audit history; Snowflake is the cross-period audit store
- [[bulk-migrations-owned-by-mdm-phase-1]] — [[graph-team]]-owned CLI in Phase 1
- [[d-and-b-caching-and-auto-parent]] — D&B cached in Dynamo with TTL; scheduled refresh → curation revision loop
- [[uuid-system-id-with-display-id]] — system ID = UUID, display ID = legacy Graph party ID

## Cutover sequence (added 2026-05-13)

```
┌─────────────────────────────────────────────────────────────────────┐
│ Phase 1 cutover                                                     │
│                                                                     │
│  PCT + MDM cutover (single feature flag) ──┐                        │
│   [[pct-and-mdm-go-live-together]]         │                        │
│                                            ▼                        │
│  InRisk integration goes live  ────────────► (≥ 2 weeks before)     │
│   [[inrisk]] Phase-1 epic                                          │
│   [[inrisk-cuts-over-before-high-volume]]                          │
│                                            │                        │
│                                            ▼                        │
│  [[high-volume]] turns on at 2026-09-01 ◄── HARD GATE              │
│                                                                     │
│  During cutover window: MDM dual-writes to old graph for            │
│  revertability ([[strangle-the-graph-via-proxy-events]] refinement) │
└─────────────────────────────────────────────────────────────────────┘
```

## Out of Phase 1 (flagged but not done here)

- **Feature tagging** — InRisk continues pulling its static list from the existing Postgres table + widget past cutover; eradication deferred to a later phase as an InRisk-specific change. See [[feature-tagging-moves-to-inrisk]] (refined 2026-05-13; reinforced to the wider InRisk audience 2026-05-14).
- **Sanctions / NTT / Boomi orchestration** — wrong place by group consensus, audit pressure this year; not a Phase-1 deliverable. See [[sanctions-processing]] · [[ntt]] · [[open-questions#OQ-032]]. Phase 1 leaves the flow unchanged: proxy events drive Boomi just as Graph events do today.
- **Anna's UX team involvement** — UX improvements to the new widget surface are deferred; Phase 1 ships UX as similar to today's as practical to avoid layering pretty-up onto cutover. Reinforced 2026-05-14: Joe's InRisk widget is now committed to **parity-not-enhancement** explicitly. Rory to speak to Anna ahead of cutover to warm users; Anna identity tracked at [[open-questions#OQ-033]].

## Pending Phase-1 decisions (not OQs)

- **InRisk-side backfill of the new ID columns** (raised 2026-05-14 by [[joe-worsfold]]). Story 2 adds `party_id` + `version_id` to InRisk's party / broker / party_snapshot tables. For **existing pre-cutover rows**: backfill with known MDM party-IDs, or leave null + go-forward from cutover? Sanctions is the principal impact surface — pre-cutover party-ID resolution affects whether a sanctions re-check can be ID-driven or has to keep using the snapshot. Mirror of the [[no-historic-client-backfill-into-mdm]] question on the InRisk side. Owners: [[joe-worsfold]] · [[john-trahearn]]; to be decided ahead of Story 2 low-level on 2026-05-19. _Not formalised as an OQ per user steer._
- **API-spec instability communication** (added 2026-05-19, operational). The new MDM API spec is _"subject to change until go-live, won't change massively, but there may be some additions or deletions"_ ([[alex-sillars]], [[sources/20260519-party-integration-timelines]]). Already in [[artificial]]'s hands ahead of the 2026-05-20 kick-off. Every external consumer onboarded between now and 1 Sep needs this caveat made explicit. Not a Phase-1 deliverable per se; a coordination protocol carried by Action #32. Tracked on [[party-application]]'s Pending section.
- **Implementation-progress sentiment (2026-05-19)**. [[rory-beattie]] close of [[sources/20260519-mdm-implementation-strategy]]: _"we're a lot further along than I suspected we even were. So I'm not overly worried about this."_ [[joe-worsfold]]: _"we're in the like dotting I's, crossing T's, make sure there's no unknown unknowns, and then it's the thing I was always between team collaboration is the thing that I worry about — not that it'll be bad, just that it takes time."_ Captured here as a baseline against which to re-assess in future ingests; the practical implication is that the remaining critical-path risk is **inter-team coordination** rather than build progress.

## Delivery shape
The Phase-1 delivery trio is [[intercept-backfill-projection]]:
1. **Intercept** — writes divert to MDM from a defined cut-in point
2. **Backfill** — 100% of historical Knowledge Graph state replayed into MDM (display IDs preserved)
3. **Projection** — MDM emits in the existing Graph event shape via a proxy adapter (bidirectional round-trip must hold)

All three must be verified before the single-feature-flag cutover.

## Open questions gating this phase
See [[open-questions]] (filter by `project: party-rearch`, `phase: phase-1`). Highest-leverage:

- [[open-questions#OQ-002]] — Data Universe tolerance for flattened `(party-id, submission-id)` schema. **Single biggest Phase-1 branch point.**
- [[open-questions#OQ-004]] — Graph API consumer audit spike.
- [[open-questions#OQ-013]] — HV integration-shape specifics.
- [[open-questions#OQ-017]] — MDM delivery squad shape. Blocks sprint-planning.
- [[open-questions#OQ-014]] — DataOps change-management lead time.
- [[open-questions#OQ-035]] — Concrete InRisk MDM-cutover date (new 2026-05-13). Sizes how much buffer HV actually gets before the 1-Sep gate.
- [[open-questions#OQ-036]] — Widget-response field alignment (new 2026-05-13). Mechanic alignment for InRisk's Phase-1 integration.
- [[open-questions#OQ-041]] — **Proxy-event-with-InRisk-data design fork** (new 2026-05-19). InRisk endpoint vs KG pull. **Gates the proxy adapter.**
- [[open-questions#OQ-042]] — Integration env JumpCloud SAML Cognito access (new 2026-05-19). Unblocks integration-env smoke testing.
- [[open-questions#OQ-043]] — Dynamo migration tooling Phase-1 scope (new 2026-05-19). Self-service-in-Phase-1 vs fast-follow vs ad-hoc.
- [[open-questions#OQ-044]] — PCT new-UI dataops parity confirmation (new 2026-05-19).
- [[open-questions#OQ-010-R]] — Knowledge Graph identity correction (re-opened 2026-05-26). Not a Phase-1 deliverable; bears on Option B of OQ-041.

## Predecessors / successors
- **No predecessor** — first phase.
- **Successor**: [[party-rearch-phase-2]]. Phase 2 picks up the items explicitly deferred here (feature-tagging handover to [[inrisk]]; direct Dynamo→DU stream replacing proxy events; full self-serve bulk-migration UX; end-state sanctions flow; [[inrisk-engine]] buy-in on the final-state Party contract).

## Done-state
- **[[inrisk]] integrated with MDM in production ≥ 2 weeks before 2026-09-01** ([[inrisk-cuts-over-before-high-volume]]); Party MDM Integration epic's 4–5 stories all shipped.
- MDM serving HV traffic in production on 2026-09-01, **after** the InRisk-first cutover buffer.
- Legacy Knowledge Graph decommissioned (or on a clear decommission-only schedule post-cutover). Cutover-window dual-write to old graph stopped.
- New PCT live; old PCT switched off at cutover; Jira retained read-only.
- No schema change required on [[data-universe]]'s side.
- Old feature-tagging backend still alive for InRisk to pull its static list (out-of-Phase-1 deferral, per refined [[feature-tagging-moves-to-inrisk]]).
- Sanctions / Boomi orchestration unchanged in Phase 1; Phase 2+ workstream queued.
- Phase-1 decisions above all shipped or formally deferred.

## Related
- [[party-rearch]] — project overview
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]
- [[contract-buckets]] · [[intercept-backfill-projection]] · [[roles-vs-views-litmus-test]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — Phase-1 scope calls (no-backfill; feature-tagging deferred; bulk-migration CLI)
- [[sources/20260422-meeting-transcript-session-2]] — strangler pattern, PCT co-delivery, InRisk interim changes, D&B and UUID decisions
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — InRisk-first cutover sequence; two component-libraries call; party-tagging vs feature-tagging Phase-1 boundary; sanctions / NTT / Boomi flagged out-of-Phase-1; AWS estate confirmation
- [[sources/20260514-inrisk-high-level-refinement]] — InRisk epic ordered + gated (Stories 1 & 2 ready; 3/4/5 spike-gated); third InRisk table (party_snapshot) added to data-model story; drop-in-replacement / parity-not-enhancement principle for the InRisk widget; TOBA-status filter as a parity requirement; InRisk-side backfill question raised
- [[sources/20260519-party-integration-timelines]] — [[artificial]] confirmed as second new Phase-1 Party consumer (with HV as the Convex-side delivery layer); [[boomi]] gateway role confirmed; Convex × Artificial fortnightly cadence + Slack channel + kick-off (2026-05-20); InRisk-first cutover sequence restated to Artificial; strangler-pattern stability promise restated externally; new HV-side people identified ([[srini]], [[luca]], Sam-as-FDA)
- [[sources/20260519-mdm-implementation-strategy]] — MDM implementation snapshot: monorepo + DDD; auto-generated REST API; revision-based versioning + full audit / access-log / recovery stack; widget = single component with three parameterised variants (raw React + close-to-raw CSS, owned by [[billy-calladine]]); Dynamo in dev + integration (integration env access blocked); three iterations of backfill done; **proxy-event-with-InRisk-data design fork** ([[open-questions#OQ-041]]) gates the adapter; **Knowledge Graph correction** ([[open-questions#OQ-010-R]]); UI/UX descope in-meeting reinforces parity-not-enhancement; sentiment _"a lot further along than I suspected"_
