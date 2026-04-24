# Index

_Last updated: 2026-04-22 (lint pass, post Session 1 Pass B)_
_Pages: 2 sources · 6 applications · 30 entities (9 teams + 1 platform + 20 people) · 3 concepts · 8 decisions · 3 analyses (standing: dependency-map + ownership-matrix + open-questions) · 1 overview_
_Status: post-lint-pass — Knowledge Graph reclassified as internal Neo4j datastore of [[party-application]]; new [[data-universe]] / [[eclipse]] / [[roles-vs-views-litmus-test]] / [[intercept-backfill-projection]] pages; new people [[daria-romanovskaia]] / [[steve-perry]] / [[chris-woodward]]; OQ-010 and OQ-031 resolved._

> The LLM reads this first on every query. Entries = link + one-line summary + optional metadata (`src: N` = number of sources, `owner: …`, date, etc.).

## Overview
- [[overview]] — programme thesis, pillars, tensions, open questions _(evolving, src: 2)_

## Applications
- [[party-application]] — core Party store + routing; re-architecture subject; Neo4j Knowledge Graph → Dynamo+OpenSearch (owner: [[graph-team]], src: 2)
- [[party-curation-tool]] — PCT; Next.js UI for party curation; co-ships with MDM (owner: [[graph-team]], user: [[dataops-team]], src: 2)
- [[inrisk]] — IR2; Policy Admin System; interim-state consumer with 3 required changes (owner: [[prebind-team]], src: 2)
- [[inrisk-engine]] — InRisk Core; API-first rewrite targeting **final-state** Party contract; out of interim scope (owner: [[devx-team]], src: 2)
- [[high-volume]] — HV; API-via-Boomi consumer; production 1 Sep (forcing-function for Phase 1) (owner: [[high-volume-team]], src: 1)
- [[eclipse]] — external data feed consumed by [[inrisk]]; stub; scope call on ongoing ingestion into [[party-application]] tracked at [[open-questions#OQ-001]] (src: 1, new in lint pass)

## Entities

### Teams / working units
- [[graph-team]] — owns [[party-application]] and [[party-curation-tool]]; aka "The Party Team"; ~13 people (src: 2)
- [[prebind-team]] — owns [[inrisk]]; aka "The Strikers" (src: 2)
- [[devx-team]] — owns [[inrisk-engine]] (src: 2)
- [[dataops-team]] — primary users of [[party-curation-tool]]; sponsored by [[data-quality-team]] (src: 1)
- [[tech-tooling]] — portfolio-level oversight; [[andrea-read]] heads engineering, [[will-bone]] heads product (Interim), [[rory-beattie]] programme oversight (src: 2)
- [[analytics-team]] — owns [[data-universe]] (Snowflake-backed analytics platform); schema-impact contact for flattening decision (aka "Data Universe Team") (src: 2)
- [[architecture-team]] — cross-portfolio architectural review; home of [[scott-gruber]] (broker SME) and [[suzanna-whitefield]] (src: 2)
- [[data-quality-team]] — business sponsors / owners of [[dataops-team]]; headed by [[hugh-lobban]] (src: 1)
- [[high-volume-team]] — owns [[high-volume]]; main Party-integration contact is [[simon-hulbert]] (src: 1)

### Platforms
- [[data-universe]] — analytics data platform (Snowflake at its core); owned by [[analytics-team]]; primary downstream consumer of [[party-application]] events (src: 1, new in lint pass)

### People
- [[alex-sillars]] — PO, [[graph-team]] (src: 2)
- [[joe-worsfold]] — Tech Lead / Architect, [[graph-team]]; strangler-pattern originator (src: 2)
- [[sergiu-postolachi]] — Scrum Master, [[graph-team]] (src: 2)
- [[ben-joseph]] — Engineer / Developer, [[graph-team]] (src: 1)
- [[billy-calladine]] — Engineer / Developer, [[graph-team]]; spine-rewrite-insight owner (src: 2)
- [[tomas-sivo]] — Business Analyst, [[graph-team]]; owns PCT functional requirements (src: 1)
- [[andrea-read]] — Head of Technology Engineering, [[tech-tooling]] (src: 1)
- [[will-bone]] — Interim Head of Product, [[tech-tooling]] (src: 1)
- [[rory-beattie]] — programme oversight, [[tech-tooling]]; this wiki's human curator (src: 2)
- [[suzanna-whitefield]] — Architect, [[architecture-team]] ("Suzy") (src: 2)
- [[scott-gruber]] — Architect, [[architecture-team]]; broker-retrieval SME (src: 1)
- [[antonie-labuschagne]] — Tech Lead, [[devx-team]] ("Anton"); explicitly scoped out of Phase-1 design per Session 1 (src: 2)
- [[daria-romanovskaia]] — Product Owner, [[prebind-team]] (src: 1, new in lint pass)
- [[john-trahearn]] — Tech Lead, [[prebind-team]]; prototyped the InRisk party-ID reference table (src: 2)
- [[kris-mokrzycki]] — Tech Lead, [[prebind-team]] (phonetic "Chris"); co-prototyped with John (src: 2)
- [[hugh-lobban]] — Head of Data Quality, [[data-quality-team]]; PCT-rollout sponsor figure (src: 1)
- [[paul-rogers]] — Data & Process Analyst, [[analytics-team]]; flattening-decision counterpart (src: 1)
- [[steve-perry]] — Programme Manager, [[data-universe]] (src: 1, new in lint pass)
- [[chris-woodward]] — Architect, [[data-universe]] (src: 1, new in lint pass)
- [[simon-hulbert]] — Architect, [[high-volume-team]]; main Party-integration contact (src: 1)

## Concepts
- [[contract-buckets]] — three-bucket (operational · data distribution · enrichment) scaffolding introduced by [[alex-sillars]] in Session 1; the lens the whole programme reads every Party-consumer contract through (src: 1)
- [[roles-vs-views-litmus-test]] — Joe Worsfold's one-question test for deciding whether an attribute belongs on the party (role) or on a consumer projection (view) (src: 1, new in lint pass)
- [[intercept-backfill-projection]] — the three-phase delivery shape that implements [[strangle-the-graph-via-proxy-events]]; named in Session 2 (src: 1, new in lint pass)

_Further candidates for future concept pages: **proxy-event strangler** (standalone page), **version-stamped sanction checks**, **single-feature-flag cutover**, **line-in-the-sand migration principle**, party versioning (ID + version / timestamp), client-ID flattening, One Re / CUO grouping, D&B enrichment, S&P ingestion, feature-tagging, display-ID vs system-ID._

## Decisions
- [[strangle-the-graph-via-proxy-events]] — **accepted**; MDM emits Graph-shape proxy events to Data Universe; no dual-running. **Origin: Session 1** (morning DU-events segment); consolidated in Session 2 (src: 2)
- [[pct-and-mdm-go-live-together]] — **accepted**; new PCT and MDM ship as one release; single-flag coupled rollout with the search widget (src: 2)
- [[d-and-b-caching-and-auto-parent]] — **accepted**; D&B cached in Dynamo with TTL; auto-create draft + ultimate-parent with provenance; scheduled refresh → revision loop (Session 1 addition) (src: 2)
- [[uuid-system-id-with-display-id]] — **accepted**; system ID = UUID, display ID = legacy Graph party ID (src: 1)
- [[no-historic-client-backfill-into-mdm]] — **accepted** (new in Session 1 Pass A); MDM history starts at cutover; pre-cutover InRisk client-ID-scoped history stays in [[inrisk]] / [[data-universe]] (src: 1)
- [[no-pct-audit-backfill]] — **accepted** (new in Session 1 Pass B); new PCT doesn't carry Jira audit history; Snowflake is the cross-period audit/SLA store; mirror of the MDM-side decision (src: 1)
- [[feature-tagging-moves-to-inrisk]] — **accepted** (new in Session 1 Pass B); Phase 1 no change; Phase 2+ migrate Postgres table + widget to [[prebind-team]] / [[inrisk]] ownership (src: 1)
- [[bulk-migrations-owned-by-mdm-phase-1]] — **accepted** (new in Session 1 Pass B); [[graph-team]]-owned CLI in Phase 1; full self-serve UX is Phase 2+ (src: 1)

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — 1st architecture-design session (morning); three-bucket contract framework; strangler-pattern origin; Eclipse retirement; feature-tagging long-term ownership; bulk-migration CLI scope; MDM-squad-shape tension; no-historic-backfill decision (2026-04-22)
- [[sources/20260422-meeting-transcript-session-2]] — 2nd architecture-design session (afternoon); strangle pattern, PCT co-delivery, InRisk interim-state changes, D&B + UUID decisions (2026-04-22)

## Analyses
### Standing (auto-maintained)
- [[dependency-map]] — inter-application dependencies; current vs Phase 1 target vs end state; contract-buckets scaffolding (src: 2)
- [[ownership-matrix]] — workstreams × owners; **20 open actions** (src: 2)
- [[open-questions]] — standing register of unresolved questions; OQ-NNN IDs; referenced from application/decision/entity pages (**29 open**, 16 resolved as of lint pass) (src: 2)

---

## How to read this file
- **Applications** — one page per software system, current / target / transitional states tracked separately.
- **Entities** — teams, people, orgs, vendors, platforms. Teams and people are under `wiki/entities/`; platforms sit alongside as the `type: entity` "platform" sub-class.
- **Concepts** — abstract ideas, frameworks, vocabulary.
- **Decisions** — ADR-style records of architectural decisions.
- **Sources** — one page per ingested raw source (meeting note, spec, diagram, etc.). Raw-file references in source pages use an inline code-span (e.g. `` `raw/<filename>.md` ``) rather than a wiki link, to keep the raw layer out of the graph view.
- **Analyses** — cross-cutting synthesis. Standing analyses (prefixed above) are kept current on every ingest.
- **Open questions** — [[open-questions]] is the single source of truth; individual pages reference `OQ-NNN` IDs rather than restating questions.
