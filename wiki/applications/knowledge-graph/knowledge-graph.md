---
type: application
title: Knowledge Graph
aliases: [kg]
created: 2026-05-26
updated: 2026-05-26
tags: [application, stub, graph-team]
owner: graph-team
state: current
projects: [party-rearch]
sources: [20260519-mdm-implementation-strategy]
source_count: 1
status: stub
---

# Knowledge Graph

> **Architecture:** detailed current-state tech, touchpoints, and constraints will live on `[[knowledge-graph-architecture]]` once authored. This identity page covers role-in-portfolio, ownership, and cross-project change items.

## Summary

A **separate graph database** owned by [[graph-team]] that acts as a **read-replica containing both [[inrisk]] and [[party-application]] data**. Its primary role is to **power [[inrisk]]'s search functionality**. Distinct from [[party-application]]'s own current-state primary datastore (a separate Neo4j instance internal to the Party Application), and distinct from the [[data-universe]] (Snowflake-based analytics platform) — KG is a graph DB specifically scoped to InRisk-side search.

> ⚠ **Contradiction with 2026-04-22 lint pass** — the lint pass concluded KG was _"the Neo4j instance internal to [[party-application]]"_ and resolved [[open-questions#OQ-010]] on that basis. Per user clarification on 2026-05-26 (in the context of the [[sources/20260519-mdm-implementation-strategy]] ingest), this is incorrect: KG is a **sibling system** to party-application, also owned by [[graph-team]]. The earlier resolution is superseded by `OQ-010-R` on [[open-questions]]. See **Aliases / previous wiki framing** below.

## Aliases / previous names
- KG (handle Joe uses in-call)

### Previous wiki framing (superseded)
- _"Internal Neo4j of party-application"_ — this was the 2026-04-22 lint-pass framing; superseded 2026-05-26 per user clarification. Historical references on [[contract-buckets]], [[party-application]], [[party-rearch-dependency-map]], and the resolved row for OQ-010 on [[open-questions]] are being corrected as part of the [[sources/20260519-mdm-implementation-strategy]] ingest. Source pages are not rewritten (per AGENTS.md append-only spirit); their original text stays in place.

## Purpose

A graph representation of InRisk + Party data, used as the **search backend for InRisk**. Holds InRisk submission / requirement / client structure plus the corresponding party records, so that InRisk's search-time queries (party-by-name, party-by-broker, party-by-submission-context, etc.) can be served against a single graph rather than against InRisk + Party stores joined at query-time.

## Current state

- **Owner**: [[graph-team]] — sibling responsibility to [[party-application]] and [[party-curation-tool]].
- **Production status**: in production (powers InRisk search today).
- **Datastore kind**: graph DB. Specific engine (Neo4j vs another graph engine) pending ingest.
- **Data shape**: read-replica of InRisk + Party data. Per [[joe-worsfold]] in [[sources/20260519-mdm-implementation-strategy]]: _"KG is, like, isolated to — even the party information in KG is the party data from the in-risk side, so it's kind of like KG is actually just a graph representation of in-risk."_ Practical implication: KG's party data is sourced from the InRisk side rather than directly from [[party-application]].
- **Consumed by**: [[inrisk]] (search functionality — primary). Any other consumers TBD.
- **Eventual consistency**: KG's data is eventually consistent with its upstream sources (InRisk + Party). Joe's framing in-call: _"I just don't trust the eventual consistency problem of being accurate enough"_ for using KG as a source-of-truth for proxy-event reconstruction.

## Target state (under [[party-rearch]])

KG is **not** in scope for re-architecture under [[party-rearch]]. It is referenced because it is one of the candidate fallback options for the **proxy-event-with-InRisk-data design fork** (see [[open-questions#OQ-041]]):

- **Option A** (Joe's preference) — InRisk exposes an endpoint `client_id → {submissions, requirements}` that [[party-application]] calls when emitting a proxy event. KG is not involved.
- **Option B** (alternative considered, parked over consistency concerns) — [[party-application]] pulls the InRisk-side context from KG when emitting a proxy event. Avoids waiting on an InRisk-side endpoint, but inherits KG's eventual-consistency lag and creates a Graph-team-owned read-path dependency inside the proxy adapter.

Whichever option is chosen, KG itself is not modified. Phase-1 work does not change KG's role; if [[party-application]]'s primary store fully retires under [[party-rearch-phase-2]] and KG's party-side feed shifts source, that's a Phase-2+ consideration.

## Change ownership

- **Working unit**: [[graph-team]]
- **Workstreams in this programme**: only the OQ-041 design-fork conversation; otherwise out of scope.
- **Cross-team coupling**: [[prebind-team]] (consumer of the search functionality KG powers).

## Pending / unknown

- **Specific graph DB engine** (Neo4j is the default assumption given the Graph-team's stack, but not confirmed).
- **Upstream feed mechanism** — how InRisk + Party data lands in KG (event-driven, batch, CDC, etc.) is not yet captured in the wiki.
- **Other consumers** beyond [[inrisk]] search.
- **End-state direction** — if [[party-application]] migrates fully off Neo4j to Dynamo + OpenSearch in Phase 1, and InRisk's search needs eventually move to MDM-direct, KG's long-term role is not specified. Out of scope for [[party-rearch-phase-1]].
- **Resolution of [[open-questions#OQ-041]]** (KG-pull vs InRisk-endpoint for the proxy adapter) — if Option B is chosen, KG inherits a new responsibility as a read source for proxy events.

## Related decisions
_None directly. KG sits adjacent to [[strangle-the-graph-via-proxy-events]] only via OQ-041's design fork._

## Related
- [[graph-team]] — owner
- [[inrisk]] — primary consumer (search functionality)
- [[party-application]] — sibling Graph-team-owned application; not the same datastore as KG (correction recorded on the [[party-application]] identity page)
- [[party-curation-tool]] — sibling Graph-team-owned application
- [[open-questions#OQ-010]] / [[open-questions#OQ-010-R]] — the re-opened identity question, with the new (sibling-system) answer
- [[open-questions#OQ-041]] — the proxy-event-with-InRisk-data design fork that surfaces KG as Option B
- [[party-rearch-dependency-map]] — KG context restated in the current-state diagram
- [[contract-buckets]] — historical Knowledge-Graph framing under "data distribution" updated to reflect the correction
- [[sanctions-processing]] — sanctions-orchestration's proxy-event consumer chain interacts with whichever side of OQ-041 wins

## Sources
- [[sources/20260519-mdm-implementation-strategy]] — first source in this wiki that materially treats KG as a sibling system (rather than as an internal datastore of party-application). User-clarified on ingest: _"KG is Knowledge Graph - One of the Graph Databases that the Graph team own. It contains InRisk and Party data as a read-replica. It powers the InRisk search functionality."_
