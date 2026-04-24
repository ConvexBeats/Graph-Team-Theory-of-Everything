---
type: project
title: Standardise Secrets Management
aliases: [standardise-secrets, secrets-standardisation]
created: 2026-04-22
updated: 2026-04-22
tags: [project, security, stub]
slug: secrets-management
status: planned
applications: []
sources: []
source_count: 0
---

# Standardise Secrets Management

_Project stub. Seeded from the project brief supplied on 2026-04-22; will flesh out on first ingest._

## Thesis
Following the **Shai-Hulud 2.0 supply-chain attack (INC-140574)**, the **Architecture Steering Group (ASG)** has mandated a transition to a standardised secrets-management framework. The goal is to:

1. **Eliminate long-lived credentials in CI/CD** — replace static tokens and access keys with short-lived, workload-identity-driven credentials.
2. **Centralise service secrets in AWS Secrets Manager** under a **unified schema** — consistent naming, tagging, ownership metadata, and access control across every service in the portfolio.
3. **Enable automated rotation** — so that secret compromise stops being a standing risk rather than an operational exception.

## Driver
- **INC-140574** — Shai-Hulud 2.0 supply-chain attack. Underlying concern: long-lived credentials in the CI/CD surface are a blast-radius multiplier for any upstream compromise.
- **Governance**: ASG-mandated — this is not a team-level initiative but a portfolio-level directive.

## Scope (to confirm)
This project is expected to be **cross-portfolio** — every application running CI/CD or holding service secrets is potentially in scope. Concrete scope set to be confirmed in the first ingest (project-kickoff meeting, spec, or ASG brief).

| Application / system | Expected Phase-1 exposure | Notes |
|---|---|---|
| (TBD — enumerate after first ingest) | — | Expected candidates: [[party-application]], [[party-curation-tool]], [[inrisk]], [[inrisk-engine]], [[high-volume]], and their CI/CD pipelines. Every non-stub application page currently in the wiki is a candidate until scoped out. |

## Phases
- [[secrets-management-phase-1]] — **planned**. Scope, target systems, and timeline to be defined from first ingest.

## Cross-project considerations (portfolio-level)
- **Overlap with [[party-rearch-phase-1]]**: several applications in-flight for [[party-rearch]] (notably [[party-application]], [[party-curation-tool]], [[inrisk]]) are very likely in scope for this project too. Any secrets-handling change landing on those applications between now and 2026-09-01 must be coordinated with [[party-rearch-phase-1]] — change-window conflicts and team-bandwidth conflicts on [[graph-team]] and [[prebind-team]] are the first things to triage.

## Open questions (initial)
These will be moved into the global [[open-questions]] register at first ingest with the `project: secrets-management` tag:

- Which team owns delivery of this project?
- Which applications are in Phase-1 scope? Which are Phase 2+?
- What is the target date for Phase-1 delivery, and what is the relationship to the ASG mandate's stated deadline (if any)?
- Is the unified schema already defined in an ASG document, or does this project own its design?
- Is AWS IAM Roles Anywhere / OIDC federation the chosen mechanism for CI/CD short-lived credentials, or is that still a design call?
- Which CI/CD platforms need integration (GitHub Actions, Jenkins, GitLab, AWS CodeBuild, Boomi pipelines)?
- What is the rotation target — cadence, automation level, and failure-mode behaviour?

## Related
- [[overview]] — portfolio view
- [[secrets-management-phase-1]]
- [[party-rearch]] — other in-flight project; coordination required on shared applications

## Sources
- _None yet — awaiting first raw source (ASG brief, kickoff transcript, or spec)._
