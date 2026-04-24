---
type: analysis
title: Ownership Matrix
created: 2026-04-22
updated: 2026-04-22
tags: [analysis, standing, ownership, actions]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Ownership Matrix

A standing analysis of workstreams, owners, and open actions for the Party Application re-architecture programme. Refreshed on every ingest. Open-action count is the headline metric.

## Workstreams × Owners

| Workstream | Primary owner | Supporting | Notes |
|---|---|---|---|
| **MDM core build** (data model, intercept, 100% backfill, projections, proxy adapter) | [[graph-team]] | [[tech-tooling]] (oversight) | ~2 sprints with focused squad |
| **New PCT build** (Next.js on open-API spec) | [[graph-team]] | [[dataops-team]] (user-side), [[data-quality-team]] (sponsor via [[hugh-lobban]]) | Ships with MDM — [[pct-and-mdm-go-live-together]] |
| **Proxy-event emission to Data Universe** | [[graph-team]] | [[analytics-team]] (consumer) | [[strangle-the-graph-via-proxy-events]] |
| **InRisk interim changes** (party-ID storage, messaging, broker retrieval) | [[prebind-team]] ([[john-trahearn]], [[kris-mokrzycki]]) | [[graph-team]] ([[joe-worsfold]]), [[tech-tooling]] ([[rory-beattie]]), [[architecture-team]] ([[scott-gruber]] on broker SME) | Broker-retrieval change is the largest |
| **Analytics Team schema-impact check** | [[analytics-team]] ([[paul-rogers]]) | [[graph-team]] ([[joe-worsfold]]) | Gating for flatten-vs-reconstruct decision |
| **D&B caching + auto-parent flow** | [[graph-team]] | "enriched team" (name TBD) | [[d-and-b-caching-and-auto-parent]] |
| **S&P ingestion (Phase 1)** | [[analytics-team]] → [[graph-team]] | — | Stays manual; no change in Phase 1 |
| **Widget / SDK / Chakra strategy** | [[graph-team]] + [[tech-tooling]] | [[prebind-team]] | Deferred pending InRisk workshop; HV not a blocker |
| **HV Party integration (via Boomi)** | [[high-volume-team]] ([[simon-hulbert]]) | [[graph-team]] (API contract), [[tech-tooling]] ([[rory-beattie]]) | API-only, no widget; forcing function for 1 Sep |
| **InRisk Engine (final-state targeting)** | [[devx-team]] ([[antonie-labuschagne]]) | [[graph-team]] (final-state contract provider) | Not interim; out of Phase 1 |
| **Anti-patterns / data-fixes workshop** | [[joe-worsfold]] | [[prebind-team]] | Addresses client-ID cascade implications |
| **Go-live checklist + test engagement** | [[alex-sillars]] | QA / testers (TBD) | Start now, not late |
| **DataOps business-case framing for PCT rollout** | [[will-bone]] | [[hugh-lobban]] ([[data-quality-team]]) | Sponsor-level messaging |
| **Bulk-migration CLI (Phase 1)** | [[graph-team]] | [[dataops-team]] (user) | [[bulk-migrations-owned-by-mdm-phase-1]]; Phase 2+ self-serve UX: [[open-questions#OQ-006]] |
| **D&B scheduled-refresh cadence + revision-on-change** | [[graph-team]] ([[joe-worsfold]]) | "enriched team" ([[open-questions#OQ-012]]) | Session 1 addition to [[d-and-b-caching-and-auto-parent]]; cadence: [[open-questions#OQ-011]] |
| **Graph API consumer audit (spike)** | [[graph-team]] ([[joe-worsfold]], [[billy-calladine]]) | — | [[open-questions#OQ-004]]; feeds [[dependency-map]] and answers [[open-questions#OQ-001]] |
| **PCT functional-requirements walkthrough** | [[graph-team]] ([[joe-worsfold]]) + [[sergiu-postolachi]] | — | Session 1: confirms no functional gap vs new PCT; walks against [[tomas-sivo]]'s authoring workstream |
| **PCT functional-requirements authoring** | [[tomas-sivo]] ([[graph-team]]) | — | Resolved Pass B |
| **Feature-tagging — Phase 2+ migration to [[inrisk]]** | [[prebind-team]] (Phase 2+) · [[graph-team]] (current host) | Michael Hay ([[open-questions#OQ-027]]) | [[feature-tagging-moves-to-inrisk]]; timing: [[open-questions#OQ-019]] |
| **MDM-delivery squad shape** | [[rory-beattie]] (primary) | [[alex-sillars]] (consulted) | [[open-questions#OQ-017]] — Rory's call; merits further discussion |
| **Eclipse-ingestion retirement confirmation** | [[alex-sillars]] | [[party-application]] | [[open-questions#OQ-001]]; expected to resolve via spike [[open-questions#OQ-004]] |

## Open actions

Total open: **20** (12 from Session 2 + 9 from Session 1 − 1 resolved in Pass B).

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
| 11 | InRisk widget / SDK / Chakra V2-vs-V3 discussion | [[rory-beattie]] + [[alex-sillars]] | [[party-curation-tool]] · [[inrisk]] | open |
| 12 | Confirm broker retrieval moves InRisk-direct (removes URD path) | [[rory-beattie]] | [[inrisk]] | open — part of broker workshop |
| 13 | Spike: audit Graph API usage — which endpoints are hit, by which consumers, at what volume | [[joe-worsfold]] · [[billy-calladine]] | [[party-application]] | open — Session 1 |
| 14 | Walk current PCT end-to-end with [[sergiu-postolachi]] to confirm no functional gap in new PCT | [[joe-worsfold]] | [[party-curation-tool]] | open — Session 1 |
| 15 | Decide MDM-delivery squad shape (focused 3 vs. whole ~13-person squad) | [[rory-beattie]] | [[party-application]] | open — Session 1; [[open-questions#OQ-017]]; blocks sprint-planning |
| 16 | Ticketise: backfill, intercept, adapter, new PCT, coupled rollout flag | [[alex-sillars]] · [[joe-worsfold]] | [[party-application]] | open — Session 1 (precursor to Action 8's "tickets created") |
| 17 | Define D&B scheduled-refresh cadence (monthly / quarterly) | [[joe-worsfold]] | [[party-application]] | open — Session 1; [[open-questions#OQ-011]] |
| 18 | Graph-API consumer audit spike — resolves Eclipse retirement question | [[joe-worsfold]] · [[billy-calladine]] | [[party-application]] | open — Session 1; [[open-questions#OQ-001]] · [[open-questions#OQ-004]] |
| 19 | Build MDM-owned bulk-migration CLI (CSV → preview → approved revision) | [[joe-worsfold]] | [[party-application]] | open — Session 1; scoped under [[bulk-migrations-owned-by-mdm-phase-1]] |
| 20 | Identify remaining Session 1 PCT-neighbourhood people (Michael Hay) | [[rory-beattie]] | [[party-curation-tool]] | open — [[open-questions#OQ-027]] |

## Ownership gaps

All identity gaps are tracked centrally in [[open-questions]]. Summary of currently-unidentified people touching this matrix:
- _(resolved — PreBind PO is [[daria-romanovskaia]])_
- "Enriched team" identity — [[open-questions#OQ-012]]
- QA / test lead — no open-question ID yet (low-urgency; revisit when go-live checklist begins)
- Michael Hay — [[open-questions#OQ-027]]
- Session 2 first-name-only: Sam ([[open-questions#OQ-024]]), Bob ([[open-questions#OQ-025]]), Josie ([[open-questions#OQ-026]])
- Session 1 first-name-only: Kartik ([[open-questions#OQ-028]]), Jenny/Ginny ([[open-questions#OQ-029]]), Andrew Tennant ([[open-questions#OQ-030]])
- Session 2 speakers: Speakers 7 and 15 ([[open-questions#OQ-023]])

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
