---
type: source
title: InRisk High Level Refinement (2026-05-14)
kind: meeting
created: 2026-05-26
updated: 2026-05-26
tags: [meeting, party-rearch, phase-1, inrisk, high-level]
raw: raw/20260514 - InmRisk High Level Refinement.md
author: [amardeep-badhan, john-trahearn, joe-worsfold, billy-calladine, kris-mokrzycki, jason-owen, andrew-turner, katarina-voskarova, rastislav-sepelak]
date_of_source: 2026-05-14
project: party-rearch
status: draft
---

# InRisk High Level Refinement (2026-05-14)

## Summary
The Thursday-after-the-follow-up. The [[prebind-team]] convened the wider InRisk high-level call and [[john-trahearn]] walked the 5-story **Party MDM Integration epic** through the room. The epic is now ordered and concrete: **Story 1 (remove manual client creation on new requirement) and Story 2 (data-model migration: party-ID + version-ID on existing party, broker, and party-snapshot tables) are ready for low level immediately**; **Stories 3/4/5 (widget integration: client / broker / party-tagging) are gated on a spike** between [[joe-worsfold]] and the InRisk side because integration mechanics, response-shape alignment, and SDK posture all need to be agreed before any low-level work. [[joe-worsfold]] explicitly rolled back his Chakra-3-with-design-system vision for InRisk and committed to a **bespoke, parity-only widget for InRisk** matching the current auth / RBAC / session flow — a **drop-in replacement now, enhancement later as a separate change**. Joe also flagged the **InRisk-side backfill question** at the end of the call (backfill historic client / broker / party-snapshot records with MDM IDs, or null and go-forward from cutover?) with sanctions impact called out. The wider attendance brought new InRisk-side names into the wiki: [[amardeep-badhan]] (Scrum Master, chaired the call), [[andrew-turner]] (PO, ran the agenda; resolves [[open-questions#OQ-030]] — "Andrew Tennant" was a transcription drift), [[jason-owen]] (BA, asked the Lloyd's-broker and TOBA-status questions), [[rastislav-sepelak]] (BA), [[katarina-voskarova]] (BA).

## Bibliographic info
- **Author(s) / attendees**: [[amardeep-badhan]] (PreBind Scrum Master, chaired the call), [[john-trahearn]] (PreBind TL, drove the agenda), [[joe-worsfold]] (Graph TL), [[billy-calladine]] ([[graph-team]] engineer, ex-Striker; widget Q&A), [[kris-mokrzycki]] (PreBind TL), [[jason-owen]] (PreBind BA), [[andrew-turner]] (PreBind PO, ran wider HL agenda — _departed 2026-05-29 → succeeded by [[daria-romanovskaia]]; forward-looking PO references redirect per [[AGENTS|§5.5]]_), [[katarina-voskarova]] (PreBind BA, "Kati"), [[rastislav-sepelak]] (PreBind BA, "Rasto").
- **Date**: 2026-05-14, ~27 min (Party-MDM portion: ~26 min; remaining 1 min was Andrew handing the wider InRisk high-level back to the agenda)
- **Venue / source system**: video call transcript (machine-generated VTT)
- **Raw file**: `raw/20260514 - InmRisk High Level Refinement.md`

## Key claims

### The Party MDM Integration epic is now fully scoped and ordered
1. **Five stories, two ready, three gated on a spike.** Stories 1 and 2 (manual-client-creation cleanup + data-model migration) are taken to low level immediately; Stories 3, 4, 5 (widget integration for client / broker / party-tagging) wait for a spike between Joe and the InRisk side. John: _"from our side, we've got two stories which we can take to low level straight away and make a good start on. That's good news, because we kind of want to get started on this work. We'll wait for you on the widget integration stuff before we can progress these to kind of a more solid, low-level conversation."_
2. **Story 1 — Manual client creation cleanup, "the demon".** Joe sequencing context: _"as long as that manual creation remains, it sort of stops us moving towards the new MDM solution, so that kind of clears the decks and allows a clean move."_ The case is narrow — one remaining flow on **requirement creation** when selecting a client, where the party widget defers back to InRisk to create the party manually if both Party Search and the D&B fallback fail to find a match. Every other manual-create flow in InRisk already routes through the party widget; this one didn't get migrated as the codebase evolved. [[billy-calladine]] confirmed the widget side already supports it (used in the **renewals** flow today) and that it is _"just a flag that you can enable within… the code that already exists"_. John then confirmed: _"that's the only remaining spot. Yeah, that's confirmed."_ Ready for low level.
3. **Story 2 — Data-model migration: party + broker + party-snapshot tables.** Add two columns to each of three independent tables: **MDM party ID** (Joe: `UUID v7`) and **MDM party version ID** (Joe: integer). John explicit on the naming caveat: _"so you'd have it on the party table, the broker table, and we'd also store it into the party snapshot. They're all independent."_ Note that on InRisk's side the party table is **badly named** — its primary key column is `client_id`: _"it's actually the party table, although it's a bit confusing, because the party table, the ID of it is client ID."_ Plus DTO/backend-object updates against the schema change. Ready for low level.
4. **Story 3 — Client-widget integration, feature-flagged.** Plus story 4 (broker-widget) and story 5 (party-tagging). John: _"introduce a feature flag that basically toggles between using the existing workflow with the existing party widget, or when we turn the feature flag on, it will launch the new party widget, and trigger the additional logic of storing those two IDs that will come back."_ Same shape as the 2026-05-13 sketch; commonality across client / broker / tagging not yet validated.
5. **Renewals flow folds into the client story.** Billy raised it; Kris confirmed: _"to the existing widget. So, in theory, that doesn't change, because we just need to probably rewire that to the new widget…"_ John: _"wherever we are showing the widget now, we'd essentially be just calling, show the new widget. Which would be… feature equivalent to what we've got."_ Renewals does not become its own story.

### Widget posture rolled back to parity-not-enhancement
1. **Drop-in replacement now, enhancement later.** John: _"For as much as possible, it's going to be a drop-in replacement for now, and then should we need to change things in the future, that'll be something that comes separately."_ Joe accepted: _"you guys are always able to push back to us, because my original scope was don't affect in-risk too much, so I just went crazy and was like, no, I want it to be really modern and different, but, yeah, we can roll it back a bit until that."_
2. **Bespoke InRisk widget — auth/RBAC/session parity.** Joe: _"we can build a widget that's bespoke for you guys that is gonna match the same sort of auth flow, but I do need to sort of take this one away, probably, with Billy."_ Required surface area for parity: session, RBAC controls, look-and-feel. UX enhancements are deferred.
3. **Why bespoke not shared.** Joe explored the alternative — handing InRisk the Chakra-3 widget with TanStack, an SDK, a non-trivial package-manager renaming dance to coexist with InRisk's Chakra 2 — and concluded the maintenance cost of two libraries is cheaper than forcing InRisk through that integration under the September gate. Joe: _"that probably sounds worse than me just making another one for now."_ Reinforces and concretises [[inrisk-cuts-over-before-high-volume]]'s two-component-libraries call.
4. **Feature parity must include current widget filtering parameters.** Jason Owen surfaced the Lloyd's-brokers thread: the existing widget passes a request like _"give me all the parties relevant to this **TOBA status** (1984-approved, or 1987-approved in the new one)"_ — see §_TOBA terminology and Lloyd's-brokers context_ below. Joe: _"on our side, obviously, we're aiming for feature parity, and then also, like, feature enhancements in the future, but yeah, we need the same functionality to exist on our side. It might not right now, but I think that's where we need to like, get all of the, known unknowns out, and sort of get them into the new world."_

### Spike required before low level on Stories 3/4/5
1. **What the spike unlocks.** Joe enumerated the blockers: how the widget is integrated (SDK shape, hooks, providers), what data the widget returns from MDM's OpenSearch given OpenSearch indexes differently than the underlying Dynamo storage, whether the return shape covers everything InRisk's code consumes today. John: _"do we need to bottom out what the widget is gonna be doing before we can take these to low level, it feels like probably that's the case."_ Joe: _"Yeah, I think it almost feels like probably need a spike, because there's going to be a lot of questions in low level that I won't be able to answer."_
2. **Spike pairing.** Joe + [[billy-calladine]] (ex-Striker, has more InRisk context than the rest of [[graph-team]]) + InRisk side. Joe: _"I don't know, MR Andrew, and Alex, and figure out whether or not we want to like… Line up our sprints so that we've got someone on our side who's free to pair on a [spike] with some [InRisk]."_ Joint pairing explicitly preferred over throw-over-the-fence to avoid information loss.
3. **Timing.** Not next sprint — Joe: _"that might be too soon, mainly just because I obviously have something, but I only have it in the D3 chakra world with the design studio, so if I'm having to sort of like, rebuild some of that, that will take longer."_ To be confirmed at the **ad-hoc high level the following day (Friday 2026-05-15)** where Joe will discuss with Billy and [[alex-sillars]] and report back.

### Feature-tagging carve-out reinforced (no new information)
1. Billy asked a clarifying question about **party tagging vs feature tagging** boundary; John confirmed feature tagging is _"driven by party. It's not going to be part of this initial phase. But it is something that is on the agenda to kind of, I think, basically reassign, because it doesn't make sense to the party kind of own feature tagging."_
2. **Old party system stays alive until handover.** John: _"the old party system will remain to support it. Until we get a plan to change it over."_ Mirror of the 2026-05-13 carve-out; reinforces [[feature-tagging-moves-to-inrisk]] and the Phase-1 boundary captured under [[open-questions#OQ-019]].

### Cutover sequencing reinforced
1. Joe summarised the cutover stance for the wider audience: aspirational **mid-August** for InRisk's MDM cutover, **hard 1 September** for HV. Joe: _"keep, sort of, mid-August in your head, but the big part of that is around we obviously need time to validate, test, and confirm some things, before a hard deadline."_
2. **HV-on-MDM-first explicitly rejected again.** Joe: _"They could be calling our new system first before you guys, but I think, obviously, we've discussed, and I think it makes way more sense that we get in-risk onto MDM first before high volume, because otherwise we'll end up supporting two architectures at once."_ Reinforces [[inrisk-cuts-over-before-high-volume]] without modifying it.
3. **Calendar pressure acknowledged not feared.** Kris: _"in my head, I had something like June or July, but yeah, it still gives us enough room for easily implementing that."_ Joe: _"we've got time. I think, ultimately, like, we'll do our best to dig out all of the like, unknown unknowns and everything, but I'm sure there'll be, like, extra features to add, or bugs to fix as we go."_

### InRisk-side backfill question — new strand, no OQ
1. Joe flagged at the end of the call: _"we'll add those columns to the data model, and we just decide whether or not we're gonna backfill them with party IDs that we have for old records, or leave them as null, and go from a point in time, but I think we have to discuss that, because it's going to have an impact on… Mostly on sanctions, but some other stuff."_
2. **The mirror question to [[no-historic-client-backfill-into-mdm]] on the InRisk side.** MDM-side history starts at cutover; the InRisk-side question is whether the new ID columns on the party / broker / party-snapshot tables are **backfilled** for pre-cutover records, or left **null** with go-forward semantics. Different impact surface — sanctions is the main one (pre-cutover party-ID resolution for older submissions affects whether a sanctions re-check can be ID-driven or has to keep using the snapshot).
3. **Not formalised as an OQ** per Rory's steer — captured as a pending note on the affected pages.

### Manual-record provenance (Jason Owen's question, deflected to Party side)
1. **Question**: does a manually-created party (created via the new widget when both Party Search and D&B fail) get reconciled or curation-checked, or is it taken verbatim? Jason: _"Does the manual record ever get reconciled? Is that played back into the party side?"_
2. **Joe deflected to the Party side**: _"I think it's on our side, essentially, as to whether or not this goes into the curation flow and gets double-checked, which I assume it does for new parties, or just gets taken verbatim. But I think for you guys, let us worry about that, we'll make sure that it's [provenance preserved]."_ Joe added: _"what we're trying to do is stop any creation on your side, so that we always have it, and then you can just rely on us to maintain it."_
3. **Not a new architectural decision** — this is the existing Party-side curation flow; captured here for traceability of the question but no wiki change needed beyond noting it on [[party-curation-tool]]'s curation-flow context (touched lightly).

### TOBA terminology and Lloyd's-brokers context
1. **"October status" in transcript = TOBA status** (Terms of Business Agreement) per user. Captured here so future re-reads of the source don't take the transcription literally.
2. **Lloyd's brokers are a separate workflow.** Jason raised whether "the brokers we're talking about" are the Lloyd's brokers or the retail / provincial brokers. John clarified that Lloyd's brokers are a distinct workflow (Lloyd's 2), but **TOBA-status filtering applies to both** and is a current widget filter that must continue in the new widget.
3. **TOBA approval years**: 1984-approved (current) and 1987-approved (new) referenced in the transcript as TOBA cohorts the widget filters by. The new MDM widget must support the same filter parameters at parity.

## Decisions made

_No new ADRs — this call reinforces and concretises decisions already on the books._

- **Reinforces** [[inrisk-cuts-over-before-high-volume]] — cutover sequencing restated; design-system-agnostic library framing now extended with the **parity-not-enhancement** principle for the InRisk widget itself (not just the styling stack).
- **Reinforces** [[feature-tagging-moves-to-inrisk]] — Phase-1 carve-out re-stated to wider audience; old party system stays alive past cutover until handover plan exists.
- **Refines** [[inrisk]] Phase-1 epic — story ordering and gating explicit: Stories 1 & 2 ready for low level; Stories 3/4/5 gated on the spike.
- **Refines** [[strangle-the-graph-via-proxy-events]] context — adds the unanswered InRisk-side backfill question (existing-records-get-MDM-IDs vs null-and-go-forward) as a pending Phase-1 detail with sanctions implications.

## Actions

| Action | Owner | Application | State |
|---|---|---|---|
| Run the widget-integration **spike** with Joe + Billy + Alex + Andrew Turner — bottom out SDK posture, OpenSearch/Dynamo response shape, integration mechanic — before progressing Stories 3/4/5 to low level | [[joe-worsfold]] · [[billy-calladine]] · [[alex-sillars]] · [[andrew-turner]] | [[inrisk]] · [[party-application]] | open — gates Stories 3/4/5 |
| Confirm spike scheduling at the **ad-hoc high level on 2026-05-15** (the day after this session) | [[joe-worsfold]] | [[inrisk]] · [[party-application]] | open — explicit follow-up commitment |
| Take **Story 1 (manual-client-creation cleanup)** to low level — feature-flag the widget's manual-create path on, remove the InRisk-side legacy logic | [[john-trahearn]] · [[kris-mokrzycki]] | [[inrisk]] | open — Story 1 ready immediately |
| Take **Story 2 (data-model migration)** to low level — add `party_id` (UUID v7) + `version_id` (int) to party, broker, and party_snapshot tables, plus DTOs | [[john-trahearn]] · [[kris-mokrzycki]] | [[inrisk]] | open — Story 2 ready immediately |
| Decide **InRisk-side backfill** policy for the new columns on existing client / broker / party-snapshot records — backfill with known MDM IDs, or null + go-forward from cutover; sanctions impact is the main consideration | [[joe-worsfold]] · [[john-trahearn]] | [[inrisk]] · [[party-application]] | open — flagged in-call; not formalised as an OQ |
| Build the **bespoke InRisk widget** (parity with current auth / RBAC / session; same TOBA-status filtering as today's widget) on the design-system-agnostic library | [[joe-worsfold]] | [[party-application]] | open — restates the second-library action from 2026-05-13 with the parity-not-enhancement framing |

## Applications mentioned
- [[inrisk]] — five-story epic ordered; Stories 1 & 2 ready for low level; Stories 3/4/5 gated on spike; bespoke parity widget agreed; **party table named `client_id`** caveat captured; **party-snapshot** added as a third table that needs the data-model addition.
- [[party-application]] — bespoke InRisk widget commitment; widget already published; spike will pin SDK posture and response-shape; backfill question on the InRisk-side mirror flagged.
- [[party-curation-tool]] — touched lightly via the manual-create provenance question (curation flow is the answer to whether manually-created parties get reconciled).
- [[high-volume]] — referenced as the 1-Sep hard date; unaffected by widget call (API-only).

## Entities mentioned
- [[amardeep-badhan]] (new — PreBind Scrum Master, chaired the call) · [[john-trahearn]] · [[joe-worsfold]] · [[billy-calladine]] · [[kris-mokrzycki]] · [[jason-owen]] (new) · [[andrew-turner]] (new; resolves [[open-questions#OQ-030]]) · [[katarina-voskarova]] (new) · [[rastislav-sepelak]] (new) · [[alex-sillars]] (referenced — spike pairing target) · [[prebind-team]] · [[graph-team]]
- **Transcription artefacts (do not record as entities)**:
  - **"MR Andrew"** in the transcript is [[andrew-turner]] — same drift class as previous sessions' "Darius" / "Boyle" / "Antoine"; not a separate person. Documented here so future re-reads don't re-create a stub.

## Concepts mentioned
- **TOBA (Terms of Business Agreement)** — broker-onboarding regulatory category; 1984-approved and 1987-approved are the cohorts referenced. The party-search widget filters by TOBA status today; the new widget must do the same. **Not yet a wiki concept page** — single-source mention; flag for a concept page when it appears again.
- [[feature-tagging-moves-to-inrisk]] — re-stated for the wider audience; no change.
- [[inrisk-cuts-over-before-high-volume]] — re-stated for the wider audience; extended with the **parity-not-enhancement** widget framing.

## Contradicts
- _None._ All claims are consistent with [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] and the prior sessions; the new content is concretisation, ordering, and the introduction of new names + the InRisk-side backfill question.

## Reinforces
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — every decision from that call is re-affirmed here in front of the wider InRisk audience: 5-story epic, party-tagging in / feature-tagging out, InRisk-first cutover ≥ 2 weeks before HV, two component libraries, sanctions out of Phase 1.
- [[feature-tagging-moves-to-inrisk]] — old party backend stays alive past cutover restated explicitly.
- [[inrisk-cuts-over-before-high-volume]] — sequencing restated; the widget half of the ADR gains the parity-not-enhancement framing as a specific implementation principle for Joe's second library.

## My notes (LLM)
- **Two new substantive deltas vs 2026-05-13**: (a) the story-ordering and gating between "ready now" and "spike-blocked" is new and immediately useful for sprint planning; (b) the InRisk-side backfill question is a new strand of work — not new evidence, but a previously-implicit question now explicitly on the table with a sanctions hook. Per user direction the latter is _not_ formalised as an OQ but captured as a pending note on the affected pages.
- **Wider InRisk team enters the wiki.** Five new PreBind people (BA × 3, PO × 1, SM × 1) join [[prebind-team]]'s member roster. Most of them are peripheral to the MDM thread — Jason Owen is the only one to make substantive MDM-relevant contributions (TOBA filtering question, manual-record provenance question). [[amardeep-badhan]] is captured as Scrum Master on user direction (single facilitator line in this transcript) — counterpart to [[sergiu-postolachi]] on [[graph-team]]. Stubbed conservatively; can deepen as future sessions surface them.
- **OQ-030 closes cleanly via transcription drift.** Andrew Turner = Andrew Tennant per user — the Pass-A Session-1 transcription drift class is wider than originally thought (Darius / Boyle / Antoine / "Mr Andrew" / Andrew Tennant). Worth keeping in mind: any time a Pass-A name doesn't map to anyone in the room, the surrounding context probably resolves it via a known speaker.
- **Parity-not-enhancement is doing real work here.** Joe explicitly rolled back the "modern and different" widget vision in front of the wider audience — that's a public commitment, not just a private trade-off. Worth highlighting on [[inrisk-cuts-over-before-high-volume]]'s widget section so the rollback is preserved.
- **The InRisk party table being keyed by `client_id`** is a small but useful concrete detail for [[inrisk-architecture]] — clarifies why the existing schema has been described loosely as "client and broker tables" when really the party table is also one of them, badly named.

## Open questions
_No new OQs (per user steer). Existing OQs reinforced / advanced below._

- [[open-questions#OQ-030]] — **resolved** by this source: "Andrew Tennant" is [[andrew-turner]] (Product Owner on [[prebind-team]]); transcription drift.
- [[open-questions#OQ-035]] (concrete InRisk MDM-cutover date) — unchanged; mid-August target re-stated.
- [[open-questions#OQ-036]] (widget-response field alignment) — sharpened: this is exactly what the spike will resolve. Joe expects to **adjust the response shape** when InRisk starts using the widget.
- [[open-questions#OQ-019]] (feature-tagging migration timing) — unchanged; reinforced.
- **Backfill of InRisk's new ID columns** (existing client / broker / party-snapshot rows): backfill with known MDM party-IDs, or null + go-forward from cutover? Sanctions is the main impact surface. **Tracked as a pending note** on [[inrisk]], [[party-application]], and [[party-rearch-phase-1]] per user steer; not promoted to an OQ.
