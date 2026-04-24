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
    ├── overview.md              # top-level synthesis / evolving thesis
    ├── open-questions.md        # standing register of unresolved questions (OQ-NNN IDs)
    ├── applications/            # one page per application (current/target/transitional state)
    ├── decisions/               # architectural decision records (ADR-style)
    ├── entities/                # every first-class non-software actor or system in this wiki
    │   ├── people/              # every human (one page per person)
    │   ├── teams/               # every working unit (one page per team)
    │   └── platforms/           # platforms / vendor systems / external sources that aren't in-scope applications
    ├── concepts/                # abstract ideas, frameworks, themes
    ├── sources/                 # one summary page per ingested source
    ├── analyses/                # query-derived outputs + standing analyses
    │   ├── dependency-map.md    # standing: inter-application dependencies (current + target)
    │   └── ownership-matrix.md  # standing: change items × working units
    └── _templates/              # page templates you copy from (person.md · team.md · platform.md · application.md · decision.md · concept.md · source.md · analysis.md)
```

### Filename rules
- Lowercase, kebab-case: `party-application.md`, `graph-team.md`.
- One page = one noun. Don't merge unrelated ideas onto one page.
- Source summary filenames mirror the raw filename: `raw/2026-04-22-party-arch-meeting.md` → `wiki/sources/2026-04-22-party-arch-meeting.md`.
- Use `[[wiki-links]]` (Obsidian-style) for cross-references. Prefer bare page name: `[[party-application]]`. Use folder-qualified form only to disambiguate when two pages share a slug.
- **Filenames must be globally unique across entity subfolders** (`people/`, `teams/`, `platforms/`). Two files with the same slug in different subfolders would make `[[slug]]` ambiguous to Obsidian; if a collision is unavoidable, disambiguate both filenames (e.g. `sam-smith.md` instead of `sam.md`).
- Raw-file references from wiki pages use an **inline code-span with a relative path** (e.g. `` `raw/20260422 - Meeting Transcript.md` ``), not a wiki link. This keeps the raw layer out of Obsidian's graph view.

### Vocabulary (project-specific)
To avoid "party" overload in this project:
- **business party** — the insurance-domain entity (broker, client, underwriter, etc.) that the `party-application` manages.
- **application** — a piece of software (e.g. `party-application`, `inrisk`). Always a page under `wiki/applications/`.
- **working unit** (or **team**) — a delivery group (e.g. `graph-team`). Lives under `wiki/entities/teams/` with `tags: [team]`.
- **person** — a named human. Lives under `wiki/entities/people/` with `tags: [person]` and a `team:` frontmatter field pointing at their team slug.
- **platform** — a first-class system that isn't an in-scope application in this programme: analytics platforms (e.g. `data-universe`), vendor systems (e.g. D&B, S&P, NTT), shared services. Lives under `wiki/entities/platforms/` with `tags: [platform]` and an `owner:` frontmatter field pointing at the owning team.

### Choosing an entities subfolder
When creating a new entity page, decide with this cascade:

1. Is it a named human? → `wiki/entities/people/`. Template: `_templates/person.md`.
2. Is it a delivery/working unit (has members, owns work)? → `wiki/entities/teams/`. Template: `_templates/team.md`.
3. Is it a system that isn't an in-scope application on this programme (data platform, vendor, external feed, shared service)? → `wiki/entities/platforms/`. Template: `_templates/platform.md`.
4. Is it a piece of software actively re-architected or affected by this programme? → **Not an entity** — it's an `application`, goes in `wiki/applications/`.

If a candidate fits none of these (e.g. a physical place, a regulatory body) and no existing subfolder makes sense, flag it in the ingest as a schema question rather than inventing a new subfolder silently.

---

## 4. Page format (YAML frontmatter is mandatory)

Every wiki page starts with frontmatter. This powers Obsidian Dataview and lets us query the wiki programmatically.

```markdown
---
type: application | decision | entity | concept | source | analysis | overview
title: Human-readable Title
aliases: [alt-name-1, acronym]            # optional — for apps/entities with legacy names
created: 2026-04-22
updated: 2026-04-22
tags: [tag-1, tag-2]
owner: <team-slug>                        # working unit accountable (apps, decisions, change items)
application: [<app-slug>]                 # which app(s) a decision / analysis touches
state: current | target | transitional    # for application pages only
sources: [source-slug-1, source-slug-2]   # for entity/concept/analysis pages
source_count: 2                            # optional, for sorting by evidence weight
status: stub | draft | mature             # general pages
                                           # for decisions: proposed | accepted | rejected | superseded-by-<slug>
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

- **Application** (`wiki/applications/`): `Aliases`, `Purpose`, `Current state`, `Target state`, `Transitional states`, `Change ownership`, `Pending / unknown`, `Related decisions`.
- **Decision** (`wiki/decisions/`): `Context`, `Options considered`, `Decision`, `Consequences`, `Supersedes / superseded by`.
- **Entity** — three sub-types, each with its own template:
  - **Person** (`wiki/entities/people/`): `Key facts` (Role, Team, Role in programme), `Relationships`, `Claims (with citations)`, `Open questions`. Frontmatter includes a `team:` slug.
  - **Team** (`wiki/entities/teams/`): `Key facts`, `Members (known in this wiki)` (table), `Applications / platforms owned`, `Current workstreams`, `Deferred / future decisions owned`, `Relationships`, `Claims`.
  - **Platform** (`wiki/entities/platforms/`): `Key facts` (Owning team, Core technology, Role in programme), `Current shape (today)`, `Target-state exposure (from this programme)`, `Relationships`, `Claims`. Frontmatter includes an `owner:` slug.
- **Concept** (`wiki/concepts/`): `Definition`, `Mechanics / how it works`, `Variants`, `Contradictions or open debates`.
- **Source** (`wiki/sources/`): `Bibliographic info`, `Key claims`, `Decisions made` (if meeting), `Actions` (if meeting, with owner + state), `Entities/concepts mentioned`, `Contradicts`, `Reinforces`.
- **Analysis** (`wiki/analyses/`): `Question asked`, `Method`, `Findings`, `Confidence`, `Follow-ups`.
- **Overview** (`wiki/overview.md`): `Thesis`, `Pillars`, `Tensions`, `What I'd want to know next`.

Templates live in `wiki/_templates/` — copy, don't inline boilerplate.

---

## 5. Workflows

### 5.1 Ingest (human adds a source)

**Trigger**: human drops a file into `raw/` and says "ingest this" (or names the file).

Steps:
1. **Read** the full source. If it references images in `raw/assets/`, view them.
2. **Discuss**: briefly tell the human (≤5 bullets) the key takeaways, what surprised you, and which existing wiki pages this will touch. Wait for their steer before writing, unless they've said "batch mode".
3. **Write the source summary** at `wiki/sources/<slug>.md` using the source template.
4. **Update affected pages**: create or revise application/decision/entity/concept pages. A single source commonly touches 5–15 pages. Bias toward updating many small pages rather than dumping everything into one.
5. **Add cross-links** both directions (if page A mentions B, link A→B; also make sure B has a "Related" entry back to A where warranted).
6. **Flag contradictions**: if this source contradicts an existing claim, add a `> ⚠ Contradiction:` callout on the affected page with citations to both sources. Do not silently overwrite.
7. **Refresh standing analyses** — update `wiki/analyses/dependency-map.md`, `wiki/analyses/ownership-matrix.md`, **and `wiki/open-questions.md`** if anything in scope changed. See §5.1b for open-questions rules.
8. **Update `index.md`** — add the new source, new pages, updated counts.
9. **Update `wiki/overview.md`** if the thesis shifts.
10. **Append to `log.md`** using the exact format in §7.

#### 5.1a Meeting-note sub-workflow

Meeting notes are a first-class source type. When the source is a meeting note / transcript / call summary:

- Extract **attendees** — link each to their team entity if identifiable; create stub entity pages for unknown attendees.
- Extract **decisions made** — if any decision is architecturally meaningful, promote it to a `wiki/decisions/<slug>.md` page rather than leaving it only in the source summary.
- Extract **actions assigned** — each action: `owner` (working unit or individual), `application` it concerns, one-line description, `state: open`. Fold actions into `ownership-matrix.md`.
- Extract **open questions** — add to [[open-questions]] register (see §5.1b) and to `overview.md → What I'd want to know next`. Reference the `OQ-NNN` ID from any affected page's `Pending / unknown` section rather than duplicating the question text.

#### 5.1b Open-questions sub-workflow

`wiki/open-questions.md` is the **single source of truth** for unresolved questions across the programme. Do not scatter open questions across many pages — reference IDs from this register.

On every ingest:

1. **Resolutions first.** For each question the new source answers or refines, move it from the Open table to the Resolved table at the bottom, adding `Answer`, `Resolved` (date), and `Source` columns. Never silently delete.
2. **New questions.** Append any new unresolved questions raised by the source, with the next unused `OQ-NNN` (IDs are never reused; see the register's own Conventions section).
3. **Cross-references.** On affected wiki pages, replace ad-hoc "open question: …" prose with `see [[open-questions#OQ-NNN]]`. Pages summarise; the register holds the canonical wording.
4. **Contradictions.** If a new source contradicts a prior _resolution_, create a new ID (e.g. `OQ-014-R`) in the Open table and add a `⚠ Contradiction` callout to the page where the original answer lives.
5. **Categorisation.** Use the register's category tables (`scope / architecture calls`, `timeline / cadence`, `design open calls`, `people identification`, `adr-candidate`). Split a question across categories only if it's genuinely two questions.

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

---

## 6. `index.md` format

Content-oriented catalog. Organized by section. Each entry: link + one-line summary + optional metadata.

```markdown
# Index

_Last updated: 2026-04-22_
_Pages: X sources · Y entities · Z concepts · N analyses_

## Overview
- [[overview]] — current synthesis / thesis

## Entities
- [[alice-chen]] — lead researcher at Foo Lab (src: 3)
- …

## Concepts
- [[hidden-markov-models]] — probabilistic sequence model (src: 5)
- …

## Sources
- [[sources/attention-is-all-you-need]] — Vaswani et al. 2017 (ingested 2026-04-22)
- …

## Analyses
- [[analyses/transformers-vs-rnns]] — comparison table (2026-04-22)
- …
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

## 10. Project scope

**Subject**: re-architecture of the **Party Application** (aka *Graph* / *Party MDM* / *MDM*) and its supporting **Party Curation Tool** (PCT). The Party Application is the authoritative store for *business parties* (brokers, clients, underwriters, etc.) consumed across the insurance-deal lifecycle.

**Core shift**: from delivering large party payloads that downstream systems snapshot-cache, to delivering **versioned party IDs** that downstream systems resolve near-real-time against the Party Application.

**Key milestone**: new architecture must be landed ahead of the **High Volume** application's go-live, which will be a major consumer of parties.

**Applications in scope** (seeded in `wiki/applications/`):
- `party-application` — core store and routing (owner: [[graph-team]])
- `party-curation-tool` — PCT, curation frontend on Jira workflow (owner: [[graph-team]], primary user: [[dataops-team]])
- `inrisk` — IR2, live Policy Admin System for prebind (owner: [[prebind-team]], aka *Strikers*)
- `inrisk-engine` — API-first InRisk rewrite, co-defines target Party data model (owner: [[devx-team]])
- `high-volume` — HV, high-throughput deal processing, future Party consumer (owner: **unknown**, flag)

**Human's role**: project oversight, detailed design, timeline management. Bias summaries toward cross-cutting visibility, ownership clarity, and decision/risk tracking rather than deep implementation detail — unless a source is itself deep implementation.

**Insurance lines** the Party Application ultimately serves (for context, not primary wiki subjects): Specialty, Lloyd's Market, Reinsurance, Aerospace, Accident & Health, Marine, Casualty, Crisis Management, ELA, Energy, FAS, Political Risk & Surety, Property, Whole Account. *ELA and FAS expansions pending confirmation.*

**Source mix expected**: meeting notes, architecture diagrams (image files in `raw/assets/`), internal wiki exports, UI screengrabs, specs, API documentation.
