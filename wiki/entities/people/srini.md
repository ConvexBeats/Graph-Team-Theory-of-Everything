---
type: entity
title: Srini
aliases: []
created: 2026-05-26
updated: 2026-05-26
tags: [person, architect]
team: high-volume-team
sources: [20260519-party-integration-timelines]
source_count: 1
status: stub
---

# Srini

## Summary
**Srini** is the **Boomi Integration Architect on the HV / [[artificial]] integration project**. Owns the Convex-side Boomi connectivity work needed to gateway [[artificial]] → [[party-application]] MDM API for the Phase-1 cutover. First substantive surfacing in the wiki was [[sources/20260519-party-integration-timelines]] (2026-05-19); identified by first name only — surname TBD ([[open-questions#OQ-040]]).

## Key facts
- **Role**: Boomi Integration Architect
- **Team**: [[high-volume-team]] _(working assumption — Srini was introduced as being "on the [HV/Artificial] project"; whether he sits inside the HV team proper or in a shared integration / FDA function is not yet confirmed)_
- **Role in programme**: stands up [[boomi]] connectivity between [[artificial]] and [[party-application]] for the Phase-1 cutover; counterpart to [[joe-worsfold]] on the Party side

## Relationships
- Works with [[joe-worsfold]] on Boomi connectivity to [[party-application]] (pair-up agreed in [[sources/20260519-party-integration-timelines]]; [[joe-worsfold]] is the Party-side counterpart).
- Works for / with [[simon-hulbert]] on the HV/Artificial integration project; Simon flagged Srini as _"central"_ — _"he's gonna be involved in lots of this anyway"_ — and proposed Srini attend the Convex stand-ups as part of the integration team.
- Has done this before with other vendor providers: per [[simon-hulbert]], _"he's done this before lots of times with lots of different providers, so it shouldn't … and all he's doing is effectively a pass-through, so he'll get a link between Boomi and Party, and then he'll hand out the credentials to the … guys in artificial."_

## Claims
Every claim should cite a source.

- Srini is the Boomi Integration Architect on the HV/Artificial integration project — [[simon-hulbert]], [[sources/20260519-party-integration-timelines]].
- Srini will set up the [[boomi]] connectivity between [[artificial]] and [[party-application]], partnering with [[joe-worsfold]] on the Party side — [[simon-hulbert]] + [[alex-sillars]], [[sources/20260519-party-integration-timelines]].
- Srini's setup work is the **unblocking dependency** for Artificial to exercise the new MDM API: until credentials + security tokens are issued, Artificial's adoption can't begin even though the Party-side dev-env DynamoDB is ready with fake data — [[alex-sillars]], [[sources/20260519-party-integration-timelines]].
- _inference_: based on prior-vendor experience referenced by [[simon-hulbert]], Srini is likely **embedded across multiple Convex × vendor integration projects**, not solely on Artificial. Worth confirming.

## Open questions
- **Surname** — only "Srini" captured. Tracked at [[open-questions#OQ-040]].
- **Formal team** — sits inside [[high-volume-team]] proper, or in a separate integrations / FDA function that supports HV? Sam (FDA for integrations) is also adjacent to the HV/Artificial project and may share that home; see [[open-questions#OQ-024]] + [[open-questions#OQ-039]].
- **Scope of involvement** — likely involved in lots of Convex × vendor integrations; map of which ones is open.

## Related
- [[high-volume-team]] — current team affiliation (working assumption)
- [[boomi]] — the platform Srini is the Convex-side architect for
- [[artificial]] · [[high-volume]] · [[party-application]] — the integration this source surfaces
- [[joe-worsfold]] — Party-side counterpart on Boomi connectivity
- [[simon-hulbert]] — Architect on the HV/Artificial project that Srini is part of
- [[party-rearch-ownership-matrix]] action #33 — Srini's primary open action

## Sources
- [[sources/20260519-party-integration-timelines]] — first mention; established role + connectivity workstream + counterpart pairing
