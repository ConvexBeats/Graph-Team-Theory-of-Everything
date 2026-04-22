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
├── AGENTS.md                 # this schema
├── index.md                  # content catalog (read this first on queries)
├── log.md                    # chronological record (append-only)
├── raw/                      # immutable source documents
│   ├── assets/               # images, PDFs, data files referenced by sources
│   └── <source-files>.md     # articles, clippings, transcripts, notes
└── wiki/
    ├── overview.md           # top-level synthesis / evolving thesis
    ├── entities/             # people, orgs, products, places, systems
    ├── concepts/             # abstract ideas, topics, themes, frameworks
    ├── sources/              # one summary page per ingested source
    ├── analyses/             # query-derived outputs worth keeping
    └── _templates/           # page templates you copy from
```

### Filename rules
- Lowercase, kebab-case: `hidden-markov-models.md`, `alice-chen.md`.
- One page = one noun. Don't merge unrelated ideas onto one page.
- Source summary filenames mirror the raw filename: `raw/attention-is-all-you-need.md` → `wiki/sources/attention-is-all-you-need.md`.
- Use `[[wiki-links]]` (Obsidian-style) for cross-references. Always link by page name, not path: `[[hidden-markov-models]]`.

---

## 4. Page format (YAML frontmatter is mandatory)

Every wiki page starts with frontmatter. This powers Obsidian Dataview and lets us query the wiki programmatically.

```markdown
---
type: entity | concept | source | analysis | overview
title: Human-readable Title
created: 2026-04-22
updated: 2026-04-22
tags: [tag-1, tag-2]
sources: [source-slug-1, source-slug-2]   # for entity/concept/analysis pages
source_count: 2                            # optional, for sorting by evidence weight
status: stub | draft | mature              # your honest assessment of the page
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

- **Entity** (`wiki/entities/`): `Key facts`, `Timeline`, `Relationships`, `Claims (with citations)`.
- **Concept** (`wiki/concepts/`): `Definition`, `Mechanics / how it works`, `Variants`, `Contradictions or open debates`.
- **Source** (`wiki/sources/`): `Bibliographic info`, `Key claims`, `Notable quotes`, `Entities/concepts mentioned`, `Contradicts`, `Reinforces`.
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
4. **Update affected pages**: create or revise entity/concept pages. A single source commonly touches 5–15 pages. Bias toward updating many small pages rather than dumping everything into one.
5. **Add cross-links** both directions (if page A mentions B, link A→B; also make sure B has a "Related" entry back to A where warranted).
6. **Flag contradictions**: if this source contradicts an existing claim, add a `> ⚠ Contradiction:` callout on the affected page with citations to both sources. Do not silently overwrite.
7. **Update `index.md`** — add the new source, new pages, updated counts.
8. **Update `wiki/overview.md`** if the thesis shifts.
9. **Append to `log.md`** using the exact format in §7.

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

## 10. Bootstrap state

- Wiki is empty. First ingest will populate `wiki/sources/`, create initial entity/concept pages, and seed `wiki/overview.md`.
- Domain is not yet declared. On the first ingest, ask the human to confirm the project's scope (one sentence) so the overview has a thesis to grow from.
