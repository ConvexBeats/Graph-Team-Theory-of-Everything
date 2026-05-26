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

## [2026-04-22] schema | projects + phases restructure

- **New page types**: `project`, `phase`, `portfolio-overview` added to the `type:` enum. Templates created: `_templates/project.md`, `_templates/phase.md`.
- **New frontmatter fields**:
  - `projects: [<slug>, …]` — on cross-project pages (applications, entities, platforms, concepts) that touch multiple projects.
  - `project: <slug>` — on project-scoped pages (sources, decisions, phase pages, project-scoped analyses).
  - `phase: [<phase-id>, …]` — on decisions / analyses scoped to specific phases within a project (empty list = project-level, not phase-bound).
  - `phase_id`, `slug`, `gate_date` — on phase / project pages.
- **New folder**: `wiki/projects/<slug>/`. Project-scoped files (project overview, phase pages, project-scoped dependency map and ownership matrix) now live here, prefixed with the project slug for global filename uniqueness (e.g. `party-rearch.md`, `party-rearch-phase-1.md`, `party-rearch-dependency-map.md`).
- **Moved**:
  - `wiki/overview.md` → `wiki/projects/party-rearch/party-rearch.md` (is now the party-rearch project thesis). Frontmatter changed to `type: project` with slug/status/applications fields.
  - `wiki/analyses/dependency-map.md` → `wiki/projects/party-rearch/party-rearch-dependency-map.md`.
  - `wiki/analyses/ownership-matrix.md` → `wiki/projects/party-rearch/party-rearch-ownership-matrix.md`.
  - `wiki/analyses/` retained with a `.gitkeep` — reserved for cross-project / portfolio-level standing analyses as they emerge.
- **New files**:
  - `wiki/overview.md` — rewritten from scratch as the **portfolio overview** (projects table, cross-project themes, how-to-read guide). Previously the programme-level thesis; that content is now on `[[party-rearch]]`.
  - `wiki/projects/party-rearch/party-rearch-phase-1.md` — MDM + PCT co-ship to 1-Sep gate; full scope table, 8 phase-1 decisions, open questions, done-state.
  - `wiki/projects/party-rearch/party-rearch-phase-2.md` — post-cutover end-state stub; deferred items inventory.
  - `wiki/projects/standardise-secrets/standardise-secrets.md` — stub project page for the ASG-mandated secrets-management project (Shai-Hulud 2.0 / INC-140574 driver); scope/owner/timeline TBD at first ingest.
  - `wiki/projects/standardise-secrets/standardise-secrets-phase-1.md` — placeholder phase stub.
- **Link rewrites**: 35 replacements across 18 files:
  - `[[dependency-map]]` → `[[party-rearch-dependency-map]]`
  - `[[ownership-matrix]]` → `[[party-rearch-ownership-matrix]]`
  - `[[overview]]` / `[[overview#Pillars]]` rewrites where the reference meant the old programme thesis (now `[[party-rearch]]` / `[[party-rearch#Pillars]]`); references on the new portfolio page and project stubs that correctly point at `[[overview]]` were left alone.
- **Frontmatter injections**: `project:` (+`phase:` on decisions) added to 8 decisions, 2 sources, and 6 applications. 24 frontmatter lines added total. No pre-existing keys were overwritten.
- **open-questions.md**: added a mandatory **`Project`** column as column 2 on every table (Open + Resolved). All 31 existing rows assigned `party-rearch`. The `How to read` section documents the new column. IDs remain global across projects — the namespace is not per-project.
- **AGENTS.md updates**:
  - §3 Folder conventions: tree updated to show `wiki/projects/<slug>/`, `open-questions.md` noted as global with a `Project` column, and filename uniqueness rule extended to cover project-scoped files (slug-prefixed).
  - §3 Vocabulary: `project` and `phase` added; `application`/`person`/`team`/`platform` clarified as **cross-project** entities.
  - §3 New cascade: **Choosing a project/phase location** — the decision tree for where a new project-scoped page belongs.
  - §4 Frontmatter: extended enum with `project | phase | portfolio-overview`; new optional fields `project`, `projects`, `phase`, `phase_id`, `slug`, `gate_date` documented.
  - §4 Page-type-specific sections: new rows for `project`, `phase`, and `portfolio-overview` (replacing the old "overview" entry).
  - §5.1 Ingest workflow: new Step 1 "Identify the project"; standing-analyses refresh now targets project-scoped files; portfolio overview only updated when the portfolio itself changes.
  - §5.1a Meeting-note sub-workflow: ADRs must carry `project:` + `phase:` from creation; actions fold into the project-scoped ownership matrix.
  - §5.1b Open-questions: Project column is mandatory; IDs are global across all projects.
  - §6 `index.md` example: rewritten with Portfolio / Projects sections, project-scoped analyses listed under each project, cross-project sections marked as such.
  - §10 "Project scope" → **"Portfolio scope"**: now a portfolio summary listing both in-flight projects with a one-line thesis each; application list clarified as cross-project.
- **index.md**: regenerated with Portfolio + Projects top-level sections, project-scoped analyses listed under each project, `projects:` metadata added to application entries, and updated counts (2 projects · 3 phases · 3 project-scoped analyses).
- touched (structural): [[AGENTS]], [[index]], [[overview]] (new portfolio content), [[party-rearch]] (ex-overview), [[party-rearch-phase-1]] (new), [[party-rearch-phase-2]] (new), [[party-rearch-dependency-map]] (ex-dependency-map), [[party-rearch-ownership-matrix]] (ex-ownership-matrix), [[standardise-secrets]] (new), [[standardise-secrets-phase-1]] (new), [[_templates/project]] (new), [[_templates/phase]] (new), [[open-questions]] (+Project column), plus 18 files that had their `[[dependency-map]]`/`[[ownership-matrix]]`/`[[overview]]` links redirected, and 16 pages that received project/phase frontmatter.

## [2026-04-22] note | rename project slug `standardise-secrets` → `secrets-management`

- Per user preference, renamed the second project's slug from `standardise-secrets` to `secrets-management`. Human-readable title unchanged ("Standardise Secrets Management"); only the slug and its file/folder names changed.
- Renamed: `wiki/projects/standardise-secrets/` → `wiki/projects/secrets-management/`, `standardise-secrets.md` → `secrets-management.md`, `standardise-secrets-phase-1.md` → `secrets-management-phase-1.md`.
- 31 slug replacements across 6 files: [[AGENTS]], [[index]], [[overview]], [[open-questions]], [[secrets-management]], [[secrets-management-phase-1]].
- Kept `standardise-secrets` as an alias on [[secrets-management]]'s frontmatter so readers searching for the old slug still land on the page.
- This log entry intentionally preserves the old slug in historical entries above — the log is append-only.

## [2026-04-22] analysis | party-rearch Phase 1 — Summary & Applications-Affected (filed)

- **Query**: "Could you give me a summary of Phase 1 of the MDM party rearchitecture project and which applications are affected?"
- **Filed as**: [[party-rearch-phase-1-summary]] at `wiki/projects/party-rearch/party-rearch-phase-1-summary.md` (`type: analysis`, `project: party-rearch`, `phase: [phase-1]`).
- Focused digest: what Phase 1 is, the 6-row applications-affected table (with change-intensity and shape-of-change), the 7 Phase-1 ADRs, the [[intercept-backfill-projection]] delivery shape, and the top-5 open questions gating the 1-Sep cutover. Distinct from [[party-rearch-phase-1]] (canonical, exhaustive) by being applications-first and readable in one sitting.
- **Confidence**: high on structure (re-assembly of already-reviewed content), medium-to-low on HV integration shape ([[open-questions#OQ-013]]) and MDM squad shape ([[open-questions#OQ-017]]) which remain open — noted on-page with explicit follow-ups to refresh when resolved.
- No new pages created beyond the summary itself; no existing page content altered.
- touched: [[index]] (added summary under party-rearch analyses; page-count bumped 3 → 4 project-scoped analyses), [[party-rearch-phase-1-summary]] (new).

## [2026-04-22] schema | introduce architecture pages (current-state + phase-target)

- **Motivation**: identity pages for applications were carrying both "role / ownership / change items" (identity) and "tech / touchpoints / constraints" (architecture). As more phases land, the latter needs a per-phase target and a fold-in workflow — mixed into the identity page that becomes unreadable. Splitting the two concerns.
- **Structure introduced**:
  - **Current-state architecture** (cross-project, per app): `wiki/applications/<app-slug>-architecture.md`. `type: application-architecture`, `state: current`. One per application. This is the living reference; phase completions fold targets into it.
  - **Phase-end target architecture** (project-scoped, per (project, phase, app)): `wiki/projects/<slug>/<slug>-<phase-id>-<app-slug>-architecture.md`. `type: phase-application-architecture`, `state: phase-target | shipped | superseded`, `baseline: <app-slug>-architecture`, with a load-bearing **Delta from current state** section plus a standalone **Full end-of-phase shape** section.
- **AGENTS.md changes**: added both types to the type enum; added `baseline` + architecture-specific `state` frontmatter; added architecture entries to the folder tree (both `wiki/applications/` and `wiki/projects/<slug>/`); added entries 4 & 5 to the project-location cascade (phase-target architectures go under the project folder; current-state architectures stay cross-project); added **§5.4 Phase completion** workflow — the fold-in routine that moves old current-state claims to **Superseded claims** and promotes the phase target to the new baseline; added page-section guidance for both architecture types.
- **Templates** (new): [[_templates/application-architecture]], [[_templates/phase-application-architecture]].
- **Skeletons created** (current-state, `status: stub`, ready to be populated from forthcoming raw-folder ingests):
  - [[party-application-architecture]] — richest skeleton; pre-seeded with Neo4j-as-primary-store, payload-event contract, Graph-API consumer-audit open question, Eclipse scope call
  - [[party-curation-tool-architecture]] — Next.js-on-Jira, D&B integration, Chakra-widget tension pre-seeded
  - [[inrisk-architecture]] — integration-surface-focused stub (client-ID / submission / requirement nesting; Eclipse ingestion; HV-integration open question)
  - [[high-volume-architecture]] — lean stub; HV is greenfield so "current state" is a baseline for its target page to delta from
- **Identity pages updated** (4): callout added at top linking to the architecture page; architecture page added to **Related** — [[party-application]], [[party-curation-tool]], [[inrisk]], [[high-volume]]. Identity-page content intentionally not cut — migration of detail from identity to architecture page happens organically with the first real architecture ingest.
- **[[party-rearch-phase-1]]**: scope table followed by a new **Architecture pages per application** sub-table pairing each affected app's current-state page with its (to-be-created) phase-1 target page.
- **Phase-target pages deliberately NOT created yet** — they'll be authored from authoritative source material when the user drops architecture detail into the raw folder. Current skeletons make clear what "to be created" pages will be named so cross-links can be established in advance.
- **No content loss**: identity pages retain all their pre-existing detail; the architecture pages are additive scaffolding only.
- touched (new, 6): [[_templates/application-architecture]], [[_templates/phase-application-architecture]], [[party-application-architecture]], [[party-curation-tool-architecture]], [[inrisk-architecture]], [[high-volume-architecture]].
- touched (edited, 7): [[AGENTS]] (§3 folder tree + §3 cascade + §4 type-enum + §4 frontmatter + §4 page-type sections + new §5.4 workflow), [[index]] (new "Application architecture" section, updated counts, updated "How to read"), [[party-application]], [[party-curation-tool]], [[inrisk]], [[high-volume]] (architecture callout + Related link), [[party-rearch-phase-1]] (architecture sub-table).

## [2026-04-22] schema | per-application folders under `wiki/applications/`

- **Motivation**: with two pages per app (identity + current-state architecture) in a flat folder and richer per-app material likely ahead (API contracts, data models, runbooks, diagrams), the flat layout was about to get cluttered. Moved to one folder per application so every app has a clear home for all its material. Symmetric with `wiki/projects/<slug>/`.
- **Before**: `wiki/applications/<app-slug>.md` and `wiki/applications/<app-slug>-architecture.md` sitting flat alongside each other for 6 apps.
- **After**: `wiki/applications/<app-slug>/<app-slug>.md` + `.../<app-slug>-architecture.md`, with room for `<app-slug>-<aspect>.md` siblings (e.g. `party-application-api-contracts.md`, `party-application-data-model.md`, `party-application-runbook.md`) and a `diagrams/` subfolder when they become needed.
- **No link breakage**: Obsidian resolves wiki links by basename, so every existing `[[party-application]]`, `[[party-curation-tool-architecture]]`, etc. continues to resolve without edits. No link rewrites were needed across the wiki. Confirmed by `rg` sweep for path-style references — none found.
- **Files moved**: 10 (6 identity + 4 current-state architecture). Filenames unchanged; only the path changed.
- **Schema changes** ([[AGENTS]]):
  - Folder tree: `applications/` entry rewritten to show `applications/<app-slug>/` with identity + architecture + optional richer pages.
  - Cascade: entry 5 (current-state architecture) updated to the new path; **new entry 7** added to govern richer per-app pages (API contracts, data models, runbooks, diagrams) — they live in the per-app folder alongside identity and architecture; entries 8 & 9 renumbered.
  - Page-section guidance: `application` and `application-architecture` paths updated.
- **Templates** updated: both [[_templates/application]] and [[_templates/application-architecture]] got a `<!-- Lives at: … -->` comment clarifying the target path; the application template gained a "Architecture" callout block and `projects: []` frontmatter field in line with existing pages. Also cleaned up two stale placeholders: [[_templates/source]] and [[_templates/decision]] replaced illustrative `[[applications/...]]` path-style placeholders with basename-style `[[app-slug]]` to match the convention used everywhere else.
- **[[index]]**: "How to read" entry for Applications rewritten to describe the per-app folder; date-stamp updated.
- touched (structural, 10 moved): `wiki/applications/party-application/{party-application.md, party-application-architecture.md}`, `wiki/applications/party-curation-tool/{party-curation-tool.md, party-curation-tool-architecture.md}`, `wiki/applications/inrisk/{inrisk.md, inrisk-architecture.md}`, `wiki/applications/inrisk-engine/inrisk-engine.md`, `wiki/applications/high-volume/{high-volume.md, high-volume-architecture.md}`, `wiki/applications/eclipse/eclipse.md`.
- touched (edited, 5): [[AGENTS]], [[index]], [[_templates/application]], [[_templates/application-architecture]], [[_templates/source]] + [[_templates/decision]] (placeholder tidy).

## [2026-04-22] schema | per-phase folders under `wiki/projects/<slug>/`

- **Motivation**: same scaling pressure as the per-app migration, one level in. Each phase is about to grow past a single page — phase page + phase summary + N phase-target architectures + potential phase retros/digests. Phase 1 of [[party-rearch]] alone will reach ~6–8 files once its phase-target architectures are populated. Moved to a folder-per-phase layout so each phase has a clear home.
- **Before**: `wiki/projects/<slug>/<slug>-phase-N.md` (and `<slug>-phase-N-<analysis>.md`, `<slug>-phase-N-<app>-architecture.md`) sitting flat in the project folder alongside the project overview and cross-phase standing analyses.
- **After**: `wiki/projects/<slug>/phase-N/` contains every phase-scoped artefact. The project root keeps only **cross-phase** material — the project overview, dependency map, ownership matrix, and future cross-phase analyses.
- **Critical design choice — filenames keep the `<slug>-phase-N-` prefix.** The folder path repeats what's in the filename, which looks redundant, but dropping the prefix would collide on basename with current-state architecture pages (e.g. `party-application-architecture.md` already exists in `wiki/applications/party-application/`). Redundancy preserves basename uniqueness and thus unambiguous Obsidian link resolution. This is the same rule applied to app folders — folder is organisational, basename is identity.
- **No link breakage**: Obsidian resolves wiki links by basename, so every existing `[[party-rearch-phase-1]]`, `[[party-rearch-phase-1-summary]]`, `[[party-rearch-phase-2]]`, `[[secrets-management-phase-1]]` resolves without any rewrites. Confirmed by `rg` sweep for stale path references — only matches are in historical `log.md` prose (which is append-only and deliberately preserves prior placements).
- **Files moved**: 4.
  - `wiki/projects/party-rearch/party-rearch-phase-1.md` → `.../phase-1/party-rearch-phase-1.md`
  - `wiki/projects/party-rearch/party-rearch-phase-1-summary.md` → `.../phase-1/party-rearch-phase-1-summary.md`
  - `wiki/projects/party-rearch/party-rearch-phase-2.md` → `.../phase-2/party-rearch-phase-2.md`
  - `wiki/projects/secrets-management/secrets-management-phase-1.md` → `.../phase-1/secrets-management-phase-1.md`
- **Schema changes** ([[AGENTS]]):
  - Folder tree: `projects/<slug>/` now shows `phase-N/` subfolders containing the phase overview, phase-target architectures, and phase-scoped analyses; project-level entries stay above the phase folder.
  - Filename rules: added a bullet about phase-scoped files carrying the `<slug>-phase-N-` prefix and why the folder repeats it; filename-uniqueness rule expanded to include per-application and per-phase subfolders.
  - Cascade: entries 2, 4, and 5 updated to the new path shape (phase overview, phase-scoped analysis, phase-target architecture); **new entry 4 added** to govern phase-scoped analyses (e.g. `party-rearch-phase-1-summary`) distinctly from cross-phase ones (entry 3); entries 5–10 renumbered accordingly.
  - Page-section guidance: paths for `phase` and `phase-application-architecture` updated.
- **Templates** updated: [[_templates/phase]] and [[_templates/phase-application-architecture]] gained `<!-- Lives at: … -->` comments clarifying the nested path and the sibling pages expected in the same phase folder. Template bodies unchanged.
- **[[index]]**: "How to read" Projects entry rewritten to describe the phase-subfolder structure; date-stamp updated.
- **Phase-scoped analyses given their own cascade entry**: a phase-scoped analysis like `party-rearch-phase-1-summary` now has an unambiguous home (inside the phase folder, with `project:` + `phase:` frontmatter), distinct from cross-phase standing analyses like `party-rearch-dependency-map` (which stay at the project root, with no `phase:` field). This clarifies the earlier user question "why didn't `party-rearch-phase-1-summary` go in `wiki/analyses/`?" — it's project- AND phase-scoped, so it lives in its phase folder.
- touched (structural, 4 moved): files listed above.
- touched (edited, 4): [[AGENTS]], [[index]], [[_templates/phase]], [[_templates/phase-application-architecture]].

---

## [2026-05-11] query | is the feed to the Data Universe changing in Phase 1?

- **Query**: "Is the feed to the Data Universe changing as part of the Phase 1 delivery for September?"
- **Answer (filed inline in chat, not as a wiki page)**: No on event shape; yes on producer. Phase 1 keeps the DU feed's event shape unchanged via [[strangle-the-graph-via-proxy-events]] — MDM emits proxy events in the existing Graph shape, satisfying [[ben-joseph]]'s bidirectional-mappability criterion. The underlying producer migrates from Neo4j (Graph) to DynamoDB + OpenSearch (MDM). Phase 2+ replaces proxy events with a direct Dynamo → DU stream (Snowpipe-style). Single biggest Phase-1 branch point that could change the answer is [[open-questions#OQ-002]] — DU tolerance for a flattened `(party-id, submission-id)` shape; current intent is to preserve current contract.
- **Wiki pages cited**: [[party-rearch-phase-1]] · [[strangle-the-graph-via-proxy-events]] · [[data-universe]] · [[open-questions#OQ-002]] · [[party-rearch-phase-1-summary]].
- No analysis page filed (answer is a synthesis of already-filed material; not load-bearing enough to warrant a standalone analysis).
- touched: none (read-only query).

## [2026-05-18] ingest | 20260513 InRisk Integration with Party MDM — Follow-up

- **Source**: `raw/20260513 - InRisk Integration With Party MDM Follow Up.md` — third party-rearch architecture-design call (47 min on 2026-05-13). First dedicated session on the **InRisk side** of Phase 1.
- **Attendees**: [[rory-beattie]], [[joe-worsfold]] ([[graph-team]] TL), [[john-trahearn]] + [[kris-mokrzycki]] ([[prebind-team]] TLs), [[suzanna-whitefield]] ([[architecture-team]]), [[sergiu-postolachi]] ([[graph-team]] SM — brief).
- **Source page created**: [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — key claims grouped under 5 themes (InRisk Phase-1 epic; party-tagging vs feature-tagging; cutover sequencing; Chakra V2/V3 resolution; sanctions/NTT/Boomi), decisions, actions, applications/entities/concepts mentioned, contradictions/reinforces, LLM notes, source-page open-questions.

**Decisions** (per user steers in chat):

- **New ADR**: [[inrisk-cuts-over-before-high-volume]] (**accepted** · phase-1). InRisk cuts over to MDM ≥ 2 weeks before 1 Sep HV go-live (concrete date open at [[open-questions#OQ-035]]); HV switches on at the gate. **Rolls in** the two-component-libraries call: [[joe-worsfold]] publishes a second, **design-system-agnostic component library** for [[inrisk]] alongside the Chakra-3-with-design-system widget for [[party-curation-tool]]. **Resolves** [[open-questions#OQ-005]]. HV is API-only and unaffected by the library decision.
- **Refinement** to [[strangle-the-graph-via-proxy-events]]: brief **dual-write to old graph during the cutover window** for revertability. Not a contradiction of "no dual sources of truth" — MDM is still the source of truth in the live path; the old graph receives a write-through for revert safety only. Non-prod gets the same pattern, easier to toggle back and forth.
- **Refinement** to [[feature-tagging-moves-to-inrisk]]: Phase-1 carve-out is explicit — old backend (Postgres table + API + widget) stays alive past cutover so [[inrisk]] continues pulling its static list. **Party tagging in Phase 1 / feature tagging out** boundary recorded. **Static-list hypothesis** from [[suzanna-whitefield]]: feature tagging may now be a static list (no new options in 2 years); if so, future migration becomes an InRisk-classifications-style change rather than a Postgres-table handover. Captured under [[open-questions#OQ-019]].

**New pages** (4):

- [[ntt]] — new platform under `wiki/entities/platforms/`. Vendor sanctions-check application accessed via API; first-class entity now that it sits at the edge of an active workstream (Phase 2+ sanctions-domain rework).
- [[sanctions-processing]] — new concept under `wiki/concepts/`. The sanctions-screening domain; describes the current Boomi-orchestrated call-chain (Party events → Boomi → InRisk one-submission-at-a-time → NTT), pain points (cascading false positives, no batch endpoint on InRisk, opaque cache + idempotency in Boomi, audit pressure this year), and the room's consensus that the orchestration should live in its own domain.
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — the source page (above).
- [[inrisk-cuts-over-before-high-volume]] — the new ADR (above).

**New open questions** (5):

- [[open-questions#OQ-032]] (scope/architecture) — sanctions-domain location. Out of Phase 1; audit pressure this year; escalation [[rory-beattie]] · [[suzanna-whitefield]] → [[andrea-read]].
- [[open-questions#OQ-035]] (timeline) — concrete InRisk MDM-cutover date (≥ 2 weeks before 1 Sep).
- [[open-questions#OQ-036]] (design open call) — widget-response field alignment ([[sergiu-postolachi]]'s question: will Joe's new widget return everything InRisk needs given OpenSearch ≠ Dynamo indexing).
- [[open-questions#OQ-033]] (people identification) — Anna (UX / product lead).
- [[open-questions#OQ-034]] (people identification) — Marty (sanctions / InRisk-adjacent).

**Resolved**: [[open-questions#OQ-005]] (Chakra V2 vs V3) → two component libraries from Joe; closed by [[inrisk-cuts-over-before-high-volume]].

**Advanced** (not resolved): [[open-questions#OQ-008]] sharpened with the "wrong place" framing to pair with [[open-questions#OQ-032]]; [[open-questions#OQ-019]] now has a concrete branch (Suzy's static-list investigation) and a softer migration shape (InRisk-classifications-style).

**Applications updated**:

- [[inrisk]] — restructured around the concrete **Party MDM Integration epic** (5-story table); party-tagging-vs-feature-tagging Phase-1 boundary; SDK-style widget integration; sanctions touchpoint flagged; new "Cutover sequencing" sub-section; pending-items list extended to OQ-032, OQ-035, OQ-036.
- [[inrisk-architecture]] — `state` and constraint detail added (datastore = relational; existing client + broker tables to gain party-ID + version-ID columns; SDK-style widget integration on Joe's design-system-agnostic library; no design-system surface in InRisk today; Boomi single-submission-API-call pain point; sanctions touchpoint to [[ntt]] via [[sanctions-processing]]).
- [[party-application]] — sources 2 → 3; AWS estate clarified as existing 3-account/4-env (not AWS 2.0); two component-library widget surface called out in Target state; sanctions framing expanded with the "wrong place" consensus; OQ-005 marked resolved inline; OQ-032/035/036 added to Pending; [[inrisk-cuts-over-before-high-volume]] added to Related decisions; [[feature-tagging-moves-to-inrisk]] refinement note.
- [[party-application-architecture]] — AWS estate (existing 3-account/4-env); Lambdas confirmed; two component libraries documented under Technologies & services; sanctions/spine-rewrite link added under Known constraints; new related decisions linked.

**Phase + summary updated**:

- [[party-rearch-phase-1]] — outer gate / inner gate framing introduced; InRisk row in Scope-Applications table raised from "medium" to "high" intensity with story-level detail; new **Cutover sequence** diagram; new **Out of Phase 1** sub-section (feature tagging unchanged at cutover; sanctions/NTT/Boomi orchestration; Anna's UX team involvement); done-state extended to include InRisk-first cutover and old-feature-tagging-backend-alive-past-cutover lines; OQ-035/036 added to gating questions; sources 2 → 3.
- [[party-rearch-phase-1-summary]] — mirror updates: outer/inner gate framing; InRisk row updated to the concrete epic; new "Flagged but out of Phase 1" line for sanctions; new ADR listed under Phase-1 decisions; OQ-035/036 added to highest-leverage; follow-ups list extended; Confidence section updated.

**Project overview updated**:

- [[party-rearch]] — sources 2 → 3. Pillars restructured: new pillar #3 ("InRisk cuts over before HV") slotted in; pillar #5 split out as "Party tagging in, feature tagging out". Tensions revised: Chakra tension removed (resolved); new tension on mid-August inner-gate; new tension on sanctions-domain location. What I'd want to know next re-ranked: OQ-035 added at #3; OQ-032/008 added at #6; OQ-005 dropped. Resolved clarifications section appended with OQ-005.

**Standing analyses refreshed**:

- [[party-rearch-ownership-matrix]] — InRisk workstream description updated to reflect concrete epic; new **Sanctions-domain ownership** workstream added; Widget/Chakra workstream marked resolved; 8 new actions appended (#21–28) covering Thursday/Tuesday cadence, widget-mechanic brush-up, Billy's Thursday attendance, Joe's second library, Suzy's static-list investigation, sanctions escalation, Anna warming, widget-response field alignment; action #11 marked resolved; total open actions now **27**; source-history entry added.
- [[party-rearch-dependency-map]] — current-state diagram extended with Boomi → InRisk-API → NTT call-chain; Phase-1 target diagram updated to reflect InRisk-first cutover, two-component-libraries widget split, sanctions-orchestration-unchanged-in-Phase-1, additive client+broker data-model, cutover-window dual-write; Key Phase-1 changes list rewritten; cross-cutting rows added for cutover sequencing, Joe's second library, widget-response field alignment, sanctions-domain ownership; Chakra row marked resolved; feature-tagging row note updated; source-history entry added.

**Index regenerated**: counts updated (3 sources · 9 decisions · 4 concepts · 2 platforms · 33 open questions · 17 resolved); [[ntt]] added under Platforms; [[sanctions-processing]] added under Concepts; [[inrisk-cuts-over-before-high-volume]] added under Decisions; 2026-05-13 source listed; existing per-application and per-architecture bullets refreshed with the new framing.

**Notes / things not done per user steers**:

- "Ringo special" terminology — **explicitly not referenced anywhere in the wiki** per user direction.
- **CDM** — origin context for feature tagging touched only obliquely; **not given its own wiki coverage** (user direction: unrelated initiative).
- **Anna** and **Marty** — kept as OQ entries ([[open-questions#OQ-033]], [[open-questions#OQ-034]]) rather than entity stubs; insufficient surname/team detail for first-class entity pages.

**Pages with breaking-change risk** (none, all changes are additive or refinements):

- Existing wiki-links unchanged; no path rewrites required.
- ADR file added; ADR file modified (additions only); no superseded markers needed yet.

- touched (new, 4): [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]], [[ntt]], [[sanctions-processing]], [[inrisk-cuts-over-before-high-volume]].
- touched (edited, 14): [[strangle-the-graph-via-proxy-events]], [[feature-tagging-moves-to-inrisk]], [[inrisk]], [[inrisk-architecture]], [[party-application]], [[party-application-architecture]], [[party-rearch-phase-1]], [[party-rearch-phase-1-summary]], [[party-rearch-ownership-matrix]], [[party-rearch-dependency-map]], [[party-rearch]], [[open-questions]], [[index]], [[log]] (this entry).

## [2026-05-26] ingest | 20260514 InRisk High Level Refinement

- **Source**: `raw/20260514 - InmRisk High Level Refinement.md` — Thursday-after-the-2026-05-13-follow-up. 27-minute Party-MDM portion of the wider InRisk High Level call where [[john-trahearn]] walked the 5-story Party MDM Integration epic through the wider InRisk team. First substantive appearance of [[prebind-team]]'s BA cohort + second PO in the wiki.
- **Attendees**: [[john-trahearn]], [[joe-worsfold]], [[billy-calladine]], [[kris-mokrzycki]], [[jason-owen]] (new), [[andrew-turner]] (new; resolves OQ-030), [[katarina-voskarova]] (new), [[rastislav-sepelak]] (new). Amardeep Badhan opened the call with one MC-style line; role/team unconfirmed; not stubbed (per user steer — insufficient signal). [[alex-sillars]] referenced but not in room.
- **Source page created**: [[sources/20260514-inrisk-high-level-refinement]] — keyed by 7 thematic claim clusters (epic ordering, parity-not-enhancement widget, spike-required, feature-tagging carve-out reinforced, cutover sequencing reinforced, InRisk-side backfill question, TOBA + Lloyd's-broker context), decisions made (none new — refinements only), 6 actions, applications/entities/concepts mentioned, contradictions/reinforces, LLM notes, open-questions section.

**No new ADRs.** This source is concretisation + refinement of decisions already on the books, plus four new people and one new "pending decision" strand.

**Refinements**:

- **[[inrisk-cuts-over-before-high-volume]]** — added the **parity-not-enhancement principle** for the InRisk widget itself (refinement of part B). The two-libraries call was a styling-stack decision; the 2026-05-14 refinement extends it into a full posture on the InRisk widget surface: same auth / RBAC / session, same filter parameters (including **TOBA status** for the Lloyd's-vs-retail broker split), same look-and-feel; UX improvements deferred. Joe rolled back his "modern and different" vision in front of the wider audience. Open-risks section extended with the TOBA-parity requirement (surfaced by [[jason-owen]]) and the widget-integration spike (pair Joe + Billy + Alex + Andrew Turner) gating Stories 3/4/5.
- **[[feature-tagging-moves-to-inrisk]]** — reinforced by re-statement to the wider audience; new "Reinforcement (2026-05-14 wider InRisk HL)" section quotes John's restated carve-out. No content change.
- **[[inrisk]]** — Phase-1 epic restructured into the new ordered + gated table; **Stories 1 & 2 ready for low level immediately** (manual-client-creation cleanup as the foundational "demon"; data-model migration on **three tables** — party + broker + party_snapshot — with `party_id` UUID v7 + `version_id` int); **Stories 3/4/5 spike-gated** (widget-integration spike with Joe + Billy + Alex + Andrew Turner). Drop-in-replacement / parity-not-enhancement principle captured. Renewals flow folds into the client story. Pending / unknown section gains the **InRisk-side backfill** note (per user steer, not formalised as an OQ).
- **[[inrisk-architecture]]** — datastore detail updated: three independent tables (party_table keyed by legacy `client_id`, broker_table, party_snapshot) get UUID v7 + int columns. Widget-integration section rewritten to reflect query-parameter-driven current flow vs SDK-style new flow + TOBA-status filter parity. Manual-create flow change captured.
- **[[party-application]]** + **[[party-application-architecture]]** — widget-surface description updated with the parity-not-enhancement framing on the InRisk-side library; Pending / unknown section on [[party-application]] gains the InRisk-side backfill note (sanctions-impact called out).
- **[[party-rearch-phase-1]]** + **[[party-rearch-phase-1-summary]]** — scope-applications table InRisk row rewritten to reflect Stories 1 & 2 ready + 3/4/5 gated + drop-in-replacement posture. Out-of-Phase-1 + new **Pending Phase-1 decisions** section on the phase page captures the backfill question. Summary's Follow-ups list: 2026-05-14 follow-up marked done; new follow-ups added for spike completion + backfill decision.
- **[[party-rearch-dependency-map]]** — Phase-1 diagram InRisk arrow refined (3 tables; UUID v7 + int types; manual-create flow change; drop-in-replacement posture). Key Phase-1 changes list updated. Three new cross-cutting rows: widget-integration spike (gates Stories 3/4/5); TOBA-status filter parity; InRisk-side backfill decision.
- **[[party-rearch-ownership-matrix]]** — InRisk workstream description updated. Action #22 (widget-mechanic brush-up) marked **resolved 2026-05-14** (current flow confirmed query-parameter-driven). Actions #21 + #23 marked **partial** (Thursday HL done; Stories 1 & 2 only at Tuesday low-level; Billy attended). **Three new actions** added (#29 widget-integration spike; #30 InRisk-side backfill policy; #31 TOBA-filter parity confirmation). Total open: 27 − 1 + 3 = **29**. Ownership-gaps refreshed: Andrew Tennant resolved → Andrew Turner; Anna / Marty / Amardeep notes added.
- **[[party-rearch]]** — Pillar #4 ("InRisk does the minimum for Phase 1") extended with the ordering + gating + parity-not-enhancement framing. Source count 3 → 4.

**New person stubs (4) — all on [[prebind-team]]**:

- [[andrew-turner]] — Product Owner. **Resolves [[open-questions#OQ-030]]** ("Andrew Tennant"): per user, transcription drift; same person. The 2026-05-14 line "MR Andrew" is also him. Aliases on the page: `andrew-tennant`, `mr-andrew`.
- [[jason-owen]] — Business Analyst. Only one of the four to make substantive MDM contributions (surfaced TOBA-status filter parity requirement; manual-record provenance question).
- [[katarina-voskarova]] ("Kati") — Business Analyst. Peripheral on this source (Ajax-related workstream).
- [[rastislav-sepelak]] ("Rasto") — Business Analyst. Peripheral on this source (Syndicate workstream).

**Team page updated**: [[prebind-team]] — members table extended (2 POs, 2 TLs, 3 BAs known); claim added that division of PO remit between [[daria-romanovskaia]] and [[andrew-turner]] is not formally documented; source-history bumped.

**Open-questions register**:

- **[[open-questions#OQ-030]] resolved** → [[andrew-turner]] (PO, [[prebind-team]]); "Andrew Tennant" was a Pass-A transcription drift; "MR Andrew" (2026-05-14) is the same person.
- **No new OQs** per user steer. The InRisk-side backfill question is tracked as a pending note on the affected pages, not as an OQ.
- Source-history entry appended.
- Open count: 33 − 1 = **32**; resolved count: 17 + 1 = **18**.

**Index regenerated**: counts updated (4 sources · 24 people · prebind-team members refreshed · 32 open OQs / 18 resolved); new source listed under Sources; OQ-030 resolution noted on [[andrew-turner]]'s person entry; InRisk row updated with the new framing; status-line date moved to 2026-05-26.

**Notes / things flagged but not done**:

- **Amardeep Badhan** — single MC-style line, role/team unconfirmed. Mentioned by name on the source page only; no entity page; no OQ. Will revisit if surfaces again.
- **TOBA (Terms of Business Agreement)** — captured on the source page and as parity-requirement context across [[inrisk]] / [[inrisk-architecture]] / [[inrisk-cuts-over-before-high-volume]] / [[party-rearch-dependency-map]], but **not given its own concept page**. Single-source mention; flag for a concept page when it appears again. Also fixed the "October status" transcription error per user direction — the raw transcript reads "October status" but the substance is TOBA.
- **InRisk-side backfill** — captured as a Pending Phase-1 decision on [[party-rearch-phase-1]] + Pending/unknown on [[inrisk]] + [[party-application]] + ownership-matrix action #30 + dependency-map cross-cutting row. **Not** an OQ per user steer.
- **"MR Andrew"** transcription drift documented as an alias on [[andrew-turner]] and on the source page only; not added to the open-questions resolved table (resolution is OQ-030 = Andrew Tennant).

**Pages with breaking-change risk**: none — additive or refinement only; no wiki-link path rewrites required.

**Incidental lint-fixes uncovered by the OQ-030 resolution**:

- [[sources/20260422-meeting-transcript-session-1]] — two lines updated to remove "Andrew Tennant" from the "still pending" list and add him to the resolved list pointing at [[andrew-turner]].
- [[eclipse]] — pre-existing bug discovered (independent of today's ingest): four references to `OQ-030` should have been `OQ-001` (the Eclipse retirement scope question; OQ-030 is the Andrew Tennant identification question). The bug pre-dated this ingest but was newly exposed because following `OQ-030` from [[eclipse]] would have landed readers on the resolved Andrew-Turner entry rather than the Eclipse scope conversation. All four refs corrected; one of them also extended with the OQ-004 pairing per the open-questions register.

- touched (new, 5): [[sources/20260514-inrisk-high-level-refinement]], [[jason-owen]], [[andrew-turner]], [[katarina-voskarova]], [[rastislav-sepelak]].
- touched (edited, 15): [[prebind-team]], [[open-questions]], [[inrisk]], [[inrisk-architecture]], [[inrisk-cuts-over-before-high-volume]], [[feature-tagging-moves-to-inrisk]], [[party-rearch-phase-1]], [[party-rearch-phase-1-summary]], [[party-rearch-ownership-matrix]], [[party-rearch-dependency-map]], [[party-rearch]], [[party-application]], [[party-application-architecture]], [[sources/20260422-meeting-transcript-session-1]], [[eclipse]], [[index]], [[log]] (this entry).

## [2026-05-26] note | Amardeep Badhan promoted to a person entity (correction to today's ingest)

Corrects the "Notes / things flagged but not done" bullet on the 2026-05-14 InRisk HL Refinement ingest entry above (line _"Amardeep Badhan — single MC-style line, role/team unconfirmed…"_). User-declared: [[amardeep-badhan]] is **Scrum Master on [[prebind-team]]**.

- **New entity page**: [[amardeep-badhan]] — stub, source_count: 1, role recorded as PreBind Scrum Master and counterpart to [[sergiu-postolachi]] ([[graph-team]] SM) for the cross-team Party-MDM-Integration cadence.
- [[prebind-team]] — members table extended to 8 (added SM row); claims + sources updated to reflect the addition.
- [[sources/20260514-inrisk-high-level-refinement]] — author list extended; attendees line promotes Amardeep from "facilitator unconfirmed" to "PreBind Scrum Master, chaired the call"; transcription-artefacts bullet removed; "Entities mentioned" extended; "My notes" updated to "Five new PreBind people (BA × 3, PO × 1, SM × 1)" with a sentence on the SM-counterpart framing.
- [[index]] — totals bumped (35 → 36 entities; 24 → 25 people); [[prebind-team]] summary updated to "8 known members (2 POs, 2 TLs, 1 SM, 3 BAs)"; new People row inserted between [[kris-mokrzycki]] and [[jason-owen]].
- **No OQ activity.** No decisions revised. No architecture pages touched.

- touched (new, 1): [[amardeep-badhan]].
- touched (edited, 4): [[prebind-team]], [[sources/20260514-inrisk-high-level-refinement]], [[index]], [[log]] (this entry).

## [2026-05-26] schema | Added §5.5 Person departure workflow + `departed:` / `succeeded_by:` person frontmatter + Past members team sub-table

User-triggered by the [[andrew-turner]] → [[daria-romanovskaia]] PreBind-PO handover. Captured as a generic schema convention so future departures follow the same rules.

**AGENTS.md edits**:

- **§4 frontmatter spec** — added optional `departed: YYYY-MM-DD` and `succeeded_by: <person-slug>` fields for person pages.
- **§4 page-type-specific sections** — extended the Person bullet to flag the new fields and the required `Departure` section; extended the Team bullet to add the optional `Past members` sub-table.
- **§5.5 Person departure (handover to a successor)** — new workflow. Eight numbered steps codifying the historical-record-stays-verbatim / forward-looking-references-redirect rule. Covers entity-page frontmatter, successor claim, team `Past members` move, project / decision / app redirects, source-page inline footnotes, peripheral Related-list repoints, index annotation, and the log-append rule. Explicitly notes that future ingests inherit the convention automatically when a `departed:` field is set.

**Why surface-level vs deep schema change**: the convention is additive — no existing pages were invalidated, no required fields changed, no folder layout changed. Departing people are an inevitable wiki event and the rule needed to exist before the first departure rather than be hand-rolled per incident.

- touched (edited, 1): `AGENTS.md`.

## [2026-05-26] note | Applied PreBind-PO handover: [[andrew-turner]] → [[daria-romanovskaia]]

First exercise of the §5.5 departure convention. User-declared (no source page): [[daria-romanovskaia]] took over from [[andrew-turner]] as PreBind PO on 2026-05-26; Andrew leaves the organisation on 2026-05-29. All forward-looking references redirected to Daria; historical source-page references preserved verbatim with a small inline footnote.

**Person & team pages**:

- [[andrew-turner]] — frontmatter `departed: 2026-05-29` + `succeeded_by: daria-romanovskaia` added; new `## Departure` section right after Summary; Summary restated to past-tense PO; Open-questions section closed (the "Division of PO remit" question is resolved by the handover). Historical claims, aliases, and source references retained verbatim.
- [[daria-romanovskaia]] — Summary restated to "sole Product Owner from 2026-05-26"; new Claim citing the user-declared handover; `## Sources` extended with [[sources/20260514-inrisk-high-level-refinement]] under a "not present at the call; inherits forward-looking PO responsibilities" framing; source_count 1 → 2; Open-questions section refreshed (added spike-scheduling question).
- [[prebind-team]] — Members table split: active members (7 rows) + new `## Past members` sub-table (1 row: Andrew, departed 2026-05-29, succeeded by Daria). "Division of PO remit not formally documented" line removed (resolved). Claims refreshed with handover detail. Related list moved Andrew to a past-member line. Sources updated with a user-declared 2026-05-26 entry.

**Forward-looking redirects** (Andrew → Daria, with first-mention attribution to Andrew preserved):

- [[party-rearch-ownership-matrix]] — InRisk workstream Primary-owner cell; action #29 (widget-integration spike pairing). Ownership-gaps section refreshed. Source history appended with the 2026-05-26 user-declared entry.
- [[party-rearch-dependency-map]] — Phase-1 narrative bullet on the InRisk 5-story epic; Widget-integration spike cross-cutting row.
- [[party-rearch-phase-1]] — InRisk row in `## Scope — applications` (spike pairing list).
- [[inrisk]] — opening paragraph of `### Phase-1 epic and stories` (spike pairing); Widget-integration spike paragraph.
- [[inrisk-cuts-over-before-high-volume]] — Open-risks paragraph on the widget-integration spike; Sources line for the 2026-05-14 source.

**Historical source-page footnotes** (preserve original text; append inline redirect):

- [[sources/20260514-inrisk-high-level-refinement]] — attendees line, on first Andrew mention.
- [[sources/20260422-meeting-transcript-session-1]] — pending-identification line where Andrew Tennant resolved.

**Peripheral Related-lists repointed** (nav aids; Andrew → Daria):

- [[amardeep-badhan]] — Relationships paragraph + Related line.
- [[jason-owen]] · [[katarina-voskarova]] · [[rastislav-sepelak]] — Related lines.

**Index**:

- [[index]] — status-line annotated with the handover + AGENTS §5.5 addition. [[prebind-team]] summary line restated to 7 active + 1 past member. [[daria-romanovskaia]] row updated to sole PO (src 1→2). [[andrew-turner]] row annotated as departed.

**Open-questions register**:

- No new OQs. The "Division of PO remit" question (carried implicitly on [[andrew-turner]]) is resolved by the handover, not by source evidence — no formal OQ-NNN was ever opened for it (it was a wiki-internal note), so no Resolved-table row added.

**Deliberately not touched**:

- Older claim "PO on [[prebind-team]] — user-declared (2026-05-26)" on [[andrew-turner]] stayed verbatim. The departure is additive context.
- `wiki/sources/20260514-inrisk-high-level-refinement.md` action-table row (line 83) listing Andrew Turner on the spike pairing — kept as the historical record of who was named at the time; the redirect is captured at the attendee-line footnote and on the project-scoped pages.
- All `## Claims` and `## Sources` sections on Andrew Turner's peers — those are historical evidence, not forward-looking nav.

- touched (new, 0).
- touched (edited, 15): `AGENTS.md` (separate `schema` entry above), [[andrew-turner]], [[daria-romanovskaia]], [[prebind-team]], [[party-rearch-ownership-matrix]], [[party-rearch-dependency-map]], [[party-rearch-phase-1]], [[inrisk]], [[inrisk-cuts-over-before-high-volume]], [[sources/20260514-inrisk-high-level-refinement]], [[sources/20260422-meeting-transcript-session-1]], [[amardeep-badhan]], [[jason-owen]], [[katarina-voskarova]], [[rastislav-sepelak]], [[open-questions]] (OQ-030 row annotated with the redirect note), [[index]], [[log]] (this entry).

## [2026-05-26] ingest | 20260519 Party integration timelines (Convex × Artificial onboarding prep call)

- **Source**: `raw/20260519 - Party integration timelines.md` — 27-minute three-way prep call held the day before a Convex × [[artificial]] kick-off (2026-05-20). First substantive surfacing in the wiki of **[[artificial]] as a third-party vendor platform underpinning [[high-volume]]**, and the **[[boomi]]** integration broker that gateways the relationship.
- **Attendees**: [[rory-beattie]] · [[simon-hulbert]] (Architect on HV/Artificial integration project) · [[alex-sillars]] (PO, [[graph-team]]) + **Convex – Orega – 6B Boardroom** speaker (HV-side PM coordinator, unidentified — see [[open-questions#OQ-037]]). Named-but-not-present: [[joe-worsfold]] (referenced as YAML-spec author), [[srini]] (Boomi Integration Architect, new entity), [[luca]] (HV-side scheduler, new entity), **Sam** (FDA for integrations — advances [[open-questions#OQ-024]]).
- **Source page created**: [[sources/20260519-party-integration-timelines]] — themed key-claims (Artificial as consumer · Boomi as gateway · MDM build-status from Alex · strangler / sequencing restated to a customer · coordination shape); 6 actions; applications + entities + concepts mentioned; 4 new open-questions.

**No new ADRs.** This source is **concretisation + coordination + reframing**, not a fresh architectural decision. Operational notes (Boomi-gateway pattern, API-spec instability caveat, fortnightly cadence + Slack channel) captured on the relevant pages.

**Two new platform entities** (per user steers):

- [[artificial]] — **full platform entity** (`wiki/entities/platforms/artificial.md`). Convex's mid-implementation third-party platform underpinning [[high-volume]]; new MDM consumer for Phase 1 via [[boomi]]. Records the two-stage integration (read path priority + change-event push later), the Convex × Artificial coordination shape, the strangler-pattern stability promise as restated to Artificial, and the InRisk-first sequencing as restated to Artificial.
- [[boomi]] — **promoted from prose to a first-class platform entity** (`wiki/entities/platforms/boomi.md`). Convex's vendor integration broker. Hosts two distinct integration patterns: sanctions orchestration today (wrong-place, see [[sanctions-processing]] + [[open-questions#OQ-032]]) and the Artificial → Party MDM gateway from Phase-1 cutover. Convex-side owning team is TBD.

**Two new person entity stubs** (per user steer "all three"):

- [[srini]] — Boomi Integration Architect on the HV/Artificial integration project. Pairs with [[joe-worsfold]] on the Party-side setup. Counterpart to Sam (FDA for integrations) and [[simon-hulbert]] (Architect) in what looks like a shared integration / FDA function. Surname TBD ([[open-questions#OQ-040]]).
- [[luca]] — HV-side coordinator named as the scheduler for the fortnightly Convex × Artificial catch-ups. Possibly the same person as the in-room Boardroom-PM ([[open-questions#OQ-037]]). Surname + formal role TBD ([[open-questions#OQ-038]]).
- **Convex – Orega – 6B Boardroom speaker** — NOT stubbed per user steer (insufficient signal); tracked at [[open-questions#OQ-037]].

**Deep reframing of [[high-volume]]** (per user steer "deep reframe"):

- [[high-volume]] (identity page) rewritten so the lead framing is _"HV is Convex's implementation of the [[artificial]] vendor platform for high-throughput insurance-deal flow"_ — replacing the prior _"greenfield in-house high-throughput consumer"_ framing. New sections: "Vendor platform underpinning HV" (Artificial + Boomi as named platforms); coordination shape (fortnightly cadence, Slack channel, contacts roster); two-stage Party integration. Expanded Pending list with the new identification OQs.
- [[high-volume-architecture]] reframed alongside — clarifies HV-as-product is largely the Artificial vendor platform's tech stack (out of scope for this wiki), while the Convex-side integration surface is what actually lives here. Boomi flagged as the unblocking dependency. API-spec instability + Artificial's faster-than-Convex cadence captured as known constraints. Awaits [[simon-hulbert]]'s diagrams to populate properly.

**Lighter refinements**:

- [[party-application]] + [[party-application-architecture]] — [[artificial]] added as a second new Phase-1 downstream consumer alongside [[high-volume]] (both via [[boomi]]). New **API-spec instability caveat** captured in Pending / Known-constraints sections (operational, not an OQ). Backend-completeness call-out added (curation extension, Neo4j→Dynamo migration, DU/sanctions event-emission completeness — restated from Alex's snapshot in the source).
- [[strangle-the-graph-via-proxy-events]] — new **Refinement (2026-05-19)** section: the strangler-pattern stability promise was restated **to a third-party consumer** for the first time (verbatim quote preserved); records the "joined-up InRisk + Party data" carve-out Alex made; flags the corollary that the promise has now been stated externally and is harder to soften.
- [[inrisk-cuts-over-before-high-volume]] — new **Reinforcement (2026-05-19)** clause: sequencing restated to [[artificial]] / Simon-side directly. No change to the ADR; recorded because this is the first communication of the sequence to a vendor counterparty.
- [[sanctions-processing]] — all `Boomi` mentions linkified to `[[boomi]]`; **note added** explaining that Boomi now has a first-class platform page and that the sanctions orchestration described here is one of two distinct patterns hosted on Boomi (the other being Artificial gateway).
- [[simon-hulbert]] — broader role framing (Architect on the HV/Artificial integration project, not just "HV-team-internal"). 5 new claims from this source.
- [[high-volume-team]] — members table extended (Srini, Luca added); Sam now annotated as FDA for integrations; new claims + new workstreams (Boomi connectivity, Convex × Artificial cadence, architecture diagrams) added.

**Project-level updates**:

- [[party-rearch]] — Pillar #6 ("HV is API-only via Boomi") extended to clarify HV ≡ Convex's implementation of Artificial; **Known applications in scope** section gains a new "External / vendor platforms consuming Party in Phase 1" sub-list ([[artificial]] + [[ntt]] both via [[boomi]]). Tension #2 reframed around Artificial-side integration design. Source count 4 → 5.
- [[party-rearch-phase-1]] — [[high-volume]] row in Scope-Applications table updated to capture the Artificial reframing, the YAML spec / kick-off context, and Boomi connectivity as the immediate unblock. **Pending Phase-1 decisions** section extended with the API-spec instability communication protocol (operational, not an OQ).
- [[party-rearch-phase-1-summary]] — same row updated; new "External / vendor platforms in scope" sub-section added; Follow-ups extended with the Artificial kick-off + Boomi connectivity completion as new revisit triggers.

**Standing analyses refreshed**:

- [[party-rearch-dependency-map]] — Phase-1 diagram extended: HV arrow re-labelled to clarify HV ≡ Convex-side delivery of [[artificial]]; **new Artificial arrow** added for the read path + change-event push back (both via [[boomi]] gateway). [[boomi]] given consistent wiki-link treatment across all diagrams. Key Phase-1 changes list updated. **5 new cross-cutting rows**: Artificial integration shape; Boomi connectivity for Artificial (the immediate unblock); Convex × Artificial fortnightly cadence + Slack channel; HV-side architecture diagrams; InRisk-cutover sequencing now-restated-to-Artificial. End-state arrow updated.
- [[party-rearch-ownership-matrix]] — **HV Party integration** workstream reframed; **two new workstreams added** (Artificial vendor onboarding; Boomi connectivity for Artificial). **6 new actions** (#32–#37) covering 2026-05-20 kick-off, Boomi setup, fortnightly cadence, Slack channel, architecture diagrams, MDM front-of-house build-out. Total open actions: 29 + 6 = **35**. Ownership-gaps section refreshed: Sam advanced; Luca + Srini surname + Boardroom-PM added; Amardeep marked resolved (per the 2026-05-26 follow-up entry).

**Open-questions register**:

- **OQ-024 advanced** — Sam confirmed as _"FDA for integrations"_; surname + line manager still TBD; pairs with new OQ-039.
- **4 new OQs**:
  - **OQ-037** (people identification) — Convex – Orega – 6B Boardroom speaker identity; possibly [[luca]].
  - **OQ-038** (people identification) — [[luca]]'s surname + formal role.
  - **OQ-039** (scope/architecture) — what does "FDA" stand for? Likely Functional / Field Delivery Architect.
  - **OQ-040** (people identification) — [[srini]]'s surname.
- Open count: 32 + 4 = **36 open**; resolved count unchanged at **18**.

**Index regenerated**: counts updated (5 sources · 40 entities — 9 teams + 4 platforms + 27 people · 36 open OQs); two new platform entries ([[artificial]], [[boomi]]); two new People entries ([[srini]], [[luca]]); [[simon-hulbert]] row enriched (src 1 → 2); new source row; status line updated. [[high-volume]] application row reframed; [[party-application]] row gains the Artificial-as-consumer note.

**Things flagged but not done per user steers**:

- **Boardroom-PM** — not stubbed as an entity; tracked at [[open-questions#OQ-037]] only. May be [[luca]].
- **Artificial PM cohort** — referenced obliquely in the source (_"the project manager guys that I work with"_); no names captured; no stubs. Will surface at the 2026-05-20 kick-off.
- **No new ADRs** — the source is concretisation + coordination; the candidate _"Boomi is the integration gateway between Party and third-party vendors"_ ADR is captured as the canonical role on the new [[boomi]] platform page rather than a separate ADR (it's a recording of operational reality, not a fresh decision).
- **Older sources retain "Boomi" as inline prose** — only newly-touched sections were linkified to [[boomi]]; this respects the append-only spirit of source pages while making the new platform page discoverable from the current-state and Phase-1 dependency-map diagrams.

**Pages with breaking-change risk**: none — additive or refinement only; no wiki-link path rewrites required.

- touched (new, 5): [[sources/20260519-party-integration-timelines]], [[artificial]] (platform), [[boomi]] (platform), [[srini]] (person), [[luca]] (person).
- touched (edited, 15): [[high-volume]], [[high-volume-architecture]], [[high-volume-team]], [[simon-hulbert]], [[party-application]], [[party-application-architecture]], [[strangle-the-graph-via-proxy-events]], [[inrisk-cuts-over-before-high-volume]], [[sanctions-processing]], [[party-rearch]], [[party-rearch-phase-1]], [[party-rearch-phase-1-summary]], [[party-rearch-dependency-map]], [[party-rearch-ownership-matrix]], [[open-questions]], [[index]], [[log]] (this entry).
