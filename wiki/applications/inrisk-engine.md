---
type: application
title: InRisk Engine
aliases: [inrisk-core, new-inrisk]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, target-state, backend]
owner: devx-team
state: target
sources: []
source_count: 0
status: stub
---

# InRisk Engine

## Summary
A ground-up rewrite of [[inrisk]], backend-only and API-first. Its significance to this programme is that **its target data model is being co-defined with the [[party-application]] target data model** — so the Party Application's end-state must align with what InRisk Engine needs, not with what current [[inrisk]] expects.

## Aliases / previous names
- InRisk Core
- New InRisk

## Purpose
Replace legacy [[inrisk]] with a modern API-first backend that cleanly consumes the new versioned party model and exposes policy-admin capabilities as APIs rather than a monolithic UI-bound application.

## Current state
- **Owner**: [[devx-team]]
- **Production status**: concept + a selection of prototypes (not yet production)
- **Role in the re-architecture**: defines what "target state" means for the [[party-application]]'s consumer contract
- **Scope**: backend only

## Target state
- API-first service replacing [[inrisk]]'s legacy data model.
- Consumes [[party-application]] via the new ID + version contract from day one.
- No snapshot caching of party payloads (or, if cached, only by ID+version with clear invalidation semantics).

## Transitional states
- Prototypes → component-level usable APIs → parity with [[inrisk]] on enough functionality to begin migration.
- During transition, both [[inrisk]] and InRisk Engine may exist in parallel; the [[party-application]] must serve both.

## Change ownership
- **Working unit**: [[devx-team]]
- **Coordinates closely with**: [[graph-team]] on the target Party contract
- **Eventually replaces**: [[inrisk]] ([[prebind-team]])

## Pending / unknown
- Concrete API surface — what's in scope for v1 of the Engine.
- Prototype status — what's been proven, what's still open.
- Migration strategy from [[inrisk]] to InRisk Engine (strangler? side-by-side? domain-by-domain?).
- Relationship (if any) to [[high-volume]] — does HV consume Party directly, or via InRisk Engine APIs?

## Related decisions
_None yet._

## Related
- [[inrisk]] — predecessor
- [[party-application]] — co-defined target contract
- [[devx-team]] — owner

## Sources
_None yet._
