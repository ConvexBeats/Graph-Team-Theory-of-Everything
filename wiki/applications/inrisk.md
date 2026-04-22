---
type: application
title: InRisk
aliases: [ir2]
created: 2026-04-22
updated: 2026-04-22
tags: [application, affected, pas]
owner: prebind-team
state: current
sources: []
source_count: 0
status: stub
---

# InRisk

## Summary
The live **Policy Admin System (PAS)** for prebind. Captures all prebind policy information and is a current consumer of the [[party-application]]. Affected by the re-architecture because its party-resolution contract changes when the [[party-application]] moves from payload snapshots to ID + version references.

## Aliases / previous names
- IR2

## Purpose
Capture and manage prebind policy information across the insurance deal lifecycle.

## Current state
- **Owner**: [[prebind-team]] (informally *"The Strikers"*)
- **Production status**: in production, fully operational
- **Role in the re-architecture**: **affected consumer** of [[party-application]]
- **Party integration (current)**: receives full party payloads, snapshot-caches them locally

## Target state
- Long-term, InRisk will be superseded by the [[inrisk-engine]] rewrite. The end-state Party contract is being defined with that rewrite in mind, **not** with InRisk's current data model.
- InRisk's own position during the transition: keep working against the current Party contract until it is either migrated to the new contract or retired as [[inrisk-engine]] takes over.

## Transitional states
- InRisk must continue to receive party data in a compatible form while the Party Application runs the old and new contracts in parallel.
- Any shim or compatibility layer introduced for InRisk should be explicitly time-boxed and tied to the [[inrisk-engine]] readiness.

## Change ownership
- **Application owner**: [[prebind-team]]
- **Coordinates with**: [[graph-team]] (Party contract changes), [[devx-team]] (successor via [[inrisk-engine]])

## Pending / unknown
- Exact shape of the party payload InRisk consumes today.
- Whether InRisk snapshots parties at quote time, bind time, or both, and what refresh (if any) occurs after snapshot.
- Retirement timeline relative to [[inrisk-engine]] maturity.
- Whether InRisk participates directly in the migration or only via shim.

## Related decisions
_None yet._

## Related
- [[inrisk-engine]] — its successor
- [[party-application]] — current data supplier
- [[prebind-team]] — owner

## Sources
_None yet._
