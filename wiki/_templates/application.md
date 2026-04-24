---
type: application
title: <Application Name>
aliases: []
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [application]
owner: <team-slug>
state: current
projects: []
sources: []
source_count: 0
status: stub
---

<!--
Lives at: wiki/applications/<app-slug>/<app-slug>.md
Sibling pages in the same folder: <app-slug>-architecture.md (current-state architecture),
and optional richer per-app pages such as <app-slug>-api-contracts.md, <app-slug>-data-model.md,
<app-slug>-runbook.md, diagrams/, etc. See AGENTS.md §3 cascade entries 5 & 7.
-->

# <Application Name>

> **Architecture:** detailed current-state tech, touchpoints, and constraints live on [[<app-slug>-architecture]]. Phase-target shapes live under `wiki/projects/<project-slug>/` as `<project-slug>-<phase-id>-<app-slug>-architecture` pages.

## Summary
1–3 sentences. What the application is and why it matters across the projects that touch it. Keep architecture detail on the architecture page, not here.

## Aliases / previous names
- …

## Purpose
What the application does, for whom.

## Current state
- **Tech stack**: …
- **Owner**: [[team-slug]]
- **Production status**: …
- **Key interfaces / APIs**: …
- **Upstream dependencies**: [[other-app]]
- **Downstream consumers**: [[other-app]]

## Target state
What changes in the re-architecture. If unknown, list questions under Pending.

## Transitional states
Any intermediary / in-flight states between current and target — what they support and when they retire.

## Change ownership
- **Working unit**: [[team-slug]]
- **Workstreams**: …
- **Other teams involved**: …

## Pending / unknown
- …

## Related decisions
- [[decisions/...]]

## Related
- [[<app-slug>-architecture]] — current-state architecture (tech / touchpoints / constraints)
- [[app-or-team]] — relationship

## Sources
- [[sources/...]]
