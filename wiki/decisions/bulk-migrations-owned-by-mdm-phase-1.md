---
type: decision
title: Bulk migrations owned by the MDM team in Phase 1 (CLI-first)
created: 2026-04-22
updated: 2026-04-22
tags: [decision, ownership, phase-1, tooling]
application: [party-application, party-curation-tool]
owner: graph-team
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: accepted
project: party-rearch
phase: [phase-1]
---

# Bulk migrations owned by the MDM team in Phase 1 (CLI-first)

## Summary
Bulk data-migration operations on the new MDM (correcting many parties at once, applying a mapping rule, reassigning groupings, etc.) are, **in Phase 1**, owned and executed by the **[[graph-team]]** via a CLI tool: CSV in → diff preview → bulk auto-approved revision. [[dataops-team]] does not get a self-serve bulk UI in Phase 1; the full self-serve surface (CSV upload in [[party-curation-tool]] + diff preview + revert-button-on-revision) is explicitly **Phase 2+**.

## Context
[[dataops-team]] has historically asked to self-serve bulk fixes — they see the pattern most often, and routing every bulk correction through [[graph-team]] creates a bottleneck. The pull for a self-serve UI is real and correct.

However, a full self-serve UI for bulk migrations is **not a small feature**:

- It requires a CSV upload UX in PCT with schema-aware validation.
- It requires a diff-preview that shows the impact of the proposed change on every affected party, including derivable cross-party effects (parent links, grouping membership, version bumps).
- It requires a revert-button-on-a-revision model that is robust to intermediate user actions.
- It requires an RBAC decision about who can actually _execute_ bulk changes vs. only propose them.

Each of those is its own implementation and UX conversation. Taken together, they would be substantial scope addition in a Phase 1 that is already bounded by the 1 September HV deadline (see [[party-rearch#Pillars]] and [[party-rearch-phase-1]]).

At the same time, bulk-migration capability _is_ needed in Phase 1 — the migration itself (backfill, corrections during stabilisation, ad-hoc fixes) is a bulk-migration use case by nature.

The compromise: solve the Phase 1 _capability_ need with a thin CLI owned by [[graph-team]]; defer the DataOps _user-experience_ need to Phase 2+.

## Options considered

1. **Build the full self-serve bulk UI in [[party-curation-tool]] for Phase 1** (rejected).
   - Pros: gives [[dataops-team]] what they've asked for; no Phase 2+ migration to manage later.
   - Cons: large scope addition on a programme already constrained by the HV forcing function; adds UX + RBAC decisions with no existing pattern to copy; delays the cutover.

2. **Manual-only bulk changes via PCT's single-party flow in Phase 1** (rejected).
   - Pros: no new tooling needed.
   - Cons: unworkable — the backfill alone requires bulk operations; DataOps can't realistically do thousands of single-party edits; blocks cutover.

3. **MDM-team-owned CLI tool in Phase 1; full self-serve in Phase 2+** (chosen).
   - Pros: solves the capability need; scope is bounded (CLI + diff preview + auto-approved revision); owner is clear; Phase 2+ self-serve can be designed properly once the Phase 1 integration surface has settled.
   - Cons: [[dataops-team]] continues to route bulk requests through [[graph-team]] in Phase 1; throughput is gated on [[graph-team]] availability; the "self-serve" pull remains unresolved until Phase 2+.

## Decision
**Option 3.** Phase 1 bulk-migration tool:

- **Shape**: a CLI owned by [[graph-team]].
- **Flow**: CSV input → diff preview of the proposed changes → bulk auto-approved revision on the affected parties.
- **Scope**: used for programme-internal migrations (backfill, corrections, mapping rules) and ad-hoc DataOps requests that arrive via [[graph-team]].
- **RBAC**: execution authority stays with [[graph-team]] in Phase 1.

Phase 2+ self-serve is explicitly **deferred** — scope includes CSV upload in [[party-curation-tool]], diff preview in the UI, revert-button-on-revision, and a worked-out RBAC model. See [[open-questions#OQ-006]] for the Phase 2+ UX question.

## Consequences

- **Positive**
  - Phase 1 bulk-migration capability exists without adding a full self-serve UX to the critical path.
  - CLI ownership is clear — [[graph-team]] builds it, [[graph-team]] runs it.
  - The backfill and cutover work have an operational tool they can actually use.
  - Phase 2+ self-serve design can be informed by what the CLI actually gets used for (real-data usage patterns before the UX decisions get locked in).

- **Negative / trade-offs**
  - [[dataops-team]]'s self-serve ask remains unfulfilled through Phase 1; continues to route requests through [[graph-team]].
  - [[graph-team]] absorbs the throughput cost of being the bulk-execution bottleneck for the duration of Phase 1.
  - The CLI, being internal, won't have the UX polish a curator-facing UI would — care needed to make the diff-preview unambiguous.

- **Open risks**
  - If DataOps volume spikes post-cutover (likely during stabilisation), [[graph-team]] capacity on CLI runs could become a bottleneck. Mitigation: clear prioritisation and a rate-limiting agreement with [[data-quality-team]] ([[hugh-lobban]]) on which requests are live-critical.
  - CSV schema drift over time — as MDM evolves, the CLI input schema needs to keep pace. Needs a versioning convention from day one.
  - Phase 2+ might not happen if the CLI works "well enough" — see [[open-questions#OQ-006]] for the explicit trigger.

## Relationship to other decisions
- Complements [[pct-and-mdm-go-live-together]] by _not_ adding bulk-migration UX to the Phase 1 PCT scope.
- Independent of [[d-and-b-caching-and-auto-parent]], though the same MDM-team-owned CLI surface could host D&B-driven batch refreshes if that's the simplest path (implementation detail, not ADR scope).
- Reinforces the "**End-state items stay on the roadmap, off the critical path**" pillar — self-serve is roadmap-visible but explicitly post-Phase-1.

## Supersedes / superseded by
- Does not supersede any prior ADR. Not yet superseded.

## Related
- [[party-application]] — home of the CLI
- [[party-curation-tool]] — Phase 2+ home of the self-serve UI
- [[graph-team]] — Phase 1 owner
- [[dataops-team]] — Phase 1 requestor; Phase 2+ user
- [[data-quality-team]] · [[hugh-lobban]] — business sponsor for the DataOps prioritisation conversation
- [[open-questions#OQ-006]] — Phase 2+ UX scope

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — locus of the decision (morning session, DataOps self-serve discussion)
