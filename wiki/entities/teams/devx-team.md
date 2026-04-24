---
type: entity
title: DevX Team
aliases: []
created: 2026-04-22
updated: 2026-04-22
tags: [team]
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: draft
---

# DevX Team

## Summary
The working unit building [[inrisk-engine]], the API-first rewrite of [[inrisk]]. Targets the **final state** of the Party Application contract — not the interim. Goes live when the final-state Party contract is realised. Does **not** participate in this programme's Phase 1 interim work.

## Key facts
- **Kind**: internal engineering team
- **Role in programme**: consumer of the **final-state** Party contract; not in interim scope

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[antonie-labuschagne]] | Tech Lead |

_Other members pending._

## Applications owned
- [[inrisk-engine]]

## Current workstreams
- Prototype → production path for [[inrisk-engine]], designed against the final-state Party contract.
- Not working on interim MDM integration.

## Correction note (Pass B)
- Pass A described DevX as "co-defining" the target Party model. Corrected per user guidance: [[graph-team]] defines the final-state contract; DevX consumes that definition. [[devx-team]]'s go-live is gated on that final state.
- **John & Chris** were incorrectly attributed to DevX in Pass A — they are PreBind Team Tech Leads ([[john-trahearn]], [[kris-mokrzycki]]), not DevX.

## Relationships
- Consumes the final-state Party contract from [[graph-team]] — not a builder within Phase 1.
- Eventually supersedes [[prebind-team]]'s [[inrisk]] with [[inrisk-engine]].

## Claims
- Owns [[inrisk-engine]] — user-declared.
- [[antonie-labuschagne]] is Tech Lead on this team — user-declared (Pass B).

## Open questions
- Team size and composition beyond [[antonie-labuschagne]].
- Current prototype coverage / readiness of [[inrisk-engine]].
- What specific deliverables DevX needs from this programme on the final-state Party contract (event model, version semantics, role schema).

## Related
- [[inrisk-engine]] · [[antonie-labuschagne]]
- [[graph-team]] · [[prebind-team]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
