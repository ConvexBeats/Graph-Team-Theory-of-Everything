---
type: source
title: Party integration timelines — Artificial onboarding prep call (2026-05-19)
kind: meeting
created: 2026-05-26
updated: 2026-05-26
tags: [source, meeting, party-rearch, artificial, high-volume, boomi]
raw: raw/20260519 - Party integration timelines.md
author: [rory-beattie, simon-hulbert, alex-sillars, boardroom-orega-6b]
date_of_source: 2026-05-19
project: party-rearch
status: draft
---

# Party integration timelines — Artificial onboarding prep call (2026-05-19)

## Summary
27-minute three-way call between [[rory-beattie]] ([[tech-tooling]]), [[simon-hulbert]] (Architect on the [[artificial]] integration project, mapped in this wiki to [[high-volume-team]]), [[alex-sillars]] ([[graph-team]] PO), and an in-room **Convex – Orega 6B Boardroom** colleague (HV-side PM coordinator — unidentified, see [[open-questions#OQ-037]]). Held the day before a Convex × [[artificial]] kick-off the four had teed up for 2026-05-20 to align on Artificial's adoption of the new Party MDM API. First substantive surfacing in the wiki of **[[artificial]] as a third-party vendor platform** consuming Party, and the **[[boomi]]** integration broker that will gateway that consumption. Confirms — once and to the customer — the Phase-1 cutover sequencing ([[inrisk-cuts-over-before-high-volume]]) and the strangler-pattern stability promise ([[strangle-the-graph-via-proxy-events]]). No new ADRs.

## Bibliographic info
- **Author(s) / attendees**: [[rory-beattie]] (programme oversight, [[tech-tooling]]); [[simon-hulbert]] (Architect on the [[artificial]] integration project, [[high-volume-team]] in this wiki — joined late, double-booked); [[alex-sillars]] (PO, [[graph-team]]); **Convex – Orega 6B Boardroom** speaker (HV-side PM coordinator; **unidentified**, possibly [[luca]] or a colleague — see [[open-questions#OQ-037]]).
- **Named-but-not-present**:
  - [[srini]] — Boomi Integration Architect on the HV/Artificial project (HV-side). Will set up the Artificial → Party Boomi connectivity; counterpart for [[joe-worsfold]] on the Party side.
  - [[luca]] — HV-side coordinator who will schedule the fortnightly catch-ups; role TBC ([[open-questions#OQ-038]]).
  - **Sam** — _"FDA for integrations"_ on the Artificial project; "FDA" acronym unresolved ([[open-questions#OQ-039]]). Accepted the 2026-05-20 Convex × Artificial call. Advances [[open-questions#OQ-024]] (Sam identity) — role now partially known (integration FDA), full name still missing.
  - [[joe-worsfold]] — referenced as the author of the YAML interface spec already sent to Artificial; the obvious Party-side partner for [[srini]] on Boomi connectivity.
- **Date**: 2026-05-19, ~12:33–13:01 BST (~27 min).
- **Venue / source system**: Convex video call (Convex – Orega – 6B Boardroom in-room + remote joiners); transcript captured via meeting-recording tool.
- **URL**: n/a
- **Raw file**: `raw/20260519 - Party integration timelines.md`

## Key claims

Grouped by theme for retrieval.

### Artificial as a Party consumer

1. **[[artificial]] (a third-party vendor) is being adopted by Convex to underpin the high-volume insurance workflows and now intends to consume the new Party MDM API.** The Boardroom speaker: _"they're keen to adopt Party as part of their integrations."_ This is the first substantive treatment in the wiki of Artificial in scope as a Party consumer; prior wiki references were passing.
2. **[[joe-worsfold]] has already shared the Party MDM API spec with Artificial** as a YAML interface-definition file. Simon: _"I got a YAML file, an interface definition file from… I think it was Joe sent it on."_ Artificial's response: _"this looks good, can we have a play with it?"_ Spec is **subject to change until go-live** but per [[alex-sillars]] _"won't change massively, but there may be some additions or deletions from it."_ Suitable for testing purposes today.
3. **Convex × Artificial kick-off scheduled for 2026-05-20.** Sam (FDA for integrations) has accepted the call. Artificial's PMs had pushed back on attending the meeting (_"can we just send them a list of the questions … and then they'll have a meeting at some point next week"_); Boardroom-speaker ignored the pushback in favour of getting people on the call.
4. **Artificial's development cadence is materially faster than Convex's.** Simon: _"they measure their development or configuration in their world in minutes and hours, or hours and days … if everything's lined up perfectly, then it could take us minutes to do this, get this system working … and now they want to start, and they're like, well, give us access to all of your things."_ Implication: Convex must hand them a clean, stable enough integration surface; Artificial will not absorb iteration cycles well.
5. **Two-phase Artificial integration**:
   - **(a)** Artificial **reads** Party (initial). Needs the MDM API to be callable + a fake/test dataset to play with — _"if a party… gets sanction checked, and there's a failure or a pass, we need to update that in Artificial straight away."_ — wait, that's the second phase. (a) is the simpler read path: party lookup.
   - **(b)** Party → Artificial **change-event push** (later) so Artificial knows when a party's details change (incl. sanctions pass/fail). Per [[alex-sillars]]: _"we're not expected to do any work. We're going to emit events. We're going to tell you the party role is a cover holder, and we're going to expect you to update the data."_ — i.e. Convex emits, Artificial consumes; round-trip-style.

### Boomi as the gateway

6. **[[boomi]] will be the integration broker between [[artificial]] and [[party-application]].** This is the same pattern as today's sanctions flow (Boomi → [[inrisk]] → [[ntt]] — see [[sanctions-processing]]). Per [[alex-sillars]]: _"I think Boomi is going to act as the interface between artificial and party."_
7. **Boomi access for third parties is the unblocking dependency.** Alex: _"One of the things we're struggling with is the Boomi … access for third parties … the database, the structure is there, so that if they were to call that API, we would have fake data, admittedly, but data available for them to receive back. But there's still that work on security authorisations for them to be able to get a client ID and security token."_
8. **[[srini]] (HV-side Boomi Integration Architect) will run the Artificial-side Boomi setup**, partnering with [[joe-worsfold]] on the Party side. Simon: _"We've got Srini, who's the Boomi Integration Architect, on the project … he's done this before lots of times with lots of different providers … he'll get a link between Boomi and Party, and then he'll hand out the credentials to the guys in artificial."_

### MDM build-status snapshot (from [[alex-sillars]])

9. **Dev / soon-to-be-integration environment**: DynamoDB stood up with **all of the Party attributes that appear in the Artificial-shared spec**. Fake-data path is live; if Artificial called the API today they'd get back fake data.
10. **What's still being built**: extend curation features (deeply coupled to the data model); migrate existing data from **Neo4j ParseDB into DynamoDB**; emit events to **DU + sanctions** encompassing all the data DU/sanctions consume today (Alex specifically called out submission-ID fan-out: _"if we're not going to store some submission IDs in Party, we still need to send that as part of our event. So where do we get that from?"_).
11. **Front-facing API surface is in good shape for testing**; Alex: _"for that, we're in a good place"_ — but _"all of those things would need to be delivered in order for us to say we can go live with the full solution."_ Restates the Phase-1 critical-path framing on [[party-rearch-phase-1]].

### Strangler-pattern + sequencing — restated to a customer

12. **Strangler-pattern promise restated to Artificial**, verbatim from [[alex-sillars]]: _"Everything that's going to come out of Party after we go live on the 1st of September is exactly what it comes out today. We're going to do that in a different way. We're going to obtain that information through a different source. It might not be Party, it might be joined-up InRisk and Party data, but it will still hit sanctions, DU, in exactly the same way."_ Reinforces [[strangle-the-graph-via-proxy-events]] and explicitly flags the **InRisk+Party-joined-up** sourcing as one of the implementation modes inside the strangler.
13. **InRisk-cuts-over-before-HV reaffirmed to the Artificial counterparty**, verbatim from [[rory-beattie]]: _"InRisk … goes first … we don't want to go from one user to a million users, all using a different version of the same thing and not be able to have a rollback strategy … our intention is to move those at least a couple of weeks ahead of when we think we'll be able to land with you guys."_ Simon: _"the party stuff, but then we want to get InRisk over first … and they'll deliver, and then once they've delivered, we will roll out shortly after that."_ Reinforces [[inrisk-cuts-over-before-high-volume]] without changing it.
14. **InRisk integration is "a small footprint" for Artificial / HV side**, per [[rory-beattie]]: _"you just need some information from InRisk. They're set up, they have endpoints that you probably can use straight away, so that's slightly different."_ HV/Artificial → InRisk is a thin link; HV/Artificial → Party is the big one. Useful framing for [[party-rearch-dependency-map]]'s integration-shape annotations.

### Coordination shape

15. **Fortnightly cadence agreed** between [[alex-sillars]], [[rory-beattie]], [[simon-hulbert]]. **Artificial invited in** (Alex's call: _"I would get artificially involved. I think all of us are semi-planning technical, so kind of makes sense that we can talk about those levels with them"_); [[srini]] likely added. Cadence may tighten as 1 Sep approaches. Boardroom-speaker / [[luca]] will own scheduling.
16. **Single Slack channel** for Artificial integration coordination, with a **per-application prefix convention** at the start of each message so multi-integration channels stay navigable. Pragmatic choice; not a policy.
17. **Artificial needs a top-level architecture diagram + sequence diagrams** to point at. [[simon-hulbert]] has both; will send the top-level diagram to [[rory-beattie]] first, with the sequence diagrams as a follow-up. Useful raw-folder candidate if shared.

## Decisions made
_None promoted to ADR. The source is concretisation + coordination, not a fresh architectural decision._

- **Operational notes captured** (not ADRs):
  - **[[boomi]] is the integration broker between [[artificial]] and [[party-application]]** — restating the same pattern that already operates for [[sanctions-processing]] ([[boomi]] → [[inrisk]] → [[ntt]]). Recorded on [[boomi]], [[artificial]], [[party-application]], [[high-volume]], and [[party-rearch-dependency-map]].
  - **API spec is subject to change until 1-Sep go-live, but not massively** — flagged on [[party-application]]'s Pending section + [[party-rearch-phase-1]] as a coordination-risk note for the Artificial counterparty.
  - **Convex × Artificial fortnightly cadence + dedicated Slack channel** — owned by [[luca]] (or Boardroom-PM) on the HV side, with [[srini]] + Artificial-side PMs included. Captured on [[party-rearch-ownership-matrix]].

## Actions

| # | Action | Owner | Application(s) | State |
|---:|---|---|---|---|
| 32 | Run the Convex × Artificial kick-off call 2026-05-20; walk through API spec + sequencing + dependency-on-InRisk-first cutover | [[rory-beattie]] · [[alex-sillars]] · [[simon-hulbert]] + Boardroom-PM | [[artificial]] · [[party-application]] · [[high-volume]] | open — 2026-05-19 |
| 33 | Stand up Boomi connectivity between [[artificial]] and [[party-application]]: pair [[srini]] (Boomi Integration Architect, HV-side) with [[joe-worsfold]] (Party-side); issue Artificial-side credentials + security tokens via Boomi | [[srini]] · [[joe-worsfold]] | [[boomi]] · [[party-application]] · [[artificial]] | open — 2026-05-19; **unblocks Artificial-side dev work** |
| 34 | Set up fortnightly Convex × Artificial integration catch-up (Alex + Rory + Simon + Srini + Artificial reps); cadence may tighten as 1 Sep approaches | [[luca]] (or Boardroom-PM) | [[artificial]] · [[party-application]] · [[high-volume]] | open — 2026-05-19 |
| 35 | Set up shared Slack channel for Artificial integration coordination with per-application prefix convention | Boardroom-PM | [[artificial]] · [[high-volume]] · [[party-application]] | open — 2026-05-19 |
| 36 | [[simon-hulbert]] to send [[rory-beattie]] the top-level HV/Artificial integration architecture diagram + the sequence diagrams that detail individual Party touchpoints | [[simon-hulbert]] | [[high-volume]] · [[artificial]] · [[party-application]] | open — 2026-05-19; **raw-folder candidate** when received |
| 37 | Build out the remaining MDM front-of-house pieces in dev: extend curation features (party + party-snapshot data-model coupling); migrate Neo4j ParseDB → DynamoDB; emit DU + sanctions events including submission-ID fan-out for fields not stored in Party | [[graph-team]] ([[alex-sillars]] · [[joe-worsfold]]) | [[party-application]] | open — restated 2026-05-19; tracked on [[party-rearch-phase-1]] |

_All actions appended to [[party-rearch-ownership-matrix]] as actions #32–#37 (numbering continues from #31 set on 2026-05-14)._

## Applications mentioned
- [[party-application]] — the subject; the MDM API is what Artificial will consume via Boomi
- [[high-volume]] — Convex implementation of the [[artificial]] vendor platform; this source materially reframes HV
- [[inrisk]] — referenced as (a) the precondition for HV/Artificial cutover, (b) a small additional integration HV/Artificial needs (some information only)
- [[party-curation-tool]] — _not mentioned_ but referenced indirectly via "curation features" still to extend

## Entities mentioned

### Platforms
- [[artificial]] — **first substantive treatment** in the wiki. Third-party vendor platform that Convex's HV programme is implementing; new Party MDM consumer for Phase 1; integrating via Boomi.
- [[boomi]] — **promoted to a first-class platform entity in this ingest** (had been loose prose across [[sanctions-processing]], [[high-volume]], [[party-rearch-dependency-map]] up to now). Integration broker — sanctions today + Artificial gateway from cutover.
- [[ntt]] — referenced indirectly via the sanctions analogy.
- [[data-universe]] — referenced as the downstream consumer that the strangler-pattern promise applies to (alongside sanctions).

### People

**Convex-side, in scope of this wiki**:
- [[rory-beattie]] — programme oversight; ran the framing of MDM scope to Simon.
- [[alex-sillars]] — PO, [[graph-team]]; on-record source for the dev-env MDM build status.
- [[simon-hulbert]] — Architect on the Artificial integration project; mapped to [[high-volume-team]] in this wiki. _This source materially broadens his role: not just "HV API contract" but "Artificial-onboarding architect across HV + Party + InRisk touchpoints"._
- [[joe-worsfold]] — referenced as author of the YAML API spec; Party-side counterpart to Srini on Boomi setup.
- [[sergiu-postolachi]] — referenced (_"Joe or Sergio"_) as a possible attendee at fortnightly catch-ups later in the project.

**Convex-side, new entity stubs created in this ingest**:
- [[srini]] — Boomi Integration Architect on the HV/Artificial project. Identified by first name only; surname TBC ([[open-questions#OQ-040]]). Counterpart to [[joe-worsfold]] on Boomi connectivity.
- [[luca]] — HV-side coordinator; will schedule the fortnightly Convex × Artificial catch-ups. Identified by first name only; surname + formal role TBC ([[open-questions#OQ-038]]).
- **Convex – Orega – 6B (Boardroom)** speaker — HV-side PM coordinator in the room. Speaks for and alongside [[simon-hulbert]] on scheduling and integration logistics. Almost certainly [[luca]] **or** a colleague who works directly with Luca; user-judgement deferred. Tracked as [[open-questions#OQ-037]] rather than stubbed.

**Convex-side, named-but-unresolved**:
- **Sam** — FDA for integrations. Accepted the 2026-05-20 Convex × Artificial call. **Advances [[open-questions#OQ-024]]** with the FDA-for-integrations role; FDA acronym itself is a new sub-question ([[open-questions#OQ-039]]).

**External**:
- **Artificial PM cohort** — referenced obliquely (_"the project manager guys that I work with are pushed back on the meeting tomorrow"_). No names captured.

## Concepts mentioned
- **Strangler-pattern stability promise** — restated to Artificial verbatim (claim #12 above); reinforces [[strangle-the-graph-via-proxy-events]].
- **InRisk-first cutover sequencing** — restated to Artificial verbatim (claim #13 above); reinforces [[inrisk-cuts-over-before-high-volume]].
- **Boomi as integration broker** — reaffirms the pattern that already runs in [[sanctions-processing]] (now extended to Artificial). Will be captured as the canonical role of [[boomi]] when its platform page is written.
- **API spec instability until go-live** — caveat introduced explicitly by Alex; not a Convex-internal posture, more a coordination expectation set with the consumer.

## Contradicts
- _None._ Every claim either reinforces existing wiki content or adds new, additive information.

## Reinforces
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — InRisk-first cutover sequencing; same framing now stated to a third party.
- [[sources/20260422-meeting-transcript-session-2]] — strangler-pattern promise (now restated externally); Sam-as-HV-neighbourhood identification ([[open-questions#OQ-024]]) advanced.
- [[sources/20260514-inrisk-high-level-refinement]] — InRisk integration as deliverable in own right ahead of HV gate; consistent with this source's "InRisk first, then us, a close second" framing.

## My notes (LLM)

1. **This is the first substantive source for [[artificial]] in the wiki.** The decision to promote it to a full platform entity (vs stub or no-page) was the user's call, not an LLM judgement — the source supports it: Artificial is referenced as the system Convex is implementing for HV, is a confirmed new Party consumer in Phase 1, and now has live commercial coordination (fortnightly catch-up, dedicated Slack channel, kick-off call). The "vendor platform underpinning HV" framing of [[high-volume]] follows directly from this source, where Simon explicitly maps Convex's HV programme to onboarding Artificial.
2. **Boomi being promoted to a platform entity is overdue.** Prior wiki had Boomi as inline prose in three places (sanctions, HV, dependency map). With two distinct vendor-integration patterns now hanging off it ([[ntt]] for sanctions + [[artificial]] for the Party consumer), having a single canonical page makes future ingests faster.
3. **Boardroom-speaker stays as an OQ deliberately.** They behave like an HV-side PM with scheduling authority + business-side oversight, and might well be [[luca]] themselves (the named meeting-scheduler is the same person who speaks in the room as the meeting-scheduler). But it's also possible they're Luca's manager / a colleague. Insufficient signal for a person stub; OQ is the right placement.
4. **Sam = "FDA for integrations"** advances [[open-questions#OQ-024]] but doesn't resolve it. We have a role (integration FDA) and a project (Artificial onboarding) and an indication of authority (accepted the kick-off call). What we're missing is surname + line manager + whether Sam sits inside [[high-volume-team]], inside an integrations / FDA office, or elsewhere. Worth keeping as a single OQ rather than re-categorising.
5. **"FDA" acronym** likely expands to **Functional Delivery Architect** or **Field Delivery Architect** (both common in vendor-product implementation work); could equally be a Convex-internal title. New OQ-039 flagged for the acronym specifically — quicker to resolve than full Sam-identity and would partially answer Sam's reporting structure.
6. **Strangler-pattern claim #12 deserves a callout on [[strangle-the-graph-via-proxy-events]].** It's the first time the stability promise has been stated **to a consumer** rather than as an internal architectural intent; the verbatim framing _"It might not be Party, it might be joined-up InRisk and Party data"_ is an explicit acknowledgement that the strangler-pattern's source-of-truth move includes inferring some fields from InRisk-joined data rather than directly from Party. Worth adding as a refinement to the ADR.
7. **Action #33 is the most time-sensitive new item.** Until [[srini]] + [[joe-worsfold]] complete Boomi connectivity setup + issue Artificial's credentials, Artificial cannot exercise the dev-env MDM API and the fortnightly cadence has nothing to bite on. Worth tracking against [[party-rearch-phase-1]]'s done-state.

## Open questions
_Captured here for traceability; canonical entries live in [[open-questions]]._

- **OQ-024** (advanced) — Sam now confirmed as "FDA for integrations" on the Artificial project; surname + line manager TBD.
- **OQ-037** (new, people identification) — identity of the Convex – Orega – 6B Boardroom speaker; likely [[luca]] or a colleague.
- **OQ-038** (new, people identification) — [[luca]]'s surname + formal role on the HV/Artificial project.
- **OQ-039** (new, scope/architecture) — what does "FDA" stand for in this context? (Functional Delivery Architect / Field Delivery Architect?). Required to understand Sam's reporting line.
- **OQ-040** (new, people identification) — [[srini]]'s surname (Boomi Integration Architect; "Srini" is the only handle captured).

## Related
- [[high-volume]] — materially reframed by this source
- [[high-volume-architecture]] — reframed alongside
- [[artificial]] — new platform page (this ingest)
- [[boomi]] — new platform page (this ingest)
- [[srini]] · [[luca]] — new person stubs (this ingest)
- [[party-application]] · [[party-application-architecture]] — Artificial added as second new downstream consumer; Boomi gateway recorded
- [[party-rearch-phase-1]] · [[party-rearch-phase-1-summary]] — Artificial folded into the integration-only [[high-volume]] row's framing
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] — Artificial arrow + new cross-cutting rows + new actions
- [[strangle-the-graph-via-proxy-events]] · [[inrisk-cuts-over-before-high-volume]] — both reinforced; no decision change
- [[open-questions]] — OQs above
