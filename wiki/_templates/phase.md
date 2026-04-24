---
type: phase
title: <project-slug> — Phase N (<short label>)
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [phase]
project: <project-slug>
phase_id: phase-N
status: planned       # planned | in-flight | complete | cancelled
gate_date: <YYYY-MM-DD or TBD>
---

<!--
Lives at: wiki/projects/<project-slug>/phase-N/<project-slug>-phase-N.md
Sibling pages in the same phase folder: <project-slug>-phase-N-<app-slug>-architecture.md (phase targets),
<project-slug>-phase-N-<analysis-name>.md (phase-scoped analyses — summaries, digests, retros).
Project-level cross-phase artefacts (dependency map, ownership matrix, project overview) stay one level up,
at wiki/projects/<project-slug>/. See AGENTS.md §3 project/phase cascade entries 2–5.
-->

# Phase N — <short label>

## Summary
_Two or three sentences: what this phase ships, what it doesn't, and why this split._

## Gate
_The hard external date or event that terminates this phase. If none, say so — "triggered by completion of Phase N-1 rather than a fixed date"._

## Scope — applications
_Every application the project touches gets a row, even if the change intensity is `none` in this phase — "`none` with a reason" is an explicit scope call worth recording._

| Application | Change intensity | Shape of change |
|---|---|---|
| [[<app>]] | high/medium/low/none/integration-only | … |

## Decisions in this phase
_ADRs whose `phase:` frontmatter includes this phase. List as `[[<decision-slug>]]` with a one-line summary._

- [[<decision>]] — one-line summary

## Delivery shape
_If the phase follows a named delivery pattern (e.g. [[intercept-backfill-projection]], strangler, big-bang), name and explain it here. Skip if N/A._

## Open questions gating this phase
_The highest-leverage questions in [[open-questions]] filtered by `project: <slug>` and `phase: phase-N`. Link the most critical ones; defer the long tail to the register itself._

## Predecessors / successors
- **Predecessor**: [[<prev-phase>]] or `none — first phase`.
- **Successor**: [[<next-phase>]] or `TBD` or `none — final phase`.

## Done-state
_What the world looks like after this phase lands. A reader who wants to verify "is Phase N done?" should be able to check this list._

## Related
- [[<project-slug>]] — project overview
- [[<project-slug>-dependency-map]] · [[<project-slug>-ownership-matrix]]
- (other relevant concepts / ADRs)

## Sources
- [[sources/<source-1>]] — what it contributed to this phase's definition
