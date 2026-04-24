# Log

Chronological, append-only. Each entry uses `## [YYYY-MM-DD] <kind> | <subject>` so `grep "^## \[" log.md | tail -5` gives recent activity.

Kinds: `ingest` · `query` · `analysis` · `lint` · `schema` · `note`

---

## [2026-04-22] schema | wiki bootstrapped

- Created `AGENTS.md` with full schema (layers, folder conventions, page format, workflows, interaction rules).
- Created `index.md` (empty catalog) and `log.md` (this file).
- Created folder structure: `raw/`, `raw/assets/`, `wiki/{entities,concepts,sources,analyses,_templates}/`.
- Seeded `wiki/overview.md` as a stub pending first ingest.
- Added page templates in `wiki/_templates/`.
- touched: [[AGENTS]], [[index]], [[overview]]

## [2026-04-22] schema | domain-specific extensions for re-architecture programme

- Added `wiki/applications/` and `wiki/decisions/` folders + templates.
- Added project-specific frontmatter fields: `aliases`, `owner`, `application`, `state` (applications), ADR-style `status` values for decisions.
- Added vocabulary section to disambiguate "party" (business party / application / working unit).
- Added meeting-note sub-workflow (§5.1a) — extracts attendees, decisions, actions (with owner), open questions.
- Introduced two standing analyses: [[dependency-map]] and [[ownership-matrix]], refreshed on every ingest.
- Replaced §10 Bootstrap State with §10 Project Scope reflecting declared programme.
- Fixed source template's `raw` link syntax (removed broken `../../` relative path).
- touched: [[AGENTS]]

## [2026-04-22] ingest | project scope declaration (user-supplied, not a raw source)

- Human declared programme: re-architecture of [[party-application]] ahead of [[high-volume]] go-live; shift from payload to ID+version contract; target model co-defined with [[inrisk-engine]].
- Seeded 5 application pages: [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]] — all stubs with explicit Pending sections.
- Seeded 4 team entity pages: [[graph-team]], [[dataops-team]], [[prebind-team]] (aka Strikers), [[devx-team]].
- Seeded standing analyses [[dependency-map]] and [[ownership-matrix]].
- Refreshed [[overview]] with real thesis, pillars, tensions, and what-to-know-next.
- **Explicit gaps recorded**: [[high-volume]] owner unknown; ELA and FAS insurance-line acronyms pending; full consumer inventory of [[party-application]] not yet established.
- **Source note**: this ingest was a scope declaration in chat, not a file in `raw/`. No source page created; future factual claims on the seeded pages must cite real sources.
- touched: [[AGENTS]], [[overview]], [[index]], [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]], [[graph-team]], [[dataops-team]], [[prebind-team]], [[devx-team]], [[dependency-map]], [[ownership-matrix]]

## [2026-04-22] ingest | 20260422 Meeting Transcript — Session 2 (Pass A)

- Ingested `raw/20260422 - Meeting Transcript - Session 2.md` (4,816 lines). Second architecture-design session, [[graph-team]] + [[tech-tooling]] in room.
- **Attendees identified**: [[rory-beattie]], [[sergiu-postolachi]], [[alex-sillars]] (PO, Graph Team), [[joe-worsfold]] (Tech Lead/Architect), [[ben-joseph]] (Engineer), [[will-bone]] (Interim Head of Product, Tech Tooling). Unidentified speakers: 2, 4, 7, 10, 11, 12, 14, 15 — deferred to Pass B.
- **New team entity**: [[tech-tooling]] — governance layer above product teams; holds [[rory-beattie]], [[will-bone]], [[sergiu-postolachi]]. Previously implicit.
- **New team entity**: [[analytics-team]] — DU / Snowflake downstream consumer; key counterparty for the flattening decision.
- **Decisions promoted** (per user: top 2 only): [[strangle-the-graph-via-proxy-events]] (**accepted**) and [[pct-and-mdm-go-live-together]] (**accepted**). Other decisions (UUID + display-ID, D&B caching, S&P manual Phase 1, One Re/CUO manual, client-ID flattening direction, sanctions-flow simplification future, feature-tagging deferred) kept inline on [[party-application]] pending future promotion.
- **Applications updated**: [[party-application]] rewritten with full current/target/transitional state detail and inline decisions; [[party-curation-tool]] updated with new-PCT scope, Chakra V2/V3 deferred item, 2-day crossover pattern; [[inrisk]] restructured around interim-state dependencies and the PreBind-owned "party on submission vs requirement" framing; [[inrisk-engine]] reframed as final-state-targeted, not co-defining (per user correction); [[high-volume]] updated with ≈1 Jan production vs 1 Sep training correction and Boomi-API-only integration pattern.
- **Standing analyses refreshed**: [[dependency-map]] now has current / Phase 1 target / end state sections with named cross-cutting dependencies; [[ownership-matrix]] populated with 12 open actions and named ownership gaps.
- **Overview rewritten**: new thesis, 5 pillars, 5 tensions, what-to-know-next re-prioritised around the 3 highest-leverage unknowns (HV date, DU schema tolerance, Chakra posture).
- **Contradictions recorded**:
  - HV go-live: previously 1 Sep, now training 1 Sep / production ≈1 Jan (pending Sam confirmation).
  - InRisk Engine: previously "co-defining target model"; now "targeting final state, not co-defining; goes live when final state is realised" (per user clarification).
- **Open items carried forward**: full identification of Speakers 2/4/7/10/11/12/14/15; full names + roles for mentioned-not-attending people (Scott, Anton, Hugh, Josie, Paul Rogers, Sam, Andrea, Bob, John, Chris); HV owning team identity; ELA and FAS acronym expansions; Sergiu's formal role.
- **Deferred to Pass B**: concept pages for proxy-events, graph-strangle, party-versioning, intercept-backfill-projection, client-ID flattening, feature-tagging, display-ID-vs-system-ID; additional attendee entity pages as full names land.
- touched: [[sources/20260422-meeting-transcript-session-2]], [[strangle-the-graph-via-proxy-events]], [[pct-and-mdm-go-live-together]], [[tech-tooling]], [[analytics-team]], [[rory-beattie]], [[will-bone]], [[sergiu-postolachi]], [[alex-sillars]], [[joe-worsfold]], [[ben-joseph]], [[graph-team]], [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]], [[dependency-map]], [[ownership-matrix]], [[overview]], [[index]]

## [2026-04-22] ingest | 20260422 Meeting Transcript — Session 2 (Pass B)

- **Corrections to Pass A**:
  - HV go-live reverted to **1 September** across all pages — Pass A's "1 Jan prod / 1 Sep training" framing was speculative (raw transcript line 2993) and has been deprecated per user confirmation. Retained a Contradicts note on the source page for auditability.
  - [[sergiu-postolachi]] moved from [[tech-tooling]] (inferred) → [[graph-team]] as Scrum Master (correct).
  - [[andrea-read]] moved from implicit "InRisk Engine side" → [[tech-tooling]] as Head of Technology Engineering (correct).
  - [[john-trahearn]] and [[kris-mokrzycki]] moved from implicit DevX → [[prebind-team]] Tech Leads (correct); Kris's spelling fixed from phonetic "Chris".
  - [[antonie-labuschagne]] ("Anton") disambiguated: not an InRisk PO; Tech Lead on [[devx-team]].
  - [[analytics-team]] replaces [[data-universe-team]] as the canonical team name; Data Universe is the Snowflake platform they own. Old file deleted; all backlinks updated via replace_all.
- **Speaker identifications (Pass B)**: Speakers 2/4/14 = [[suzanna-whitefield]] (Suzy, [[architecture-team]]) / [[billy-calladine]] × 2 ([[graph-team]]); Speakers 10/11/12 = [[will-bone]]. Speakers 7 and 15 remain unidentified.
- **Mentioned-people resolutions**: Scott → [[scott-gruber]] ([[architecture-team]]); Hugh → [[hugh-lobban]] ([[data-quality-team]]); Paul Rogers → [[paul-rogers]] ([[analytics-team]]); Simon Hulbert → [[simon-hulbert]] ([[high-volume-team]]). Sam, Bob, Josie still unresolved.
- **New team entities**: [[architecture-team]], [[data-quality-team]], [[high-volume-team]]. Plus [[analytics-team]] (rename of data-universe-team).
- **New person entities** (10): [[suzanna-whitefield]], [[billy-calladine]], [[scott-gruber]], [[antonie-labuschagne]], [[hugh-lobban]], [[paul-rogers]], [[andrea-read]], [[john-trahearn]], [[kris-mokrzycki]], [[simon-hulbert]].
- **Decisions promoted from inline to formal**: [[d-and-b-caching-and-auto-parent]] (**accepted**); [[uuid-system-id-with-display-id]] (**accepted**).
- **Acronym resolutions in [[overview]]**: ELA = Expense and Loss Adjustment; FAS = Free Alongside Ship.
- **Applications updated**:
  - [[high-volume]] — owner set to [[high-volume-team]]; [[simon-hulbert]] as main contact; Sam retained as unresolved first-name reference; 1 Sep restored as the deadline.
  - [[inrisk]] — Tech Leads added ([[john-trahearn]], [[kris-mokrzycki]]); [[scott-gruber]] named as broker-workshop SME; [[paul-rogers]] named as flattening counterpart; the "Anton as InRisk PO" paraphrase corrected.
  - [[inrisk-engine]] — DevX Tech Lead [[antonie-labuschagne]] recorded; Pass A's incorrect name attributions (Andrea/John/Chris on DevX) explicitly corrected in-page.
- **Team entities updated**: [[graph-team]] (added Billy and Sergiu); [[tech-tooling]] (removed Sergiu, added Andrea); [[prebind-team]] (added John and Kris, corrected Anton attribution); [[devx-team]] (added Anton, corrected Andrea/John/Kris attributions); [[dataops-team]] (added Data Quality sponsorship relationship).
- **Standing analyses refreshed**: [[ownership-matrix]] — workstreams, owners, and gaps re-mapped against resolved names; 12 open actions restated with named counterparts. [[dependency-map]] — cross-cutting dependencies re-mapped against real owner names.
- **Overview refreshed**: "what I'd want to know next" re-prioritised ([[paul-rogers]]'s schema answer now #1, HV integration-shape #2); resolved vs pending clarifications split.
- **Open questions carried forward**: PreBind PO identity; Sam / Bob / Josie; Speakers 7 and 15; "enriched team" name; feature-tagging ownership; Chakra V2/V3 decision.
- touched: [[sources/20260422-meeting-transcript-session-2]], [[d-and-b-caching-and-auto-parent]], [[uuid-system-id-with-display-id]], [[strangle-the-graph-via-proxy-events]], [[pct-and-mdm-go-live-together]], [[analytics-team]] (new), [[architecture-team]] (new), [[data-quality-team]] (new), [[high-volume-team]] (new), [[tech-tooling]], [[graph-team]], [[prebind-team]], [[devx-team]], [[dataops-team]], [[suzanna-whitefield]] (new), [[billy-calladine]] (new), [[scott-gruber]] (new), [[antonie-labuschagne]] (new), [[hugh-lobban]] (new), [[paul-rogers]] (new), [[andrea-read]] (new), [[john-trahearn]] (new), [[kris-mokrzycki]] (new), [[simon-hulbert]] (new), [[sergiu-postolachi]], [[rory-beattie]], [[will-bone]], [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]], [[dependency-map]], [[ownership-matrix]], [[overview]], [[index]]. Deleted: `wiki/entities/data-universe-team.md` (renamed).

## [2026-04-22] ingest | 20260422 Meeting Transcript — Session 1 (Pass A)

- **Source**: raw/20260422 - Meeting Transcript - Session 1.md — morning architecture-design workshop on 2026-04-22; chronologically **before** [[sources/20260422-meeting-transcript-session-2]].
- **Initial framing (user note)**: file originally uploaded as a duplicate of Session 2; user replaced with the correct Session 1 transcript. No wiki content was affected by the duplicate — read/questioning phase only.
- **Source page created**: [[sources/20260422-meeting-transcript-session-1]] — summary, attendee table with Pass-A best-effort speaker identification, 20 key claims, promoted/candidate decisions, 9 actions, applications/entities/concepts mentioned, LLM notes, 10 open questions.
- **Pass-A speaker identifications (content-driven; engine re-numbers speakers per recording, so Session 2 mappings do not apply)**: Speaker 1 = [[alex-sillars]]; Speaker 3 = [[suzanna-whitefield]]; Speaker 4 = [[billy-calladine]]; Speaker 5 = [[joe-worsfold]]. Speakers 6 and 7 — **unidentified, flagged for Pass B**. Speakers 2 / 8 / 9 / 10 / 11 appear to be transcription noise (one-word utterances).
- **Mentioned-by-reference identifications**: Anton → [[antonie-labuschagne]] (explicit scope call: InRisk Engine not designed-for in Phase 1); John → [[john-trahearn]]; Chris → [[kris-mokrzycki]]. New first-name-only references flagged for identification: **Tom Ash / Tomas** (PCT functional requirements), **Michael Hay** (feature-tagging relationship with underwriting), **Darius** (InRisk-side counterpart, likely [[prebind-team]]), **Boyle** (afternoon external-provider session lead), **Kartik**, **Jenny / Ginny**, **Andrew Tennant**.
- **Strangler pattern is Session 1 origin**: [[strangle-the-graph-via-proxy-events]] updated. The idea was invented live in the DU-events segment of Session 1; Session 2 consolidated and formalised it with Ben Joseph's bidirectional-mappability criterion. Sources list and source_count updated (1 → 2); context section split into "Origin (Session 1, morning)" and "Consolidation (Session 2, afternoon)".
- **New formal decision**: [[no-historic-client-backfill-into-mdm]] (**accepted**) — MDM version history starts at cutover; pre-cutover InRisk per-client-ID snapshots stay in [[inrisk]] / [[data-universe]]. Alex in-room: *"if that data already exists in InRisk and the DU, I can't see good reason to include that in MDM."*
- **ADR source-reinforcements**: [[pct-and-mdm-go-live-together]] (sources: 1 → 2; added "Operational shape (from Session 1)" noting single-feature-flag coupled rollout); [[d-and-b-caching-and-auto-parent]] (sources: 1 → 2; added "Refresh loop (Session 1 addition)" — scheduled monthly/quarterly re-call spawns curation revision on drift).
- **Candidate ADRs flagged (still inline)**: no-PCT-audit-backfill; single-flag coupled rollout (likely annotation on [[pct-and-mdm-go-live-together]]); drop-Eclipse; feature-tagging-moves-to-inrisk; bulk-migrations-owned-by-MDM-Phase-1. Deferred for user call in Pass B.
- **Applications refreshed**:
  - [[party-application]] — sources: 1 → 2. Added Knowledge Graph as downstream consumer; spine-rewrite event-emit behaviour; Eclipse ingestion retirement; 6 new change items (Eclipse drop, feature-tagging Phase 1/2 split, bulk-migration CLI, no-GraphQL end-state, roles-vs-views litmus test); squad-shape open question; Graph-API-audit spike as open item.
  - [[party-curation-tool]] — sources: 1 → 2. Added single-flag coupled rollout to transitional states; new "Audit history — line in the sand" section (no Jira backfill; Snowflake is audit store); party-create-timing edge case; bulk-migration CLI as Phase 1 ownership.
  - [[inrisk]] — sources: 1 → 2. Enriched three-interim-changes with Session 1 specifics (new reference table `(client_id → party_id, version)`; broker-UID retirement phased to Phase 2+); added roles-vs-views feedback loop; feature-tagging long-term ownership; manual-create edge case; new "Historical audit scope" section referencing [[no-historic-client-backfill-into-mdm]].
  - [[inrisk-engine]] — sources: 1 → 2. Added explicit Session 1 scope call ("don't worry about Anton while we're devising any solutions right now") as a design-hygiene rule, not dismissal.
  - [[high-volume]] — unchanged by Session 1; not a primary topic.
- **Standing analyses refreshed**:
  - [[ownership-matrix]] — 9 new actions appended (total now **21**: 12 from S2 + 9 from S1); 6 new workstream rows (bulk-migration CLI, D&B scheduled refresh, Graph-API audit spike, PCT functional-requirements walkthrough, PCT functional-requirements authoring, feature-tagging relationship, MDM-squad shape, Eclipse retirement). Session 1 first-name-only references added to ownership gaps section.
  - [[dependency-map]] — added three-bucket contract-scaffolding section (operational · data distribution · enrichment); current-state diagram enriched with Eclipse (retired), broker-variant event, spine-rewrite quirk note, historical-snapshot location; Phase 1 target state enriched with no-backfill, Eclipse removal, feature-tagging-unchanged, coupled rollout, bulk CLI; end-state enriched with broker-UID retirement, feature-tagging handover, S&P API migration; cross-cutting rows added for Knowledge Graph, Graph-API audit, D&B scheduled refresh, S&P end-state.
- **Overview refreshed**: thesis rewritten to foreground the three-bucket framing; pillars extended from 5 to 7 (line-in-the-sand historical migration added as pillar #5; squad-shape tension and roles-vs-views feedback loop added as pillars #6 and #7); "What I'd want to know next" re-prioritised with Graph-API spike at #2 and MDM squad shape at #3; pending clarifications split into Session 1 and Session 2 first-name-only cohorts.
- **Index.md refreshed**: Pages count updated (2 sources · 5 decisions · 21 open actions); source counts on all Session-1-touched pages bumped to 2; ADR bullet added for no-historic-client-backfill; strangler-pattern bullet notes Session 1 as origin; Session 1 source entry added.
- **Pass-B follow-ups (pending)**: speaker identifications for Speakers 6 and 7; names/teams for Tom Ash, Michael Hay, Darius, Boyle, Kartik, Jenny/Ginny, Andrew Tennant; MDM squad-shape decision; D&B refresh cadence; Eclipse-consumer confirmation; formalisation calls on the 5 candidate-ADR items listed above; Session-1 equivalents of the "what'd I want to know" prioritisation once speaker identifications land.
- touched: [[sources/20260422-meeting-transcript-session-1]] (new), [[no-historic-client-backfill-into-mdm]] (new), [[strangle-the-graph-via-proxy-events]], [[pct-and-mdm-go-live-together]], [[d-and-b-caching-and-auto-parent]], [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[dependency-map]], [[ownership-matrix]], [[overview]], [[index]]

## [2026-04-22] ingest | 20260422 Meeting Transcript — Session 1 (Pass B)

- **Scope**: Pass-B corrections and promotions following user feedback on Pass A. Source content unchanged; structural wiki changes are substantial (new register, new concept, three new ADRs, one new entity page, schema update).
- **Speaker/reference corrections applied** (all Pass A uncertainties now closed): Speaker 6 → [[billy-calladine]] (second ID for Billy in same session); Speaker 7 → [[joe-worsfold]] (second ID for Joe in same session). Tom Ash / Tomas → new entity [[tomas-sivo]] (Business Analyst, [[graph-team]]). Michael Hay → deferred to [[open-questions#OQ-027]] (stakeholder in feature-tagging; team TBD). **"Darius"** → not a person; transcription mispronunciation of "InRisk". **"Boyle"** → not a person; transcription mispronunciation of "Will" ([[will-bone]]) in reference to his afternoon-session attendance; no third recording exists.
- **Schema change**: [[AGENTS]] updated to add `wiki/open-questions.md` as a standing file and to add §5.1b "Open-questions sub-workflow" to the ingest workflow. Ingest workflow step 7 now names open-questions alongside dependency-map and ownership-matrix.
- **New standing file**: [[open-questions]] — single source of truth for unresolved questions across the programme. OQ-NNN IDs; five categories (scope/architecture, timeline/cadence, design open calls, people identification, adr-candidate). Seeded with **31 open questions** (Session 1 + Session 2 carryover) and **14 resolved entries** from the pre-register triage (OQ-R01–OQ-R14). Convention: pages reference OQ-NNN rather than restating question text.
- **New concept page**: [[contract-buckets]] — the three-bucket scaffolding (operational · data distribution · enrichment) [[alex-sillars]] introduced at the top of Session 1. Canonical lens the rest of the programme reads through; cited from [[overview]] thesis, [[dependency-map]] bucket section, all three applications' "Related" sections.
- **Three new ADRs promoted (Pass-B user calls)**:
  - [[no-pct-audit-backfill]] (**accepted**) — mirror of [[no-historic-client-backfill-into-mdm]] on the PCT side; Snowflake is audit/SLA store; old Jira stays read-only; no extended dual-running.
  - [[feature-tagging-moves-to-inrisk]] (**accepted**) — Phase 1 no change; Phase 2+ Postgres table + widget component migrate to [[prebind-team]] / [[inrisk]]. Trigger condition → [[open-questions#OQ-019]].
  - [[bulk-migrations-owned-by-mdm-phase-1]] (**accepted**) — Phase 1 = [[graph-team]]-owned CLI (CSV → diff preview → auto-approved revision); full self-serve UX explicitly Phase 2+ → [[open-questions#OQ-006]].
- **User-directed non-promotions**:
  - *Single-flag coupled rollout* — kept as annotation on [[pct-and-mdm-go-live-together]]; no standalone ADR.
  - *Drop Eclipse ingestion* — tracked as [[open-questions#OQ-001]]; expected to be resolved by the Graph-API consumer audit spike ([[open-questions#OQ-004]]). Not an ADR.
- **New entity**: [[tomas-sivo]] — Business Analyst on [[graph-team]]; owns PCT functional-requirements workstream. Transcription drift "Tomas" → "Tom Ash" documented on the entity page.
- **Existing entities updated** (Session 1 speaker-ID harvesting): [[joe-worsfold]] — second S1 speaker ID (Speaker 7) added to aliases; sources bumped to 2; Session-1-specific claims (strangler origin, roles-vs-views litmus test, squad-shape concern) added. [[billy-calladine]] — second S1 speaker ID (Speaker 6) added to aliases; sources bumped to 2; spine-rewrite insight captured as a claim; Summary rewritten. [[graph-team]] — [[tomas-sivo]] added to members table; sources bumped to 2; Open-questions section rewritten to cite OQ-017 (squad shape) rather than restate it.
- **Applications refreshed to reference new ADRs + open-questions register**:
  - [[party-application]] — Eclipse retirement moved from "accepted" to OQ-001; three new ADRs added to Related decisions; Pending/unknown section rewritten to reference OQ-IDs.
  - [[party-curation-tool]] — [[no-pct-audit-backfill]] replaces the inline "line in the sand" prose; [[bulk-migrations-owned-by-mdm-phase-1]] added; Pending/unknown section rewritten to reference OQ-IDs; [[tomas-sivo]] added to Change-ownership section.
  - [[inrisk]] — [[feature-tagging-moves-to-inrisk]] added to Related decisions and inline feature-tagging section; Pending/unknown section rewritten to reference OQ-IDs.
- **Source page refreshed**: [[sources/20260422-meeting-transcript-session-1]] — attendee table updated with Pass-B confirmations; promoted-to-ADR section now splits Pass A (no-historic-backfill) and Pass B (three new); reinforced-ADRs list cleaned; "still-inline candidates" replaced with a "Tracked as open scope call" single item for Eclipse; "Darius" and "Boyle" documented as transcription artefacts in their own sub-section; attendees / mentioned-by-reference / non-entities split cleanly; Pass-A open-questions list replaced with a resolution + deferral map to the open-questions register; contract-buckets concept page cited; LLM-notes section updated with the "transcription-artefact hypothesis" lesson.
- **Standing analyses refreshed**:
  - [[ownership-matrix]] — action count corrected to **20** (one combined people-identification action collapsed into per-person OQ rows); workstream rows updated to cite ADRs and OQ-IDs (feature-tagging → [[feature-tagging-moves-to-inrisk]] · OQ-019; bulk-CLI → [[bulk-migrations-owned-by-mdm-phase-1]]; D&B cadence → OQ-011; Graph API spike → OQ-004; Tomas Sivo resolved; squad shape → OQ-017 with Rory as primary owner). Ownership-gaps section replaced with a compact list of OQ-IDs. Resolved section grew to include OQ-R10–OQ-R14. Source-history Pass B entry added describing the collapse.
  - [[dependency-map]] — Contract-buckets scaffolding section now links the new [[contract-buckets]] concept page instead of flagging it as a candidate.
- **Overview refreshed**: thesis cites [[contract-buckets]] directly; pillar #5 extended to call out [[no-pct-audit-backfill]] explicitly; new pillar #7 added ("Phase-1 ownership calls ring-fence the critical path"). Pillar #6 (squad shape) now cites [[open-questions#OQ-017]]. "What I'd want to know next" shrunk from 10 items to 9 as the "remaining unidentified people" catch-all was replaced by individual OQ-IDs cited where they belong. "Resolved clarifications" and "Pending clarifications" sections collapsed into a single Clarifications-resolved summary pointing at the register. Related section extended to link [[contract-buckets]], [[open-questions]], and the three new ADRs.
- **Index.md regenerated** (full rewrite, not patch): pages count reflects new concept (1), new entity (17 people), new ADRs (8 total decisions), new standing register (3 total standing analyses); source counts bumped where relevant; [[tomas-sivo]] added to People; [[contract-buckets]] added to Concepts; three new ADR bullets added; [[open-questions]] added under Analyses with "31 open, 14 resolved" counter; "How to read this file" footer now mentions the OQ-NNN referencing convention.
- **Pass-B follow-ups (pending, now all in [[open-questions]])**: MDM delivery squad shape (OQ-017, Rory's call); D&B refresh cadence (OQ-011); Eclipse-consumer confirmation (OQ-001, expected to be resolved by OQ-004 spike); identifications still pending for Michael Hay (OQ-027), Kartik / Jenny-Ginny / Andrew Tennant (OQ-028–030), PreBind PO (OQ-031), "enriched team" (OQ-012), Session 2 Speakers 7 and 15 + Sam / Bob / Josie (OQ-023–026).
- touched: [[AGENTS]], [[open-questions]] (new standing file), [[contract-buckets]] (new concept), [[no-pct-audit-backfill]] (new ADR), [[feature-tagging-moves-to-inrisk]] (new ADR), [[bulk-migrations-owned-by-mdm-phase-1]] (new ADR), [[tomas-sivo]] (new entity), [[joe-worsfold]], [[billy-calladine]], [[graph-team]], [[party-application]], [[party-curation-tool]], [[inrisk]], [[sources/20260422-meeting-transcript-session-1]], [[ownership-matrix]], [[dependency-map]], [[overview]], [[index]]

---

## [2026-04-22] lint | Post-Session-1-Pass-B lint pass — findings addressed

- **Scope**: Apply user-approved fixes to the lint report. Correct broken links and stale claims; create the missing pages surfaced by the report; reclassify the Knowledge Graph; record the now-identified PreBind PO.
- **Stale claim corrected**: [[will-bone]] — "1 Sep is training" clause deprecated; 1 September stands as the programme deadline (resolves A.1).
- **Broken links repaired**:
  - `[[hugh]]` → [[hugh-lobban]] (2 sites: [[will-bone]], [[pct-and-mdm-go-live-together]]).
  - `[[pillars]]` → [[overview#Pillars]] ([[bulk-migrations-owned-by-mdm-phase-1]]).
  - `[[data-universe-team]]` breadcrumb in [[analytics-team]] rewritten to plain text (kept as alias in frontmatter).
- **Raw-file link format standardised**: source pages and `_templates/source.md` now use an inline code-span (e.g. `` `raw/<filename>.md` ``) rather than a wiki link, to keep the raw layer out of the Obsidian graph view. This is the preferred format going forward.
- **New pages created**:
  - **Platform**: [[data-universe]] — analytics data platform (Snowflake-backed) owned by [[analytics-team]], with [[steve-perry]] (Programme Manager) and [[chris-woodward]] (Architect).
  - **People**: [[steve-perry]], [[chris-woodward]], [[daria-romanovskaia]].
  - **Application stub**: [[eclipse]] — external data feed consumed by [[inrisk]]; status field deliberately omitted per user direction; more detail pending.
  - **Concepts**: [[roles-vs-views-litmus-test]] (Joe's one-question test from Session 1); [[intercept-backfill-projection]] (the three-phase delivery trio named in Session 2).
- **Knowledge Graph reclassified** (D.10): not a separate application or downstream consumer — it is the **Neo4j instance internal to [[party-application]]**, acting as the current-state primary datastore. Removed from the data-distribution bucket on [[dependency-map]] and [[contract-buckets]]; removed from the Key consumers line on [[party-application]]; OQ-010 resolved on [[open-questions]].
- **Knowledge Graph present state preserved**: [[party-application]] now calls it out explicitly as the current-state Neo4j backend that the target-state DynamoDB + OpenSearch replaces.
- **PreBind PO identified** (F.14): [[daria-romanovskaia]] — Product Owner on [[prebind-team]]. OQ-031 resolved on [[open-questions]]; the stale "Anton as PO" disclaimer removed from [[inrisk]] and [[prebind-team]]; [[overview]]'s "what I'd want to know next" list re-numbered.
- **Analytics Team page refreshed**: added [[steve-perry]] and [[chris-woodward]] to the members table; removed the corrective inline `[[data-universe-team]]` reference.
- **Index regenerated**: page counts updated (6 applications, 1 platform, 20 people, 3 concepts); new pages listed; status note updated; "Platforms" sub-section introduced under Entities.
- **Open-questions register updated**: OQ-010 and OQ-031 moved from Open to Resolved with answers. Source-history appended. Open count now 29; resolved count now 16.
- **Items explicitly ignored this pass (per user)**: OQ-012 ("enriched team" identity) and OQ-013 (HV integration-shape specifics) remain open.
- touched: [[will-bone]], [[pct-and-mdm-go-live-together]], [[bulk-migrations-owned-by-mdm-phase-1]], [[_templates/source]], [[sources/20260422-meeting-transcript-session-1]], [[sources/20260422-meeting-transcript-session-2]], [[analytics-team]], [[data-universe]] (new), [[steve-perry]] (new), [[chris-woodward]] (new), [[eclipse]] (new), [[roles-vs-views-litmus-test]] (new), [[intercept-backfill-projection]] (new), [[daria-romanovskaia]] (new), [[party-application]], [[party-curation-tool]], [[inrisk]], [[prebind-team]], [[contract-buckets]], [[dependency-map]], [[ownership-matrix]], [[overview]], [[open-questions]], [[index]]

---

## [2026-04-22] schema | Entities folder split into people / teams / platforms

- **Rationale**: the flat `wiki/entities/` folder had reached 30 pages mixing three structurally different things (humans, working units, and a platform). The distinction was already implicit in frontmatter (`tags: [person]` / `[team]` / `[platform]`) and in how pages are queried. Splitting now, before the wiki grows further, makes browsing cheaper and reduces ambiguity when new entities are added.
- **Action — folder moves (30 files, zero link rewrites)**:
  - `wiki/entities/people/` — 20 files (every human: alex-sillars, andrea-read, antonie-labuschagne, ben-joseph, billy-calladine, chris-woodward, daria-romanovskaia, hugh-lobban, joe-worsfold, john-trahearn, kris-mokrzycki, paul-rogers, rory-beattie, scott-gruber, sergiu-postolachi, simon-hulbert, steve-perry, suzanna-whitefield, tomas-sivo, will-bone).
  - `wiki/entities/teams/` — 9 files (analytics-team, architecture-team, data-quality-team, dataops-team, devx-team, graph-team, high-volume-team, prebind-team, tech-tooling).
  - `wiki/entities/platforms/` — 1 file (data-universe). First occupant; future candidates include D&B, S&P, NTT, Artificial, Launchpad if they ever get first-class pages.
- **Zero link breakage** (confirmed before the move): there were no folder-qualified `[[entities/...]]` links anywhere in the wiki. Obsidian resolves `[[name]]` by filename regardless of folder.
- **Templates split**: retired the single `_templates/entity.md`; created `_templates/person.md`, `_templates/team.md`, `_templates/platform.md` with the section skeletons each sub-type actually uses (e.g. Members table on teams; Current-shape/Target-state on platforms; Team frontmatter slug on people).
- **AGENTS.md updated**:
  - File tree under §3 rewritten to reflect the three-way split and the new template names.
  - Vocabulary section (§3) extended to define **person**, **platform** alongside the existing **business party**, **application**, **working unit**.
  - New **"Choosing an entities subfolder"** cascade under §3 to make future ingests deterministic about where a new entity page belongs.
  - New filename rule: filenames must be globally unique across entity subfolders (so `[[slug]]` stays unambiguous).
  - Raw-file reference convention (inline code-span, not wiki link) promoted from the lint-pass practice into a formal filename rule.
  - §4 Page-format reference under "Entity" replaced with a three-way breakdown — one paragraph per sub-type, listing the sections each template enforces.
- **Not changed**: `index.md` (its Teams / People / Platforms sub-sections already reflected the split); all wiki-link references across the wiki (still bare slugs).
- touched: [[AGENTS]], [[_templates/person]] (new), [[_templates/team]] (new), [[_templates/platform]] (new), and file moves for all 30 entity pages
