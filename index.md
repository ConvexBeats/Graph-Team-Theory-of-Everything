# Index

_Last updated: 2026-05-18 (2026-05-13 InRisk follow-up ingested)_
_Pages: 1 portfolio-overview · 2 projects · 3 phases · 4 project-scoped analyses · 4 application-architectures (current-state) · 1 global register · 3 sources · 6 applications · 31 entities (9 teams + 2 platforms + 20 people) · 4 concepts · 9 decisions_
_Status: wiki now organised around **projects**. [[party-rearch]] is the primary in-flight project (Sessions 1/2 + 2026-05-13 follow-up). [[secrets-management]] stubbed as the second project — ASG-mandated secrets-management standardisation post Shai-Hulud 2.0. `[[overview]]` is the portfolio view. Architecture detail lives on dedicated `*-architecture` pages alongside identity pages; see "Application architecture" below._

> The LLM reads this first on every query. Entries = link + one-line summary + optional metadata (`src: N` = number of sources, `owner: …`, date, etc.).

## Portfolio
- [[overview]] — **portfolio overview** (projects in flight, cross-project themes, how to read the wiki) _(src: 0 — derived)_

## Projects

### party-rearch — Party Application re-architecture (in-flight)
- [[party-rearch]] — project thesis: payload-snapshot → versioned-reference; outer gate 1-Sep HV; **inner gate** mid-August InRisk cutover (added 2026-05-13) (src: 3)
- **Phases**:
  - [[party-rearch-phase-1]] — MDM + PCT co-ship + InRisk-first cutover to mid-August, HV at 1 Sep; single-flag coupled rollout; no audit / no historic-client backfill (in-flight) (src: 3)
  - [[party-rearch-phase-2]] — post-cutover end-state: direct Dynamo→DU stream, feature-tagging hand-over, end-state sanctions, sanctions-domain rework, InRisk-Engine final contract (planned stub)
- **Project-scoped analyses**:
  - [[party-rearch-dependency-map]] — inter-application dependencies; current vs Phase-1 target vs end-state; contract-buckets scaffolding; sanctions/Boomi/NTT current-state added 2026-05-13 (src: 3)
  - [[party-rearch-ownership-matrix]] — workstreams × owners; **27 open actions** (src: 3)
  - [[party-rearch-phase-1-summary]] — focused Phase-1 digest: what it is, applications affected, decisions, delivery shape, top-leverage open questions (filed from a query, 2026-04-22; updated 2026-05-18) (src: 3)

### secrets-management — Standardise Secrets Management (planned, stub)
- [[secrets-management]] — ASG-mandated transition following Shai-Hulud 2.0 (INC-140574): eliminate long-lived CI/CD credentials; AWS Secrets Manager unified schema; automated rotation (stub, src: 0)
- **Phases**:
  - [[secrets-management-phase-1]] — scope, timeline, and decisions TBD at first ingest (planned stub)

## Applications _(cross-project)_
- [[party-application]] — core Party store + routing; re-architecture subject; Neo4j Knowledge Graph → Dynamo+OpenSearch; AWS estate confirmed existing 3-account/4-env (owner: [[graph-team]], projects: [party-rearch], src: 3)
- [[party-curation-tool]] — PCT; Next.js UI for party curation; co-ships with MDM; consumes Chakra-3 + design-system widget (owner: [[graph-team]], user: [[dataops-team]], projects: [party-rearch], src: 2)
- [[inrisk]] — IR2; Policy Admin System; **Party MDM Integration epic of 4–5 stories** (sized 2026-05-13); cutover ≥ 2 weeks before HV (owner: [[prebind-team]], projects: [party-rearch], src: 3)
- [[inrisk-engine]] — InRisk Core; API-first rewrite targeting **final-state** Party contract; out of Phase-1 interim scope (owner: [[devx-team]], projects: [party-rearch], src: 2)
- [[high-volume]] — HV; API-via-Boomi consumer; production 1 Sep (forcing-function for Phase 1); turns on after InRisk-first cutover window (owner: [[high-volume-team]], projects: [party-rearch], src: 1)
- [[eclipse]] — external data feed consumed by [[inrisk]]; stub; scope call on ongoing ingestion tracked at [[open-questions#OQ-001]] (projects: [party-rearch], src: 1)

## Application architecture _(current-state; cross-project)_

_Living reference pages; folded-into on phase completion per AGENTS.md §5.4. Phase-target architectures live under `wiki/projects/<project>/<phase-id>-<app>-architecture.md`._

- [[party-application-architecture]] — stub; Neo4j Knowledge Graph primary datastore; payload-event contract to downstream consumers; AWS estate (existing 3-account/4-env) confirmed; two component-library widget split (baseline for [[party-rearch]] targets) (src: 3)
- [[party-curation-tool-architecture]] — stub; Next.js on Jira workflow; D&B integration; Chakra-3 + design-system widget (src: 2)
- [[inrisk-architecture]] — stub; integration-surface focus (payload-consumer today); client + broker tables to gain party-ID + version-ID columns; SDK-style widget integration with design-system-agnostic library; sanctions touchpoint via Boomi → [[ntt]] (src: 3)
- [[high-volume-architecture]] — lean stub; greenfield build; no production state yet; consumes MDM at 1 Sep after InRisk-first cutover window (src: 1)

## Entities _(cross-project)_

### Teams / working units
- [[graph-team]] — owns [[party-application]] and [[party-curation-tool]]; aka "The Party Team"; ~13 people (src: 2)
- [[prebind-team]] — owns [[inrisk]]; aka "The Strikers" (src: 2)
- [[devx-team]] — owns [[inrisk-engine]] (src: 2)
- [[dataops-team]] — primary users of [[party-curation-tool]]; sponsored by [[data-quality-team]] (src: 1)
- [[tech-tooling]] — portfolio-level oversight; [[andrea-read]] heads engineering, [[will-bone]] heads product (interim), [[rory-beattie]] programme oversight (src: 2)
- [[analytics-team]] — owns [[data-universe]] (Snowflake-backed analytics platform); schema-impact contact for flattening decision (aka "Data Universe Team") (src: 2)
- [[architecture-team]] — cross-portfolio architectural review; home of [[scott-gruber]] (broker SME) and [[suzanna-whitefield]] (src: 2)
- [[data-quality-team]] — business sponsors / owners of [[dataops-team]]; headed by [[hugh-lobban]] (src: 1)
- [[high-volume-team]] — owns [[high-volume]]; main Party-integration contact is [[simon-hulbert]] (src: 1)

### Platforms
- [[data-universe]] — analytics data platform (Snowflake at its core); owned by [[analytics-team]]; primary downstream consumer of [[party-application]] events (src: 1)
- [[ntt]] — vendor sanctions-check application accessed via API; owning team inside Convex TBD ([[open-questions#OQ-032]]); sits at the edge of the [[sanctions-processing]] flow (src: 1)

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
- [[antonie-labuschagne]] — Tech Lead, [[devx-team]] ("Anton"); explicitly scoped out of Phase-1 design (src: 2)
- [[daria-romanovskaia]] — Product Owner, [[prebind-team]] (src: 1)
- [[john-trahearn]] — Tech Lead, [[prebind-team]]; prototyped the InRisk party-ID reference table (src: 2)
- [[kris-mokrzycki]] — Tech Lead, [[prebind-team]] (phonetic "Chris"); co-prototyped with John (src: 2)
- [[hugh-lobban]] — Head of Data Quality, [[data-quality-team]]; PCT-rollout sponsor figure (src: 1)
- [[paul-rogers]] — Data & Process Analyst, [[analytics-team]]; flattening-decision counterpart (src: 1)
- [[steve-perry]] — Programme Manager, [[data-universe]] (src: 1)
- [[chris-woodward]] — Architect, [[data-universe]] (src: 1)
- [[simon-hulbert]] — Architect, [[high-volume-team]]; main Party-integration contact (src: 1)

## Concepts _(cross-project)_
- [[contract-buckets]] — three-bucket (operational · data distribution · enrichment) scaffolding introduced by [[alex-sillars]] in Session 1 (src: 1)
- [[roles-vs-views-litmus-test]] — Joe Worsfold's one-question test for deciding whether an attribute belongs on the party (role) or on a consumer projection (view) (src: 1)
- [[intercept-backfill-projection]] — the three-phase delivery shape that implements [[strangle-the-graph-via-proxy-events]] (src: 1)
- [[sanctions-processing]] — sanctions screening domain (the gating compliance check before doing business with a counterparty); [[ntt]] is the underlying API; Boomi today orchestrates and is in the wrong place by group consensus; audit pressure this year (src: 1)

_Candidates for future concept pages: proxy-event strangler (standalone), version-stamped sanction checks, single-feature-flag cutover, line-in-the-sand migration principle, party versioning, client-ID flattening, One Re / CUO grouping, D&B enrichment, S&P ingestion, feature-tagging, display-ID vs system-ID._

## Decisions _(cross-project folder; each carries `project:` + `phase:` frontmatter)_
_All current ADRs belong to `party-rearch`._
- [[strangle-the-graph-via-proxy-events]] — **accepted** · phase-1 · MDM emits Graph-shape proxy events to DU; no dual sources of truth; cutover-window dual-write to old graph for revertability (refined 2026-05-13) (src: 3)
- [[pct-and-mdm-go-live-together]] — **accepted** · phase-1 · new PCT + MDM as one release; single-flag coupled rollout (src: 2)
- [[inrisk-cuts-over-before-high-volume]] — **accepted** · phase-1 · InRisk cuts over to MDM ≥ 2 weeks before 1 Sep; HV switches on at the gate; two component libraries (Chakra-3 + design-system for PCT, design-system-agnostic for InRisk); resolves OQ-005 (src: 1, new 2026-05-13)
- [[d-and-b-caching-and-auto-parent]] — **accepted** · phase-1 · D&B cached in Dynamo; auto-create draft + ultimate-parent; scheduled refresh → revision loop (src: 2)
- [[uuid-system-id-with-display-id]] — **accepted** · phase-1 · system ID = UUID, display ID = legacy Graph party ID (src: 1)
- [[no-historic-client-backfill-into-mdm]] — **accepted** · phase-1 · MDM history starts at cutover (src: 1)
- [[no-pct-audit-backfill]] — **accepted** · phase-1 · new PCT doesn't carry Jira audit history; Snowflake is the cross-period store (src: 1)
- [[feature-tagging-moves-to-inrisk]] — **accepted** · phase-2 · Phase-1 no change (old backend stays alive past cutover); Phase-2+ migrate ownership to InRisk; static-list hypothesis under investigation (refined 2026-05-13) (src: 2)
- [[bulk-migrations-owned-by-mdm-phase-1]] — **accepted** · phase-1 · [[graph-team]]-owned CLI; self-serve UX is Phase 2+ (src: 1)

## Sources _(cross-project; each carries `project:` frontmatter)_
- [[sources/20260422-meeting-transcript-session-1]] — party-rearch · 1st architecture-design session (morning); three-bucket framework; strangler origin; Eclipse retirement scope call; feature-tagging long-term ownership; bulk-migration CLI scope; MDM-squad-shape tension; no-historic-backfill decision (2026-04-22)
- [[sources/20260422-meeting-transcript-session-2]] — party-rearch · 2nd architecture-design session (afternoon); strangle pattern, PCT co-delivery, InRisk interim-state changes, D&B + UUID decisions (2026-04-22)
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — party-rearch · 3rd architecture-design session (follow-up); concrete InRisk Phase-1 epic (4–5 stories); party-tagging vs feature-tagging boundary; InRisk-first cutover sequencing and two-component-libraries call (resolves OQ-005); sanctions/Boomi/NTT surfaced; AWS estate confirmed (2026-05-13)

## Global standing register
- [[open-questions]] — open-questions register across all projects; OQ-NNN IDs are global; filter by `Project` column (**33 open**, 17 resolved; all currently scoped `party-rearch`) (src: 3)

## Portfolio-level analyses
- _None yet — `wiki/analyses/` reserved for cross-project synthesis._

---

## How to read this file
- **Portfolio** — [[overview]] is the cross-project view. Start there if you don't know which project the question is about.
- **Projects** — each project has its own folder under `wiki/projects/<slug>/` containing the project overview and cross-phase standing analyses (dependency map, ownership matrix). Inside that folder, **each phase has its own subfolder `phase-N/`** containing the phase overview, phase-target architecture pages, and any phase-scoped analyses (summaries, digests, retros). Filenames still carry the full `<slug>-phase-N-…` prefix even though the folder repeats it — basename uniqueness is what keeps wiki links unambiguous.
- **Applications** — every in-scope app has its own folder at `wiki/applications/<app-slug>/` containing the **identity** page (`<app-slug>.md` — owner, role, cross-project change items), the **current-state architecture** page (`<app-slug>-architecture.md` — tech, touchpoints, constraints), and any richer per-app material (API contracts, data models, runbooks, diagrams). Wiki links use basenames (`[[party-application]]`, `[[party-application-architecture]]`) — the folder structure doesn't affect how pages are linked. Multi-project applications carry `projects: [...]` frontmatter.
- **Architecture** — current-state architectures are cross-project and per-app; phase-target architectures are project-scoped (one per (project, phase, app) where meaningful change occurs) and delta from the current-state baseline. When a phase ships, its target folds into the current-state page per AGENTS.md §5.4.
- **Entities / Concepts** — cross-project. One page each.
- **Decisions** — cross-project folder; each ADR carries `project:` + `phase:` frontmatter identifying its scope.
- **Sources** — cross-project folder; each source carries a `project:` frontmatter field.
- **Open questions** — [[open-questions]] is the single source of truth; individual pages reference `OQ-NNN` IDs and never restate questions. Filter the register by `Project` column to get project-specific views.
- **Raw-file references** use inline code-spans (e.g. `` `raw/<filename>.md` ``), not wiki links — keeps the raw layer out of the graph view.
