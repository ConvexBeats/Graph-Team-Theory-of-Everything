---
type: phase-application-architecture
title: <project-slug> Phase N — <Application Name> Target Architecture
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [architecture, phase-target]
project: <project-slug>
phase: [phase-N]
application: <app-slug>
baseline: <app-slug>-architecture
state: phase-target       # phase-target | shipped | superseded
sources: []
source_count: 0
status: stub              # stub | draft | mature
---

<!--
Lives at: wiki/projects/<project-slug>/phase-N/<project-slug>-phase-N-<app-slug>-architecture.md
Baseline: wiki/applications/<app-slug>/<app-slug>-architecture.md
Sibling pages in the same phase folder: the phase overview <project-slug>-phase-N.md, plus any
phase-scoped analyses <project-slug>-phase-N-<analysis-name>.md.
On phase completion, content here is folded into the baseline per AGENTS.md §5.4, and this page's
state: moves from `phase-target` to `shipped`.
-->

# <project-slug> Phase N — <Application Name> Target Architecture

_Proposed end-of-phase shape for [[<app-slug>]], deltas relative to [[<app-slug>-architecture]]. This page becomes the **new** current-state reference for the app when this phase ships; the baseline is then moved to **Superseded claims** on [[<app-slug>-architecture]]._

## Summary
_One paragraph: how the application looks at end of phase, emphasising what is now different about it._

## Delta from current state

> **This is the page's load-bearing section.** Read this first if you already know the current state and just want to see what's changing.

Pairs with [[<app-slug>-architecture]]. Subsystem-by-subsystem diff:

### Technologies & services
- **Added**: …
- **Removed**: …
- **Swapped** (X → Y): …
- **Unchanged (but worth naming so the reader knows)**: …

### Touchpoints
- **New contracts**: …
- **Retired contracts**: …
- **Changed contracts** (semantic, not just cosmetic): …
- **Unchanged (explicit confirmation)**: …

### Internal structure
- **Added / renamed / retired modules**: …

### Constraints
- **Rough edges that go away**: …
- **New constraints introduced**: …

---

## Full end-of-phase shape

_Same section skeleton as the current-state architecture template, so a reader can read this page as a standalone "future-state" description without having to diff against the baseline in their head. Everything here should be consistent with the Delta section above._

### Technologies & services
- **Primary datastore**: …
- **Secondary stores / caches**: …
- **Compute**: …
- **Event platform / message bus**: …
- **Observability**: …
- **Other infrastructure**: …

### Touchpoints
#### Inbound API / reads
- …
#### Outbound API / writes
- …
#### Event streams
- Produced: …
- Consumed: …
#### Batch / scheduled integrations
- …

### Internal structure
_Key modules, services, or internal interfaces._

### Observability & SLOs
_Optional._

### Known constraints / rough edges
_What the new shape still doesn't solve; what we accept._

---

## Decisions driving this target
_ADRs that explain **why** this target looks the way it does. Each should carry `project: <project-slug>` · `phase: [phase-N]` in its own frontmatter._

- [[<decision-slug>]] — one-line why

## Open questions gating this target
- [[open-questions#OQ-NNN]] — one-line context

## Related
- [[<app-slug>-architecture]] — current-state baseline this page is a delta from
- [[<project-slug>]] — project overview
- [[<project-slug>-<phase-id>]] — the phase this page belongs to
- [[<project-slug>-<phase-id>-<other-app>-architecture]] — sibling target pages for other apps changing in the same phase

## Sources
- [[sources/<source-slug>]] — what this source contributed to this target
