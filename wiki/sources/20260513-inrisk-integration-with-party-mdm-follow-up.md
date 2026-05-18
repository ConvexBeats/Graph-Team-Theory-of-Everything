---
type: source
title: InRisk Integration with Party MDM — Follow-up (2026-05-13)
kind: meeting
created: 2026-05-18
updated: 2026-05-18
tags: [meeting, party-rearch, phase-1, inrisk]
raw: raw/20260513 - InRisk Integration With Party MDM Follow Up.md
author: [rory-beattie, joe-worsfold, john-trahearn, kris-mokrzycki, suzanna-whitefield, sergiu-postolachi]
date_of_source: 2026-05-13
project: party-rearch
status: draft
---

# InRisk Integration with Party MDM — Follow-up (2026-05-13)

## Summary
Third design call in the [[party-rearch]] arc and the first dedicated specifically to the **InRisk side of Phase 1**. [[john-trahearn]] brought a concrete **Party MDM Integration epic with four (likely five) stories**; the room landed several material decisions: **party tagging is in scope** for Phase 1 (via the existing widget pattern, with extra fields), **feature tagging is out of scope** (the old backend stays alive after cutover, eradication deferred to a later phase as an InRisk-specific change), **InRisk's MDM cutover lands before HV's 1-Sep go-live** as a separate deliverable (≥ 2 weeks buffer minimum), and **[[joe-worsfold]] will publish a second, design-system-agnostic component library** for InRisk to consume so InRisk doesn't have to absorb Chakra 3 + design-system risk under the September gate. The call also surfaced **sanctions / [[ntt]] / Boomi** as a new wiki strand: Boomi has absorbed sanctions logic on top of Party events, is firing on every party change including irrelevant ones, calling InRisk one submission at a time because no batch endpoint exists. Audit's eyes on sanctions this year; group consensus is that this logic is in the wrong place.

## Bibliographic info
- **Author(s) / attendees**: [[rory-beattie]], [[joe-worsfold]] ([[graph-team]] Tech Lead), [[john-trahearn]] ([[prebind-team]] Tech Lead), [[kris-mokrzycki]] ([[prebind-team]] Tech Lead), [[suzanna-whitefield]] ([[architecture-team]] Architect), [[sergiu-postolachi]] ([[graph-team]] Scrum Master, brief). [[billy-calladine]] mentioned as Thursday's high-level attendee but not in this room.
- **Date**: 2026-05-13, 11:06–11:53 (~47 min)
- **Venue / source system**: video call transcript (machine-generated)
- **Raw file**: `raw/20260513 - InRisk Integration With Party MDM Follow Up.md`

## Key claims

### InRisk Phase-1 scope is now a concrete epic + 4–5 stories
1. **Epic**: "Party MDM Integration" — [[john-trahearn]] condensed the previous call's notes into this structure. _"Hot off the press, basically, for this call."_
2. **Story 1 — Remove manual client creation on new requirement.** Tech-debt clearance: route manual creation via the widget instead of falling back to InRisk pages. John believes the existing widget already supports this (used elsewhere in InRisk); [[billy-calladine]] to confirm at Thursday's high-level session.
3. **Story 2 — Backwards-compatible data-model addition.** Store party-ID + version-IDs on InRisk's existing **client and broker tables**, plus any required DTO updates. Snapshots and existing data are retained — this story does **not** cover the work to keep extracting today's fields from the new feed; that's a separate concern about the new feed's response shape.
4. **Story 3 — Client-widget integration, feature-flagged.** Flag-on: render the new MDM widget and persist the new IDs flowing back; flag-off: behave exactly as today.
5. **Story 4 — Broker-widget integration, feature-flagged.** Mirror of story 3 for brokers. John flagged that 3 and 4 might collapse to one story depending on complexity.
6. **Story 5 (raised in-call) — Party-tagging integration.** Triggered by Joe's note that party tagging will also need MDM-side and widget-side support; John agreed to add it as a distinct story. The widget side requires Joe-side work; commonality with today's widget TBC.

### Party tagging vs feature tagging — clarified, with Phase-1 scope implications
1. **Party tagging** = tagging a party as e.g. obligor / loss-leader within InRisk — _legitimately party data_. Phase-1 in scope. Uses the existing widget pattern; Phase-1 work adds the new MDM fields.
2. **Feature tagging** = submission-level attributes that just happened to be built on Party's UX/DB pattern because Party had a "nice widget" at the time. _"It's nothing to do with parties. But it's stored in the old RT, yeah, tables, so."_ (Joe)
3. **Phase-1 decision on feature tagging — out of scope.** Joe will not decommission old-party's feature-tagging backend at cutover; InRisk continues pulling its static list from the existing table + widget. Removal is deferred. **Two contradictions to flag**: (a) reinforces [[feature-tagging-moves-to-inrisk]], whose direction is unchanged but whose framing now includes the explicit "old backend stays alive past cutover" carve-out; (b) the suggested long-term home shifts subtly — the room leant toward "a subsequent phase, probably an InRisk-specific change, not [[inrisk-engine]]" rather than a clean handover ADR.
4. **Static-list hypothesis raised by Suzy** — if no new attributes have been added in two years, feature tagging may now be effectively a static list and could be moved into InRisk classifications or similar with much less effort than originally feared. Suzy to investigate; trigger condition for [[open-questions#OQ-019]] now has a concrete branch to explore.
5. **Origin history mentioned (Suzy / Rory)**: feature tagging was built quickly to capture additional attributes that InRisk could not absorb in time on its CDM project. CDM is **out of scope** for this wiki per user direction.

### Cutover shape sharpened — InRisk lands before HV
1. **No parallel running for party create / search.** Joe: a single source of truth is required because they're designing a one-way mapper (MDM → old graph). Two-way mapping creates a data-problem mess.
2. **Brief dual-write to the old graph during the cutover window** — Joe will keep writing to the old graph from MDM so a revert remains safe. This is a **refinement of [[strangle-the-graph-via-proxy-events]]**, not a contradiction: the strangler still runs one-way (MDM is source of truth), but for the cutover window dual-write provides revertability rather than parallel sources of truth. Non-prod environments are easier to switch back and forth.
3. **InRisk-first cutover.** The party-side cutover is gated on InRisk being live on MDM **first**, and HV switches on later. The reverse (HV on MDM with InRisk still on graph) was floated and rejected — it would push the curation problem further down the line and increase complexity. Promoted to its own ADR ([[inrisk-cuts-over-before-high-volume]]) per Rory's call: _"the Sep 1st date is a hard date for the High Volume component, the InRisk integration now needs to happen in advance of that and so is a new deliverable prior to the actual cutover for High Volume."_
4. **Buffer ≥ 2 weeks before 1 Sep, minimum.** Rory: _"how long do you need to be sure? How long is a piece of string? It's, I mean, I would say at least a fortnight before. Just to be live. Um… but that's minimum, right?"_ Concrete InRisk-cutover date TBC.
5. **Sequencing tradeoff acknowledged.** Joe: _"I feel like that would be just great. We just move the migration problem further down and make it more complex than if we do in risk first."_ InRisk-first keeps the migration problem bounded.

### Chakra V2/V3 (OQ-005) materially resolved
1. **InRisk does not use any design system today.** Chakra-2 components are used as components, not as a design system. (Kris: _"we don't use any design system on NRISC, right? Which means that it's not the same thing."_)
2. **Joe's new widget is Chakra-3 with a design system** — built for [[party-curation-tool]] / [[dataops-team]].
3. **Decision in-call**: don't introduce Chakra 3 + design system into InRisk under the September gate. Group consensus that the design-system path adds too much complexity to in-flight Phase-1 work in an unfamiliar styling stack.
4. **Joe will publish a second component library** matched to InRisk's current look — design-system-agnostic — for InRisk to consume. Joe explicitly accepted maintaining two component libraries as a manageable cost compared with forcing the design-system migration into InRisk now.
5. **HV does not use a widget** — API only — so HV is not affected by either library decision. Joe: _"they'll go just to API."_
6. **Captured as part of the new ADR** ([[inrisk-cuts-over-before-high-volume]]) rather than its own ADR — the design-system call is part of the cutover-readiness story for InRisk. This resolves [[open-questions#OQ-005]].

### Sanctions / NTT / Boomi — new strand surfaced
1. **NTT is the application that performs sanctions processing** — the check that determines whether a client may have sanctions placed on them, run before any business can be done with that party. NTT is accessed via API. Captured as a platform: [[ntt]].
2. **Boomi has absorbed sanctions logic that doesn't belong to it.** Boomi sits on top of Party events and fires on every party change, even when irrelevant to sanctions. Because Boomi doesn't hold submission context, it then calls InRisk's API **one submission at a time** (apparently no batch endpoint exists) to reconstruct what it needs, and is building its own cache + idempotency checking on top of that. Joe: _"they're building, like, an entire, like, cash and item potency checking system."_ Concept captured as [[sanctions-processing]].
3. **Plan in Phase 1 is to proxy these events initially** (consistent with [[strangle-the-graph-via-proxy-events]]); over time, the goal is to stop firing the events unnecessarily.
4. **Group consensus: wrong location for sanctions logic.** Kris: _"sanctions and firing, you know, bulk, fake, let's say false positive changes that are having cascading effect on us."_ Joe: _"I don't see the benefit of Boomi in lots of different places that we currently have it."_ Rory: _"It's definitely in the wrong place, right?"_
5. **Sanctions should likely be its own domain / service.** Rory: _"we need to pull out the sanctions logic into its own service that is held independently and we work out who the owner of that is later, right? Because it interacts with multiples of our apps."_ Joe agrees the logic doesn't belong in Party (doesn't want Party to absorb submission data) nor in InRisk (doesn't want InRisk to feel responsible for sanctions).
6. **Audit pressure**: Suzanna — _"I found out today that Audit's eyes are going to be on sanctions this year."_ Raises priority on resolving the ownership question.
7. **Action**: Rory + Suzanna to escalate to [[andrea-read]]. Not a Phase-1 deliverable; new open question ([[open-questions#OQ-032]]).

### Smaller technical clarifications
1. **MDM runs on the existing AWS estate** — same 3 accounts × 4 environments InRisk uses. **Not** on AWS 2.0 yet (Joe would have preferred AWS 2.0 but it's not ready). Removes a potential cross-account integration complication Rory was worried about.
2. **MDM widget integration mechanic likely SDK-based.** Joe described instantiating an MDM SDK and passing methods / hooks to the components, distinct from today's query-parameter-driven flow on InRisk's existing widget. Mechanic alignment is a Thursday-high-level item.
3. **MDM widget is already published.** John can start using it; format / spec details to be agreed in the high-level / low-level sessions.
4. **Widget-data alignment is open** (Sergiu's question). Joe will need to confirm the widget's response shape includes everything InRisk needs in particular — _"is OpenSearch indexed slightly differently to how it's stored in Dynamo"_ — and may need to adjust the response when InRisk starts using it. Tracked at [[open-questions#OQ-036]].
5. **Anna's team (UX) involvement deferred.** Joe suggested bringing Anna's group in for UX input on the new widget. Rory pushed back — adds too much complexity to layer pretty-up onto cutover. Keep the new UX similar / identical for Phase 1; UX improvements come later, decoupled from this work. Anna's identity tracked at [[open-questions#OQ-033]].
6. **Joe scaling MDM team involvement** beyond himself + [[ben-joseph]] + (infrequent) [[billy-calladine]] — Billy taking on the curation UI work alongside the widget, knowledge-sharing intent across the [[graph-team]].

## Decisions made

- **New ADR**: [[inrisk-cuts-over-before-high-volume]] (accepted) — InRisk's MDM-integration goes live ≥ 2 weeks before 1 Sep HV go-live; rolls in the design-system call (Joe maintains two component libraries; InRisk consumes the design-system-agnostic one).
- **Refinement** to [[strangle-the-graph-via-proxy-events]] — brief dual-write to old graph during the cutover window for revertability; not a contradiction of "no dual-running", a clarification of the cutover-window behaviour.
- **Refinement** to [[feature-tagging-moves-to-inrisk]] — Phase-1 carve-out is explicit (old backend stays alive past cutover); Suzy investigating whether feature tagging is now effectively a static list, which may simplify the eventual migration into a classifications-style InRisk change rather than a Postgres-table handover.
- **Party tagging in Phase 1, feature tagging out of Phase 1** — recorded inline on [[inrisk]], [[party-rearch-phase-1]], [[party-rearch-phase-1-summary]]; not a standalone ADR (Phase-1 boundary refinement of existing decisions).

## Actions

| Action | Owner | Application | State |
|---|---|---|---|
| Bring InRisk Phase-1 epic ("Party MDM Integration") + 4–5 stories through Thursday high-level (this week) and Tuesday low-level (next week); sprint starts following Wednesday | [[john-trahearn]] | [[inrisk]] | open |
| Brush up on InRisk's current widget integration mechanic before Thursday's high-level (current query-parameter flow vs SDK-style new mechanic) | [[john-trahearn]] | [[inrisk]] | open |
| Attend Thursday's InRisk Phase-1 high-level to validate widget commonality and integration mechanic | [[billy-calladine]] | [[inrisk]] · [[party-application]] | open |
| Publish a second component library — design-system-agnostic — for [[inrisk]] to consume | [[joe-worsfold]] | [[party-application]] | open — feeds [[inrisk-cuts-over-before-high-volume]] |
| Confirm widget-response shape covers all fields InRisk needs when consuming the new widget; iterate as integration starts | [[joe-worsfold]] | [[party-application]] · [[inrisk]] | open — [[open-questions#OQ-036]] |
| Investigate whether feature tagging can be treated as a fully static list (no dynamic management), potentially simplifying its later migration into InRisk classifications | [[suzanna-whitefield]] | [[inrisk]] | open — feeds [[open-questions#OQ-019]] |
| Escalate sanctions / Boomi ownership to [[andrea-read]] | [[rory-beattie]] · [[suzanna-whitefield]] | [[ntt]] · [[party-application]] · [[inrisk]] | open — [[open-questions#OQ-032]] |
| Speak to Anna ahead of cutover to warm users to the UX change | [[rory-beattie]] | [[party-curation-tool]] · [[inrisk]] | open — Anna identity: [[open-questions#OQ-033]] |
| Reconvene as a group next week (week of 2026-05-18) to review high-level and low-level outcomes and unblock anything pending | [[rory-beattie]] | [[inrisk]] · [[party-application]] | open |

## Applications mentioned
- [[party-application]] — MDM widget already published; Joe to add a second design-system-agnostic library; AWS estate clarified as the existing 3-account / 4-env world.
- [[inrisk]] — epic + 4–5 stories now concrete; party-tagging in scope, feature-tagging deferred; SDK-style widget integration; sanctions-on-Boomi pressure point.
- [[party-curation-tool]] — primary consumer of Joe's Chakra-3-plus-design-system widget (the other library).
- [[high-volume]] — confirmed API-only, no widget; turns on after InRisk cutover.
- [[inrisk-engine]] — referenced obliquely as "future" consideration on feature tagging; not in Phase-1 scope (unchanged).

## Entities mentioned
- [[joe-worsfold]] · [[john-trahearn]] · [[kris-mokrzycki]] · [[suzanna-whitefield]] · [[rory-beattie]] · [[sergiu-postolachi]] · [[billy-calladine]] (referenced) · [[ben-joseph]] (referenced) · [[andrea-read]] (escalation target) · [[antonie-labuschagne]] (Joe: should "have it in mind in the future" re: feature tagging — disambiguation, name appeared as "Antoine" in transcript) · [[graph-team]] · [[prebind-team]] · [[architecture-team]]
- **First-name-only references**: Anna (UX team lead — [[open-questions#OQ-033]]); Marty (sanctions / Boomi context — [[open-questions#OQ-034]]); Sarah Faraway, Scott (mentioned in feature-tagging origin history, Scott resolves to [[scott-gruber]]).
- **New platform page**: [[ntt]] (vendor sanctions-check application accessed via API).

## Concepts mentioned
- [[sanctions-processing]] (new) — the domain itself; Boomi's role as current orchestrator flagged as wrong-place.
- [[strangle-the-graph-via-proxy-events]] — refined with the dual-write-during-cutover nuance.
- [[contract-buckets]] — sanctions is an `enrichment`-adjacent flow; remains correctly placed.

## Contradicts
- [[sources/20260422-meeting-transcript-session-1]] · [[sources/20260422-meeting-transcript-session-2]] — implicit "no dual-running" reading of [[strangle-the-graph-via-proxy-events]] is refined here: a short dual-write window to the old graph during cutover is acceptable for revertability. Not a substantive contradiction — captured as a refinement on the ADR rather than a `⚠ Contradiction` callout.

## Reinforces
- [[sources/20260422-meeting-transcript-session-2]] — InRisk's interim changes (party-ID storage on its client/broker tables; party-ID stamped on outgoing messaging) are reinforced by the data-model story. Broker-retrieval workshop dependency remains open.
- [[feature-tagging-moves-to-inrisk]] — direction reinforced; framing slightly evolved (target home likely InRisk-classifications rather than InRisk-Postgres-table; trigger condition still TBD via [[open-questions#OQ-019]]).
- [[pct-and-mdm-go-live-together]] — coupled rollout unchanged; this call adds the InRisk-cutover-precedes-HV layer on top.

## My notes (LLM)

- **Cutover sequencing is the single biggest delta of this source.** The implicit Phase-1 mental model has been "MDM + PCT + HV all converge on 1 Sep" — this source explicitly splits it into "InRisk-first cutover, mid-August target, then HV at 1 Sep". Worth flagging on the phase page so sprint-planning shifts accordingly.
- **Sanctions is a wholly new strand for the wiki.** Prior coverage was a one-line "sanctions via Boomi → NTT" on [[party-rearch-dependency-map]] and a future-state stub on [[party-application]]. This source adds: domain-shape framing, audit pressure this year, "wrong place" consensus, and an escalation action with a named escalation target. Strong candidate to become a Phase-2+ workstream of its own.
- **OQ-005 closes cleanly.** The two-component-libraries decision settles the Chakra V2/V3 question without further workshopping — Joe takes the work onto his side, InRisk doesn't have to absorb Chakra 3 + design-system risk under the September gate. Recording the resolution carefully because the prior framing of the OQ ("V2, minimal-render, or V3-only-when-InRisk-upgrades") suggested an InRisk-side fix; the resolution is a Party-side fix instead.
- **OQ-019 advances rather than resolves.** Suzy's static-list investigation could simplify the eventual migration substantially. Worth holding the OQ open with the new branch noted.
- **Transcription artefact**: "Antoine" in the transcript is [[antonie-labuschagne]] (Anton) — same drift pattern as previous sessions where the model produced surrogate spellings.

## Open questions
_Most carried back into [[open-questions]] with stable IDs; listed here for source-page completeness._

- [[open-questions#OQ-032]] (new) — sanctions-domain location. Where should the orchestration logic that today lives in Boomi actually live? Own service? InRisk? Party? Audit visibility this year is the forcing function.
- [[open-questions#OQ-033]] (new) — **Anna** identification. UX/product lead working on alternative party UX.
- [[open-questions#OQ-034]] (new) — **Marty** identification. Sanctions / InRisk-adjacent figure referenced by Joe; presumed to be a Boomi-side or sanctions-domain stakeholder.
- [[open-questions#OQ-035]] (new) — InRisk MDM-cutover **concrete date** (mid-August target; ≥ 2-week buffer minimum); when does feature-flag flip in production?
- [[open-questions#OQ-036]] (new) — Widget-response field alignment. Sergiu's question to Joe — will the new widget's response shape include everything InRisk needs, given OpenSearch is indexed differently than Dynamo storage? Iterate as InRisk integration begins.
- [[open-questions#OQ-019]] (advanced) — Feature-tagging migration timing. New branch: if feature tagging is now a static list (Suzy's investigation), the migration becomes an InRisk-classifications-style change rather than a Postgres-table handover.
- [[open-questions#OQ-005]] (resolved by this source) — Chakra V2/V3 widget strategy. Resolution: Joe maintains two component libraries; InRisk consumes the design-system-agnostic one; design-system widget is for PCT/DataOps.
