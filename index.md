# Index

_Last updated: 2026-05-26 (2026-05-19 MDM Implementation Strategy ingested; **Knowledge Graph correction** — [[knowledge-graph]] promoted to a first-class sibling Graph-team application after the 2026-04-22 lint-pass conclusion that KG was internal to [[party-application]] was contradicted; OQ-010 re-opened as OQ-010-R; 4 new OQs filed; [[billy-calladine]] formalised on widget rebuild + InRisk-side dispatch; PCT + MDM current-state implementation snapshot folded in; [[rory-beattie]] role posture softened to "programme manager paying closer attention")_
_Pages: 1 portfolio-overview · 2 projects · 3 phases · 4 project-scoped analyses · 4 application-architectures (current-state) · 1 global register · 6 sources · 7 applications · 40 entities (9 teams + 4 platforms + 27 people) · 4 concepts · 9 decisions_
_Status: wiki now organised around **projects**. [[party-rearch]] is the primary in-flight project (Sessions 1/2 + 2026-05-13 follow-up + 2026-05-14 InRisk HL refinement + 2026-05-19 Party integration timelines + 2026-05-19 MDM Implementation Strategy). [[secrets-management]] stubbed as the second project — ASG-mandated secrets-management standardisation post Shai-Hulud 2.0. `[[overview]]` is the portfolio view. Architecture detail lives on dedicated `*-architecture` pages alongside identity pages; see "Application architecture" below._

> The LLM reads this first on every query. Entries = link + one-line summary + optional metadata (`src: N` = number of sources, `owner: …`, date, etc.).

## Portfolio
- [[overview]] — **portfolio overview** (projects in flight, cross-project themes, how to read the wiki) _(src: 0 — derived)_

## Projects

### party-rearch — Party Application re-architecture (in-flight)
- [[party-rearch]] — project thesis: payload-snapshot → versioned-reference; outer gate 1-Sep HV; **inner gate** mid-August InRisk cutover (added 2026-05-13); HV ≡ Convex's implementation of [[artificial]] (clarified 2026-05-19); _"a lot further along than I suspected"_ sentiment captured 2026-05-19 (src: 6)
- **Phases**:
  - [[party-rearch-phase-1]] — MDM + PCT co-ship + InRisk-first cutover to mid-August, HV at 1 Sep; single-flag coupled rollout; no audit / no historic-client backfill; InRisk epic ordered+gated 2026-05-14; [[artificial]] (HV-side) as second new Phase-1 consumer via [[boomi]] gateway (added 2026-05-19); MDM implementation snapshot (monorepo + DDD + auto-gen REST API + revision-versioning + full audit stack; widget = single component with three parameterised variants, raw React + close-to-raw CSS, owned by [[billy-calladine]]; proxy-event-with-InRisk-data design fork gates the proxy adapter — [[open-questions#OQ-041]]) added 2026-05-19-pm (in-flight) (src: 6)
  - [[party-rearch-phase-2]] — post-cutover end-state: direct Dynamo→DU stream, feature-tagging hand-over, end-state sanctions, sanctions-domain rework, InRisk-Engine final contract (planned stub)
- **Project-scoped analyses**:
  - [[party-rearch-dependency-map]] — inter-application dependencies; current vs Phase-1 target vs end-state; contract-buckets scaffolding; sanctions/Boomi/NTT current-state added 2026-05-13; widget-integration spike + TOBA-filter + backfill rows added 2026-05-14; Artificial arrow + Boomi-platform-link + 5 new Artificial-onboarding cross-cutting rows added 2026-05-19; **Knowledge Graph correction** + 5 new cross-cutting rows (proxy-event design fork, integration-env unblock, Dynamo migration tooling, PCT new-UI parity, InRisk-side dispatch-code) added 2026-05-19-pm (src: 6)
  - [[party-rearch-ownership-matrix]] — workstreams × owners; **43 open actions** (src: 6)
  - [[party-rearch-phase-1-summary]] — focused Phase-1 digest: what it is, applications affected, decisions, delivery shape, top-leverage open questions (filed from a query, 2026-04-22; updated 2026-05-26 with the Artificial reframing + MDM implementation snapshot + Knowledge Graph correction) (src: 6)

### secrets-management — Standardise Secrets Management (planned, stub)
- [[secrets-management]] — ASG-mandated transition following Shai-Hulud 2.0 (INC-140574): eliminate long-lived CI/CD credentials; AWS Secrets Manager unified schema; automated rotation (stub, src: 0)
- **Phases**:
  - [[secrets-management-phase-1]] — scope, timeline, and decisions TBD at first ingest (planned stub)

## Applications _(cross-project)_
- [[party-application]] — core Party store + routing; re-architecture subject; **internal Neo4j** (production today) → Dynamo+OpenSearch (new MDM); AWS estate confirmed existing 3-account/4-env; InRisk-widget parity posture confirmed 2026-05-14; [[artificial]] added as second new Phase-1 consumer via [[boomi]] gateway 2026-05-19; **MDM implementation state captured 2026-05-19-pm** (monorepo + DDD + auto-gen REST API + revision-versioning + full audit stack) (owner: [[graph-team]], projects: [party-rearch], src: 6)
- [[party-curation-tool]] — PCT; Next.js UI for party curation; co-ships with MDM; consumes Chakra-3 + design-system widget; **new-PCT UI shape captured 2026-05-19-pm** (revisions + filters; mark-for-review / approve / request-changes workflow; dataops parity TBD — [[open-questions#OQ-044]]) (owner: [[graph-team]], user: [[dataops-team]], projects: [party-rearch], src: 3)
- [[inrisk]] — IR2; Policy Admin System; **Party MDM Integration epic, 5 stories ordered + gated 2026-05-14** (Stories 1 & 2 ready for low level; 3/4/5 spike-gated); cutover ≥ 2 weeks before HV; **widget = single component with three parameterised variants in shell container, raw React + close-to-raw CSS** (Tailwind correction 2026-05-19-pm); **InRisk-side dispatch code change** owned by [[billy-calladine]] (owner: [[prebind-team]], projects: [party-rearch], src: 5)
- [[inrisk-engine]] — InRisk Core; API-first rewrite targeting **final-state** Party contract; out of Phase-1 interim scope (owner: [[devx-team]], projects: [party-rearch], src: 2)
- [[high-volume]] — HV; **Convex's implementation of the [[artificial]] vendor platform** (reframed 2026-05-19); API-via-[[boomi]] consumer; production 1 Sep (forcing-function for Phase 1); turns on after InRisk-first cutover window (owner: [[high-volume-team]], projects: [party-rearch], src: 2)
- [[knowledge-graph]] — **KG**; Graph-team-owned graph DB; **read-replica of [[inrisk]] + [[party-application]] data**; powers [[inrisk]] search functionality (clarified 2026-05-26 — _"One of the Graph Databases that the Graph team own"_); **sibling** to [[party-application]] (not internal to it); Option B in the proxy-event-with-InRisk-data design fork ([[open-questions#OQ-041]]) (owner: [[graph-team]], projects: [party-rearch] for proxy-event-fork only, src: 1)
- [[eclipse]] — external data feed consumed by [[inrisk]]; stub; scope call on ongoing ingestion tracked at [[open-questions#OQ-001]] (projects: [party-rearch], src: 1)

## Application architecture _(current-state; cross-project)_

_Living reference pages; folded-into on phase completion per AGENTS.md §5.4. Phase-target architectures live under `wiki/projects/<project>/<phase-id>-<app>-architecture.md`._

- [[party-application-architecture]] — **draft** (was stub; promoted 2026-05-19-pm with the MDM implementation state); **internal Neo4j** today; new MDM = monorepo + DDD; Dynamo primary store; auto-generated REST API (Scala→Swagger→TS); OpenSearch for search; revision-versioning + access-log audit + DynamoDB streams + point-in-time-recovery; widget = single component with three parameterised variants, raw React + close-to-raw CSS, owned by [[billy-calladine]]; **proxy-event adapter** with design fork on InRisk-data lookup ([[open-questions#OQ-041]]); KG explicitly clarified as **separate** sibling app (baseline for [[party-rearch]] targets) (src: 6)
- [[party-curation-tool-architecture]] — **draft** (was stub; promoted 2026-05-19-pm with new PCT UI shape); Next.js on Jira workflow today; new PCT = revisions + filters table replacing tabs; mark-for-review / approve / request-changes workflow; functional-gap-analysis tickets authored by [[tomas-sivo]]; dataops parity TBD ([[open-questions#OQ-044]]) (src: 3)
- [[inrisk-architecture]] — stub; integration-surface focus (payload-consumer today); **party + broker + party_snapshot tables** to gain `party_id` (UUID v7) + `version_id` (int) columns; SDK-style widget integration on design-system-agnostic library; current widget filters by **TOBA status** (parity requirement); sanctions touchpoint via Boomi → [[ntt]] (src: 4)
- [[high-volume-architecture]] — lean stub; HV-as-product is largely [[artificial]] vendor platform (reframed 2026-05-19); Convex-side integration surface is the [[boomi]] gateway to Party + small InRisk touchpoint; consumes MDM at 1 Sep after InRisk-first cutover window (src: 2)

## Entities _(cross-project)_

### Teams / working units
- [[graph-team]] — owns [[party-application]], [[party-curation-tool]], **and [[knowledge-graph]]** (KG added 2026-05-26 with the correction); aka "The Party Team"; ~13 people (src: 3)
- [[prebind-team]] — owns [[inrisk]]; aka "The Strikers"; 7 active members (1 PO, 2 TLs, 1 SM, 3 BAs) + 1 past member ([[andrew-turner]], departed 2026-05-29 → [[daria-romanovskaia]]) (src: 2)
- [[devx-team]] — owns [[inrisk-engine]] (src: 2)
- [[dataops-team]] — primary users of [[party-curation-tool]]; sponsored by [[data-quality-team]] (src: 1)
- [[tech-tooling]] — portfolio-level oversight; [[andrea-read]] heads engineering, [[will-bone]] heads product (interim), [[rory-beattie]] programme oversight (src: 2)
- [[analytics-team]] — owns [[data-universe]] (Snowflake-backed analytics platform); schema-impact contact for flattening decision (aka "Data Universe Team") (src: 2)
- [[architecture-team]] — cross-portfolio architectural review; home of [[scott-gruber]] (broker SME) and [[suzanna-whitefield]] (src: 2)
- [[data-quality-team]] — business sponsors / owners of [[dataops-team]]; headed by [[hugh-lobban]] (src: 1)
- [[high-volume-team]] — owns [[high-volume]] (Convex's implementation of [[artificial]]); known members [[simon-hulbert]] (Architect), [[srini]] (Boomi Integration Architect), [[luca]] (coordinator); Sam = "FDA for integrations" (src: 2)

### Platforms
- [[data-universe]] — analytics data platform (Snowflake at its core); owned by [[analytics-team]]; primary downstream consumer of [[party-application]] events (src: 1)
- [[ntt]] — vendor sanctions-check application accessed via API; owning team inside Convex TBD ([[open-questions#OQ-032]]); sits at the edge of the [[sanctions-processing]] flow (src: 1)
- [[artificial]] — third-party vendor platform underpinning [[high-volume]] (Convex is mid-implementation); reads new Party MDM API via [[boomi]] from Phase-1 cutover; first substantive ingest 2026-05-19; kick-off 2026-05-20 (owner Convex-side: [[high-volume-team]] · [[simon-hulbert]], projects: [party-rearch], src: 1)
- [[boomi]] — vendor integration broker; promoted to first-class platform 2026-05-26 (was loose prose); hosts two distinct patterns — sanctions orchestration today (wrong-place, [[open-questions#OQ-032]]) + Artificial → Party MDM gateway from Phase-1 cutover; Convex-side owning team TBD (src: 2)

### People
- [[alex-sillars]] — PO, [[graph-team]]; aligned with UI/UX descope; surfaced InRisk-side dispatch-code need; wary of KG-consistency in proxy-event design fork (src: 3)
- [[joe-worsfold]] — Tech Lead / Architect, [[graph-team]]; strangler-pattern originator; **end-to-end MDM walkthrough giver 2026-05-19-pm**; owns data-model-proxying + legacy-migration epic with [[ben-joseph]]; clarified KG identity (src: 3)
- [[sergiu-postolachi]] — Scrum Master, [[graph-team]] (src: 2)
- [[ben-joseph]] — Engineer / Developer, [[graph-team]]; paired with Joe on backfill + proxy work (src: 2)
- [[billy-calladine]] — Engineer / Developer, [[graph-team]]; spine-rewrite-insight owner; **formalised owner of the widget rebuild + InRisk-side dispatch-code change 2026-05-19-pm** (src: 3)
- [[tomas-sivo]] — Business Analyst, [[graph-team]]; owns PCT functional requirements; **formalised author of PCT curation gap-analysis tickets 2026-05-19-pm** (src: 2)
- [[andrea-read]] — Head of Technology Engineering, [[tech-tooling]] (src: 1)
- [[will-bone]] — Interim Head of Product, [[tech-tooling]] (src: 1)
- [[rory-beattie]] — programme oversight, [[tech-tooling]]; this wiki's human curator; **paying closer attention to help move project to completion** (2026-05-19-pm); making live descope calls (audit, history-import, transaction-history, broker-form-history) (src: 3)
- [[suzanna-whitefield]] — Architect, [[architecture-team]] ("Suzy") (src: 2)
- [[scott-gruber]] — Architect, [[architecture-team]]; broker-retrieval SME (src: 1)
- [[antonie-labuschagne]] — Tech Lead, [[devx-team]] ("Anton"); explicitly scoped out of Phase-1 design (src: 2)
- [[daria-romanovskaia]] — **sole** Product Owner, [[prebind-team]] from 2026-05-26 (took over from [[andrew-turner]]) (src: 2)
- [[andrew-turner]] — Product Owner, [[prebind-team]] _(departed 2026-05-29 → [[daria-romanovskaia]]; resolves [[open-questions#OQ-030]] Andrew Tennant + "MR Andrew" transcription drift)_ (src: 1)
- [[john-trahearn]] — Tech Lead, [[prebind-team]]; prototyped the InRisk party-ID reference table (src: 2)
- [[kris-mokrzycki]] — Tech Lead, [[prebind-team]] (phonetic "Chris"); co-prototyped with John (src: 2)
- [[amardeep-badhan]] — Scrum Master, [[prebind-team]]; counterpart to [[sergiu-postolachi]] on [[graph-team]] (new 2026-05-26) (src: 1)
- [[jason-owen]] — Business Analyst, [[prebind-team]]; surfaced TOBA-status filter parity requirement (new 2026-05-26) (src: 1)
- [[katarina-voskarova]] — Business Analyst, [[prebind-team]] ("Kati") (new 2026-05-26) (src: 1)
- [[rastislav-sepelak]] — Business Analyst, [[prebind-team]] ("Rasto"); Syndicate workstream (new 2026-05-26) (src: 1)
- [[hugh-lobban]] — Head of Data Quality, [[data-quality-team]]; PCT-rollout sponsor figure (src: 1)
- [[paul-rogers]] — Data & Process Analyst, [[analytics-team]]; flattening-decision counterpart (src: 1)
- [[steve-perry]] — Programme Manager, [[data-universe]] (src: 1)
- [[chris-woodward]] — Architect, [[data-universe]] (src: 1)
- [[simon-hulbert]] — Architect on the HV/[[artificial]] integration project, [[high-volume-team]]; main Party-integration contact; role widened 2026-05-19 from HV-internal to Artificial-onboarding architect (src: 2)
- [[srini]] — Boomi Integration Architect on the HV/[[artificial]] integration project, [[high-volume-team]]; surname TBD ([[open-questions#OQ-040]]) (new 2026-05-26) (src: 1)
- [[luca]] — coordinator / scheduler on the HV/[[artificial]] integration project, [[high-volume-team]]; surname + formal role TBD ([[open-questions#OQ-038]]); possibly same as Boardroom-PM ([[open-questions#OQ-037]]) (new 2026-05-26) (src: 1)

## Concepts _(cross-project)_
- [[contract-buckets]] — three-bucket (operational · data distribution · enrichment) scaffolding introduced by [[alex-sillars]] in Session 1 (src: 1)
- [[roles-vs-views-litmus-test]] — Joe Worsfold's one-question test for deciding whether an attribute belongs on the party (role) or on a consumer projection (view) (src: 1)
- [[intercept-backfill-projection]] — the three-phase delivery shape that implements [[strangle-the-graph-via-proxy-events]] (src: 1)
- [[sanctions-processing]] — sanctions screening domain (the gating compliance check before doing business with a counterparty); [[ntt]] is the underlying API; Boomi today orchestrates and is in the wrong place by group consensus; audit pressure this year (src: 1)

_Candidates for future concept pages: proxy-event strangler (standalone), version-stamped sanction checks, single-feature-flag cutover, line-in-the-sand migration principle, party versioning, client-ID flattening, One Re / CUO grouping, D&B enrichment, S&P ingestion, feature-tagging, display-ID vs system-ID._

## Decisions _(cross-project folder; each carries `project:` + `phase:` frontmatter)_
_All current ADRs belong to `party-rearch`._
- [[strangle-the-graph-via-proxy-events]] — **accepted** · phase-1 · MDM emits Graph-shape proxy events to DU; no dual sources of truth; cutover-window dual-write to old graph for revertability (refined 2026-05-13); **three known consumers enumerated 2026-05-19-pm** (DU, HV-via-[[boomi]]-via-[[artificial]], [[boomi]]/sanctions); **InRisk-endpoint vs KG-pull design fork** for proxy events ([[open-questions#OQ-041]]) (src: 4)
- [[pct-and-mdm-go-live-together]] — **accepted** · phase-1 · new PCT + MDM as one release; single-flag coupled rollout (src: 2)
- [[inrisk-cuts-over-before-high-volume]] — **accepted** · phase-1 · InRisk cuts over to MDM ≥ 2 weeks before 1 Sep; HV switches on at the gate; two component libraries (Chakra-3 + design-system for PCT, design-system-agnostic for InRisk); parity-not-enhancement on the InRisk widget (refined 2026-05-14); **Tailwind correction + raw-React widget + three-variant shell + [[billy-calladine]] ownership of both widget and InRisk-side dispatch-code 2026-05-19-pm**; resolves OQ-005 (src: 3)
- [[d-and-b-caching-and-auto-parent]] — **accepted** · phase-1 · D&B cached in Dynamo; auto-create draft + ultimate-parent; scheduled refresh → revision loop (src: 2)
- [[uuid-system-id-with-display-id]] — **accepted** · phase-1 · system ID = UUID, display ID = legacy Graph party ID (src: 1)
- [[no-historic-client-backfill-into-mdm]] — **accepted** · phase-1 · MDM history starts at cutover (src: 1)
- [[no-pct-audit-backfill]] — **accepted** · phase-1 · new PCT doesn't carry Jira audit history; Snowflake is the cross-period store (src: 1)
- [[feature-tagging-moves-to-inrisk]] — **accepted** · phase-2 · Phase-1 no change (old backend stays alive past cutover); Phase-2+ migrate ownership to InRisk; static-list hypothesis under investigation (refined 2026-05-13; reinforced 2026-05-14) (src: 3)
- [[bulk-migrations-owned-by-mdm-phase-1]] — **accepted** · phase-1 · [[graph-team]]-owned CLI; self-serve UX is Phase 2+ (src: 1)

## Sources _(cross-project; each carries `project:` frontmatter)_
- [[sources/20260422-meeting-transcript-session-1]] — party-rearch · 1st architecture-design session (morning); three-bucket framework; strangler origin; Eclipse retirement scope call; feature-tagging long-term ownership; bulk-migration CLI scope; MDM-squad-shape tension; no-historic-backfill decision (2026-04-22)
- [[sources/20260422-meeting-transcript-session-2]] — party-rearch · 2nd architecture-design session (afternoon); strangle pattern, PCT co-delivery, InRisk interim-state changes, D&B + UUID decisions (2026-04-22)
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — party-rearch · 3rd architecture-design session (follow-up); concrete InRisk Phase-1 epic (4–5 stories); party-tagging vs feature-tagging boundary; InRisk-first cutover sequencing and two-component-libraries call (resolves OQ-005); sanctions/Boomi/NTT surfaced; AWS estate confirmed (2026-05-13)
- [[sources/20260514-inrisk-high-level-refinement]] — party-rearch · 4th architecture-design session (InRisk-side HL); epic ordered + gated (Stories 1 & 2 ready for low level; 3/4/5 gated on spike); 3rd data-model table added (party_snapshot); UUID-v7 + integer types confirmed; drop-in-replacement / parity-not-enhancement widget posture; TOBA-status filter as a parity requirement; InRisk-side backfill question raised; new PreBind members (Andrew Turner PO, Jason Owen BA, Kati Voskarova BA, Rasto Sepelak BA); resolves OQ-030 (Andrew Tennant = Andrew Turner) (2026-05-14)
- [[sources/20260519-party-integration-timelines]] — party-rearch · Convex × [[artificial]] kick-off prep call; HV reframed as Convex's implementation of Artificial vendor platform; [[boomi]] confirmed as gateway between Artificial and Party MDM; YAML API spec already shared with Artificial; new HV-side people surfaced ([[srini]] Boomi Integration Architect, [[luca]] coordinator, Sam confirmed as "FDA for integrations" — advances OQ-024); fortnightly Convex × Artificial cadence + Slack channel established; InRisk-first sequencing + strangler-pattern stability restated to a customer; no new ADRs (2026-05-19)
- [[sources/20260519-mdm-implementation-strategy]] — party-rearch · afternoon call · [[rory-beattie]]'s programme-management posture (paying closer attention; making live descope calls); [[joe-worsfold]]'s end-to-end MDM walkthrough (monorepo + DDD; Dynamo + OpenSearch; auto-gen REST API; backfill iterative; revision-versioning + access-logs + Dynamo streams + point-in-time-recovery; widget = three parameterised variants in shell, raw React + close-to-raw CSS, [[billy-calladine]] formalised; InRisk-side dispatch-code change also Billy's); three known proxy-event consumers enumerated (DU, HV-via-[[boomi]]-via-[[artificial]], [[boomi]]/sanctions); **InRisk-endpoint vs KG-pull design fork** (OQ-041); integration env JumpCloud SAML Cognito access blocking (OQ-042); Dynamo migration tooling unsolved (OQ-043); PCT new-UI shape (revisions + filters + mark-for-review workflow) + dataops parity TBD (OQ-044); **Knowledge Graph correction** — KG is a separate Graph-team-owned sibling read-replica DB powering [[inrisk]] search, not internal to [[party-application]]; promotes [[knowledge-graph]] to first-class app; re-opens OQ-010 as OQ-010-R (2026-05-19)

## Global standing register
- [[open-questions]] — open-questions register across all projects; OQ-NNN IDs are global; filter by `Project` column (**41 open**, 18 resolved with OQ-010 annotated as superseded by OQ-010-R; all currently scoped `party-rearch`; **OQ-010 re-opened as OQ-010-R** with the corrected KG identity, 4 new OQs filed 2026-05-19-pm: OQ-041 proxy-event-with-InRisk-data design fork, OQ-042 integration-env JumpCloud SAML Cognito access, OQ-043 Dynamo migration tooling scope, OQ-044 PCT new-UI dataops parity) (src: 6)

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
