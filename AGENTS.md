# AGENTS.md — LLM Project Wiki Schema

You are the sole maintainer of this project wiki. The human curates sources and asks questions; you do all the reading, writing, filing, and cross-referencing. Follow these rules on **every** interaction. No exceptions.

---

## 1. Prime directive

> **Compile once, keep current.** Never re-derive knowledge from raw sources on each query. Integrate new information into the persistent wiki so the synthesis compounds over time.

If an answer to a user's question is valuable enough to keep, **file it back into the wiki** (usually under `wiki/analyses/`). Explorations compound just like ingested sources.

---

## 2. Layers

| Layer | Path | Who writes it | Rule |
|---|---|---|---|
| Raw sources | `raw/` | Human only | Immutable. Read, never modify. |
| Wiki | `wiki/` | LLM only | You own it entirely. |
| Schema | `AGENTS.md` (this file) | Co-evolved | Update when conventions change. Propose edits; human approves. |
| Index | `index.md` | LLM | Content catalog. Update on every ingest. |
| Log | `log.md` | LLM | Append-only chronological record. |

---

## 3. Folder conventions

```
.
├── AGENTS.md                    # this schema
├── index.md                     # content catalog (read this first on queries)
├── log.md                       # chronological record (append-only)
├── raw/                         # immutable source documents
│   ├── assets/                  # images, PDFs, data files referenced by sources
│   └── <source-files>.md        # meeting notes, specs, diagrams, clippings
└── wiki/
    ├── overview.md              # portfolio-level view (lists projects, cross-project themes)
    ├── open-questions.md        # standing register of unresolved questions (OQ-NNN IDs) — global, with `Project` column
    ├── projects/                # one subfolder per project; everything project-scoped lives here
    │   └── <project-slug>/
    │       ├── <project-slug>.md                        # project overview / thesis
    │       ├── <project-slug>-dependency-map.md         # cross-phase standing analysis (project-scoped)
    │       ├── <project-slug>-ownership-matrix.md       # cross-phase standing analysis (project-scoped)
    │       └── phase-N/                                 # one subfolder per phase; everything phase-scoped lives here
    │           ├── <project-slug>-phase-N.md                            # phase overview (scope, gate, decisions)
    │           ├── <project-slug>-phase-N-<app-slug>-architecture.md    # end-of-phase target architecture per app (delta + full shape)
    │           └── <project-slug>-phase-N-<analysis-name>.md            # phase-scoped analyses (summaries, digests, etc.)
    ├── applications/            # one subfolder per application — cross-project
    │   └── <app-slug>/
    │       ├── <app-slug>.md                        # identity: purpose, owner, aliases, cross-project role
    │       ├── <app-slug>-architecture.md           # living current-state architecture (tech, touchpoints, constraints)
    │       └── <app-slug>-<aspect>.md               # optional richer per-app pages (api contracts, data model, runbook, diagrams/, etc.)
    ├── decisions/               # ADRs; each carries `project:` + `phase:` frontmatter — cross-project folder
    ├── entities/                # every first-class non-software actor or system — cross-project
    │   ├── people/              # every human (one page per person)
    │   ├── teams/               # every working unit (one page per team)
    │   └── platforms/           # platforms / vendor systems / external sources that aren't in-scope applications
    ├── concepts/                # abstract ideas, frameworks, themes — cross-project
    ├── sources/                 # one summary page per ingested source; each carries a `project:` field
    ├── analyses/                # query-derived + cross-project standing analyses (project-scoped analyses live under `projects/<slug>/`)
    └── _templates/              # page templates you copy from (person.md · team.md · platform.md · application.md · application-architecture.md · phase-application-architecture.md · decision.md · concept.md · source.md · analysis.md · project.md · phase.md)
```

### Filename rules
- Lowercase, kebab-case: `party-application.md`, `graph-team.md`.
- One page = one noun. Don't merge unrelated ideas onto one page.
- Source summary filenames mirror the raw filename: `raw/2026-04-22-party-arch-meeting.md` → `wiki/sources/2026-04-22-party-arch-meeting.md`.
- Use `[[wiki-links]]` (Obsidian-style) for cross-references. Prefer bare page name: `[[party-application]]`. Use folder-qualified form only to disambiguate when two pages share a slug.
- **Filenames must be globally unique** across all subfolders (entity subfolders `people/` / `teams/` / `platforms/`, per-application subfolders `applications/<app>/`, per-project subfolders `projects/<slug>/`, and per-phase subfolders `projects/<slug>/phase-N/`). Obsidian resolves `[[slug]]` by filename; collisions make links ambiguous. If a collision is unavoidable, disambiguate both filenames (e.g. `sam-smith.md` instead of `sam.md`).
- **Project-scoped files are prefixed with the project slug** so they're globally unique: `party-rearch.md`, `party-rearch-dependency-map.md`, `party-rearch-ownership-matrix.md`. Same for other projects (`secrets-management.md`, …).
- **Phase-scoped files are prefixed with the project slug _and_ the phase id**: `party-rearch-phase-1.md`, `party-rearch-phase-1-summary.md`, `party-rearch-phase-1-party-application-architecture.md`. The `phase-N/` folder repeats the prefix — this redundancy is intentional, ensuring basename uniqueness so wiki links remain unambiguous.
- Raw-file references from wiki pages use an **inline code-span with a relative path** (e.g. `` `raw/20260422 - Meeting Transcript.md` ``), not a wiki link. This keeps the raw layer out of Obsidian's graph view.

### Vocabulary (wiki-wide)
- **business party** — the insurance-domain entity (broker, client, underwriter, etc.) that the `party-application` manages. (Project-specific to party-rearch.)
- **application** — a piece of software (e.g. `party-application`, `inrisk`). **Cross-project**: one page in `wiki/applications/`, with a `projects:` frontmatter field listing every project that touches it.
- **working unit** (or **team**) — a delivery group (e.g. `graph-team`). **Cross-project**: lives under `wiki/entities/teams/` with `tags: [team]`.
- **person** — a named human. **Cross-project**: lives under `wiki/entities/people/` with `tags: [person]` and a `team:` frontmatter field pointing at their team slug.
- **platform** — a first-class system that isn't an in-scope application on any current project: analytics platforms (e.g. `data-universe`), vendor systems (e.g. D&B, S&P, NTT), shared services. **Cross-project**: lives under `wiki/entities/platforms/` with `tags: [platform]` and an `owner:` frontmatter field pointing at the owning team.
- **project** — a bounded body of work with its own thesis, driver, and phases. Examples: `party-rearch`, `secrets-management`. Lives under `wiki/projects/<slug>/` with `type: project`. Every project has an overview page, a dependency map, an ownership matrix, and one or more phase pages.
- **phase** — a delivery bucket inside a project, with its own gate/deadline and its own application-scope table. Example: `party-rearch-phase-1`. Lives under `wiki/projects/<slug>/` with `type: phase`, and carries `project:` + `phase_id:` frontmatter.

### Choosing an entities subfolder
When creating a new entity page, decide with this cascade:

1. Is it a named human? → `wiki/entities/people/`. Template: `_templates/person.md`.
2. Is it a delivery/working unit (has members, owns work)? → `wiki/entities/teams/`. Template: `_templates/team.md`.
3. Is it a system that isn't an in-scope application on any current project (data platform, vendor, external feed, shared service)? → `wiki/entities/platforms/`. Template: `_templates/platform.md`.
4. Is it a piece of software actively re-architected or affected by a current project? → **Not an entity** — it's an `application`, goes in `wiki/applications/`.

If a candidate fits none of these (e.g. a physical place, a regulatory body) and no existing subfolder makes sense, flag it in the ingest as a schema question rather than inventing a new subfolder silently.

### Choosing a project/phase location
When creating a new project- or phase-scoped page, decide with this cascade:

1. Is it the **overview / thesis for a whole project**? → `wiki/projects/<slug>/<slug>.md` (= addressable as `[[<slug>]]`). Template: `_templates/project.md`.
2. Is it a **phase overview page**? → `wiki/projects/<slug>/phase-N/<slug>-phase-N.md`. Template: `_templates/phase.md`. Every phase lives in its own subfolder `phase-N/` alongside its phase-target architectures and any phase-scoped analyses.
3. Is it a **project-scoped _cross-phase_ standing analysis** (dependency map, ownership matrix, risk register — anything reasoning across phases)? → `wiki/projects/<slug>/<slug>-<analysis-name>.md` (stays at the project root, outside any phase folder). Frontmatter: `type: analysis`, `project: <slug>`, no `phase:` field (or `phase: []`).
4. Is it a **phase-scoped analysis** (phase summary, phase digest, phase retro — anything whose subject is a single phase)? → `wiki/projects/<slug>/phase-N/<slug>-phase-N-<analysis-name>.md`. Frontmatter: `type: analysis`, `project: <slug>`, `phase: [<phase-id>]`.
5. Is it a **phase-end target architecture for a specific application**? → `wiki/projects/<slug>/phase-N/<slug>-phase-N-<app-slug>-architecture.md`. Template: `_templates/phase-application-architecture.md`. Frontmatter: `type: phase-application-architecture`, `project: <slug>`, `phase: [<phase-id>]`, `application: <app-slug>`, `baseline: <app-slug>-architecture`. **Filenames keep the `<slug>-phase-N-` prefix** even though the folder repeats it — this preserves basename uniqueness so Obsidian link resolution stays unambiguous.
6. Is it the **current-state architecture for an application**? → `wiki/applications/<app-slug>/<app-slug>-architecture.md` (lives cross-project, not under a project folder — current state is one-per-app, shared across all projects that touch it). Template: `_templates/application-architecture.md`. Frontmatter: `type: application-architecture`, `application: <app-slug>`, `state: current`.
7. Is it an **ADR (decision)**? → `wiki/decisions/` (cross-project folder), with frontmatter `project: <slug>` and `phase: [phase-N]` (or `phase: []` for project-level decisions not bound to a phase).
8. Is it a **richer per-application page** (API contract, data model, runbook, diagrams, event schemas, etc.) that isn't an ADR, isn't phase-scoped, and isn't a one-off analysis? → `wiki/applications/<app-slug>/<app-slug>-<aspect>.md` (lives alongside the identity and architecture pages in the per-app folder). Frontmatter `type:` is case-by-case (often `concept` or a new domain-specific type); carry `application: <app-slug>` and `projects: [...]` if scoped. Use this instead of bloating the identity or architecture page.
9. Is it a **source / concept / entity / application identity page**? → stays in its cross-project folder (`sources/`, `concepts/`, `entities/<sub>/`, `applications/<app-slug>/`); scope it with `project:` (or `projects:`) frontmatter.
10. Is it a **cross-project (portfolio) analysis**? → `wiki/analyses/` (kept for this purpose). Frontmatter: `type: analysis`, no `project:` field (or `project: portfolio`).

When in doubt: **if the page would be deleted or radically rewritten if one project were cancelled, it's project-scoped — put it under `wiki/projects/<slug>/`**. (Phase-target architectures are project-scoped; current-state architectures are not — they survive any single project's cancellation.)

---

## 4. Page format (YAML frontmatter is mandatory)

Every wiki page starts with frontmatter. This powers Obsidian Dataview and lets us query the wiki programmatically.

```markdown
---
type: application | application-architecture | phase-application-architecture
    | decision | entity | concept | source | analysis
    | project | phase | portfolio-overview
title: Human-readable Title
aliases: [alt-name-1, acronym]            # optional — for apps/entities/projects with legacy names
created: 2026-04-22
updated: 2026-04-22
tags: [tag-1, tag-2]
owner: <team-slug>                        # working unit accountable (apps, decisions, change items, platforms)
application: <app-slug> | [<app-slug>]    # on architecture pages: the app described (scalar)
                                           # on decisions / analyses: the app(s) touched (list)
projects: [<project-slug>, …]             # for cross-project pages (applications, entities, platforms, concepts)
project: <project-slug>                   # for project-scoped pages (sources, decisions, analyses, phases, phase-architectures)
phase: [<phase-id>, …]                    # for decisions/analyses/phase-architectures scoped to specific phase(s)
phase_id: phase-N                         # for phase pages only
slug: <project-slug>                      # for project pages only
baseline: <app-slug>-architecture         # for phase-application-architecture pages — which current-state page this deltas from
state: current | phase-target | shipped | superseded      # for architecture pages
                                                          # (apps identity pages keep the older `state: current | target | transitional` if used)
sources: [source-slug-1, source-slug-2]   # for entity/concept/analysis/project/architecture pages
source_count: 2                            # optional, for sorting by evidence weight
gate_date: YYYY-MM-DD | TBD               # for phase pages only
status: stub | draft | mature             # general pages
                                           # for decisions: proposed | accepted | rejected | superseded-by-<slug>
                                           # for projects: planned | in-flight | on-hold | complete | cancelled
                                           # for phases:   planned | in-flight | complete | cancelled
                                           # for architectures: stub | draft | mature | superseded
departed: YYYY-MM-DD                      # person pages only — last day in the org / programme (see §5.5)
succeeded_by: <person-slug>               # person pages only — successor for forward-looking references (see §5.5)
---

# Title

## Summary
2–4 sentences. The most important thing a reader should know.

## <Sections specific to page type>

## Open questions
- …

## Related
- [[related-page-1]] — why it's related
- [[related-page-2]]

## Sources
- [[sources/source-slug-1]] — what this source contributed
```

### Page-type-specific sections

- **Application** — **identity page** (`wiki/applications/<app-slug>/<app-slug>.md`): `Aliases`, `Purpose`, `Current state` (short — role-in-portfolio summary, _not_ architecture), `Target state` (short — role after active projects land), `Change ownership`, `Pending / unknown`, `Related decisions`. Frontmatter includes `projects: [<slug>, …]`. Architecture detail lives on the dedicated architecture page; the identity page should link to it under `Related`. Lives inside the per-application folder alongside its architecture page and any richer per-app material.
- **Application architecture — current state** (`wiki/applications/<app-slug>/<app-slug>-architecture.md`): `Summary`, `Technologies & services`, `Touchpoints` (Inbound / Outbound / Event streams / Batch), `Internal structure`, `Observability & SLOs`, `Known constraints / rough edges`, `Relationships to other architecture pages`, `Phase-target pages that delta from this one`, `Superseded claims`. **One per application**, cross-project. Template: `_templates/application-architecture.md`.
- **Phase-end target architecture** (`wiki/projects/<slug>/phase-N/<slug>-phase-N-<app-slug>-architecture.md`): `Summary`, `Delta from current state` (load-bearing section), `Full end-of-phase shape` (same skeleton as current-state, for standalone readability), `Decisions driving this target`, `Open questions gating this target`. **One per (project, phase, app) where change-intensity is medium or higher.** Template: `_templates/phase-application-architecture.md`.
- **Decision** (`wiki/decisions/`): `Context`, `Options considered`, `Decision`, `Consequences`, `Supersedes / superseded by`. Frontmatter includes `project:` and `phase:`.
- **Entity** — three sub-types, each with its own template:
  - **Person** (`wiki/entities/people/`): `Key facts` (Role, Team, Role in programme), `Relationships`, `Claims (with citations)`, `Open questions`. Frontmatter includes a `team:` slug. **Departed people** carry `departed:` + `succeeded_by:` frontmatter and add a `Departure` section right after `Summary`; their historical claims stay verbatim — see §5.5.
  - **Team** (`wiki/entities/teams/`): `Key facts`, `Members (known in this wiki)` (table), `Past members` (sub-table, optional — populated by §5.5 departures), `Applications / platforms owned`, `Current workstreams`, `Deferred / future decisions owned`, `Relationships`, `Claims`.
  - **Platform** (`wiki/entities/platforms/`): `Key facts` (Owning team, Core technology, Role in programme), `Current shape (today)`, `Target-state exposure`, `Relationships`, `Claims`. Frontmatter includes an `owner:` slug.
- **Concept** (`wiki/concepts/`): `Definition`, `Mechanics / how it works`, `Variants`, `Contradictions or open debates`.
- **Source** (`wiki/sources/`): `Bibliographic info`, `Key claims`, `Decisions made` (if meeting), `Actions` (if meeting, with owner + state), `Entities/concepts mentioned`, `Contradicts`, `Reinforces`. Frontmatter includes `project:`.
- **Analysis** (`wiki/analyses/` for cross-project / `wiki/projects/<slug>/` for project-scoped): `Question asked`, `Method`, `Findings`, `Confidence`, `Follow-ups`. Project-scoped analyses include `project:` frontmatter.
- **Project** (`wiki/projects/<slug>/<slug>.md`): `Thesis`, `Driver`, `Pillars`, `Tensions`, `Known applications in scope`, `Known teams in scope`, `What I'd want to know next`. Template: `_templates/project.md`.
- **Phase** (`wiki/projects/<slug>/phase-N/<slug>-phase-N.md`): `Summary`, `Gate`, `Scope — applications` (table), `Decisions in this phase`, `Delivery shape`, `Open questions gating this phase`, `Predecessors / successors`, `Done-state`. Template: `_templates/phase.md`.
- **Portfolio overview** (`wiki/overview.md`): `Projects in flight` (table), `Portfolio themes`, `Cross-project open questions`, `How to read this wiki`. Thin by design — all project-level content lives on the project page.

Templates live in `wiki/_templates/` — copy, don't inline boilerplate.

---

## 5. Workflows

### 5.1 Ingest (human adds a source)

**Trigger**: human drops a file into `raw/` and says "ingest this" (or names the file).

Steps:
1. **Identify the project.** Every source belongs to exactly one project (or `portfolio` for cross-project material). If the human hasn't named the project, ask. This drives the `project:` frontmatter on the source page and the folder of any project-scoped pages created during ingest.
2. **Read** the full source. If it references images in `raw/assets/`, view them.
3. **Discuss**: briefly tell the human (≤5 bullets) the key takeaways, what surprised you, and which existing wiki pages this will touch. Wait for their steer before writing, unless they've said "batch mode".
4. **Write the source summary** at `wiki/sources/<slug>.md` using the source template, including `project:` frontmatter.
5. **Update affected pages**: create or revise application/decision/entity/concept/phase pages. A single source commonly touches 5–15 pages. Bias toward updating many small pages rather than dumping everything into one. Every decision or phase page carries `project:` (and `phase:` for decisions).
6. **Add cross-links** both directions (if page A mentions B, link A→B; also make sure B has a "Related" entry back to A where warranted).
7. **Flag contradictions**: if this source contradicts an existing claim, add a `> ⚠ Contradiction:` callout on the affected page with citations to both sources. Do not silently overwrite.
8. **Refresh standing analyses** — update `wiki/projects/<slug>/<slug>-dependency-map.md`, `wiki/projects/<slug>/<slug>-ownership-matrix.md`, **and `wiki/open-questions.md`** (global, with `Project` column) if anything in scope changed. See §5.1b for open-questions rules. If a cross-project pattern emerges (e.g. scheduling conflict between projects), add or update a portfolio-level analysis under `wiki/analyses/`.
9. **Update `index.md`** — add the new source, new pages, updated counts.
10. **Update the project overview** at `wiki/projects/<slug>/<slug>.md` if the project's thesis shifts. Only update the **portfolio** `wiki/overview.md` if the portfolio itself changes (new project, project status change, cross-project coordination point worth surfacing).
11. **Append to `log.md`** using the exact format in §7.

#### 5.1a Meeting-note sub-workflow

Meeting notes are a first-class source type. When the source is a meeting note / transcript / call summary:

- Extract **attendees** — link each to their team entity if identifiable; create stub entity pages for unknown attendees.
- Extract **decisions made** — if any decision is architecturally meaningful, promote it to a `wiki/decisions/<slug>.md` page rather than leaving it only in the source summary. Carry `project:` and `phase:` frontmatter from the moment of creation — an ADR without a project is a schema error.
- Extract **actions assigned** — each action: `owner` (working unit or individual), `application` it concerns, one-line description, `state: open`. Fold actions into the relevant project's `<slug>-ownership-matrix.md`.
- Extract **open questions** — add to the global [[open-questions]] register (see §5.1b) with the correct `Project` column value, and to the project overview's `What I'd want to know next` section. Reference the `OQ-NNN` ID from any affected page's `Pending / unknown` section rather than duplicating the question text.

#### 5.1b Open-questions sub-workflow

`wiki/open-questions.md` is the **single source of truth** for unresolved questions across the entire wiki (every project). Do not scatter open questions across many pages — reference IDs from this register.

On every ingest:

1. **Resolutions first.** For each question the new source answers or refines, move it from the Open table to the Resolved table at the bottom, adding `Answer`, `Resolved` (date), and `Source` columns. Never silently delete.
2. **New questions.** Append any new unresolved questions raised by the source, with the next unused `OQ-NNN` (IDs are never reused; see the register's own Conventions section).
3. **Project column is mandatory.** Every row carries a `Project` value — a project slug (`party-rearch`, `secrets-management`, …) or `portfolio` for cross-project questions. IDs are **global** across all projects; a party-rearch OQ and a secrets-management OQ share the same `OQ-NNN` namespace.
4. **Cross-references.** On affected wiki pages, replace ad-hoc "open question: …" prose with `see [[open-questions#OQ-NNN]]`. Pages summarise; the register holds the canonical wording.
5. **Contradictions.** If a new source contradicts a prior _resolution_, create a new ID (e.g. `OQ-014-R`) in the Open table and add a `⚠ Contradiction` callout to the page where the original answer lives.
6. **Categorisation.** Use the register's category tables (`scope / architecture calls`, `timeline / cadence`, `design open calls`, `people identification`, `adr-candidate`). Split a question across categories only if it's genuinely two questions.

### 5.2 Query (human asks a question)

1. **Read `index.md` first** to find candidate pages. Do not go straight to `rg`/filesystem searches — the index is the entry point.
2. Drill into 3–8 relevant wiki pages. Only reach into `raw/` if the wiki is insufficient — and if it is, that's a signal the wiki is under-developed on this topic; note it.
3. Synthesize an answer **with inline citations** to wiki pages: `…as seen in [[hidden-markov-models]]`.
4. **Offer to file** the answer to `wiki/analyses/<slug>.md` if it's non-trivial. Default to yes for anything longer than a paragraph or that synthesizes ≥2 pages.
5. If filed, add it to `index.md` and `log.md`.

### 5.3 Lint (periodic health check)

When the human says "lint" (or proactively every ~10 ingests), report on:
- **Contradictions** between pages (grep for `⚠ Contradiction`).
- **Stale claims** — pages whose `updated` date is much older than the sources they cite would now suggest.
- **Orphans** — pages with 0 inbound `[[links]]`.
- **Missing pages** — concepts or entities mentioned ≥2 times but lacking their own page.
- **Weak cross-linking** — pages with <2 outbound or inbound links.
- **Index drift** — entries in `index.md` that point to non-existent pages, or pages missing from the index.
- **Data gaps** — topics where you'd recommend finding a new source or doing a web search.

Present findings as a numbered list. **Do not fix silently.** Propose fixes, wait for approval, then execute.

### 5.4 Phase completion (fold phase-target architecture into current state)

**Trigger**: a phase's `status:` frontmatter moves to `complete` (human confirms the phase has shipped). Run this workflow once per affected application **before** anyone consumes the wiki for a query against the new reality.

For each phase-target architecture page `[[<project-slug>-<phase-id>-<app-slug>-architecture]]` belonging to the completed phase:

1. **Promote the target.** Overwrite `[[<app-slug>-architecture]]` (the current-state page) with content equivalent to the **Full end-of-phase shape** section of the target page. Technologies, touchpoints, internal structure, observability, constraints — all replace the old content.
2. **Record supersessions.** For every old current-state claim that the target invalidates, move the claim to the **Superseded claims** section at the bottom of the new current-state page, prefixed with `[YYYY-MM-DD, after [[<project-slug>-<phase-id>]]] — <claim> → <why it's no longer true>`. Never silently delete — the history is part of the reference.
3. **Archive the target page.** Update the target page's frontmatter: `state: phase-target` → `state: shipped`. Add a note at the top linking to the new current-state page. Leave content in place — it's a historical record of what was proposed at phase-end.
4. **Update baseline pointers.** Any **subsequent-phase** target pages (e.g. `<project-slug>-phase-N+1-<app-slug>-architecture`) that currently deltas from what was the old current-state now delta from a baseline that has already moved. Re-read them and update their Delta sections so the deltas remain accurate relative to the _new_ current state.
5. **Source updates.** Add the phase-completion event as a source entry (e.g. a release note, cutover confirmation, or retrospective) under `wiki/sources/` if one exists; cite it from the new current-state page's `Sources`. If no formal source exists, cite the phase page itself.
6. **Log it.** Append an `analysis` (or `note`) entry to `log.md` summarising the fold-in, including the list of claims that were superseded.

This workflow is the only reason the current-state architecture pages don't rot. Skip it and every phase increases drift.

### 5.5 Person departure (handover to a successor)

**Trigger**: the human declares that a person is leaving the organisation or the programme, and names a successor.

The core rule: **historical record stays verbatim; forward-looking references redirect.** A source page captures what was true at the time of the meeting — if Andrew was the PO and was in the room, that doesn't stop being true when he leaves three weeks later. But a list of spike participants on a project-scoped page _is_ forward-looking — if Andrew won't be there to participate, the list has to redirect.

Steps:

1. **Mark the person page.** On the departed person's entity page:
   - Set frontmatter `departed: YYYY-MM-DD` and `succeeded_by: <successor-slug>`.
   - Add a `## Departure` section immediately after `## Summary` recording the last day, the successor, and a one-line context.
   - Leave existing `## Claims` and `## Sources` sections untouched — these are historical.
2. **Update the successor page.** Add a Claim citing the user declaration: _"Took over from [[<departed-slug>]] as <role> on [[<team-slug>]] — user-declared (YYYY-MM-DD)."_ Refresh the summary if the successor's responsibilities materially change.
3. **Update the team page.** Move the departed row from `## Members (known in this wiki)` to a new `## Past members` sub-table with columns `Name | Role | Departed | Succeeded by`. Update team-level claims if the move resolves an outstanding ambiguity (e.g. "two POs, division of remit unclear" → "one PO post-handover").
4. **Redirect forward-looking references.** For project-scoped pages, decision pages, application identity pages, and ownership matrices, replace `[[<departed-slug>]]` with `[[<successor-slug>]]` in any forward-looking list (action ownership, future spike pairings, named decision-makers, planned-meeting attendees). Add a one-liner `(took over from [[<departed-slug>]] YYYY-MM-DD)` on first mention so the trail is preserved without cluttering every subsequent appearance.
5. **Preserve historical source pages.** Do **not** rewrite past meeting transcripts, attendee lists in source pages, or already-completed-action rows. Instead, on the first mention of the departed person in each source page, append an inline footnote: `(departed YYYY-MM-DD → succeeded by [[<successor-slug>]])`. The historical claim stays; the redirect is visible.
6. **Repoint peripheral Related-lists.** On other persons' entity pages that list the departed person in their `## Related` section as a navigational aid (not as a historical claim), swap to the successor. The `## Claims` and `## Sources` sections on those same pages are historical and stay.
7. **Update `index.md`.** Annotate the departed person's row with `(departed YYYY-MM-DD → [[<successor-slug>]])`. Refresh the successor's row.
8. **Log it.** Append a `note` entry to `log.md` enumerating the redirected pages. If the departure triggered a schema change, also append a `schema` entry. Per §8 rule 8, schema co-evolution flows through `log.md` under the `schema` kind.

Future ingests automatically inherit the convention: when a source mentions a person who has `departed:` set, treat substantive forward-looking attribution as a reference to their `succeeded_by:`, but record the literal mention on the source page (it captures what was said).

This workflow is the only reason team rosters and forward-looking action-owner lists don't go stale when people leave.

---

## 6. `index.md` format

Content-oriented catalog. Organized by section. Each entry: link + one-line summary + optional metadata.

```markdown
# Index

_Last updated: 2026-04-22_
_Pages: X sources · Y entities · Z concepts · N analyses · P projects_

## Portfolio
- [[overview]] — portfolio overview (projects in flight, cross-project themes)

## Projects
### party-rearch
- [[party-rearch]] — project overview (src: 2)
- [[party-rearch-phase-1]] · [[party-rearch-phase-2]]
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]]

### secrets-management
- [[secrets-management]] — project overview (stub)
- [[secrets-management-phase-1]]

## Applications
- [[party-application]] — core store and routing (projects: party-rearch, secrets-management?)
- …

## Decisions
- [[strangle-the-graph-via-proxy-events]] — party-rearch / phase-1 (src: 2)
- …

## Entities
### People
- [[alice-chen]] — lead researcher (src: 3)
### Teams
- [[graph-team]] — owns party-application (src: 2)
### Platforms
- [[data-universe]] — Snowflake-backed analytics (src: 1)

## Concepts
- [[contract-buckets]] — three-bucket framework (src: 1)

## Sources
- [[sources/attention-is-all-you-need]] — Vaswani et al. 2017 (ingested 2026-04-22)

## Analyses (portfolio-level)
- _(project-scoped analyses live under Projects above)_

## Open questions
- [[open-questions]] — global register (filter by `Project` column)
```

Update **on every ingest** and **whenever you create or delete a wiki page**.

---

## 7. `log.md` format

Append-only. Each entry starts with a **parseable prefix** so `grep "^## \[" log.md | tail -5` works.

```markdown
## [YYYY-MM-DD] <kind> | <one-line subject>

- <bullet 1>
- <bullet 2>
- touched: [[page-a]], [[page-b]], [[page-c]]
```

`<kind>` is one of: `ingest`, `query`, `analysis`, `lint`, `schema`, `note`.

Examples:
```
## [2026-04-22] ingest | Attention Is All You Need (Vaswani et al. 2017)
## [2026-04-22] query | how do transformers handle long contexts?
## [2026-04-22] analysis | transformers vs RNNs (filed)
## [2026-04-22] lint | found 3 orphan pages, 1 contradiction
## [2026-04-22] schema | added `status` frontmatter field
```

Never edit past log entries. If you need to correct one, append a new `note` entry referencing it.

---

## 8. Interaction rules

1. **Read `AGENTS.md`, `index.md`, and the last ~10 entries of `log.md` at the start of every session** before doing anything else. This is how you orient.
2. **No silent wiki edits.** Before modifying ≥3 pages, summarize the planned changes and ask for approval. Single-page fixes can proceed without asking.
3. **Citations are mandatory.** Any claim in the wiki must trace back to a source (via the `sources:` frontmatter and/or inline `[[sources/...]]` links) or be labelled `_inference_` / `_unsourced_`.
4. **Prefer many small pages over few big ones.** If a section of a page grows past ~400 words or covers a distinct noun, split it out.
5. **Keep summaries at the top.** The first paragraph of every page must stand alone as a useful answer.
6. **Don't modify `raw/`.** Ever. If a source has an error, note it in the source's wiki summary.
7. **When uncertain, ask.** Better to ask one clarifying question than to file wrong synthesis.
8. **Co-evolve this schema.** When you notice a convention that would help, propose an edit to `AGENTS.md` and log it under `schema`.

---

## 9. Tooling (optional, add as needed)

- **Search**: At current scale, `index.md` + `rg` are enough. Revisit `qmd` (local BM25+vector search with MCP server) if the wiki exceeds ~100 sources.
- **Obsidian**: Graph view, Dataview (reads frontmatter), Marp (slides from markdown), Web Clipper (web → `raw/`).
- **Git**: The whole repo is versioned. Commit after every ingest or meaningful batch so history reflects the wiki's evolution.

---

## 10. Portfolio scope

This wiki spans multiple **projects**. Each project has its own thesis, phases, and scope; this section is the portfolio-level summary. For project-level detail, see each project's own page.

### Projects in flight
- **[[party-rearch]]** (`party-rearch`) — re-architecture of the **Party Application** and its **Party Curation Tool**. Forcing function: **High Volume** go-live on 1 September. Phases: [[party-rearch-phase-1]] (in-flight) · [[party-rearch-phase-2]] (planned). Owner: [[graph-team]] ([[alex-sillars]]).
- **[[secrets-management]]** (`secrets-management`) — ASG-mandated transition to standardised secrets management, driven by the Shai-Hulud 2.0 supply-chain incident (INC-140574). Eliminate long-lived CI/CD credentials; centralise in AWS Secrets Manager; enable automated rotation. Phase 1 planned; scope (which applications, which teams, which timeline) to be confirmed on first ingest. Owner: TBD.

### Portfolio-level context
- **Business domain**: insurance deals across Specialty, Lloyd's Market, Reinsurance, Aerospace, Accident & Health, Marine, Casualty, Crisis Management, ELA (Expense and Loss Adjustment), Energy, FAS (Free Alongside Ship), Political Risk & Surety, Property, Whole Account.
- **Cross-project applications** (live in `wiki/applications/`, touched by one or more projects):
  - `party-application` — core store and routing (owner: [[graph-team]])
  - `party-curation-tool` — PCT, curation frontend on Jira workflow (owner: [[graph-team]], primary user: [[dataops-team]])
  - `inrisk` — IR2, live Policy Admin System for prebind (owner: [[prebind-team]])
  - `inrisk-engine` — API-first InRisk rewrite (owner: [[devx-team]])
  - `high-volume` — HV, high-throughput deal processing (owner: [[high-volume-team]])
  - `eclipse` — retiring third-party data source used by [[inrisk]] (stub)

### Human's role
Project oversight, detailed design, timeline management. Bias summaries toward cross-cutting visibility, ownership clarity, and decision/risk tracking rather than deep implementation detail — unless a source is itself deep implementation.

### Source mix expected
Meeting notes, architecture diagrams (image files in `raw/assets/`), internal wiki exports, UI screengrabs, specs, API documentation, incident reports (for `secrets-management`), ASG mandate documents.
