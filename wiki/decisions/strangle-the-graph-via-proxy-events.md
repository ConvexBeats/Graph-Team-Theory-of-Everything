---
type: decision
title: Strangle the Graph via proxy events
created: 2026-04-22
updated: 2026-05-26
tags: [decision, migration, events]
application: [party-application]
owner: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260519-party-integration-timelines]
source_count: 4
status: accepted
project: party-rearch
phase: [phase-1]
---

# Strangle the Graph via proxy events

## Summary
Rather than dual-run the Graph DB alongside MDM during the migration, the new MDM will emit **proxy events** to the Data Universe (DU) in the existing graph event shape. Downstream consumers (primarily DU → Snowflake) see no change on cutover. The Graph DB is decommissioned once proxy emission is stable. A later phase replaces proxy events with a direct Dynamo-to-DU stream.

## Context
Maintaining both the current Graph DB (Neo4j/Cypher) and the new MDM (DynamoDB + OpenSearch) in sync through the transition was previously the assumed path. The room concluded this is operationally too painful: every new MDM feature has to be mirrored in the dying Graph DB to preserve event parity; merge/conflict handling between two live stores would create its own bug surface; and the cost grows for every day both systems run.

The strangler pattern shifts the problem: MDM becomes the write-side from day one of cutover; Graph DB is no longer maintained; a thin adapter in MDM emits events **in the Graph-DB event shape** so the DU continues to consume unchanged.

### Origin (Session 1, morning)
The idea was invented live in [[sources/20260422-meeting-transcript-session-1]], during the **DU-events** segment. The unlocking realisation was Billy's inventory of the current event shape: despite the Graph's complexity, DU consumes effectively **one event** (plus a broker variant to the common bus), and the Graph DB _rewrites the whole spine on every InRisk event anyway_, regardless of whether any field changed. That meant the adapter was a single-event mapper, not an N-event migration — a much smaller job than the room had assumed. Joe's framing: _"we can proxy the current one ... we send the event without ever writing to the graph."_ Alex's test of the idea: _"if you do it right, no one will know you've done anything at all."_

### Consolidation (Session 2, afternoon)
The decision was formally adopted in [[sources/20260422-meeting-transcript-session-2]]. The **bidirectional-mappability** criterion was named there (from [[ben-joseph]]): _"if I can take an event currently emitted from Party Graph, map it into MDM, then map the MDM record back to the exact same event, we can go in."_ This is the go/no-go test for the adapter.

### Refinement (2026-05-13 follow-up) — cutover-window dual-write for revertability
[[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] sharpened the operational behaviour at the cutover boundary itself:

- **No parallel running of MDM and old graph as sources of truth** for party create / search — that was always the intent and is reinforced here. The mapper is one-way (MDM → old events); a two-way mapper would create an unmanageable data-mess. Joe Worsfold: _"we'd end up having to have a mapper going the other way, and I just think we'll get into a data problem mess of, like, not knowing."_
- **However, for the cutover window, MDM dual-writes to the old graph** so that a revert is possible without losing data created during the window. Joe: _"essentially, even when MDM takes over, be writing to the old graph, so that if you switch back, that you'd be able to find stuff that was only just created in MDM."_
- This is **not a contradiction** of the "no dual-running" framing — MDM remains the single source of truth in the live path; the old graph receives a write-through for revertability only, not for serving reads. Once the cutover is stable, the dual-write stops and the old graph proceeds to decommission as originally planned.
- **Non-prod environments** are easier: dual-write naturally allows toggling back and forth, so non-prod doesn't need the same care that prod does. Useful for InRisk-side integration testing under the InRisk-first cutover sequence per [[inrisk-cuts-over-before-high-volume]].

### Refinement (2026-05-19) — stability promise restated to a customer, with the "joined-up InRisk + Party data" carve-out
[[sources/20260519-party-integration-timelines]] is the first time the strangler-pattern stability promise has been **stated to a third-party consumer** ([[artificial]], via [[simon-hulbert]]) rather than as an internal architectural intent. The verbatim framing from [[alex-sillars]] is worth preserving as an external contract statement: _"Everything that's going to come out of Party after we go live on the 1st of September is exactly what it comes out today. We're going to do that in a different way. We're going to obtain that information through a different source. It might not be Party, it might be joined-up InRisk and Party data, but it will still hit sanctions, DU, in exactly the same way."_

Two corollaries from this framing:

- The strangler-pattern stability is **shape-of-events**, not **source-of-fields**. For some fields, the post-cutover source may be Party directly; for others, it may be InRisk-joined-data assembled on the Party side (e.g. submission-IDs that Party doesn't store but DU / sanctions consume today — Alex specifically called this out as a build-out item). The downstream consumer cannot tell the difference; this is by design.
- The promise has now been stated to **multiple external counterparties** ([[artificial]] explicitly, with [[ntt]] / [[boomi]] implicitly downstream of sanctions). Reverting or softening it costs reputation, not just an architectural rework. Strengthens the constraint that the proxy adapter must hold its contract.

## Options considered

1. **Dual-run Graph DB and MDM, both writing, sync until cutover.**
   - Pros: familiar pattern; low-risk for downstream consumers.
   - Cons: every change to the new system must be shadowed in the old; graph-specific invariants (node uniqueness, cascading updates on renewal) become coupled bugs; cost scales with runway length; does not buy a clean exit.
2. **Rip-and-replace on cutover day, update all downstream consumers to new event shape simultaneously.**
   - Pros: clean; no legacy emission to maintain.
   - Cons: coordinates every downstream team to cut over in a single window; DU and any other consumer must be ready simultaneously; high blast radius if anything drifts.
3. **MDM becomes source of truth; emit proxy events in the existing graph shape (chosen).**
   - Pros: downstream consumers (DU especially) see zero change on cutover; Graph DB can be retired immediately after cutover; no ongoing dual-write burden; future-state direct stream is a straightforward upgrade from proxy.
   - Cons: MDM has to faithfully reproduce the legacy event shape, including some legacy quirks (e.g. client-ID nesting). Requires faithful backfill to produce correct projected events for historical records.

## Decision
**Option 3**. MDM emits proxy events in the existing Party Graph event shape. The Graph DB is read-only once intercept + backfill complete, and is decommissioned shortly after cutover.

Implementation trio from [[alex-sillars]]:
1. **Intercept** — divert writes that currently land on Graph DB into MDM (or a precursor thereof) from a defined point forward, so MDM stays current during backfill.
2. **Backfill** — ingest 100% of historical Graph DB state into MDM, preserving legacy IDs as display IDs.
3. **Proxy projection / adapter** — emit MDM changes as events in the Graph event shape into the DU's event stream.

## Consequences

- **Positive**
  - DU and other downstream consumers need no code change on cutover day.
  - Graph DB can be retired quickly — no indefinite dual-run.
  - Clear unidirectional migration path: Graph → MDM → (later) direct Dynamo stream.
  - Adapter is a small, well-bounded artefact; easy to test via the bidirectional-mappability criterion.

- **Negative / trade-offs**
  - MDM inherits the legacy event shape as a public contract; changes to it require coordinated deprecation later.
  - Some legacy quirks (notably client-ID nesting of submissions) must be reproducible either natively or via a helper call back to InRisk; see inline decision on flattening.
  - Backfill correctness is load-bearing — gaps would show up as missing or incorrect proxy events.

- **Open risks**
  - If the DU does in fact index deep-nested fields (submission under client-ID), flattening in MDM requires a proxy-time reconstruction (small InRisk API) — adds a cross-team dependency to the adapter path.
  - The runway is fixed by the [[high-volume]] go-live on **1 September**; the proxy adapter + intercept + backfill must be production-stable by then. Any slippage in any of the three components compresses the window.

## Supersedes / superseded by
- Supersedes the previously-assumed plan to maintain Graph DB in sync with MDM ("dual-running") throughout migration.
- Not yet superseded.

## Related
- [[party-application]] — primary application affected
- [[analytics-team]] — consumer of the proxy events
- [[pct-and-mdm-go-live-together]] — complementary decision; without new PCT shipping together, the intercept side of this strategy breaks
- [[inrisk-cuts-over-before-high-volume]] — companion ADR governing the cutover **sequencing** ([[inrisk]] first, then [[high-volume]] at the 1-Sep gate); the dual-write nuance recorded above is the operational corollary that makes that sequencing reversible if needed
- [[graph-team]] — owner

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — **origin** (morning of 2026-04-22); idea first proposed in the DU-events segment
- [[sources/20260422-meeting-transcript-session-2]] — consolidation / formal adoption (afternoon of 2026-04-22); bidirectional-mappability criterion named
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — cutover-window dual-write nuance for revertability; non-prod toggle-back-and-forth pattern
- [[sources/20260519-party-integration-timelines]] — first external statement of the stability promise (to [[artificial]]); "joined-up InRisk + Party data" framing as one of the implementation modes inside the strangler
