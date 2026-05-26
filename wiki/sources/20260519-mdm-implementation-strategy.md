---
type: source
title: MDM Implementation Strategy — Joe + Alex + Rory three-way (2026-05-19)
kind: meeting
created: 2026-05-26
updated: 2026-05-26
tags: [source, meeting, party-rearch, mdm, implementation, current-state, widget]
raw: raw/20260519 - MDM Implementation Strategy.md
author: [joe-worsfold, alex-sillars, rory-beattie]
date_of_source: 2026-05-19
project: party-rearch
status: draft
---

# MDM Implementation Strategy — Joe + Alex + Rory three-way (2026-05-19)

## Summary
47-minute three-way Convex video call held the **afternoon of 2026-05-19** (~14:17–15:05 BST). [[rory-beattie]] frames the call around _"we don't really have an option to back out at this stage … force ourselves down this route … how do we get it over the line"_ — i.e. a programme-delivery-focused review of MDM's current implementation state, not an architecture session. [[joe-worsfold]] walks the current codebase end-to-end (backend / curation UI / widget / API / audit / Dynamo / backfill state). [[alex-sillars]] runs a structured interrogation against that walk. Rory makes live descope calls (UI/UX parity, not enhancement), formalises owners ([[billy-calladine]] on the widget rebuild + InRisk-side dispatch; [[tomas-sivo]] on PCT curation gap-analysis tickets), and shields Joe from peripheral concerns. Closing line: _"we're a lot further along than I suspected we even were."_ Distinct meeting from the [[sources/20260519-party-integration-timelines]] call earlier the same day (~12:33–13:01 BST) — different attendees, different topic.

## Bibliographic info
- **Author(s) / attendees**: [[joe-worsfold]] (Tech Lead / Architect, [[graph-team]]); [[alex-sillars]] (Product Owner, [[graph-team]]); [[rory-beattie]] (programme oversight, [[tech-tooling]]).
- **Date**: 2026-05-19, ~14:17–15:05 BST (~47 min).
- **Venue / source system**: Convex video call; transcript captured via meeting-recording tool.
- **URL**: n/a
- **Raw file**: `raw/20260519 - MDM Implementation Strategy.md`
- **Adjacent calls** referenced in-transcript:
  - **Just before**: Joe + Alex had finished _"another call"_ immediately prior to this one (subject not stated).
  - **Just after**: Joe + Rory have a follow-on call about getting InRisk data so MDM can proxy the party event (sanctions-adjacent; the "we're talking about that tomorrow" reference in this transcript is about a wider sanctions/Simon session not captured here).
  - **Earlier the same day**: [[sources/20260519-party-integration-timelines]] — Rory + Simon + Alex + Boardroom-PM, ~12:33–13:01 BST. Different attendees, different topic.

## Key claims

Grouped by theme for retrieval.

### Rory's role posture (delivery-oriented programme management)

1. **Frames the call as delivery-focused.** _"We don't really have an option to back out at this stage … let's force ourselves down this route, and pull that band-aid … get this over the line. How do we get it over the line?"_ Pairs with the closing _"you just worry on getting the thing built"_ to Joe — the meta-stance for the rest of the call.
2. **Makes a live descope call on UI/UX.** Joe flagged the absence of user-centred design as a POC gap; Rory: _"I think that's exactly it. That's something that we can make a call on right now and say, look, things like UI, UX for feature parity components, just don't worry about that sort of thing at all, right? Let's get a working thing, right? Because that is the critical path."_ Alex agrees: _"UI, UX, I've never cared about, never will."_ Reinforces (and pre-empts) the parity-not-enhancement principle on [[inrisk-cuts-over-before-high-volume]].
3. **Formalises ownership in-meeting.** Two assignments: (a) **[[billy-calladine]] owns the widget rebuild and the InRisk-side dispatch-code change.** Rory: _"Let's formalize that, Alex. Let's make it very clear to Billy that he's responsible for that, and we need him to kind of just own it and run with it."_ Joe acknowledges: _"there's a part of me that's like — I'm sitting on the epic which is around the data-model proxying and the legacy migration, and Ben's been helping with that one, and then I think Billy's perfectly placed to help with this one, for party search and integration within RISC."_ (b) **[[tomas-sivo]] picks up curation-UI gap-analysis ticket creation**, then squad picks up from sprint backlog. Joe: _"we can lean on Tom Ash if it's a case of trying to get the tickets together, and then the rest of the squad can pick up the tickets in sprints."_ Rory parks the Tom Ash specifics: _"let's have a think about that one in a second."_
4. **Shields Joe from peripheral concerns.** On the Boomi pass-through wrapper: _"I think just leave that to those guys though, Joe — it's another thing that you don't need to worry about, like give your output, it's their problem from then on in."_ On the InRisk-side change to which-widget-they-call: _"How do you feel about making that Billy's responsibility, Joe, so you just kind of forget about it?"_
5. **Owns own newness explicitly.** Rory throughout: _"I appreciate I'm being super distracted by the dog … I'm being a bit of an idiot here … I appreciate you probably know a good bit of this already … step in if I say something that you don't agree with."_ User-side framing: _"I am still programme manager but will pay a little closer attention to help move the project to completion"_ — the role isn't expanding, just engaging more closely with the build during the final stretch.
6. **Final-state assessment.** Closing exchange: _"we're a lot further along than I suspected we even were. So I'm not overly worried about this."_ Joe: _"we're in the like dotting I's, crossing T's, make sure there's no unknown unknowns, and then it's the thing I was always between team collaboration is the thing that I worry about — not that it'll be bad, just that it takes time."_

### MDM current implementation state (Joe's walkthrough)

7. **Monorepo, DDD-structured.** Old graph had party backend / curation UI / party search as separate packages; new world has them all together in a monorepo, with a **domain area** holding most of the core model. Joe: _"because I've done it, it's domain-driven development, you've got a domain area which actually has most of the core model."_
8. **Backend core model is mostly done.** Change classification, Dynamo streaming, REST API auto-generation are built; the remaining backend work is the **proxy-event design** for party events without storing InRisk data (covered under sanctions-related "spike in sprint"). Joe: _"I've got all of the backend there as in a current state. So like where I'm actually still working on stuff is more in the space of saying I've got a final beta that I'm happy with — my version one … the same sort of shape where you've got roles and views. But if I'm trying to remove interest data, how do I make sure I can proxy the party event?"_
9. **REST API auto-generated from code.** Joe: _"the API spec gets auto-generated, so it's very easy to share with do-me-an-artificial, so I've kind of got all of my stuff in order for the collaboration."_ Spec is accessible via a Swagger-style document endpoint (currently RBAC-locked because the rest of the API is RBAC-locked), but Joe can hand-share. Joe shared a **Scala version** to [[simon-hulbert]] earlier; can share Scala or Swagger flavours. **No GraphQL** — single REST API by design (reduces total surface for ops to support; mirrors decision already on [[party-application]]).
10. **Curation UI is fully functional in dev**, with cosmetic changes pending and data backfill needed in lower environments. Joe: _"we've got a fully functioning curation UI which is already deployed — I say fully function, the dev one — I just need to do a few changes and it'll be fine. I need to backfill into those low environments, the data has to be moved over to do some of it, but it is functional."_
11. **Search widget exists but is card-based** (Cursor's UX opinion), whereas current InRisk widget is table-with-columns. Gap-analysis is the next step. Joe: _"what I built was again a cursor opinion thing — this is what I think the UX should be like, so it's cards … But currently what we actually do in party search is render a table with lots of columns for different bits of the data, which we can fairly easily do as well."_ Decision in-call: parity, not enhancement.
12. **DynamoDB stood up in dev + integration**, but **integration environment is broken** on JumpCloud SAML Cognito access — _"it's partially available in integration, it's doing the JumpCloud SAML Cognito access, so no one can actually do anything with it."_ Dev env will shortly have stable alpha-state data after Joe re-runs backfill (next 8). New OQ candidate.
13. **Data model is complete in dev** (and identical shape across all envs — just no backfill in integration yet). Subject to change but only additively. Joe: _"the model is the same across all in the sense of the shape of it … additional fields here or there, nothing fundamentally broken."_
14. **Three iterations of backfill done already.** Stable "alpha" backfill expected by end of week (2026-05-19 was a Tuesday → Fri 2026-05-22). Joe: _"I've been killing myself the last few days to just get to the point where we can rerun the backfill so that you can have hundreds or thousands of real dev data, and integration ideally because that would be more stable. Just give me a bit of time, I'll let you know as soon as it's ready, I think it would be ready by the end of this week."_ Going forward: only additive / modification extensions, no re-wiping.
15. **Backfill bypasses API validation by design.** API is strict on validation + schema; the event-flows that drive backfill write **directly to the repo** (bypassing the API) to tolerate messy Graph-world data, then fixed iteratively. Joe: _"the API is really really strict about the validation and the schema, but the event flows have access direct to Dynamo. They go straight to the repo rather than via the API, and it's kind of by design — because I'd rather take in the data as is in GraphWorld, which isn't always perfect, and then fix it as we go."_ Acknowledges this is a choice; could be hard-gated instead (multi-step backfill), to be decided once first low-env runs land.

### Three known consumers of party events; the proxy-event design fork

16. **Three known consumers of party events**, enumerated by Joe for the first time on the record: **DU, HV, Boomi (sanctions)**. _"At least three important consumers, the DU, HV, and BUMI."_ Per Joe, **DU doesn't care about submission/requirement IDs** in the event (DU already has them from InRisk).
17. **Joe wants MDM to NOT store InRisk data**, but Phase-1 proxy events today need to carry submission/requirement context because Boomi consumes them for sanctions. Two options for proxying without storing:
    - **(a) InRisk endpoint**: InRisk exposes `client_id → {submissions, requirements}` API for Joe to call when emitting an event. Joe: _"if InRisk could just say to me, here's an API endpoint that I can call and say given a client ID, give me all of the requirements and submissions, then I can still proxy that event."_ Mapper stays Joe-side; InRisk stays the owner of InRisk data.
    - **(b) Pull from Knowledge Graph**: the Graph team's existing KG (a separate read-replica graph DB also owned by the Graph team — contains InRisk + Party data; powers InRisk search) has all the InRisk info needed. Joe: _"on the spike as well, rather than try to get InRisk to create an endpoint to do it, why don't we just pull it out of Knowledge Graph, because Knowledge Graph has all of the InRisk information."_ Joe is **wary of (b)** — _"it will be eventually consistent, might cause us more problems … I just don't trust the eventual consistency problem of being accurate enough."_ Alex: _"the question of does whom-party and knowledge graph all have consistent data is one that scares me."_
    - **Joe's preference**: (a). Decision deferred until the sanctions session ("tomorrow's") with [[simon-hulbert]]. **New OQ.**
18. **End-state (Phase 2+) sanctions-event flow restated.** Each system (Party, InRisk, etc.) emits its own event; a separate sanctions domain composes if a composite is needed. Joe: _"in the future you do that, and InRisk do that, and then if Boomi need a composite of both, then that's the job of some domain — could be Boomi's, or probably someone else."_ Reinforces existing [[sanctions-processing]] consensus that Boomi is the wrong place; reinforces [[open-questions#OQ-032]]. Rory: _"the assumption we have is that to get Boomi buy-in, DU buy-in, to make the change in reasonable time, is unrealistic — so we have to come up with this proxy solution"_ — frames why Phase 1 keeps the proxy with the InRisk-data lookup, rather than restructuring events now.
19. **Boomi's current behaviour described from Party-side perspective.** Boomi consumes Party events that already contain submission/requirement context, then calls InRisk for further enrichment, builds its own composite of party + submission data, decides whether to fire sanctions checks. Joe characterises the resulting integration topology as _"this weird triangle of data going backwards and forwards"_ — coupling Party + InRisk + Boomi awkwardly. Reinforces [[sanctions-processing]] "wrong place" framing.

### Widget shape, language, and InRisk-side dispatch

20. **InRisk is primarily Tailwind, not Chakra** — corrects an assumption-of-Chakra that had crept in across earlier sources. Rory: _"that's a bit of a lie. They have some Chakra V2 components, but it's like a drop-down list here, and a button there. They're primarily on Tailwind and a few other bits and pieces. So the closer you can get to raw CSS when you build it, the better."_ Joe: _"I'll try and keep it to just raw react and yeah … I will just reuse them if I can rewire in the OpenSearch results."_ Sharpens [[inrisk-cuts-over-before-high-volume]] Part B from "design-system-agnostic library" to "raw React + close-to-raw CSS, reusing existing InRisk components where possible".
21. **PCT keeps Chakra-3 + design system.** PCT is standalone (no package-dependency clashes), so the modern stack stays. Rory: _"curation is standalone — curation doesn't have any package dependency clashes, i.e. design system, whatever else, so we can use Chakra 3 safely for party curation tool, we just can't use it at all for the widget."_ Joe: _"I can work with that, I think."_
22. **One widget component, three variants (parties / brokers / others), parameter-driven.** Joe confirms: _"the widget for me encompasses all of the different versions of that same party search widget, so parties, brokers."_ Rory probes the architecture: _"is there kind of a shell component which is the container — the drawer that manifests in the in-risk world — and then within that, you can manifest either one variant, another variant, or the third variant of the widget components within that one shell?"_ Joe: _"that is how I've built it"_, with parameter-passed differentiation (_"just like parameterized components"_); each variant hits a different OpenSearch endpoint and renders different cards.
23. **InRisk-side dispatch code exists today and needs changing.** Alex: _"in-risk have something in their code that determines which of our widgets they call. Yes, they pass parameters to us, but they specifically request it, so we'd need to change that on InRisk's side."_ This becomes the InRisk-side half of Billy's formalised remit (see claim #3).
24. **Existing widget framework on InRisk-side is unfamiliar to Joe**; in-meeting parking. Joe: _"I haven't had a look at the inrich code for a long time."_ Rory parks: _"that's a-okay, and that's kind of their problem, right? We've got enough on the flight over on this side of the park."_ Then immediately reassigns to Billy (claim #3).
25. **Cursor (AI tool) referenced as a delivery accelerant.** Rory: _"this is, like, one of the things that we can actually lean on Cursor for … Billy can just point Cursor to say rebuild this exact thing, but don't use have any reliance on that design library at all."_ Joe: _"it won't be a huge problem … the original one was just built in a couple of days off the back end of the API."_ Operational note only — not an architectural commitment.

### PCT new UI shape (revisions + filters; mark-for-review baked in)

26. **PCT new UI is revisions + filters, not Jira's tabs.** Joe: _"PCT today is obviously a table with tabs for different things, and then when you open the ticket, it's obviously Jira behind it. In the new world, like I've at least done it for the revisions part — fairly similar, but instead of having tabs, you can have filters. And then the actual tickets themselves look different."_
27. **Mark-for-review / approve / request-changes built into the ticket UI.** Joe: _"it'll have the ability to mark for review, approve, or request changes."_ Parity question outstanding: do DataOps users prefer the new flow over the old? Becomes a Phase-1 confirmation rather than a blocker. **New OQ candidate.**

### Audit, recovery, observability (already in place)

28. **Versioning system is built and functional.** Every party change is recorded as a revision. Joe: _"every single change in MDM world for the actual party is recorded as a revision — there's no sort of way around it. So you've got not just the classic created-at, updated-at stuff, but you've got a change history and a full changelog for the actual data. That's part of just the versioning system."_
29. **Access logs streamed from API Gateway → S3 (Athena-searchable).** Joe: _"access logs are all being streamed from API Gateway into an S3 bucket, so I've got full access logs that can be searched via Athena in S3."_
30. **DynamoDB streams → events + searchable audit bucket.** Joe: _"every single DynamoDB change gets streamed, because that's how Dynamo works in terms of changes. I sit on top of that and classify those changes into events, but I also take those streams and chuck those into an audit bucket that's searchable and shows what's changed every day."_
31. **Tracking is by email** for "who-did-what" (same pattern as today).
32. **Point-in-time recovery via Dynamo.** Joe: _"Dynamo does point-in-time recovery, so depending on how we structure it, we could literally get per-second rebuilds of the whole dynamo … bit like how Postgres does snapshots, but probably slightly faster because the document stores are lighter weight."_
33. **Recovery via re-stream from audit bucket also possible.** Joe: _"because I'm streaming all of my database updates into an audit file, I can just rerun those — there might be instructions to do that, actually, to re-stream."_
34. **Monitoring already set up with Datadog**; alerts still to be built out (Joe has a ticket for it).
35. **Compute is lighter than current ECS.** Joe: _"the actual deployments and services themselves are way more lightweight because I'm not using ECS — should give us a lot more flexibility and more control."_

### Dynamo migrations (Phase-1 gap / fast-follow candidate)

36. **Dynamo migrations are not Postgres-style scripts.** Joe: _"Dynamo doesn't work like Postgres — you can't do migrations like Postgres or Neo4j where you write a big Cypher or big SQL script, so that'll have to be done differently. I want to build that in a way that's actually a bit more self-service in the future, but for now we can make it just for us, and then eventually share it with [DataOps]."_ Open Phase-1-vs-fast-follow scope call. **New OQ candidate.**

### Coordination / next steps

37. **Rory to schedule a follow-up across the next couple of days** to consolidate immediate focuses and next-steps, then distribute. Rory: _"I'll set something up for the next couple of days, and we can pick up this conversation … we've got the immediate focuses, and then the next set we can lay out neatly in front of you, just to guide the way and distribute it accordingly."_
38. **Sanctions / proxy-event-design conversation is the follow-on call** Joe + Rory move into right after (Joe: _"there's another call I put in for this conversation about getting the in-risk data so I can proxy the party event"_). Alex: _"should Alex have been on that?"_ — Rory will relay back to Alex if needed.

## Decisions made
_None promoted to ADR. The source is implementation-state clarification + delivery-ownership formalisation, not fresh architectural decision-making. Live calls noted as **refinements** to existing ADRs / decisions are folded back into their canonical pages:_

- **Refinement to [[inrisk-cuts-over-before-high-volume]] (Part B / parity posture)**: InRisk is primarily Tailwind (not Chakra); the InRisk widget should be **raw React + close-to-raw CSS**, reusing existing InRisk components where possible. Sharper than "design-system-agnostic library" alone (claim #20). UI/UX parity-not-enhancement reinforced (claim #2) ahead of build.
- **Refinement to [[strangle-the-graph-via-proxy-events]]**: explicit enumeration of the **three known consumers** of party events (DU + HV + Boomi/sanctions) (claim #16); the proxy adapter's design-fork between InRisk-endpoint vs KG-pull (claim #17, deferred to sanctions session).
- **Ownership formalisation**: [[billy-calladine]] on the widget rebuild + InRisk-side dispatch change (claim #3a); [[tomas-sivo]] on PCT curation gap-analysis ticket creation (claim #3b). Both fold into [[party-rearch-ownership-matrix]].

## Actions

| # | Action | Owner | Application(s) | State |
|---:|---|---|---|---|
| 38 | Formalise [[billy-calladine]]'s ownership of the widget rebuild (raw React + close-to-raw CSS, reuse InRisk components where possible) AND the InRisk-side dispatch-code change (the existing "which widget to call" logic) | [[alex-sillars]] (to communicate to Billy) | [[party-application]] · [[inrisk]] | open — 2026-05-19 |
| 39 | Resolve the proxy-event-with-InRisk-data design fork: InRisk endpoint (`client_id → {submissions, requirements}`) vs. pull from Knowledge Graph. Joe prefers (a); decision pending sanctions session with Simon. **Gates the proxy adapter** | [[joe-worsfold]] (proposer) + [[john-trahearn]] / [[kris-mokrzycki]] (InRisk-endpoint side) | [[party-application]] · [[inrisk]] · [[knowledge-graph]] | open — 2026-05-19; tracked at new OQ |
| 40 | Unblock the integration environment: fix JumpCloud SAML Cognito access on the integration-env DynamoDB so anyone can exercise it | [[joe-worsfold]] (or platform team) | [[party-application]] | open — 2026-05-19; tracked at new OQ |
| 41 | Re-run backfill in dev (and integration once unblocked) to land **stable alpha** — hundreds–thousands of real dev rows. Target: end of week 2026-05-22 | [[joe-worsfold]] · [[ben-joseph]] | [[party-application]] | open — 2026-05-19; only additive model changes from this point forward |
| 42 | Decide Phase-1 scope for **Dynamo migration tooling**: ship a self-service tool in Phase 1, build a Graph-team-internal version + treat self-service as fast-follow, or accept the cost ad-hoc | [[joe-worsfold]] (proposer) · [[alex-sillars]] | [[party-application]] | open — 2026-05-19; tracked at new OQ |
| 43 | Confirm DataOps parity with the new PCT UI (revisions + filters; mark-for-review / approve / request-changes baked in) — can DataOps users do everything they need? | [[alex-sillars]] · [[tomas-sivo]] | [[party-curation-tool]] | open — 2026-05-19; tracked at new OQ |
| 44 | Hand the curation-UI gap-analysis ticket authoring to [[tomas-sivo]]; the squad picks up the resulting tickets in-sprint | [[alex-sillars]] (route the ask) | [[party-curation-tool]] | open — 2026-05-19 |
| 45 | Rory to schedule a follow-up call in the next couple of days to consolidate immediate focuses + next steps + distribute | [[rory-beattie]] | [[party-application]] · [[party-curation-tool]] · [[inrisk]] | open — 2026-05-19 |

_All actions appended to [[party-rearch-ownership-matrix]] as actions #38–#45 (numbering continues from #37 set on the [[sources/20260519-party-integration-timelines]] ingest)._

## Applications mentioned
- [[party-application]] — the subject; current implementation state walked end-to-end.
- [[party-curation-tool]] — fully functional in dev; new UI shape (revisions + filters; mark-for-review baked in).
- [[inrisk]] — primary widget consumer; InRisk-side dispatch-code change required; primarily Tailwind not Chakra (correction).
- [[high-volume]] — referenced as one of the three event consumers via Boomi; widget irrelevant (API-only).
- [[knowledge-graph]] — **first time materially in scope as a separate sibling application**: explicitly identified as a separate read-replica graph DB owned by [[graph-team]] (NOT internal Neo4j of party-application — corrects the 2026-04-22 lint pass conclusion). Joe proposes it as the fallback datastore for InRisk-context lookup during proxy emission.

## Entities mentioned

### People

**In-room**:
- [[joe-worsfold]] — driving the implementation walkthrough; owns the data-model-proxying + legacy-migration epic.
- [[alex-sillars]] — running the structured interrogation. Notable verbatim: _"UI, UX, I've never cared about, never will."_
- [[rory-beattie]] — programme-delivery framing; live descope + ownership-formalisation calls.

**Named-but-not-present**:
- [[billy-calladine]] — formalised as owner of widget rebuild + InRisk-side dispatch-code change.
- [[tomas-sivo]] ("Tom Ash" in transcript) — formalised as PCT curation gap-analysis ticket author.
- [[ben-joseph]] — Joe's pair on backfill + spike work.
- [[simon-hulbert]] — sanctions session referent ("tomorrow's" conversation; Joe already shared the Scala API spec).
- [[john-trahearn]] / [[kris-mokrzycki]] — referenced as the InRisk-side counterparts who tried Chakra 3 + Design Studio (it didn't work — restated in-meeting).
- **"Chris and John"** in Joe's _"Chris and John were willing to try to see if they could"_ → resolves to [[kris-mokrzycki]] + [[john-trahearn]] respectively; consistent with Pass-B resolution of "Kris" / "Chris" phonetic drift on [[sources/20260422-meeting-transcript-session-1]].

### Teams
- [[graph-team]] — owners of MDM, PCT, the new widget, and now (corrected) the Knowledge Graph.
- [[prebind-team]] — InRisk-side counterparts; consumers of the widget; targets of the InRisk-side dispatch code change.

### Platforms / applications
- [[knowledge-graph]] — new first-class entity (see correction below).
- [[boomi]] — referenced as the sanctions-orchestration consumer of party events; pass-through wrapper work for HV/Artificial parked back with the HV-side team.
- [[data-universe]] — referenced as one of the three event consumers (doesn't care about submission/requirement IDs — already has them from InRisk).
- [[ntt]] — referenced in passing in the sanctions context.

## Concepts mentioned
- **Roles-vs-views model** ([[roles-vs-views-litmus-test]]) — Joe re-references the same model from his backend work: _"I've got a final beta that I'm happy with — the same sort of shape where you've got roles and views."_
- **Strangler pattern** ([[strangle-the-graph-via-proxy-events]]) — implicit throughout; the proxy-event-with-InRisk-context lookup question is the key open thread.
- **Sanctions processing** ([[sanctions-processing]]) — the "wrong place" framing reinforced from Party-side observation of the Boomi triangle.

## Contradicts
- **[[sources/20260422-meeting-transcript-session-2]] + 2026-04-22 lint pass conclusion on Knowledge Graph.** The lint pass resolved [[open-questions#OQ-010]] by reclassifying KG as _"the Neo4j instance internal to [[party-application]]"_. This source contradicts that: per user clarification on 2026-05-26, **Knowledge Graph is a separate read-replica graph DB also owned by [[graph-team]]** — contains InRisk + Party data; powers InRisk search functionality. Joe's in-call line _"why don't we just pull it out of Knowledge Graph, because Knowledge Graph has all of the InRisk information"_ only makes sense if KG is a sibling system, not party-application's internal store. Corrections:
  - New [[knowledge-graph]] application page created.
  - OQ-010 re-opened as **OQ-010-R**, with the corrected answer.
  - [[contract-buckets]] · [[party-rearch-dependency-map]] · [[party-application]] · [[party-application-architecture]] · [[party-rearch-phase-1-summary]] · [[open-questions]] updated to reflect the sibling-system framing.
  - ⚠ Contradiction callouts added on the affected pages with citations to both this source and the 2026-04-22 lint conclusion.
- **Joe's primary-store framing on [[party-application]]**: the existing identity-page language _"Knowledge Graph — a Neo4j instance sitting inside the Party Application infrastructure"_ is corrected. The party-application's current primary datastore is its own Neo4j (which is being replaced by Dynamo + OpenSearch under [[party-rearch-phase-1]]); KG is a separate Graph-team-owned graph DB.

## Reinforces
- **[[inrisk-cuts-over-before-high-volume]] (Part B parity posture)** — Alex's "UI, UX, I've never cared about, never will" + Rory's live descope on UI/UX parity precede and reinforce the formal parity-not-enhancement principle.
- **[[sources/20260514-inrisk-high-level-refinement]]** on widget mechanics — confirms current InRisk widget is query-parameter-driven and Joe's new widget should retain the parameterised dispatch (parties / brokers / others) at parity.
- **[[strangle-the-graph-via-proxy-events]]** — proxy adapter remains the right call for Phase 1; restated reason (Boomi/DU/InRisk would need to change their consumer code to absorb a restructured event, _"unrealistic to get buy-in in reasonable time"_).
- **[[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] on sanctions** — reinforces the "wrong place" framing for the Boomi-orchestrated sanctions flow, this time from Joe's Party-side observation of the integration topology (_"this weird triangle of data going backwards and forwards"_).
- **[[party-rearch-phase-1-summary]] HV row** — Joe's three-consumer enumeration (DU + HV + Boomi) reinforces the existing Phase-1 consumer footprint and aligns with the Artificial reframing from [[sources/20260519-party-integration-timelines]].
- **[[sources/20260519-party-integration-timelines]] claim #2** — Joe shared a Scala version of the API spec to Simon "the other day" (in addition to the YAML the Boardroom call referenced). Operational detail; advances the picture of the API surface area Joe has exposed to integration partners.

## My notes (LLM)

1. **The Knowledge Graph correction is the most consequential single thing in this ingest.** Joe's proposal to "pull from KG" was either (a) a confusion in the transcript or (b) requires KG to be a sibling system to party-application. The 2026-04-22 lint conclusion that KG = party-application's internal Neo4j was the "tidiest" of the available interpretations at the time, but it doesn't survive contact with this source's content. User-clarified on ingest. The downstream cleanup (re-opening OQ-010, creating [[knowledge-graph]], reframing party-application's primary-store description, updating contract-buckets and the dependency map) is the bulk of the structural work in this ingest.
2. **Rory's role posture sits between two unhelpful framings.** "Architect who designs" overclaims; "passive observer" undersells. The right framing per user steer is _"programme manager paying closer attention to help move the project to completion"_. Captured on [[rory-beattie]] with that softened wording. Multiple verbatim quotes preserved as evidence of style (descope / formalise / shield) rather than promoting Rory's claims further.
3. **The InRisk-endpoint vs KG-pull design fork is genuinely open.** Joe stated a preference but no decision was taken in-meeting; it's deferred to a sanctions-context call. Worth a new OQ rather than a pending-note, because it (a) gates the proxy adapter, (b) has cross-team implications (InRisk-side endpoint work vs Graph-team-side KG-read work), and (c) is the kind of question that can drift without a formal owner.
4. **Three known consumers of party events** is the cleanest single-line summary of the Phase-1 event topology we have. Worth promoting onto [[strangle-the-graph-via-proxy-events]] and [[party-rearch-dependency-map]] as a structural fact rather than burying it in claims. Note the Artificial reframing from [[sources/20260519-party-integration-timelines]] adds a fourth consumer (Artificial via Boomi for the change-event-push-back path) — but the enumeration in this source predates that integration going live, and Joe's framing _"at least three"_ leaves room for the fourth.
5. **The Tailwind correction matters tactically but doesn't change ADR direction.** [[inrisk-cuts-over-before-high-volume]] Part B already said "design-system-agnostic"; the Tailwind detail is a concretisation, not a contradiction. Folded into the architecture page + ADR Open-risks rather than introducing a new ADR.
6. **The audit/recovery section is the page-of-detail that [[party-application-architecture]] was waiting for.** Existing architecture page was a stub on the entire Observability + Internal-structure axes. This source lets us write substantive Technologies-and-Services + Observability sections rather than leaving them as "_pending ingest_" placeholders.
7. **Backfill bypassing API validation by design** is the kind of choice that becomes a known-constraint later. Captured on the architecture page as an explicit design choice (with Joe's reasoning) rather than letting it look like an oversight in a future review.
8. **"We're a lot further along than I suspected we even were" is worth preserving verbatim** on the project page or summary. Sentiment indicators from programme-oversight are useful inputs to "what should I worry about next" — and this one closes a question that has been implicit on [[party-rearch-phase-1]]'s done-state.

## Open questions
_Captured here for traceability; canonical entries live in [[open-questions]]._

- **OQ-010-R** (re-opened, scope/architecture) — Knowledge Graph identity / role. New answer: separate read-replica graph DB owned by [[graph-team]], contains InRisk + Party data, powers InRisk search functionality. Supersedes the 2026-04-22 lint-pass resolution of OQ-010.
- **OQ-041** (new, scope/architecture) — Proxy-event-with-InRisk-data design fork. InRisk endpoint vs Knowledge-Graph pull. Joe prefers the InRisk endpoint; KG pull has eventual-consistency concerns. **Gates the proxy adapter.** Decision pending sanctions session with [[simon-hulbert]].
- **OQ-042** (new, scope/architecture) — Integration environment access. JumpCloud SAML Cognito access on the integration-env DynamoDB is broken (_"no one can actually do anything with it"_). Unblocking task for any integration-env smoke testing or Artificial-side dev exercise.
- **OQ-043** (new, design open call) — Dynamo migration tooling Phase-1 scope. Ship a self-service tool in Phase 1, build Graph-team-internal first + fast-follow, or accept the cost ad-hoc. Joe's preference is self-service eventually, but timing is open.
- **OQ-044** (new, design open call) — PCT new-UI dataops parity confirmation. Will DataOps users prefer revisions + filters (with mark-for-review / approve / request-changes baked in) over the existing table + tabs + Jira flow? Needs user-testing or walkthrough confirmation before cutover.

## Related
- [[knowledge-graph]] — new application page (this ingest)
- [[party-application]] · [[party-application-architecture]] — heavy current-state detail update
- [[party-curation-tool-architecture]] — new UI shape
- [[inrisk-architecture]] — Tailwind correction; InRisk-side dispatch-code change formalised
- [[inrisk-cuts-over-before-high-volume]] — Part B widget posture sharpened (raw React + close-to-raw CSS)
- [[strangle-the-graph-via-proxy-events]] — 3-consumer enumeration; InRisk-endpoint vs KG-pull design fork
- [[rory-beattie]] · [[billy-calladine]] · [[joe-worsfold]] · [[alex-sillars]] · [[tomas-sivo]] · [[ben-joseph]] · [[graph-team]] — role / claim updates
- [[party-rearch]] · [[party-rearch-phase-1]] · [[party-rearch-phase-1-summary]] · [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] — standing refreshes
- [[open-questions]] — OQ-010 re-opened as OQ-010-R; OQ-041–OQ-044 added
- [[sources/20260519-party-integration-timelines]] — sibling source from earlier the same day (different attendees, different topic)
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] · [[sources/20260514-inrisk-high-level-refinement]] — sanctions / widget framing reinforced
