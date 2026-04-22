---
type: application
title: High Volume
aliases: [hv]
created: 2026-04-22
updated: 2026-04-22
tags: [application, forcing-function, in-development]
owner: unknown
state: target
sources: []
source_count: 0
status: stub
---

# High Volume

## Summary
An in-development application for processing large volumes of insurance deals at higher pace. **The forcing function for this re-architecture**: the new [[party-application]] contract must be in place ahead of High Volume's go-live.

## Aliases / previous names
- HV

## Purpose
Process large volumes of deals at higher pace than current systems support.

## Current state
- **Owner**: **unknown** — to be confirmed
- **Production status**: in development
- **Role in the re-architecture**: primary motivating consumer of the target [[party-application]] contract

## Target state
- Consumes [[party-application]] under the new ID + version contract from day one.
- Scale and throughput characteristics likely drive the latency / caching requirements of the new Party contract (see [[party-application]] → Pending).

## Transitional states
- N/A — HV is new, not migrating from a current state.

## Change ownership
- **Working unit**: **unknown**
- **Coordinates with**: [[graph-team]] (Party consumer contract), possibly [[devx-team]] if HV consumes via [[inrisk-engine]] APIs rather than Party directly

## Pending / unknown
- **Owning working unit** — must be identified.
- Target go-live date (the hard milestone for this programme).
- Integration shape: does HV consume [[party-application]] directly, or via [[inrisk-engine]]?
- Throughput / QPS expectations against the Party Application.
- Whether HV has its own curation needs or relies entirely on [[party-curation-tool]].

## Related decisions
_None yet._

## Related
- [[party-application]] — target data supplier
- [[inrisk-engine]] — possible intermediary
- [[overview]] — HV go-live is the programme's key milestone

## Sources
_None yet._
