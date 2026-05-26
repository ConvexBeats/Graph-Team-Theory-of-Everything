---
type: entity
title: Billy Calladine
aliases: [speaker-4, speaker-14, s1-speaker-4, s1-speaker-6]
created: 2026-04-22
updated: 2026-05-26
tags: [person, engineer]
team: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260514-inrisk-high-level-refinement, 20260519-mdm-implementation-strategy]
source_count: 4
status: draft
---

# Billy Calladine

## Summary
Developer / Engineer on the [[graph-team]] with the deepest knowledge of the current Graph DB event shape and the existing party-search widget. Attended both 2026-04-22 architecture sessions. In Session 1 (morning), provided the load-bearing insight that every InRisk event causes the Graph to rewrite the whole spine and emit a single event — the observation that made the strangler / proxy-event approach feasible ([[strangle-the-graph-via-proxy-events]]). **Formalised 2026-05-19 as the owner of the Phase-1 widget rebuild AND the corresponding InRisk-side dispatch-code change** ([[inrisk-cuts-over-before-high-volume]] Part B; [[party-rearch-ownership-matrix]] action #38).

## Key facts
- **Role**: Developer / Engineer
- **Team**: [[graph-team]]
- **Phase-1 remit (formalised 2026-05-19)**: widget rebuild for [[inrisk]] (raw React + close-to-raw CSS; one component, three parameterised variants — parties / brokers / other — in a shell container) **and** the InRisk-side dispatch-code change (the existing "which widget to call" logic). Pairs with [[joe-worsfold]] on the party-search side and with [[prebind-team]] on the InRisk-side integration.

## Relationships
- Works alongside [[alex-sillars]], [[joe-worsfold]], [[sergiu-postolachi]], and [[ben-joseph]] on the [[graph-team]].
- Pairs with [[prebind-team]] ([[john-trahearn]], [[kris-mokrzycki]], [[daria-romanovskaia]]) on the InRisk-side integration for the widget stories.
- Pairs with [[joe-worsfold]] + [[alex-sillars]] + [[daria-romanovskaia]] on the **widget-integration spike** (gates [[inrisk]] Stories 3/4/5).

## Claims
- Developer / Engineer on [[graph-team]] — user-declared (Pass B).
- Attended the 2026-04-22 Session 2 as Speakers 4 and 14 — [[sources/20260422-meeting-transcript-session-2]].
- Attended the 2026-04-22 Session 1 as Speakers 4 and 6 (transcription engine assigned two IDs) — [[sources/20260422-meeting-transcript-session-1]].
- **Spine-rewrite-on-every-InRisk-event insight** (Session 1): established that despite the Graph's complexity, DU effectively consumes one event shape. Directly unlocked [[strangle-the-graph-via-proxy-events]].
- Co-owner of the **Graph API consumer audit spike** with [[joe-worsfold]] — see [[open-questions#OQ-004]].
- **Confirmed at the 2026-05-14 InRisk High Level** that the existing party-search widget already supports the manual-create path needed by Story 1 (used in renewals today) — turns the InRisk-side change into a flag toggle plus removal of legacy logic.
- **Formalised 2026-05-19 as widget rebuild + InRisk-side dispatch-code owner** ([[sources/20260519-mdm-implementation-strategy]], [[party-rearch-ownership-matrix]] action #38). [[rory-beattie]]: _"Let's formalize that, Alex. Let's make it very clear to Billy that he's responsible for that, and we need him to kind of just own it and run with it."_ [[joe-worsfold]]: _"I think Billy's perfectly placed to help with this one, for party search and integration within RISC."_

## Current actions (open)
- **Widget rebuild** for [[inrisk]] (raw React + close-to-raw CSS; reuse existing InRisk components where possible). Pairs with [[joe-worsfold]] on the new widget component itself ([[party-rearch-ownership-matrix]] action #38 — Billy's half).
- **InRisk-side dispatch-code change** — update the existing "which widget to call" logic on InRisk's side to consume the new single-component-three-variants widget. Pairs with [[prebind-team]] for the integration ([[party-rearch-ownership-matrix]] action #38 — InRisk-side half).
- **Widget-integration spike** with [[joe-worsfold]] + [[alex-sillars]] + [[daria-romanovskaia]] — bottom out SDK posture, OpenSearch/Dynamo response shape, integration mechanic; gates [[inrisk]] Stories 3/4/5 ([[party-rearch-ownership-matrix]] action #29).
- **Graph API consumer audit spike** with [[joe-worsfold]] — [[open-questions#OQ-004]].

## Open questions
- Specialisation (backend / full-stack / data). Widget-rebuild remit suggests strong frontend / full-stack leaning.

## Related
- [[graph-team]] · [[alex-sillars]] · [[joe-worsfold]] · [[sergiu-postolachi]] · [[ben-joseph]]
- [[party-application]] · [[party-application-architecture]] · [[party-curation-tool]]
- [[inrisk]] · [[inrisk-architecture]] · [[prebind-team]] · [[john-trahearn]] · [[kris-mokrzycki]] · [[daria-romanovskaia]]
- [[inrisk-cuts-over-before-high-volume]] (Part B owner) · [[strangle-the-graph-via-proxy-events]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; spine-rewrite insight
- [[sources/20260422-meeting-transcript-session-2]] — afternoon
- [[sources/20260514-inrisk-high-level-refinement]] — confirmed widget already supports manual-create path
- [[sources/20260519-mdm-implementation-strategy]] — widget rebuild + InRisk-side dispatch-code change formalised as Billy's remit
