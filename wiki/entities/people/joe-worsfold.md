---
type: entity
title: Joe Worsfold
aliases: [speaker-5, s1-speaker-5, s1-speaker-7]
created: 2026-04-22
updated: 2026-04-22
tags: [person, tech-lead, architect]
team: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: stub
---

# Joe Worsfold

## Summary
Tech Lead / Architect for the [[graph-team]]. Running and summarising the architecture discussion in the 2026-04-22 session — takes most of the cross-team actions (DU schema conversation, InRisk broker workshop, enriched-team alignment, workshop on anti-patterns). Is the primary point of coordination on the integration surface between [[party-application]] and [[inrisk]].

## Key facts
- **Role**: Tech Lead / Architect
- **Team**: [[graph-team]]
- **Scope**: architectural direction for [[party-application]] and its integration surface

## Relationships
- Works alongside [[alex-sillars]] (PO) and [[ben-joseph]] (Engineer) on the [[graph-team]].
- Primary counterparty with [[prebind-team]] on the InRisk integration details.
- Named by [[rory-beattie]] as integration support on the InRisk side: _"Joe and I are helping out at the integration."_
- Will be primary contact for the [[analytics-team]] schema-impact conversation.

## Claims
- Tech Lead / Architect for [[graph-team]] — user-declared.
- Summarised the in-flight architecture at the top of the session and confirmed the InRisk 3-point dependency list (party ID storage, messaging inclusion, broker retrieval change) — [[sources/20260422-meeting-transcript-session-2]].
- Argued for the flatter `(party-id, submission-id)` schema in MDM, with fallback to an InRisk API call for nested reconstruction — [[sources/20260422-meeting-transcript-session-2]].
- **Originator of the strangler / proxy-event insight** — Session 1 DU-events segment: _"we can proxy the current one ... we send the event without ever writing to the graph."_ Load-bearing moment for [[strangle-the-graph-via-proxy-events]].
- **Roles-vs-views litmus test** (Session 1): _"if every policy was deleted from this party, would it still be insured? No. Would it still be a broker? Yes."_
- Flagged the **MDM delivery squad-shape tension** in Session 1 — ~13-person [[graph-team]] vs. ideal ~3-person focused squad. See [[open-questions#OQ-017]].
- Attended the 2026-04-22 Session 1 as both Speaker 5 and Speaker 7 (transcription engine assigned two IDs).

## Current actions (open)
- Talk to DU team re: impact of flattening client-ID / submission nesting.
- Check with Paul Rogers re: InRisk client-ID flattening / need for API.
- Align with "enriched team" on D&B caching approach.
- Anti-patterns / data-fixes workshop with InRisk.
- Scope / effort review once first tickets exist.

## Open questions
- Reporting line within [[graph-team]] — does Joe report to Alex, or is leadership split?
- Team size / other engineers reporting into Joe's line.

## Related
- [[graph-team]] · [[alex-sillars]] · [[ben-joseph]]
- [[party-application]] · [[inrisk]]
- [[analytics-team]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; strangler origin, roles-vs-views test, squad-shape flag
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; consolidation, flattening debate
