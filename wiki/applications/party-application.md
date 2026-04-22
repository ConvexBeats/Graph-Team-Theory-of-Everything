---
type: application
title: Party Application
aliases: [graph, party-mdm, mdm]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, core]
owner: graph-team
state: current
sources: []
source_count: 0
status: stub
---

# Party Application

## Summary
The authoritative store and routing service for **business parties** (clients, brokers, underwriters, etc.) consumed across the insurance-deal lifecycle. Today it hands out large payloads that downstream systems snapshot. The re-architecture shifts it to a **versioned party reference** model — downstream systems receive a party ID + version and resolve details against the service near real-time.

## Aliases / previous names
- Graph
- Party MDM
- MDM

## Purpose
Provides version-controlled party records to every application that needs to display, route to, or transact against a business party. A single party can take multiple **roles** (client, broker, …) across different deal contexts.

## Current state
- **Owner**: [[graph-team]]
- **Production status**: in production, being refactored from the ground up
- **Integration shape**: downstream systems receive full party payloads, which are **snapshot-cached** on their side
- **Key consumers (known)**: [[inrisk]], [[party-curation-tool]]; [[high-volume]] on the roadmap
- **Tech stack**: _pending_
- **Key interfaces / APIs**: _pending_
- **Upstream dependencies**: _pending_ — Dun & Bradstreet integration is reached via [[party-curation-tool]]; unclear whether the Party Application itself consumes any upstream master data.

## Target state
- Downstream systems receive a **party ID + version**, not a payload.
- Downstream systems call back near-real-time to resolve details for display / use.
- Target data model co-defined with the [[inrisk-engine]] rewrite.
- Version semantics explicit enough that consumers can pin, refresh, or roll forward deterministically.

## Transitional states
> ⚠ Placeholder — the real transitional shape must be captured once design discussions start landing.

Various intermediary in-flight states will exist to keep current consumers operating ([[inrisk]] and the [[party-curation-tool]] workflow) while the model flips. Expect at least:
- A period where both the old payload contract and the new ID+version contract are served in parallel.
- A consumer-by-consumer migration rather than a big bang.

## Change ownership
- **Working unit**: [[graph-team]]
- **Co-defining**: [[devx-team]] (via [[inrisk-engine]] — target data model)
- **Affected consumers**: [[prebind-team]] (via [[inrisk]]), [[dataops-team]] (via [[party-curation-tool]]), **owner of [[high-volume]]** (unknown)

## Pending / unknown
- Current and target API contracts (schemas, endpoints, auth).
- Party version semantics: monotonic integer? semver-like? immutable snapshot per version?
- Caching policy downstream systems should adopt under the new model.
- SLA / latency budget for near-real-time resolution, especially under [[high-volume]] load.
- How party **roles** are modelled: per-deal vs. per-relationship vs. on the party record itself.
- Whether Dun & Bradstreet enrichment lands on the party itself or stays scoped to [[party-curation-tool]].

## Related decisions
_None yet._

## Related
- [[party-curation-tool]] — human-facing companion for editing parties
- [[inrisk-engine]] — co-defining target data model
- [[high-volume]] — forcing-function consumer
- [[graph-team]] — owner

## Sources
_None yet._
