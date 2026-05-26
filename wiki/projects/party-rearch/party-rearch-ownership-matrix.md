---
type: analysis
title: Party Re-Architecture — Ownership Matrix
created: 2026-04-22
updated: 2026-05-26
tags: [analysis, standing, ownership, actions]
project: party-rearch
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement, 20260519-party-integration-timelines]
source_count: 5
status: draft
---

# Party Re-Architecture — Ownership Matrix

A standing analysis of workstreams, owners, and open actions for the Party Application re-architecture programme. Refreshed on every ingest. Open-action count is the headline metric.

## Workstreams × Owners

| Workstream | Primary owner | Supporting | Notes |
|---|---|---|---|
| **MDM core build** (data model, intercept, 100% backfill, projections, proxy adapter) | [[graph-team]] | [[tech-tooling]] (oversight) | ~2 sprints with focused squad |
| **New PCT build** (Next.js on open-API spec) | [[graph-team]] | [[dataops-team]] (user-side), [[data-quality-team]] (sponsor via [[hugh-lobban]]) | Ships with MDM — [[pct-and-mdm-go-live-together]] |
| **Proxy-event emission to Data Universe** | [[graph-team]] | [[analytics-team]] (consumer) | [[strangle-the-graph-via-proxy-events]] |
| **InRisk interim changes** (Party MDM Integration epic: 5 stories — tech-debt clearance, data-model addition, client + broker + tagging widget integration) | [[prebind-team]] ([[john-trahearn]], [[kris-mokrzycki]], [[daria-romanovskaia]] PO — took over from [[andrew-turner]] 2026-05-29) | [[graph-team]] ([[joe-worsfold]], [[billy-calladine]] for widget Q&A), [[tech-tooling]] ([[rory-beattie]]), [[architecture-team]] ([[scott-gruber]] on broker SME) | **Ordered + gated 2026-05-14**: Stories 1 (manual-client-creation cleanup) & 2 (data-model: 3 tables — party + broker + party_snapshot) ready for low level 2026-05-19; Stories 3/4/5 gated on widget-integration spike (Joe + Billy + Alex + [[daria-romanovskaia]] — inherited from [[andrew-turner]]). Drop-in-replacement / parity-not-enhancement widget posture. Cutover lands ≥ 2 weeks before HV per [[inrisk-cuts-over-before-high-volume]] |
| **Analytics Team schema-impact check** | [[analytics-team]] ([[paul-rogers]]) | [[graph-team]] ([[joe-worsfold]]) | Gating for flatten-vs-reconstruct decision |
| **D&B caching + auto-parent flow** | [[graph-team]] | "enriched team" (name TBD) | [[d-and-b-caching-and-auto-parent]] |
| **S&P ingestion (Phase 1)** | [[analytics-team]] → [[graph-team]] | — | Stays manual; no change in Phase 1 |
| **Widget / SDK / Chakra strategy** | [[graph-team]] ([[joe-worsfold]]) | [[prebind-team]] (consumer) | **Resolved 2026-05-13** ([[inrisk-cuts-over-before-high-volume]]): two component libraries — Chakra-3 + design-system for PCT, design-system-agnostic for InRisk; HV API-only. Closes [[open-questions#OQ-005]]. Joe's open action: publish the second library |
| **Sanctions-domain ownership / off-Boomi migration** | _unassigned_ | [[rory-beattie]] · [[suzanna-whitefield]] to escalate to [[andrea-read]] | Surfaced 2026-05-13. Out of Phase 1; audit pressure this year. See [[sanctions-processing]] · [[ntt]] · [[open-questions#OQ-032]] |
| **HV Party integration (via [[boomi]])** | [[high-volume-team]] ([[simon-hulbert]]) | [[graph-team]] (API contract), [[tech-tooling]] ([[rory-beattie]]) | API-only, no widget; forcing function for 1 Sep. **Reframed 2026-05-19**: HV ≡ Convex's implementation of [[artificial]] — so this workstream covers the HV-side delivery and the Artificial-side onboarding together |
| **[[artificial]] vendor onboarding (Party-consumer)** | [[high-volume-team]] ([[simon-hulbert]]) + Artificial-side PMs (unnamed) | [[graph-team]] ([[joe-worsfold]] for API spec + Boomi pairing), [[tech-tooling]] ([[rory-beattie]]) | New 2026-05-19. Two-stage: (a) read path priority — kick-off 2026-05-20, YAML spec already shared; (b) change-event push back later. Fortnightly Convex × Artificial cadence + dedicated Slack channel + Boomi connectivity setup are the operational workstreams that live underneath this |
| **[[boomi]] connectivity for Artificial** | [[high-volume-team]] ([[srini]]) | [[graph-team]] ([[joe-worsfold]]) | New 2026-05-19. **Unblocking dependency** for Artificial-side dev work; until credentials + security tokens are issued, the dev-env DynamoDB sits idle from Artificial's perspective. Same broker pattern as [[sanctions-processing]] today |
| **InRisk Engine (final-state targeting)** | [[devx-team]] ([[antonie-labuschagne]]) | [[graph-team]] (final-state contract provider) | Not interim; out of Phase 1 |
| **Anti-patterns / data-fixes workshop** | [[joe-worsfold]] | [[prebind-team]] | Addresses client-ID cascade implications |
| **Go-live checklist + test engagement** | [[alex-sillars]] | QA / testers (TBD) | Start now, not late |
| **DataOps business-case framing for PCT rollout** | [[will-bone]] | [[hugh-lobban]] ([[data-quality-team]]) | Sponsor-level messaging |
| **Bulk-migration CLI (Phase 1)** | [[graph-team]] | [[dataops-team]] (user) | [[bulk-migrations-owned-by-mdm-phase-1]]; Phase 2+ self-serve UX: [[open-questions#OQ-006]] |
| **D&B scheduled-refresh cadence + revision-on-change** | [[graph-team]] ([[joe-worsfold]]) | "enriched team" ([[open-questions#OQ-012]]) | Session 1 addition to [[d-and-b-caching-and-auto-parent]]; cadence: [[open-questions#OQ-011]] |
| **Graph API consumer audit (spike)** | [[graph-team]] ([[joe-worsfold]], [[billy-calladine]]) | — | [[open-questions#OQ-004]]; feeds [[party-rearch-dependency-map]] and answers [[open-questions#OQ-001]] |
| **PCT functional-requirements walkthrough** | [[graph-team]] ([[joe-worsfold]]) + [[sergiu-postolachi]] | — | Session 1: confirms no functional gap vs new PCT; walks against [[tomas-sivo]]'s authoring workstream |
| **PCT functional-requirements authoring** | [[tomas-sivo]] ([[graph-team]]) | — | Resolved Pass B |
| **Feature-tagging — Phase 2+ migration to [[inrisk]]** | [[prebind-team]] (Phase 2+) · [[graph-team]] (current host) | Michael Hay ([[open-questions#OQ-027]]) | [[feature-tagging-moves-to-inrisk]]; timing: [[open-questions#OQ-019]] |
| **MDM-delivery squad shape** | [[rory-beattie]] (primary) | [[alex-sillars]] (consulted) | [[open-questions#OQ-017]] — Rory's call; merits further discussion |
| **Eclipse-ingestion retirement confirmation** | [[alex-sillars]] | [[party-application]] | [[open-questions#OQ-001]]; expected to resolve via spike [[open-questions#OQ-004]] |

## Open actions

Total open: **35** (12 from Session 2 + 9 from Session 1 − 1 resolved in Pass B + 8 new from 2026-05-13 follow-up − 1 resolved by 2026-05-13 follow-up + 3 new from 2026-05-14 InRisk HL − 1 resolved by 2026-05-14 — see action #22 — + 6 new from 2026-05-19 Party integration timelines).

_This section tracks actions ("things to do"). Open **questions** ("things to find out") live in [[open-questions]] and are referenced from individual action rows where relevant._

| # | Action | Owner | Application(s) | State |
|---:|---|---|---|---|
| 1 | Align with [[high-volume-team]] (main contact [[simon-hulbert]]) on Party integration shape and any timeline risks to 1 Sep | [[rory-beattie]] | [[high-volume]] | open — 1 Sep is fixed |
| 2 | Dedicated workshop with InRisk on broker retrieval change; include [[scott-gruber]] | [[rory-beattie]] | [[inrisk]] | open — target Mon/Tue after Romania |
| 3 | Schedule follow-up InRisk architecture session | [[rory-beattie]] | [[inrisk]] | open |
| 4 | Check with [[paul-rogers]] on Data Universe schema impact of flattening client-ID / submission nesting | [[joe-worsfold]] | [[party-application]] | open — gating decision |
| 5 | Check with [[paul-rogers]] re: InRisk client-ID flattening / need for nesting-reconstruction API | [[joe-worsfold]] | [[inrisk]] | open |
| 6 | Align with enriched team on D&B caching approach (see [[d-and-b-caching-and-auto-parent]]) | [[joe-worsfold]] | [[party-application]] | open |
| 7 | Anti-patterns / data-fixes workshop with InRisk (both-sides impact) | [[joe-worsfold]] | [[inrisk]] · [[party-application]] | open |
| 8 | Create + schedule tickets (data model, intercept, backfill, projections, proxy adapter) | [[alex-sillars]] | [[party-application]] | open |
| 9 | Involve testers early; build go-live checklist | [[alex-sillars]] | [[party-application]] | open |
| 10 | Scope / effort review once first tickets exist | [[joe-worsfold]] | [[party-application]] | open |
| 11 | InRisk widget / SDK / Chakra V2-vs-V3 discussion | [[rory-beattie]] + [[alex-sillars]] | [[party-curation-tool]] · [[inrisk]] | **resolved 2026-05-13** — see [[inrisk-cuts-over-before-high-volume]]; closes [[open-questions#OQ-005]] |
| 12 | Confirm broker retrieval moves InRisk-direct (removes URD path) | [[rory-beattie]] | [[inrisk]] | open — part of broker workshop |
| 13 | Spike: audit Graph API usage — which endpoints are hit, by which consumers, at what volume | [[joe-worsfold]] · [[billy-calladine]] | [[party-application]] | open — Session 1 |
| 14 | Walk current PCT end-to-end with [[sergiu-postolachi]] to confirm no functional gap in new PCT | [[joe-worsfold]] | [[party-curation-tool]] | open — Session 1 |
| 15 | Decide MDM-delivery squad shape (focused 3 vs. whole ~13-person squad) | [[rory-beattie]] | [[party-application]] | open — Session 1; [[open-questions#OQ-017]]; blocks sprint-planning |
| 16 | Ticketise: backfill, intercept, adapter, new PCT, coupled rollout flag | [[alex-sillars]] · [[joe-worsfold]] | [[party-application]] | open — Session 1 (precursor to Action 8's "tickets created") |
| 17 | Define D&B scheduled-refresh cadence (monthly / quarterly) | [[joe-worsfold]] | [[party-application]] | open — Session 1; [[open-questions#OQ-011]] |
| 18 | Graph-API consumer audit spike — resolves Eclipse retirement question | [[joe-worsfold]] · [[billy-calladine]] | [[party-application]] | open — Session 1; [[open-questions#OQ-001]] · [[open-questions#OQ-004]] |
| 19 | Build MDM-owned bulk-migration CLI (CSV → preview → approved revision) | [[joe-worsfold]] | [[party-application]] | open — Session 1; scoped under [[bulk-migrations-owned-by-mdm-phase-1]] |
| 20 | Identify remaining Session 1 PCT-neighbourhood people (Michael Hay) | [[rory-beattie]] | [[party-curation-tool]] | open — [[open-questions#OQ-027]] |
| 21 | Bring InRisk Phase-1 epic ("Party MDM Integration") + 4–5 stories through Thursday high-level (2026-05-14) and Tuesday low-level (2026-05-19); sprint starts Wed 2026-05-20 | [[john-trahearn]] | [[inrisk]] | **partial 2026-05-14** — Thursday HL done; Stories 1 & 2 go to low-level on Tuesday; Stories 3/4/5 wait on widget-integration spike |
| 22 | Brush up on InRisk's current widget integration mechanic (query-param flow vs SDK-style new flow) before Thursday's high-level | [[john-trahearn]] | [[inrisk]] | **resolved 2026-05-14** — current flow confirmed query-parameter-driven; new flow SDK-style; gap will be closed via the spike |
| 23 | Attend Thursday's InRisk Phase-1 high-level to validate widget commonality and integration mechanic | [[billy-calladine]] | [[inrisk]] · [[party-application]] | **partial 2026-05-14** — Billy attended, confirmed manual-create widget path already supported in renewals flow; spike pairing role open |
| 24 | Publish a second, design-system-agnostic component library for [[inrisk]] to consume | [[joe-worsfold]] | [[party-application]] | open — 2026-05-13; feeds [[inrisk-cuts-over-before-high-volume]] |
| 25 | Investigate whether feature tagging can be treated as a fully static list (no dynamic management), potentially simplifying its later migration into InRisk classifications | [[suzanna-whitefield]] | [[inrisk]] | open — 2026-05-13; feeds [[open-questions#OQ-019]] |
| 26 | Escalate sanctions / Boomi orchestration ownership to [[andrea-read]] | [[rory-beattie]] · [[suzanna-whitefield]] | [[ntt]] · [[party-application]] · [[inrisk]] | open — 2026-05-13; [[open-questions#OQ-032]] |
| 27 | Speak to Anna ahead of cutover to warm users to the UX change | [[rory-beattie]] | [[party-curation-tool]] · [[inrisk]] | open — 2026-05-13; Anna identity: [[open-questions#OQ-033]] |
| 28 | Confirm widget-response shape covers all fields InRisk needs once integration starts | [[joe-worsfold]] | [[party-application]] · [[inrisk]] | open — 2026-05-13; [[open-questions#OQ-036]] |
| 29 | Run **widget-integration spike** (pair Joe + Billy + Alex + [[daria-romanovskaia]] — slot inherited from [[andrew-turner]] on his 2026-05-29 departure) to bottom out SDK posture, OpenSearch/Dynamo response shape, integration mechanic before Stories 3/4/5 go to low level. Confirmation of spike scheduling at the ad-hoc HL on 2026-05-15 | [[joe-worsfold]] · [[billy-calladine]] · [[alex-sillars]] · [[daria-romanovskaia]] | [[inrisk]] · [[party-application]] | open — 2026-05-14 (spike pairing redirected 2026-05-26 per departure of [[andrew-turner]]); gates Stories 3/4/5; [[open-questions#OQ-036]] is the principal output |
| 30 | Decide **InRisk-side backfill** policy for new ID columns on existing client / broker / party-snapshot rows (backfill with known MDM party-IDs vs null + go-forward); sanctions is the principal impact surface. Decision needed before Story 2 low-level on 2026-05-19 | [[joe-worsfold]] · [[john-trahearn]] | [[inrisk]] · [[party-application]] | open — 2026-05-14; not formalised as an OQ |
| 31 | Confirm bespoke InRisk widget on the design-system-agnostic library preserves all current widget filter parameters at parity — particularly **TOBA status** (Lloyd's-vs-retail broker workflow) | [[joe-worsfold]] | [[party-application]] · [[inrisk]] | open — 2026-05-14; surfaced by [[jason-owen]]; feeds [[inrisk-cuts-over-before-high-volume]] parity-not-enhancement refinement |
| 32 | Run the Convex × [[artificial]] kick-off call 2026-05-20; walk through API spec + sequencing (InRisk-first per [[inrisk-cuts-over-before-high-volume]]) + dependency on Boomi connectivity | [[rory-beattie]] · [[alex-sillars]] · [[simon-hulbert]] + Boardroom-PM | [[artificial]] · [[party-application]] · [[high-volume]] | open — 2026-05-19 |
| 33 | Stand up [[boomi]] connectivity between [[artificial]] and [[party-application]]: pair [[srini]] (HV-side Boomi Integration Architect) with [[joe-worsfold]] (Party-side); issue Artificial-side credentials + security tokens | [[srini]] · [[joe-worsfold]] | [[boomi]] · [[party-application]] · [[artificial]] | open — 2026-05-19; **unblocks Artificial-side dev work** — dev-env DynamoDB is already stood up with fake data per the YAML spec |
| 34 | Set up fortnightly Convex × Artificial integration catch-up ([[alex-sillars]] + [[rory-beattie]] + [[simon-hulbert]] + [[srini]] + Artificial reps); cadence may tighten as 1 Sep approaches | [[luca]] (or Boardroom-PM) | [[artificial]] · [[party-application]] · [[high-volume]] | open — 2026-05-19 |
| 35 | Set up shared Slack channel for Artificial integration coordination with per-application prefix convention | Boardroom-PM | [[artificial]] · [[high-volume]] · [[party-application]] | open — 2026-05-19 |
| 36 | [[simon-hulbert]] to send [[rory-beattie]] the top-level HV/Artificial integration architecture diagram + the sequence diagrams that detail individual Party touchpoints | [[simon-hulbert]] | [[high-volume]] · [[artificial]] · [[party-application]] | open — 2026-05-19; **raw-folder candidate** when received |
| 37 | Build out the remaining MDM front-of-house pieces in dev: extend curation features (party + party-snapshot data-model coupling); migrate Neo4j ParseDB → DynamoDB; emit DU + sanctions events including submission-ID fan-out for fields not stored in Party | [[graph-team]] ([[alex-sillars]] · [[joe-worsfold]]) | [[party-application]] | open — restated 2026-05-19; tracked also on [[party-rearch-phase-1]] |

## Ownership gaps

All identity gaps are tracked centrally in [[open-questions]]. Summary of currently-unidentified people touching this matrix:
- _(resolved — PreBind PO is [[daria-romanovskaia]]; [[andrew-turner]] departs 2026-05-29 — [[daria-romanovskaia]] becomes sole PO; the dual-PO state was transitional)_
- _(resolved — "Andrew Tennant" is [[andrew-turner]]; transcription drift. Forward-looking references redirect to [[daria-romanovskaia]] per [[AGENTS|§5.5]].)_
- _(resolved 2026-05-26 — Amardeep Badhan promoted to entity → [[amardeep-badhan]], Scrum Master on [[prebind-team]])_
- "Enriched team" identity — [[open-questions#OQ-012]]
- QA / test lead — no open-question ID yet (low-urgency; revisit when go-live checklist begins)
- Michael Hay — [[open-questions#OQ-027]]
- Session 2 first-name-only: **Sam** (advanced 2026-05-19: confirmed _"FDA for integrations"_ on the Artificial project — full name + line manager still TBD) — [[open-questions#OQ-024]]; FDA acronym TBD at [[open-questions#OQ-039]]. Bob ([[open-questions#OQ-025]]), Josie ([[open-questions#OQ-026]])
- Session 1 first-name-only: Kartik ([[open-questions#OQ-028]]), Jenny/Ginny ([[open-questions#OQ-029]])
- Session 2 speakers: Speakers 7 and 15 ([[open-questions#OQ-023]])
- 2026-05-13 follow-up first-name-only: Anna ([[open-questions#OQ-033]]), Marty ([[open-questions#OQ-034]])
- 2026-05-19 first-name-only: **Convex – Orega – 6B Boardroom speaker** ([[open-questions#OQ-037]] — HV-side PM, possibly [[luca]] or a colleague); [[luca]] surname + formal role ([[open-questions#OQ-038]]); [[srini]] surname ([[open-questions#OQ-040]])

## Resolved

_Historical resolutions; current resolutions now live in [[open-questions#Resolved]] with IDs._

- ~~HV owning team~~ → [[high-volume-team]] with main contact [[simon-hulbert]]. ([[open-questions#Resolved|OQ-R08]])
- ~~Sergiu's team affiliation~~ → [[graph-team]] Scrum Master. ([[open-questions#Resolved|OQ-R02]])
- ~~Paul Rogers's team~~ → [[analytics-team]]. ([[open-questions#Resolved|OQ-R03]])
- ~~Anton's role~~ → [[antonie-labuschagne]], Tech Lead, [[devx-team]]. ([[open-questions#Resolved|OQ-R04]])
- ~~Hugh's role~~ → [[hugh-lobban]], Head of Data Quality, [[data-quality-team]]. ([[open-questions#Resolved|OQ-R05]])
- ~~Session 1 Speakers 6 and 7~~ → [[billy-calladine]] and [[joe-worsfold]] (transcription engine assigned each a second ID). ([[open-questions#Resolved|OQ-R10]], [[open-questions#Resolved|OQ-R11]])
- ~~Tom Ash / Tomas~~ → [[tomas-sivo]]. ([[open-questions#Resolved|OQ-R12]])
- ~~Darius~~ → not a person (transcription of "InRisk"). ([[open-questions#Resolved|OQ-R13]])
- ~~Boyle~~ → not a person (transcription of "Will"). ([[open-questions#Resolved|OQ-R14]])

## Source history
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-2]] — Pass A populated; Pass B refreshed with resolved owners and corrected attributions
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-1]] Pass A — added 9 new actions (spike, walkthrough, squad shape, ticketisation, D&B cadence, Eclipse retirement, bulk-CLI, people identification, speaker ID); added 6 new workstreams
- 2026-04-22 — [[sources/20260422-meeting-transcript-session-1]] Pass B — collapsed people-identification actions into [[open-questions]] (OQ-017, OQ-023–031); dropped the "identify Speakers 6/7" action (resolved); dropped the "PCT neighbourhood people" combined action in favour of per-person OQ rows; promoted three candidate ADRs ([[no-pct-audit-backfill]], [[feature-tagging-moves-to-inrisk]], [[bulk-migrations-owned-by-mdm-phase-1]]) — relevant workstream rows updated to cite them.
- 2026-05-18 — [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — InRisk workstream re-scoped from "3 interim changes" framing to concrete "Party MDM Integration epic" with 4–5 stories; added a new **Sanctions-domain ownership / off-Boomi migration** workstream (out of Phase 1, escalation owner Rory + Suzy → Andrea); marked the **Widget / SDK / Chakra strategy** workstream resolved and the Chakra-V2-vs-V3 action (#11) resolved by [[inrisk-cuts-over-before-high-volume]]; added 8 new actions (#21–28) covering Thursday/Tuesday cadence, widget-mechanic brush-up, Billy to validate, Joe's second library, Suzy's static-list investigation, sanctions escalation, Anna warming, widget-response field alignment.
- 2026-05-26 — [[sources/20260514-inrisk-high-level-refinement]] — InRisk workstream description updated to reflect 5-story ordering (Stories 1 & 2 ready; 3/4/5 gated) + drop-in-replacement / parity-not-enhancement posture + 3-table data-model. **Resolved** action #22 (current widget mechanic now confirmed query-parameter-driven). **Partial** actions #21 + #23 (Thursday HL done; Tuesday low-level scope reduced to Stories 1 & 2). **Added 3 new actions** (#29 widget-integration spike pairing; #30 InRisk-side backfill policy; #31 TOBA-filter parity confirmation). Ownership-gaps refreshed: Andrew Tennant resolved → [[andrew-turner]]; added Anna / Marty / Amardeep notes.
- 2026-05-26 — user-declared handover (no source page) — [[andrew-turner]] departing 2026-05-29; [[daria-romanovskaia]] takes over as sole PreBind PO (handover effective 2026-05-26). InRisk workstream Primary-owner cell and action #29 spike pairing redirected from [[andrew-turner]] → [[daria-romanovskaia]]; ownership-gaps refreshed; departure convention captured in [[AGENTS|§5.5]].
- 2026-05-26 — [[sources/20260519-party-integration-timelines]] — **HV Party integration** workstream reframed (HV ≡ Convex's implementation of [[artificial]]); **two new workstreams added**: Artificial vendor onboarding (Party-consumer) and Boomi connectivity for Artificial. **Six new actions** added (#32–#37) covering 2026-05-20 kick-off, Boomi setup ([[srini]] ↔ [[joe-worsfold]]), fortnightly cadence, Slack channel, architecture diagrams, and the remaining MDM front-of-house build-out. Ownership-gaps section refreshed: Sam advanced (now confirmed FDA for integrations); new identification gaps for [[luca]], [[srini]] surname, and Boardroom-PM. Open-action count: 29 + 6 = **35**.
