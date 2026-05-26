---
type: entity
title: Simon Hulbert
aliases: [simon]
created: 2026-04-22
updated: 2026-05-26
tags: [person, architect]
team: high-volume-team
sources: [20260422-meeting-transcript-session-2, 20260519-party-integration-timelines]
source_count: 2
status: stub
---

# Simon Hulbert

## Summary
**Architect on the HV / [[artificial]] integration project**, attached in this wiki to [[high-volume-team]], and the **main contact for Party integration on the HV side**. Simon is the primary counterparty for [[graph-team]] / [[tech-tooling]] when shaping the new MDM API contract that [[high-volume]] (via [[artificial]], via [[boomi]]) will consume. Following the 2026-05-19 prep call, his role in the programme is broader than just HV-internal API alignment — he sits across HV + Artificial-onboarding + InRisk-touchpoint coordination, and is the Convex-side single-point-of-contact for the 2026-05-20 Convex × Artificial kick-off.

## Key facts
- **Role**: Architect (HV / [[artificial]] integration project)
- **Team**: [[high-volume-team]] _(working assumption — Simon was previously declared HV-team Architect in Pass B of [[sources/20260422-meeting-transcript-session-2]]; in 2026-05-19 he appears more as "Architect on the Convex × Artificial integration project", which sits inside or alongside HV team. Treating him as HV-team unless future source refines this.)_
- **Specific programme role**: main Party-integration contact for HV/Artificial; runs the Convex × Artificial integration relationship

## Relationships
- [[high-volume-team]] member.
- Primary counterpart for [[rory-beattie]] on Party-integration coordination, for [[graph-team]] ([[alex-sillars]], [[joe-worsfold]]) on API contract shape, and for the (yet-unnamed) Artificial-side PMs and technical contacts.
- Works closely with [[srini]] (Boomi Integration Architect on the HV/Artificial project) and [[luca]] (scheduling / coordination).
- Sits alongside the in-room **Convex – Orega – 6B Boardroom** PM coordinator on the HV/Artificial project (possibly [[luca]] — see [[open-questions#OQ-037]]).

## Claims
- Architect on [[high-volume-team]], main Party-integration contact — user-declared (Pass B of [[sources/20260422-meeting-transcript-session-2]]).
- Architect on the Convex × [[artificial]] integration project; broader than HV-internal API alignment — [[sources/20260519-party-integration-timelines]] (Simon ran the meeting from the HV/Artificial side, named Srini as Boomi Integration Architect, agreed cadence + Slack channel, and committed to sending integration diagrams).
- Holds top-level + sequence integration diagrams for HV/Artificial; to send the top-level diagram to [[rory-beattie]] first, followed by the sequence diagrams — Simon, [[sources/20260519-party-integration-timelines]].
- Confirmed InRisk-first cutover sequencing to the Artificial counterparty: _"the party stuff, but then we want to get in-risk over first … and they'll deliver, and then once they've delivered, we will roll out shortly after that"_ — Simon, [[sources/20260519-party-integration-timelines]]. Reinforces [[inrisk-cuts-over-before-high-volume]].
- Characterised Artificial's development cadence as minutes/hours/days — _"if everything's lined up perfectly, then it could take us minutes to do this, get this system working … and now they want to start, and they're like, well, give us access to all of your things"_ — Simon, [[sources/20260519-party-integration-timelines]].

## Open questions
- Simon's preferred integration patterns (event-driven vs sync API) for HV/Artificial use cases.
- HV / Artificial's expected Party-write volume and throughput shape — Simon is the right person to characterise this ([[open-questions#OQ-013]]).
- Whether HV / Artificial will consume any Party concerns beyond "look up / create party" (e.g. D&B enrichment visibility, version-change notifications, sanctions-result filtering).
- Whether Simon is **HV-team-proper** or sits in a separate integrations / FDA-adjacent function that supports HV alongside other vendor integrations — Sam (FDA for integrations) and [[srini]] (Boomi) being similarly cross-project is one possible structural pattern.

## Related
- [[high-volume-team]] · [[high-volume]]
- [[artificial]] — third-party vendor platform Simon is the Convex-side Architect for
- [[boomi]] — integration broker (Simon's day-to-day partner [[srini]] runs the connectivity)
- [[srini]] · [[luca]] — closely-collaborating HV-side counterparts
- [[graph-team]] · [[party-application]] · [[joe-worsfold]] · [[alex-sillars]]
- [[rory-beattie]]
- [[inrisk-cuts-over-before-high-volume]] — sequencing rule Simon has now restated to a customer
- [[strangle-the-graph-via-proxy-events]] — stability promise that Alex restated in Simon's company on 2026-05-19

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — first mention; HV-team architect framing.
- [[sources/20260519-party-integration-timelines]] — broader role surfaced (Convex × Artificial integration architect); named Srini as Boomi Integration Architect; committed to sending integration diagrams; restated cutover sequencing to Artificial.
