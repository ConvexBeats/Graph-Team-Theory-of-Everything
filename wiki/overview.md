---
type: portfolio-overview
title: Portfolio Overview
created: 2026-04-22
updated: 2026-04-22
tags: [portfolio]
status: evolving
---

# Portfolio Overview

_Top-level view across all projects captured in this wiki. Each project has its own project page with the full thesis, phases, dependency map, ownership matrix, and ADRs. This portfolio page only carries what makes sense **across** projects._

## Projects in flight

| Project | Slug | Driver | Primary apps affected | Phase status | Primary owner |
|---|---|---|---|---|---|
| [[party-rearch]] | `party-rearch` | Re-architecture of [[party-application]] from Neo4j large-payload snapshots to Dynamo+OpenSearch versioned references. Forcing-function: [[high-volume]] go-live on 1 Sep. | [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]], [[eclipse]] | [[party-rearch-phase-1]] in-flight · [[party-rearch-phase-2]] planned | [[graph-team]] ([[alex-sillars]]) |
| [[secrets-management]] | `secrets-management` | ASG-mandated transition to a standardised secrets-management framework following **Shai-Hulud 2.0 (INC-140574)**. Eliminate long-lived CI/CD credentials; centralise service secrets in AWS Secrets Manager under a unified schema; enable automated rotation. | TBD (expected to be cross-portfolio — every app running CI/CD or holding a service secret) | [[secrets-management-phase-1]] planned | TBD |

## Portfolio themes

_Patterns and dependencies that cut across projects. Populated as cross-project evidence accumulates — currently thin because most content is party-rearch-derived._

- **Shared deadline pressure on Phase 1 of party-rearch.** The 1 September HV gate is a party-rearch concern, but any secrets-management change that touches [[party-application]], [[party-curation-tool]], or [[inrisk]] in the same window becomes a _cross-project_ concern. Coordinate sequencing.
- **Cross-cutting applications.** [[party-application]], [[inrisk]], and [[party-curation-tool]] are all likely in scope for both projects. First concrete question for `secrets-management` ingest: confirm scope and identify which of those applications' Phase 1 secrets-handling changes overlap the party-rearch cutover window.
- **Shared ownership bottlenecks.** [[graph-team]] and [[prebind-team]] are already fully-loaded on party-rearch Phase 1; any secrets-standardisation ask against those teams in the 1-Sep window is a prioritisation conflict to surface early.

## Cross-project open questions

See [[open-questions]] — questions with `project: portfolio` cut across projects; questions with a specific project slug are scoped to that project.

## How to read this wiki

- **Start here** for the portfolio view; click into any project page for its full thesis.
- **Each project** has its own `projects/<slug>/` folder containing: project overview, phase pages, a project-scoped dependency map, and a project-scoped ownership matrix.
- **Applications, teams, people, and platforms** are **cross-project** — one page per entity, with backlinks showing which projects touch it.
- **Decisions (ADRs)** carry a `project:` + `phase:` frontmatter field identifying their scope.
- **Sources** carry a `project:` frontmatter field identifying which project they were collected for.
- **Open questions** live in one global register ([[open-questions]]) with a `project:` column so questions can be filtered by project.

## Related
- [[index]] — full catalog of pages across the wiki
- [[open-questions]] — global register, project-filterable
- [[party-rearch]] · [[secrets-management]]
