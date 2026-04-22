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
    ├── applications/            # one page per application (current/target/transitional state)
    ├── decisions/               # architectural decision records (ADR-style)
    ├── entities/                # people, teams/working units, orgs, vendors
    ├── concepts/                # abstract ideas, frameworks, themes
    ├── sources/                 # one summary page per ingested source
    ├── analyses/                # query-derived outputs + standing analyses
    │   ├── dependency-map.md    # standing: inter-application dependencies (current + target)
    │   └── ownership-matrix.md  # standing: change items × working units
    └── _templates/              # page templates you copy from
```

### Filename rules
- Lowercase, kebab-case: `party-application.md`, `graph-team.md`.
- One page = one noun. Don't merge unrelated ideas onto one page.
- Source summary filenames mirror the raw filename: `raw/2026-04-22-party-arch-meeting.md` → `wiki/sources/2026-04-22-party-arch-meeting.md`.
- Use `[[wiki-links]]` (Obsidian-style) for cross-references. Prefer bare page name: `[[party-application]]`. Use folder-qualified form only to disambiguate when two pages share a slug.

### Vocabulary (project-specific)
To avoid "party" overload in this project:
- **business party** — the insurance-domain entity (broker, client, underwriter, etc.) that the `party-application` manages.
- **application** — a piece of software (e.g. `party-application`, `inrisk`). Always a page under `wiki/applications/`.
- **working unit** (or **team**) — a delivery group (e.g. `graph-team`). Lives under `wiki/entities/` with `tags: [team]`.

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
- **Entity** (`wiki/entities/`): `Key facts`, `Timeline`, `Relationships`, `Claims (with citations)`. Teams additionally: `Applications owned`, `Current workstreams`.
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
7. **Refresh standing analyses** — update `wiki/analyses/dependency-map.md` and `wiki/analyses/ownership-matrix.md` if anything in scope changed.
8. **Update `index.md`** — add the new source, new pages, updated counts.
9. **Update `wiki/overview.md`** if the thesis shifts.
10. **Append to `log.md`** using the exact format in §7.

#### 5.1a Meeting-note sub-workflow

Meeting notes are a first-class source type. When the source is a meeting note / transcript / call summary:

- Extract **attendees** — link each to their team entity if identifiable; create stub entity pages for unknown attendees.
- Extract **decisions made** — if any decision is architecturally meaningful, promote it to a `wiki/decisions/<slug>.md` page rather than leaving it only in the source summary.
- Extract **actions assigned** — each action: `owner` (working unit or individual), `application` it concerns, one-line description, `state: open`. Fold actions into `ownership-matrix.md`.
- Extract **open questions** — add to the relevant page's `Pending / unknown` section, and to `overview.md → What I'd want to know next`.

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
