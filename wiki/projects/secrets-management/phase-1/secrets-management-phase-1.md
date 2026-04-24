---
type: phase
title: secrets-management — Phase 1 (stub)
created: 2026-04-22
updated: 2026-04-22
tags: [phase, planned, stub]
project: secrets-management
phase_id: phase-1
status: planned
gate_date: TBD
---

# Phase 1 — stub

_Placeholder. Will be expanded from the first ingest (ASG mandate document, kickoff meeting transcript, or equivalent)._

## Gate
TBD — ASG-mandated deadline (if stated) to be captured on first ingest.

## Expected Phase-1 shape
Likely to include some combination of:

- **Eliminate long-lived credentials in CI/CD** — replace with short-lived, federated credentials (probable mechanism: OIDC federation to AWS IAM Roles). Scope: a named set of high-priority pipelines.
- **Establish unified secrets schema in AWS Secrets Manager** — naming convention, required tags (owner, environment, service, rotation-policy), access-control patterns.
- **Stand up rotation automation** for secrets where the upstream provider supports it.
- **Migrate a first tranche of services** onto the unified schema — not all services, prioritising those with greatest blast-radius exposure.

Explicit Phase-1 boundaries (what's **not** in this phase — deferred to Phase 2+) to be confirmed at kickoff.

## Scope — applications
TBD at kickoff. Candidates to evaluate:

- [[party-application]] · [[party-curation-tool]] · [[inrisk]] · [[inrisk-engine]] · [[high-volume]]
- Any other portfolio application with CI/CD or stored service secrets.

## Decisions in this phase
_None yet — will be promoted from the first ingest as appropriate._

## Open questions gating this phase
See [[open-questions]] (filter by `project: secrets-management`, `phase: phase-1`). Initial seeded set is on [[secrets-management]]; these will be assigned `OQ-NNN` IDs on first ingest.

## Cross-project coordination
- **[[party-rearch-phase-1]]** overlap — if any of [[party-application]], [[party-curation-tool]], or [[inrisk]] are in Phase-1 scope for this project, the change-window and team-bandwidth overlap with party-rearch's 1-Sep gate must be surfaced. Likely the first scheduling question.

## Predecessors / successors
- **No predecessor** — first phase.
- **Successor**: TBD.

## Done-state
TBD at kickoff.

## Related
- [[secrets-management]] — project overview
- [[overview]] — portfolio view
- [[party-rearch-phase-1]] — coordinate scheduling if applications overlap

## Sources
- _None yet._
