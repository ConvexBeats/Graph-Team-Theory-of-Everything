---
type: source
title: "Meeting Transcript — Session 2 (2026-04-22)"
kind: meeting
created: 2026-04-22
updated: 2026-04-22
tags: [meeting, architecture-review, session-2]
raw: raw/20260422 - Meeting Transcript - Session 2.md
date_of_source: 2026-04-22
status: draft
project: party-rearch
---

# Meeting Transcript — Session 2 (2026-04-22)

## Summary
Second architecture-design session on the Party Application re-architecture, attended by the Graph Team, Tech Tooling, and the Architecture Team. The room landed the load-bearing decisions that make the programme deliverable: MDM will "strangle the graph" by emitting **proxy events** to the Data Universe (DU, owned by the Analytics Team) in the existing graph event shape, so downstream consumers see no change on cutover; and PCT + MDM must go live together. Also clarified current-state tech (Neo4j/Cypher) vs. target (DynamoDB + OpenSearch), the versioned party API surface (`by ID+version` / `by ID+timestamp` / `by ID latest`), the InRisk interim-state dependencies (3 changes), and a first-cut timeline (~2 sprints for core MDM work from Graph Team, ~2–3 months overall). The programme's forcing function remains **[[high-volume]] go-live on 1 September**; in-room speculation about a split training/production date is deprecated per Pass B confirmation.

## Bibliographic info
- **Date**: 2026-04-22
- **Format**: meeting transcript (machine-generated from audio; speaker-numbered with named speakers where auto-identified)
- **Raw file**: `raw/20260422 - Meeting Transcript - Session 2.md`

## Attendees

| Speaker tag | Name | Role | Team |
|---|---|---|---|
| Rory Beattie | [[rory-beattie]] | Project oversight / detailed design / timeline management | [[tech-tooling]] |
| Sergiu Postolachi | [[sergiu-postolachi]] | Scrum Master | [[graph-team]] |
| Speaker 2 | [[suzanna-whitefield]] | Architect | [[architecture-team]] |
| Speaker 4 · Speaker 14 | [[billy-calladine]] | Engineer / Developer | [[graph-team]] |
| Speaker 5 | [[joe-worsfold]] | Tech Lead / Architect | [[graph-team]] |
| Speaker 6 | [[alex-sillars]] | Product Owner | [[graph-team]] |
| Speaker 9 | [[ben-joseph]] | Engineer / Developer | [[graph-team]] |
| Speakers 10 · 11 · 12 · 13 | [[will-bone]] | Interim Head of Product | [[tech-tooling]] |
| Speaker 7 | _unidentified_ | — | — |
| Speaker 15 | _unidentified_ | — | — |

Identified-by-reference (mentioned but not attending), resolved in Pass B:
- **Scott** → [[scott-gruber]], Architect, [[architecture-team]] — required SME for broker workshop
- **Anton** → [[antonie-labuschagne]], Tech Lead, [[devx-team]] — _not_ InRisk PO; on DevX side
- **Hugh** → [[hugh-lobban]], Head of Data Quality, [[data-quality-team]] — business sponsor / owner of [[dataops-team]]
- **Paul Rogers** → [[paul-rogers]], Data & Process Analyst, [[analytics-team]] — point-of-contact for client-ID flattening decision
- **Andrea** → [[andrea-read]], Head of Technology Engineering, [[tech-tooling]] — _(corrected: Pass A incorrectly placed her on InRisk Engine side)_
- **John** → [[john-trahearn]], Tech Lead, [[prebind-team]] — _(corrected: Pass A incorrectly placed him on DevX)_
- **Chris** → [[kris-mokrzycki]], Tech Lead, [[prebind-team]] — correct spelling is **Kris**; _(corrected: Pass A incorrectly placed him on DevX)_
- **Simon Hulbert** → [[simon-hulbert]], Architect, [[high-volume-team]] — main Party-integration contact for HV

Still unresolved:
- **Sam** — in-session reference to someone associated with the HV go-live; identity and team unknown.
- **Bob** — referenced in-session in the InRisk / InRisk-Engine neighbourhood; identity unknown.
- **Josie** — referenced with respect to a company-matching tool; identity unknown.

## Key claims

1. **Strangle-the-graph is the chosen migration.** Rather than dual-running Graph DB and MDM, MDM will emit **proxy events** into the DU in the current graph event shape; consumers see no change on cutover; Graph DB retires once stable. Eventually replaced by a direct stream from MDM's Dynamo to the DU.
2. **PCT + MDM ship as a unit.** Not shipping the new PCT would require maintaining a separate path that feeds manual curation changes into MDM via DynamoDB; considered too messy to operate alongside a moving Party contract. Dropping PCT is effectively not an option.
3. **InRisk interim dependencies (3).** For Phase 1: (a) store Party ID in a new InRisk table _(already agreed in prior call)_, (b) include Party ID in InRisk's outgoing messaging, (c) amend broker retrieval to come **directly from InRisk** rather than be passed via our URD. Broker change needs a dedicated workshop including Scott.
4. **Versioned party API surface** — MDM supports: (a) `get by ID + version` (pinned snapshot), (b) `get by ID + timestamp` (as-of), (c) `get by ID` (latest). Deterministic per what the consumer asks. The *"do we push a version-change event when a party changes?"* question is a business/PO call, not a technical one — each consumer decides what they need.
5. **Backend shift.** Current: Neo4j + Cypher (Graph DB). Target: DynamoDB (party records, D&B cache, views) + OpenSearch (discoverability, fuzzy matching). Feature-tagging stays on the current feature-tech side for now and only migrates once InRisk buys in — flagged as an end-state item, not Phase 1.
6. **Timeline.** Alex's gut-feel for Graph Team's work: ~2 sprints for data model + proxy + adapter with a focused subset of the squad; ~2–3 months (≈6 sprints) overall including InRisk dev work, testing, and change management. **High Volume go-live: 1 September** — in-session speculation around a 1 January production variant is deprecated (see Contradicts).
7. **Backend sequencing.** Alex's three tech workstreams for the Graph Team: (i) **intercept** events from the current system to keep MDM updated during the transition, (ii) **100% backfill** from Graph into MDM, (iii) build the **proxy adapter** / projections into DU-compatible event shape.
8. **InRisk Engine framing clarified.** InRisk Engine is designed against the **final state** of the Party Application, not the interim. It will go live when that final state is realised. This programme's InRisk work is explicitly for the interim — no conflation.
9. **Client ID / submission / requirement shape (see Open questions).** Current InRisk cascades all submissions on a requirement onto the latest client ID when a renewal happens, losing the "what party was this submission actually tied to" linkage in-band. Phase 1 proposal: flatten `(party-id, submission-id)` rows in MDM; if the DU objects to the flatter shape, Graph Team build a small API on InRisk's side to reconstruct the nested shape for proxy event emission. Long-term, client ID disappears entirely in favour of party ID + version on submission.
10. **D&B (Dun & Bradstreet) pattern simplified.** New flow: if a party is found in D&B, create the draft party and its ultimate-parent entry automatically (no double curation ticket), with full provenance; cache the D&B lookup in Dynamo with a TTL.
11. **S&P ingestion stays manual** through the DU for Phase 1. Revisit in a later phase whether Party becomes source-of-truth for S&P via CDC/Snowpipe.
12. **One Re groupings and CUO groupings** remain manual in the new PCT for Phase 1; rules-based auto-grouping is a Phase 2 aspiration.
13. **Sanctions flow short-circuit** — currently round-trips via Boomi → NTT → back. A simpler future shape: NTT result lands on the party directly; Party then informs InRisk about all affected submissions. Flagged as an enabled-future, out of Phase 1 scope.
14. **IDs.** System ID will be a **UUID** (MDM-internal). A **display ID** field will hold the legacy graph human-readable ID during and after migration, so users still see the numbers they're used to.

## Decisions recorded

### Promoted to formal decision pages (Pass A)
- [[strangle-the-graph-via-proxy-events]] — **accepted**. MDM emits proxy events mimicking current graph event shape; no dual-running; Graph DB retired after stabilisation.
- [[pct-and-mdm-go-live-together]] — **accepted**. New PCT cannot be decoupled from MDM delivery; they ship as a unit.

### Promoted to formal decision pages (Pass B)
- [[d-and-b-caching-and-auto-parent]] — **accepted**. D&B responses cached in Dynamo with TTL; on match, draft party + ultimate-parent auto-created with full provenance. Supersedes the current double-ticket curation pattern.
- [[uuid-system-id-with-display-id]] — **accepted**. System ID is a UUID; display ID field carries the legacy Graph party ID. Users continue to see familiar numbers; system routes on opaque UUIDs.

### Still captured inline (candidates for later promotion)
- **Flatten client ID + submission rows in MDM** — proposed, pending a DU schema-impact check with [[paul-rogers]]. Fallback: reconstruct the nested shape from an InRisk API during proxy event emission. _(Owner: [[joe-worsfold]].)_
- **S&P ingestion remains manual for Phase 1** — accepted; revisit when Party becomes plausible source-of-truth.
- **One Re / CUO manual curation in new PCT for Phase 1** — accepted; rules-based auto-grouping deferred.
- **Widget Chakra V2 vs V3 strategy** — deferred; decision pending InRisk workshop. Options on the table: ship a V2 widget, ship a "Teddy Facts" minimal-render version, or ship V3 only once InRisk upgrades. HV is not a blocker (API via Boomi).
- **Party on submission vs requirement** — deferred; explicitly a **[[prebind-team]] architectural choice** about their Requirement → Submission data flow spine, not a Party-side decision.

## Actions

| Action | Owner | Application | State | Notes |
|---|---|---|---|---|
| Align with [[high-volume-team]] (main contact [[simon-hulbert]]) on Party integration shape and any timeline risks to the 1 Sep deadline | [[rory-beattie]] | [[high-volume]] | open | Replaces the earlier "confirm 1 Jan vs 1 Sep" action — 1 Sep stands |
| Dedicated workshop with InRisk on broker retrieval change; include Scott | [[rory-beattie]] | [[inrisk]] | open | Proposed for Mon/Tue after Romania trip |
| Schedule follow-up InRisk architecture session | [[rory-beattie]] | [[inrisk]] | open | Target: week after Romania (Mon/Tue) |
| Talk to DU team re: impact of flattening client-ID / submission nesting | [[joe-worsfold]] | [[party-application]] | open | If DU don't index the nested field, we can flatten freely |
| Check with Paul Rogers re: InRisk client-ID flattening / need for API | [[joe-worsfold]] | [[inrisk]] | open | Determines fallback path if DU push back |
| Align with "enriched team" on D&B caching approach | [[joe-worsfold]] | [[party-application]] | open | — |
| Anti-patterns / data-fixes workshop with InRisk (both-sides impact) | [[joe-worsfold]] | [[inrisk]] | open | — |
| Create + schedule tickets: data-model revisions, intercept, backfill, projections, proxy adapter | [[alex-sillars]] | [[party-application]] | open | Into near-term sprints |
| Involve testers early; build go-live checklist | [[alex-sillars]] | [[party-application]] | open | — |
| Scope / effort review once first tickets exist | [[joe-worsfold]] | [[party-application]] | open | Follow-up session after ticketisation |
| InRisk widget / SDK / Chakra discussion (V2 vs V3 render strategy) | [[rory-beattie]] + [[alex-sillars]] | [[party-curation-tool]] · [[inrisk]] | open | Not to be solved in-team; needs InRisk in the room |
| Confirm broker retrieval moves InRisk-direct (removes URD path) | [[rory-beattie]] | [[inrisk]] | open | Part of the broker workshop |

Open action count: **12**. These are the input to the next [[party-rearch-ownership-matrix]] refresh.

## Applications mentioned
- [[party-application]] — re-architecture subject; most of the session.
- [[party-curation-tool]] — mandatory co-delivery with MDM.
- [[inrisk]] — interim-state modifications required.
- [[inrisk-engine]] — explicitly out of scope for the interim; targets the final state.
- [[high-volume]] — go-live date correction; consumes via Boomi, not the widget.

## Entities mentioned
- Teams: [[graph-team]], [[tech-tooling]], [[architecture-team]], [[prebind-team]], [[devx-team]], [[dataops-team]], [[data-quality-team]], [[analytics-team]], [[high-volume-team]].
- People (attendees): [[rory-beattie]], [[sergiu-postolachi]], [[alex-sillars]], [[joe-worsfold]], [[ben-joseph]], [[billy-calladine]], [[suzanna-whitefield]], [[will-bone]].
- People (mentioned, wiki pages created in Pass B): [[scott-gruber]], [[antonie-labuschagne]], [[hugh-lobban]], [[paul-rogers]], [[andrea-read]], [[john-trahearn]], [[kris-mokrzycki]], [[simon-hulbert]].
- People (mentioned, identity pending): Sam, Bob, Josie.
- External / platforms: Boomi (integration broker, HV path), NTT (sanctions provider), Dun & Bradstreet, S&P, Snowflake (the Data Universe platform, owned by [[analytics-team]]), Chakra UI (V2/V3), Tailwind, Neo4j, DynamoDB, OpenSearch, Jira.

## Concepts mentioned
_Not yet promoted to standalone concept pages in this pass — candidates for Pass B: proxy events, graph-strangle migration, party versioning (ID + version / timestamp), intercept-backfill-projection, client-ID flattening, One Re grouping, CUO grouping, D&B enrichment, S&P ingestion, feature-tagging, display-ID vs system-ID._

## Contradicts

> ⚠ **HV go-live date — speculation deprecated** — in-session, [[will-bone]] raised the possibility that 1 Sep was training and real production was "1st of January or whatever" (line 2993 of the raw transcript). Pass B has confirmed that **1 September remains the deadline**; the training/production split is not a real programme constraint. All pages reflect 1 Sep. Retained here as the original claim for auditability; do not propagate the 1 Jan framing into later work.

> ⚠ **InRisk Engine ↔ target Party model framing** — prior [[inrisk-engine]] framing described the target model as *"co-defined"* with DevX. This session clarified the dynamic more precisely: the Party re-architecture defines the final state; [[inrisk-engine]] will proceed **on the assumption of that final state**, and goes live when that final state is realised. Updated on [[inrisk-engine]] accordingly.

## Reinforces
_First real source — nothing to reinforce yet. Future ingests should reference this page when they corroborate or extend the claims above._

## My notes (LLM)
- The transcript is auto-generated; there are long filler stretches (line 1–175 is pre-meeting small talk; lines 3300–3500 contain multilingual noise from the transcription engine). All claims above are drawn from speaker-attributed content in the substantive sections.
- **Speaker identification (post-Pass B)**: Speakers 2 · 4 · 5 · 6 · 9 · 10 · 11 · 12 · 13 · 14 are all resolved (see Attendees table). Speakers 7 and 15 remain unidentified; contribution-weighted they are not load-bearing for any of the key claims.
- The meeting's energy in the final quarter is notably higher-confidence than the middle, where the client-ID/submission/requirement question caused visible strain. That strain is the signal: that topic is the single hardest unresolved sub-problem and should remain on the watchlist.
- **Pass B corrections to earlier attributions**: Sergiu → [[graph-team]] (Scrum Master), not Tech Tooling; Andrea → [[tech-tooling]] (Head of Technology Engineering), not DevX; John & Kris → [[prebind-team]] Tech Leads, not DevX; Anton → [[devx-team]] Tech Lead, not InRisk PO. Originals preserved in the file history; all applied corrections have an explicit "Correction note (Pass B)" on the affected pages.

## Open questions

1. ~~**HV production date** — 1 Jan confirmed?~~ **Resolved in Pass B**: 1 September stands; no training/prod split.
2. ~~**HV owning team** — still unknown.~~ **Resolved in Pass B**: [[high-volume-team]], main contact [[simon-hulbert]].
3. **Analytics Team's tolerance for flattening submission nesting** — do they index the nested field or ignore it? (Owner: [[joe-worsfold]] ↔ [[paul-rogers]].)
4. **InRisk behaviour on party merges** — when a draft party gets merged into an existing one post-curation, how does InRisk reconcile the placeholder ID? Does it use the `by ID` lookup to follow the merge, or does it need an explicit merge event?
5. **Party on requirement vs submission** — a **[[prebind-team]] architectural choice**. Requirement and Submission are two stages on the spine of their main data flow structure; moving Party from Requirement-level to Submission-level is a domain decision PreBind will make in their own time. Flagged for them, not owned by us.
6. **Version-change events** — do any consumers actually want them, or does "pull latest on render" cover everyone? PO-level question per consumer.
7. **Chakra V2/V3 strategy** — pending InRisk workshop.
8. **Feature-tagging migration** — currently out of Phase 1; when does it move to MDM, and who owns that call?
9. **Snowpipe from Dynamo to Data Universe** — feasibility assessment for replacing proxy events in end state.
10. **D&B cascade depth** — only ultimate parent today; any appetite for multi-level parent chains? (Meeting: no.)
11. **Remaining unidentified attendees** — Speakers 7 and 15.
12. **Remaining unidentified referenced people** — Sam (HV-side), Bob (InRisk neighbourhood), Josie (company-matching tool).

## Related
- [[party-rearch]] · [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]
- [[strangle-the-graph-via-proxy-events]] · [[pct-and-mdm-go-live-together]] · [[d-and-b-caching-and-auto-parent]] · [[uuid-system-id-with-display-id]]
- [[party-application]] · [[party-curation-tool]] · [[inrisk]] · [[inrisk-engine]] · [[high-volume]]
