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
