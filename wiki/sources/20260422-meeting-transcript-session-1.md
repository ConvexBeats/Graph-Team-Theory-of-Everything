---
type: source
title: "Meeting Transcript — Session 1 (2026-04-22)"
kind: meeting
created: 2026-04-22
updated: 2026-04-22
tags: [meeting, architecture-review, session-1, origin-of-strangle-graph]
raw: raw/20260422 - Meeting Transcript - Session 1.md
date_of_source: 2026-04-22
status: draft
project: party-rearch
---

# Meeting Transcript — Session 1 (2026-04-22)

## Summary
First of two architecture-design workshops on 22 April 2026 (this is the **morning** session; [[sources/20260422-meeting-transcript-session-2]] is the afternoon follow-on). Chaired by [[alex-sillars]] with [[sergiu-postolachi]] facilitating the workshop structure. The session walked every consumer contract of the Party Application under a new **three-bucket framework** — _operational_, _data distribution_, _enrichment_ — and systematically worked out, for each one, (a) the target-state design, (b) the interim-state design, and (c) the cross-team dependencies.

The load-bearing breakthrough of the session: the **strangler / proxy-event** approach was invented live in the DU-events segment. The room concluded MDM can emit a proxy event in the existing graph event shape, collapsing what everyone had previously assumed would be a painful dual-run into a comparatively small adapter job. Alex: _"if you do it right, no one will know you've done anything at all."_ Joe: _"having the graph strangled off completely is the thing that gets us into a place where I'm zero-ops."_

The session closed into lunch with ~8 of 10 target-state contracts agreed, a first-cut of InRisk-side dependencies (one new reference table holding party ID + version; party ID added to existing emit events; broker UID eventually sourced from party ID), and an open question about squad shape — Joe flagged that the 13-person squad is too large to focus cleanly on this work at pace.

## Bibliographic info
- **Date**: 2026-04-22 (morning)
- **Format**: meeting transcript (WebVTT, auto-generated; numbered speakers plus some named)
- **Raw file**: `raw/20260422 - Meeting Transcript - Session 1.md`
- **Position**: chronologically **before** [[sources/20260422-meeting-transcript-session-2]]; same day

## Attendees

_Speaker numbers are freshly auto-assigned by the transcription engine for this recording and do not match Session 2. Pass B incorporates user corrections._

| Speaker tag | Name | Role | Team | Confidence |
|---|---|---|---|---|
| Rory Beattie | [[rory-beattie]] | Project oversight | [[tech-tooling]] | named |
| Sergiu Postolachi | [[sergiu-postolachi]] | Scrum Master / workshop facilitator | [[graph-team]] | named |
| Speaker 1 | [[alex-sillars]] | Product Owner | [[graph-team]] | confirmed |
| Speaker 3 | [[suzanna-whitefield]] | Architect | [[architecture-team]] | confirmed |
| Speaker 4 | [[billy-calladine]] | Engineer / Developer | [[graph-team]] | confirmed |
| Speaker 5 | [[joe-worsfold]] | Tech Lead / Architect | [[graph-team]] | confirmed |
| Speaker 6 | [[billy-calladine]] | Engineer / Developer | [[graph-team]] | **confirmed (Pass B)** — transcription engine assigned Billy a second ID |
| Speaker 7 | [[joe-worsfold]] | Tech Lead / Architect | [[graph-team]] | **confirmed (Pass B)** — transcription engine assigned Joe a second ID |
| Speakers 2 · 8 · 9 · 10 · 11 | _filler / brief_ | — | — | low — mostly one-word utterances; likely transcription noise or very brief side speakers |

Identified-by-reference (mentioned but not attending):
- **Anton** → [[antonie-labuschagne]], [[devx-team]]. Context: _"don't worry about Anton while we're devising any solutions right now"_ — explicit scope decision that InRisk Engine is not to be designed for in this programme (confirmed in S2).
- **John** → [[john-trahearn]], [[prebind-team]] Tech Lead. Context: already prototyped a new InRisk reference table for party data. Joe cites this as evidence InRisk is "very happy" with the interim plan.
- **Chris / Kris** → [[kris-mokrzycki]], [[prebind-team]] Tech Lead. Context: same prototype context as John.
- **Tom Ash / Tomas** → [[tomas-sivo]], Business Analyst on [[graph-team]]. Context: owns the **functional-requirements workstream** for the new [[party-curation-tool]]. Transcription drifted "Tomas" → "Tom Ash" across references. **Resolved Pass B.**
- **Michael Hay** — stakeholder in the feature-tagging functionality. Team and stakeholder-role scope still open — see [[open-questions#OQ-027]].
- **Kartik**, **Jenny / Ginny** — name-only references, context thin. See [[open-questions#OQ-028]], [[open-questions#OQ-029]].
- **Andrew Tennant** — resolved 2026-05-26 to [[andrew-turner]] (Product Owner, [[prebind-team]]); transcription drift. See [[open-questions#OQ-030]] (resolved). _Note: [[andrew-turner]] departs 2026-05-29 → succeeded by [[daria-romanovskaia]]; forward-looking references redirect per [[AGENTS|§5.5]]._

### Transcription artefacts (not people)
Two references previously treated as unidentified people were **transcription mispronunciations** and should not be resolved as entities:

- **"Darius"** — mispronunciation of **"InRisk"** by the transcription engine. Not a person. All references in the raw transcript tagged as "Darius" refer to the InRisk application.
- **"Boyle"** — mispronunciation of **"Will"** ([[will-bone]]), in reference to Will joining the afternoon session ([[sources/20260422-meeting-transcript-session-2]]). Not a person to identify; no separate afternoon external-provider recording exists. The "external-provider deep-dive" alluded to at the end of Session 1 was simply Session 2.

## Key claims

1. **Three-bucket contract framework.** Alex proposes organising every Party consumer contract into three buckets: (i) **operational** — how parties get created / curated (search widget, manual creation via application, manual creation via PCT, CUO grouping, PCT UI itself); (ii) **data distribution** — who reads Party events (DU, party versioning in InRisk + Artificial, Launchpad, party reporting, EVA, broker dashboards, knowledge graph); (iii) **enrichment** — external sources flowing in (D&B, S&P, One Re, sanctions). Every design decision is framed against this grid. _(Session scaffolding adopted; see [[contract-buckets]] candidate concept.)_

2. **Strangle-the-graph invented live.** In the DU-events section, Joe lands the insight: because DU consumes exactly one event (the "spine rewrite" emit, plus a broker variant to the common bus), MDM can emit that same event shape out of Dynamo and the DU sees nothing change. Decision: _"we send the event without ever writing to the graph."_ This is the canonical origin of [[strangle-the-graph-via-proxy-events]] — Session 2 consolidates and formalises it.

3. **Currently, every InRisk event rewrites the whole spine.** Billy: regardless of whether a single field changed, the existing Graph implementation rewrites the whole spine and emits an event about it. This is cited as both the flakiness of the current system and the reason the proxy adapter is simpler than expected — there is effectively one event to proxy, not many.

4. **Party ID storage in InRisk is the only load-bearing interim dependency.** Agreed with [[john-trahearn]] / [[kris-mokrzycki]]: InRisk will add a new reference table holding `(client_id → party_id, version)`. Events that currently emit client data will additionally emit the party ID. MDM reads that to build views. No other InRisk schema change is required for Phase 1.

5. **No backfill of historic InRisk client snapshots into MDM.** "Draw a line in the sand" — if someone needs to know how a party was viewed historically, they use [[data-universe]] (DU has the history) or the InRisk database, not MDM. MDM carries full party version history from cutover forward. _(Promoted to [[no-historic-client-backfill-into-mdm]].)_

6. **No audit-history backfill for the new PCT.** The new PCT won't carry across the Jira audit history. SLA reporting and audit analysis is Snowflake's job (joins old-Jira and new-PCT tables). Jira stays available read-only for historical lookup; old PCT UI switched off one day to the next. _(Captured inline; candidate for promotion.)_

7. **Single-feature-flag coupled rollout.** The switch from old-search to new-search in InRisk is the _same flag_ that switches old-PCT to new-PCT. Originally Joe considered decoupling these ("one then the other"); agreed it becomes simpler to ship them together: one flag, one cutover, same domain boundary. This is the operational shape of [[pct-and-mdm-go-live-together]].

8. **Broker flow — eventually retire the broker UID on the InRisk side.** Currently: InRisk receives a broker-UID from us, users search by it. Target: InRisk stores party ID in the same table. Broker role is just another role on a party; no separate broker-UID lifecycle. In the interim (Phase 1) we still send a broker UID alongside so InRisk isn't forced into a simultaneous schema migration. Phase 2+: broker UID retires.

9. **OpenSearch is load-bearing for discoverability _and_ reporting shape.** The search widget UI is "the same flow, different backend" — OpenSearch indexes flatten roles, views (insured/reinsured), aliases, CUO groupings, parent / ultimate-parent. Re-indexing is an operational tool (Lambda-triggered) that the team expects to run often as the schema evolves.

10. **Roles vs. views — litmus test.** Joe's rule: _"if every policy was deleted from this party, would it still be insured? No. Would it still be a broker? Yes."_ Roles are intrinsic to the party; views (insured/reinsured) are ascribed by the consumer (InRisk) and must be fed back from them to MDM to maintain the current DU projection shape. This resolves a latent disagreement about how to model views.

11. **Dynamo + OpenSearch, no GraphQL facade.** Joe confirms the target shape: single API, single Dynamo-backed store, OpenSearch for discoverability. _"There's no graph file, just as a heads up — everything's in one API, it's a lot simpler."_ (Fewer technologies for DevOps to support; cited as an end-state win.)

12. **Eclipse enrichment is dropped.** Sergiu flags Eclipse ingestion; Alex confirms it has no current consumers — the data has not been updating for months, Eclipse IDs are already resolved via the InRisk-client-ID path. Decision: no Eclipse ingestion in the new world. (Will be added to current-state diagram and struck through for documentation only.)

13. **Feature-tagging is an InRisk domain responsibility.** Joe: _"it's an InRisk responsibility, really, it's their domain. They just asked us to do it because we had a nice widget."_ In Phase 1, leave feature-tagging in its existing Postgres table untouched. In Phase 2 / end-state, migrate the Postgres table and the search widget component to InRisk; they own it thereafter. This formalises the scope call that was flagged in [[party-rearch-ownership-matrix]].

14. **D&B cache TTL is scheduled, not reactive.** Joe's update on the D&B flow: cache in Dynamo with a TTL; a scheduled job (monthly / quarterly — to be agreed) triggers a re-call of D&B and spawns a curation revision if the record has changed. Extends but does not contradict [[d-and-b-caching-and-auto-parent]]; adds the _"scheduled refresh → revision"_ loop as an implementation detail for that ADR.

15. **S&P — two paths exist.** Suzy clarifies S&P has both a DU-stored copy and an API. The API also performs entity resolution internally. Neither is in scope for Phase 1 (stays manual via DU), but the API path is the future pattern when the Party becomes the source of truth for S&P.

16. **Sanctions flow — driven by revision events in new world.** The new PCT's `revision.proposed` event triggers a worker that raises the sanctions check; sanction status is stamped back onto the **version** of the party (GitHub-commit-verified analogy). Current flow (InRisk-triggered, round-tripping via Boomi / NTT) is unchanged in Phase 1; Phase 2+ simplifies per S2's "sanctions short-circuit" flag.

17. **Bulk migrations via a CLI / lightweight UI, owned by the MDM team in Phase 1.** Historically DataOps has asked to self-serve bulk fixes; agreed not to build the full self-serve path in Phase 1. Phase 1: MDM team owns a CLI that takes a CSV, diff-previews the changes, creates a bulk auto-approved revision. The revert-button-on-a-revision idea is explicitly a Phase 2+ feature.

18. **Manual-create-without-submit edge case is acknowledged but deferred.** If a party is created by InRisk via the search widget and then no submission is raised, DataOps has no audit trail of _"why does this party exist?"_ Joe's preferred fix — _"send the party.create from InRisk at `submit` time, not `widget open` time"_ — is noted but not forced. Phase 1: accept the orphan-party noise as minor; Phase 2: consider SDK-mediated create-on-submit.

19. **Team-size tension surfaced (not resolved).** Joe flags the shape problem: the Graph Team is ~13 people, squads optimally 7, and the new MDM needs deep context to build. Two paths: (a) carve a ~3-person focused MDM squad that owns the context but creates a knowledge bottleneck, or (b) involve the whole squad and spend longer on context-sharing. No decision; flagged for Alex + Rory to pick up. _(Open action: decide squad shape for MDM delivery.)_

20. **InRisk is eager to do the work, not reluctant.** Joe's read from the prior tech-lead conversations (John / Kris): _"they want to do it, it simplifies their own model, they have space to do it because they haven't got requirements in the pipeline."_ This is the critical precondition for the whole strategy — the InRisk-side interim changes (party-ID storage, broker-UID retirement, table re-indexing) are in their interest, not imposed.

## Decisions recorded

### Promoted to formal decision pages (Pass A)
- [[no-historic-client-backfill-into-mdm]] — **accepted**. MDM starts carrying full version history from cutover forward; historical InRisk client audit / as-of-date queries are served by [[data-universe]] or the InRisk DB. Complements [[strangle-the-graph-via-proxy-events]] by bounding the backfill scope.

### Promoted to formal decision pages (Pass B)
- [[no-pct-audit-backfill]] — **accepted**. New PCT does not backfill Jira audit history; Snowflake is the cross-period audit/SLA store; old Jira stays read-only post-cutover. Mirror of [[no-historic-client-backfill-into-mdm]] on the PCT side.
- [[feature-tagging-moves-to-inrisk]] — **accepted**. Phase 1: no change. Phase 2+: Postgres table and widget component migrate from [[graph-team]] to [[prebind-team]] / [[inrisk]] ownership.
- [[bulk-migrations-owned-by-mdm-phase-1]] — **accepted**. Phase 1 bulk migrations executed by [[graph-team]] via a CLI (CSV → diff preview → auto-approved revision). Full self-serve UX is Phase 2+.

### Reinforced / origin-cited (already existed)
- [[strangle-the-graph-via-proxy-events]] — this session is the **origin**. S2 consolidated; S1 invented. ADR updated to cite both sources.
- [[pct-and-mdm-go-live-together]] — single-feature-flag coupling is the operational shape of this decision; annotated (not promoted to its own ADR).
- [[d-and-b-caching-and-auto-parent]] — scheduled TTL-driven refresh → revision loop added as an implementation note on the ADR.
- [[uuid-system-id-with-display-id]] — no substantive new content in this session.

### Tracked as open scope call (not an ADR)
- **Drop Eclipse ingestion** — scope call, not formalised as an ADR. The Graph-API audit spike ([[open-questions#OQ-004]]) is expected to confirm no live consumer before the retirement is acted on. Tracked at [[open-questions#OQ-001]].

## Actions

| Action | Owner | Application | State | Notes |
|---|---|---|---|---|
| Spike: audit Graph API usage — which endpoints are hit, by which consumers, at what volume | [[joe-worsfold]] · [[billy-calladine]] | [[party-application]] | open | Input into decommissioning / proxy-coverage decision |
| Walk the current PCT end-to-end with [[sergiu-postolachi]] to confirm no functional gap in new PCT | [[joe-worsfold]] | [[party-curation-tool]] | open | Complements Tom Ash's functional-requirements work |
| Decide MDM-delivery squad shape (focused 3 vs. whole 13-person squad) | [[alex-sillars]] · [[rory-beattie]] | [[party-application]] | open | Blocking for sprint-planning the intercept / backfill / adapter work |
| Ticketise: backfill, intercept, adapter, new PCT, coupled rollout flag | [[alex-sillars]] · [[joe-worsfold]] | [[party-application]] | open | Post-session consolidation; feeds Session 2's effort estimate |
| Run the InRisk-side dependency workshop (party-ID table, broker flow, coupled rollout) | [[rory-beattie]] | [[inrisk]] | open | Reinforces the S2 action; scope includes John / Kris on domain, Scott on broker |
| Define D&B scheduled-refresh cadence (monthly / quarterly) and revision-generation path | [[joe-worsfold]] | [[party-application]] | open | Implementation note on [[d-and-b-caching-and-auto-parent]] |
| Confirm Eclipse-ingestion retirement with any remaining potential consumer | [[alex-sillars]] | [[party-application]] | open | Data has not updated for months; low risk |
| Scope the MDM-owned bulk-migration CLI tool (CSV → preview → approved revision) | [[joe-worsfold]] | [[party-application]] | open | Phase-1 self-serve substitute for DataOps bulk work |
| Identify remaining [[party-curation-tool]] neighbourhood people ([[open-questions#OQ-027]] Michael Hay) | [[rory-beattie]] | [[party-curation-tool]] · [[inrisk]] | open | Wiki-level follow-up |

Open action count: **9**. Applied to [[party-rearch-ownership-matrix]] in Pass A.

_Pass B resolutions to the Pass A action list:_
- The "Tom Ash / Tomas / Darius" identification action is **resolved**: Tomas Sivo identified on [[graph-team]]; "Darius" confirmed as transcription mispronunciation of "InRisk", not a person.

## Applications mentioned
- [[party-application]] — primary application under re-architecture; discussed contract-by-contract.
- [[party-curation-tool]] — replatformed, Jira dependency dropped, coupled flag with search widget.
- [[inrisk]] — interim changes confirmed: party-ID reference table; party ID on emitted events; broker-UID retirement in later phase.
- [[inrisk-engine]] — explicitly scoped **out** of this programme's design: _"don't worry about Anton while we're devising any solutions right now"_ — reinforces [[inrisk-engine]]'s final-state-only framing.
- [[high-volume]] — referenced as the programme's forcing function for the 1 September deadline. An in-room _"could we ship HV without PCT?"_ speculation was raised by Joe and dismissed.
- [[eclipse]] (external) — used by [[inrisk]]; scope call on ongoing ingestion into [[party-application]] deferred ([[open-questions#OQ-001]]).
- Knowledge Graph — named in-session as a data-distribution consumer after Sergiu flagged it missing; subsequently clarified in the lint pass as the **internal Neo4j datastore inside [[party-application]]**, not a separate downstream consumer. See [[open-questions|OQ-010 resolution]].

## Entities mentioned
- Teams: [[graph-team]], [[tech-tooling]], [[architecture-team]], [[prebind-team]], [[devx-team]], [[analytics-team]], [[dataops-team]].
- People (attendees): [[rory-beattie]], [[sergiu-postolachi]], [[alex-sillars]], [[joe-worsfold]] (Speakers 5 and 7), [[billy-calladine]] (Speakers 4 and 6), [[suzanna-whitefield]]. All attendee speakers identified.
- People (mentioned): [[antonie-labuschagne]], [[john-trahearn]], [[kris-mokrzycki]], [[tomas-sivo]] (formerly "Tom Ash / Tomas"), [[andrew-turner]] (formerly "Andrew Tennant", resolved 2026-05-26 — [[open-questions#OQ-030]]). Still pending identification: **Michael Hay** ([[open-questions#OQ-027]]), **Kartik** ([[open-questions#OQ-028]]), **Jenny / Ginny** ([[open-questions#OQ-029]]).
- Non-entities (transcription artefacts): **"Darius"** = mispronunciation of "InRisk"; **"Boyle"** = mispronunciation of "Will" (= [[will-bone]]) in the context of his afternoon-session attendance. Both removed from the people-to-identify list.
- Platforms: Neo4j + Cypher (legacy backend), DynamoDB (target), OpenSearch (target), Jira (being removed from PCT path), Boomi (D&B integration), Eventbridge / SNS topics (event transport), Snowflake / Data Universe (consumer of proxy events), AWS Lambda (re-index trigger).

## Concepts mentioned
- [[contract-buckets]] — formalised as a standalone concept page in Pass B. Three-bucket (operational · data distribution · enrichment) framework introduced by [[alex-sillars]] and used to scaffold the entire session.

_Still candidates for standalone concept pages: **roles-vs-views litmus test**, **proxy-event strangler**, **intercept-backfill-projection trio**, **version-stamped sanction checks**, **single-feature-flag cutover**, **line-in-the-sand migration principle**._

## Contradicts
_Nothing in this session contradicts a prior wiki claim — this session is chronologically the earliest source. Claims here were **refined** (not overturned) by [[sources/20260422-meeting-transcript-session-2]]; see Reinforces below._

## Reinforces
- [[sources/20260422-meeting-transcript-session-2]] — Session 2 is the afternoon follow-on; it consolidates the proxy-event decision, names the adapter-bidirectional-mappability test, confirms the timeline (~2 sprints core + ~2–3 months overall), and promotes the first round of formal ADRs.
- [[strangle-the-graph-via-proxy-events]] — Session 1 is the **invention** moment; Session 2 is where the decision was formally taken.
- [[pct-and-mdm-go-live-together]] — reinforced by the single-flag-cutover discussion.
- [[d-and-b-caching-and-auto-parent]] — implementation details (TTL + scheduled refresh → revision) added.

## My notes (LLM)
- The transcript runs ~2h40m and ends at lunch. The _"afternoon session on external providers"_ alluded to in-session is simply [[sources/20260422-meeting-transcript-session-2]]; there is **no third recording**. The name "Boyle" in the raw transcript is a transcription mispronunciation of "Will" (= [[will-bone]]) in reference to his attending the afternoon session. (Resolved Pass B.)
- Speaker identification is substantially less reliable than Session 2. The engine re-numbers speakers per recording and **assigns the same person to multiple IDs within a single session**: Billy = Speakers 4 and 6; Joe = Speakers 5 and 7. Session 2's user-confirmed mappings do not apply. All Session 1 attendee speakers are now resolved (Pass B).
- Lines ~745–820 are the break between the "search widget + PCT" segment and the "InRisk dependencies" segment; the auto-transcription picks up a lot of non-meeting noise (_"South Dakota"_, _"Twitter"_, _"Go on"_) — ignored for content extraction.
- Joe's team-shape concern (claim 19) is load-bearing for the [[party-rearch-ownership-matrix]] — tracked as [[open-questions#OQ-017]].
- The session's key narrative shift: the room enters expecting the biggest problem to be the InRisk interim schema; leaves with the InRisk problem being _"small"_ and the new insight being that the DU cutover is a single-event adapter, not an N-event migration.
- **Emotionally**: the _"walking through my field of weeping, my Gladiator reference"_ line from Joe (line 5061) marks the moment the room relaxes — the proxy insight lands and the shape of the programme becomes navigable. Useful emotional tell if we ever need to time-stamp when the plan crystallised.
- **Pass B lesson for future ingests**: auto-transcription mispronunciations of in-room nouns (like "Darius" for "InRisk", "Boyle" for "Will") can masquerade as unidentified people for a long time before a user cross-check catches them. When a "person" surfaces only once with thin context, treat transcription-artefact as a live hypothesis before populating an entity page.

## Open questions

All Pass A open questions are now tracked in the standing [[open-questions]] register. Resolutions and deferrals (Pass B):

- ✓ **Speaker 6** → [[billy-calladine]]. Resolved — see [[open-questions#Resolved|OQ-R10]].
- ✓ **Speaker 7** → [[joe-worsfold]]. Resolved — see [[open-questions#Resolved|OQ-R11]].
- ✓ **Tom Ash / Tomas** → [[tomas-sivo]], Business Analyst on [[graph-team]]. Resolved — see [[open-questions#Resolved|OQ-R12]].
- ✓ **Darius** — not a person (transcription mispronunciation of "InRisk"). Resolved — see [[open-questions#Resolved|OQ-R13]].
- ✓ **Boyle and the afternoon-provider session** — not a person (transcription mispronunciation of "Will", referring to Will joining Session 2). No third recording. Resolved — see [[open-questions#Resolved|OQ-R14]].
- ⟶ **Michael Hay** — deferred to [[open-questions#OQ-027]] (stakeholder in feature-tagging; team/scope TBD).
- ⟶ **MDM delivery squad shape** — deferred to [[open-questions#OQ-017]] (Rory's call; merits further discussion).
- ⟶ **D&B refresh cadence** — deferred to [[open-questions#OQ-011]].
- ⟶ **Eclipse retirement consumer check** — deferred to [[open-questions#OQ-001]]; expected to be answered by the Graph-API audit spike ([[open-questions#OQ-004]]).
- ⟶ **Full Graph API consumer map** — deferred to [[open-questions#OQ-004]] (spike).
- ⟶ **Bulk migration tool Phase 2+ UX** — deferred to [[open-questions#OQ-006]]; Phase 1 is resolved via [[bulk-migrations-owned-by-mdm-phase-1]].

## Related
- [[party-rearch]] · [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] · [[open-questions]]
- [[sources/20260422-meeting-transcript-session-2]]
- [[contract-buckets]] — concept formalised from this session
- [[strangle-the-graph-via-proxy-events]] · [[pct-and-mdm-go-live-together]] · [[d-and-b-caching-and-auto-parent]] · [[uuid-system-id-with-display-id]] · [[no-historic-client-backfill-into-mdm]] · [[no-pct-audit-backfill]] · [[feature-tagging-moves-to-inrisk]] · [[bulk-migrations-owned-by-mdm-phase-1]]
- [[party-application]] · [[party-curation-tool]] · [[inrisk]] · [[inrisk-engine]] · [[high-volume]]
- [[tomas-sivo]] (new entity, Pass B)
