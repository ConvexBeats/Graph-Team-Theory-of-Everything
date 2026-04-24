---
type: application-architecture
title: <Application Name> — Current-state Architecture
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [architecture, current-state]
application: <app-slug>
state: current
sources: []
source_count: 0
status: stub   # stub | draft | mature | superseded
---

<!--
Lives at: wiki/applications/<app-slug>/<app-slug>-architecture.md
Sibling pages: the identity page <app-slug>.md, plus any richer per-app pages (api-contracts, data-model, runbook, etc.).
Phase-target architecture pages that delta from this baseline live at:
  wiki/projects/<project-slug>/<project-slug>-<phase-id>-<app-slug>-architecture.md
See AGENTS.md §3 cascade entries 4 & 5, and §5.4 for the phase-completion fold-in workflow.
-->

# <Application Name> — Current-state Architecture

_Living reference for the **as-is** technical shape of [[<app-slug>]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims either updated in place or moved to **Superseded** at the bottom._

## Summary
_One paragraph: the application's current architectural shape in plain English. Read this first if you only have 60 seconds._

## Technologies & services
_Primary technologies the application is built on today. Group by role, not vendor._

- **Primary datastore**: …
- **Secondary stores / caches**: …
- **Compute**: …
- **Event platform / message bus**: …
- **Observability**: …
- **Other infrastructure**: …

## Touchpoints

> **All integration contracts with other applications and platforms.** Name the other side with a `[[wiki-link]]` wherever possible. If the contract is formal (versioned API, named event topic) use the contract's own name; if informal (e.g. a scheduled Boomi flow), describe the shape in prose.

### Inbound API / reads
_Who calls in, what they call, what they get back._

- `[[<caller>]] → <endpoint> (auth / version)` — purpose; volume / cadence

### Outbound API / writes
_What this app reaches out to._

- `<endpoint> → [[<callee>]]` — purpose; trigger

### Event streams
#### Produced
- `<topic or event name>` → consumed by [[<consumer>]] — purpose; schema ref
#### Consumed
- `<topic or event name>` ← produced by [[<producer>]] — purpose; schema ref

### Batch / scheduled integrations
- `<cadence>` — direction, system, purpose (e.g. "nightly ETL → [[data-universe]]")

## Internal structure
_Key modules, services, or internal interfaces worth naming. Skip if the app is a single deployable with no internal structure worth surfacing._

## Observability & SLOs
_Optional. Logging, metrics, tracing surfaces; any SLOs in force._

## Known constraints / rough edges
_Why things are the way they are. Deliberate trade-offs, legacy constraints, known debt. This is the context that makes current-state claims understandable._

## Relationships to other architecture pages
_Cross-links to architecture pages for the applications this one talks to most directly._

- [[<other-app>-architecture]] — why they're coupled

## Phase-target pages that delta from this one
_Auto-maintained list of target-state pages that are defined relative to this current state. Updated whenever a new phase target is added or a phase ships (see §5 phase-completion workflow in AGENTS.md)._

- [[<project-slug>-<phase-id>-<app-slug>-architecture]] — project · phase · status

## Superseded claims
_When a phase ships, any current-state claims it invalidates are moved here with a dated note, rather than being silently rewritten. Gives readers the history._

- [YYYY-MM-DD, after [[<project-slug>-<phase-id>]]] — _claim_ → _why it's no longer true_

## Related
- [[<app-slug>]] — identity page (owner, role, aliases)
- [[<project-scoped-dep-map>]] — cross-app view of touchpoints

## Sources
- [[sources/<source-slug>]] — what this source contributed
