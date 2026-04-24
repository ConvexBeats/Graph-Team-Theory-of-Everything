---
type: entity
title: PreBind Team
aliases: [strikers]
created: 2026-04-22
updated: 2026-04-22
tags: [team]
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: draft
---

# PreBind Team

## Summary
The working unit that owns [[inrisk]], the current production Policy Admin System (PAS) for prebind. Also referred to as **"The Strikers"**. Responsible for the three interim-state InRisk changes required by this programme (party-ID storage, messaging, broker retrieval). Also the domain-owner of the deferred **"party on requirement vs submission"** architectural choice — their Requirement → Submission data-flow spine is the thing that decision is about.

## Key facts
- **Kind**: internal engineering team
- **Aliases**: "The Strikers"
- **Role in programme**: owner of an **affected consumer** of [[party-application]]; responsible for the three interim-state changes to [[inrisk]]

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[daria-romanovskaia]] | Product Owner |
| [[john-trahearn]] | Tech Lead |
| [[kris-mokrzycki]] | Tech Lead |

_Other members pending._

## Applications owned
- [[inrisk]]

## Current workstreams
- **InRisk interim changes** (Phase 1): party-ID storage on InRisk's party table; Party-ID on outgoing InRisk messaging; broker retrieval moved direct from InRisk (URD path removed).
- Coordination with [[graph-team]] ([[joe-worsfold]]) on anti-patterns and data-fixes workshop.
- Maintain [[inrisk]] operational continuity through the transition.

## Deferred architectural decisions owned
- **Party on requirement vs submission** — explicitly a PreBind domain decision, not a Party-side decision. Requirement and Submission are two stages on the spine of their main data flow structure; any move of Party from Requirement-level to Submission-level is theirs to make. See [[inrisk]] / [[sources/20260422-meeting-transcript-session-2]].

## Correction note (Pass B)
- Pass A mis-attributed John and Kris to the "InRisk Engine / DevX side" — they are PreBind Team Tech Leads. Corrected.

## Relationships
- Depends on [[graph-team]] for Party data; primary counterpart for [[joe-worsfold]] on interim integration.
- [[devx-team]] is building [[inrisk-engine]] — the eventual successor to their application, but on the **final-state** Party contract, not interim.
- [[architecture-team]] ([[scott-gruber]]) provides SME input on the broker-retrieval flow.
- [[data-quality-team]] adjacent via [[dataops-team]] (curation quality affects InRisk's downstream data).

## Claims
- Owns [[inrisk]] — user-declared.
- [[john-trahearn]] and [[kris-mokrzycki]] are Tech Leads on this team — user-declared (Pass B).
- [[daria-romanovskaia]] is Product Owner on this team — user-declared (lint pass).

## Open questions
- Team size and composition beyond the known PO and two Tech Leads.
- Stance on InRisk's retirement timeline — who decides when [[inrisk]] sunsets in favour of [[inrisk-engine]]?

## Related
- [[inrisk]] · [[inrisk-engine]]
- [[daria-romanovskaia]] · [[john-trahearn]] · [[kris-mokrzycki]]
- [[graph-team]] · [[devx-team]] · [[architecture-team]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
