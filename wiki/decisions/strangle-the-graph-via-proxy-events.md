---
type: decision
title: Strangle the Graph via proxy events
created: 2026-04-22
updated: 2026-04-22
tags: [decision, migration, events]
application: [party-application]
owner: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: accepted
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
- [[graph-team]] — owner

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — **origin** (morning of 2026-04-22); idea first proposed in the DU-events segment
- [[sources/20260422-meeting-transcript-session-2]] — consolidation / formal adoption (afternoon of 2026-04-22); bidirectional-mappability criterion named
