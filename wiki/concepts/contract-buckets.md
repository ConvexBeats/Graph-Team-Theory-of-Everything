---
type: concept
title: Contract Buckets — Operational · Data Distribution · Enrichment
aliases: [three-bucket-framework, contract-framework]
created: 2026-04-22
updated: 2026-04-22
tags: [concept, framework, methodology]
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: draft
---

# Contract Buckets

## Summary
A three-bucket framework, introduced by [[alex-sillars]] at the top of [[sources/20260422-meeting-transcript-session-1]], for organising every **consumer contract** of the [[party-application]]. Every integration into or out of the Party is placed into exactly one of three buckets: **Operational**, **Data Distribution**, or **Enrichment**. The framework is the scaffolding the Session 1 workshop used to walk every contract systematically (target state → interim state → cross-team dependencies). It is now the canonical lens the programme uses to read any Party-related change.

## Definition

A **contract bucket** is a category that describes _the kind of relationship_ a consumer has with the [[party-application]]:

- **Operational** — the consumer **creates or curates parties** (writes in, or orchestrates writes).
- **Data distribution** — the consumer **reads party data** (subscribes to events or queries the API).
- **Enrichment** — the consumer supplies **external reference data** that gets attached to parties.

A given application can appear in more than one bucket (e.g. [[inrisk]] is in operational _and_ data-distribution); the bucket describes the _relationship_, not the application.

## Mechanics / how it works

For any contract between the [[party-application]] and a counterparty, answer two questions:

1. **Which bucket does this contract sit in?**
2. **Within that bucket, what is the target state, the interim state, and the cross-team dependency?**

The three-bucket framing forces the design conversation to treat related contracts together. In Session 1 this exposed two structurally useful facts:

- The operational bucket is **dominated by create/curate shape**, not by reads — which is why the new [[party-curation-tool]] and the search widget have to be treated together, and why they share a single feature flag (see [[pct-and-mdm-go-live-together]]).
- The data-distribution bucket turned out to be **much smaller than the room assumed**: once Billy inventoried the event shape, it became clear that despite the Graph's complexity, DU really consumes _one_ event (plus a broker variant on the common bus). That collapse is what made [[strangle-the-graph-via-proxy-events]] feasible.

## Current population (Session 1 walkthrough)

### Operational bucket
How parties get created or curated:

- **Search widget** (embedded in [[inrisk]]) — party lookup + create-on-miss
- **Manual creation via application** (existing pattern, limited to certain roles today)
- **Manual creation via [[party-curation-tool]]** (the curator's own UI)
- **CUO groupings** — specific higher-order curation action; manual in Phase 1
- **PCT UI itself** — curator-driven edits, merges, version bumps

### Data distribution bucket
Who reads party events / queries the Party API:

- **[[data-universe]]** (owned by [[analytics-team]]) — primary event consumer (spine-rewrite event + broker-variant)
- **Party versioning in [[inrisk]] + Artificial** — local snapshot caches today; ID+version references in Phase 1
- **Launchpad** — downstream consumer (referenced in-session; not deep-dived)
- **Party reporting** — internal reporting pipeline
- **EVA** — referenced in-session as a party-reading consumer
- **Broker dashboards** — consume the broker-variant on the common bus

_Knowledge Graph was originally named as a consumer in this bucket in Session 1; subsequently clarified (lint pass) as the internal Neo4j datastore within [[party-application]] rather than a downstream consumer — it does not belong in this bucket._

### Enrichment bucket
External reference data flowing in:

- **Dun & Bradstreet (D&B)** — Phase 1: cached in Dynamo with scheduled refresh → revision (see [[d-and-b-caching-and-auto-parent]])
- **S&P** — Phase 1: stays manual via DU; Phase 2+: move to direct S&P API with built-in entity resolution
- **One Re groupings** — Phase 1: manual curation in PCT; rules-based auto-grouping is Phase 2+
- **Sanctions** — Phase 1: current NTT round-trip unchanged; Phase 2+: revision-event-triggered, stamped on the party version
- **Eclipse** — retired in the new world; no live consumer, pending confirmation via the Graph-API audit spike (see [[open-questions#OQ-001]])

## Variants / adjacent frameworks

- [[roles-vs-views-litmus-test]] (Joe Worsfold, Session 1) — a finer-grained distinction _inside_ the operational/data-distribution boundary: roles are intrinsic to the party; views are ascribed by consumers and fed back.
- [[intercept-backfill-projection]] (Session 2) — the delivery shape for the data-distribution bucket under the strangler strategy.

## Why it matters

The framework is load-bearing for three reasons:

1. **It makes Phase 1 scope negotiable.** Every contract sits in one bucket; each bucket has its own Phase 1 posture (operational: co-ship PCT + MDM; data distribution: proxy events; enrichment: cache + manual where needed). A change request lands in one bucket and its downstream implications are obvious.
2. **It surfaces the small-adapter insight.** Without the bucket split, the room assumed the data-distribution change was N-shaped. With the split, it was obvious the DU-facing work was a single-event adapter — which is what unlocked [[strangle-the-graph-via-proxy-events]].
3. **It's the right mental model for the long term.** End-state decisions (direct Dynamo → DU stream, S&P API, feature-tagging handover) all move _within_ a bucket; the framework keeps each migration self-contained.

## Contradictions / open debates

_None recorded yet._ The framework is as-proposed; no counter-proposal has been raised.

## Related
- [[dependency-map]] — the physical map is organised around this framework (see its "Contract buckets" section).
- [[strangle-the-graph-via-proxy-events]] — the strategy that came out of applying the framework to the data-distribution bucket.
- [[pct-and-mdm-go-live-together]] — the operational-bucket consequence.
- [[d-and-b-caching-and-auto-parent]] — the flagship enrichment-bucket decision.

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — introduction of the framework and full walk-through of every contract
