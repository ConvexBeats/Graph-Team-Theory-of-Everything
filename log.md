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
