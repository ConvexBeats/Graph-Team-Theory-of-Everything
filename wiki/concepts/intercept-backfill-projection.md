---
type: concept
title: Intercept → Backfill → Projection
aliases: [intercept-backfill-projection-trio, three-phase-delivery]
created: 2026-04-22
updated: 2026-04-22
tags: [concept, migration, delivery-pattern]
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: draft
---

# Intercept → Backfill → Projection

## Summary
The three-step delivery shape the programme has settled on for moving [[party-application]] from its current Neo4j-backed Knowledge Graph onto the new DynamoDB + OpenSearch MDM **without breaking any downstream consumer**. Named in [[sources/20260422-meeting-transcript-session-2]]. Sits inside the **data-distribution** slot of [[contract-buckets]] and is the operational shape of [[strangle-the-graph-via-proxy-events]].

Each step is deliberately separable: each one can be built, verified, and (where possible) dry-run independently. Together they form the minimal choreography that lets MDM reach feature-parity with the Knowledge Graph from the day of cutover.

## The three steps

### 1. Intercept
From a defined cut-in point, **writes that would land in the Knowledge Graph are diverted to MDM** (or a precursor that feeds MDM). Reads continue to come from the Knowledge Graph until cutover — the point of intercept is write-side only.

- **Goal**: keep MDM current from cut-in onward so that when backfill completes, no new deltas are accumulating on the old store.
- **Owner shape**: a ticketised workstream on [[graph-team]] ([[alex-sillars]] coordinating; [[ben-joseph]] / [[joe-worsfold]] as implementers).
- **Failure mode if skipped**: MDM drifts behind the Knowledge Graph by however long backfill takes; no safe cutover window.

### 2. Backfill
**100% of historical Knowledge Graph state** is replayed into MDM, with legacy Graph party IDs preserved as display IDs (per [[uuid-system-id-with-display-id]]). Parent / ultimate-parent links are reconstructed in a second pass using the legacy ID as the anchor.

- **Goal**: MDM reaches content parity with the Knowledge Graph at the moment of cutover.
- **Scope boundary**: this is the **data** backfill only. It explicitly **does not** include Jira-side audit-history backfill (see [[no-pct-audit-backfill]]) nor InRisk historic client-ID snapshots (see [[no-historic-client-backfill-into-mdm]]).
- **Failure mode if incomplete**: consumers start missing parties after cutover; erodes the "no consumer change" promise.

### 3. Projection
MDM emits events **in the existing Knowledge Graph event shape**, via a proxy adapter. Acceptance criterion (per [[ben-joseph]]): **bidirectional mappability** — event → MDM record → event must round-trip exactly.

- **Goal**: preserve the [[data-universe]]'s current consumption contract (and every other data-distribution consumer per [[contract-buckets]]); no schema change on their side.
- **Implementation note**: the proxy adapter is a small job because, per Session 1, the Knowledge Graph effectively emits a **single spine-rewrite event** (plus a broker variant). It is a one-event mapper, not an N-event migration.
- **Failure mode if incorrect**: downstream data shape drifts even if the record content is right; DU projections break silently.

## Ordering and dependencies
- **Intercept precedes backfill.** Without intercept, backfill tries to hit a moving target.
- **Projection can be built in parallel** with intercept and backfill — it has no data dependency on either (it operates on the emit side of MDM) — but **must be verified** before the PCT+MDM coupled go-live (see [[pct-and-mdm-go-live-together]]).
- **All three must hold simultaneously** at the cutover moment: intercept on, backfill complete, projection emitting. If any one is missing, cutover is blocked.

## Why it matters

1. **It decomposes the migration into three independently-verifiable steps.** Each has its own test shape (intercept: write-echo check; backfill: record-count + spot-diff; projection: round-trip mapping test). Programme risk goes down because _where_ it could break is named.
2. **It lets scope calls stick.** Because each step is named and scoped, the "don't backfill Jira audit into the new PCT" call ([[no-pct-audit-backfill]]) and "don't backfill historic InRisk client snapshots" call ([[no-historic-client-backfill-into-mdm]]) are easy to state and enforce — they are non-requirements of the backfill step.
3. **It is the spec for the "small adapter" claim.** The projection step is the thing that makes the strangler feel feasible to the room; naming it separately forces its small-adapter property to be tested, not assumed.

## Contradictions / open debates
- **Backfill cut-off policy.** The point-in-time from which MDM history starts is "cutover forward" per [[no-historic-client-backfill-into-mdm]] — for Party data. Knowledge Graph history itself is fully backfilled (by definition of step 2). Make sure these two scopes are not conflated in future conversations.
- **Bidirectional round-trip test authorship.** The test that asserts event → MDM → event equivalence is a single choke-point of correctness for the whole migration; no explicit owner yet (tracked at [[party-rearch-ownership-matrix]]).

## Related
- [[strangle-the-graph-via-proxy-events]] — the overarching strategic decision this pattern implements.
- [[contract-buckets]] — this pattern is the delivery shape of the **data-distribution** bucket.
- [[pct-and-mdm-go-live-together]] — the cutover moment that consumes all three steps.
- [[no-historic-client-backfill-into-mdm]] · [[no-pct-audit-backfill]] — scope-bound non-requirements of the backfill step.
- [[uuid-system-id-with-display-id]] — the ID scheme that makes backfill addressing tractable.
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — the trio is named explicitly; acceptance criterion on step 3 is [[ben-joseph]]'s.
