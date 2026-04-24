---
type: decision
title: Feature-tagging moves to InRisk ownership (Phase 2+)
created: 2026-04-22
updated: 2026-04-22
tags: [decision, ownership, phase-2, feature-tagging]
application: [party-application, inrisk]
owner: graph-team
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: accepted
---

# Feature-tagging moves to InRisk ownership (Phase 2+)

## Summary
Feature-tagging — a Postgres-backed tag system currently hosted in the [[graph-team]]'s surface (Postgres table + a purpose-built widget used by the underwriting community) — is, by domain, an **InRisk concern**. Session 1 confirmed it was only built on our side because _"we had a nice widget"_. In Phase 1, feature-tagging stays exactly as it is today (unchanged Postgres table and widget component). In **Phase 2+**, both the Postgres table **and** the widget component migrate to [[inrisk]] ownership under [[prebind-team]]; [[graph-team]] no longer maintains them.

## Context
Today's arrangement:

- There is a Postgres table holding feature-tag data, physically owned by [[graph-team]] and referenced by the underwriting community.
- There is a widget — originally built for [[party-curation-tool]]-style uses — that was repurposed for feature-tagging because it happened to render the right shape.
- The relationship with the underwriting teams is held by **Michael Hay** ([[open-questions#OQ-027]]).

Two structural problems with the status quo:

1. **Domain mismatch.** Feature-tagging is about the insurance business (which party features / characteristics drive underwriting treatment), not about party identity or curation. It's an InRisk domain responsibility that happens to sit on a Party-team-owned substrate.
2. **Blast radius on the re-architecture.** Keeping feature-tagging in [[graph-team]]'s surface means every Party-side change has to consider its impact on an InRisk-domain concern; complicates the [[party-application]] re-architecture unnecessarily.

Joe Worsfold, Session 1: _"it's an InRisk responsibility, really, it's their domain. They just asked us to do it because we had a nice widget."_

## Options considered

1. **Migrate feature-tagging to InRisk in Phase 1** (rejected).
   - Pros: cleanest end-state; no scope drift.
   - Cons: adds meaningful scope to Phase 1 when [[prebind-team]] is already taking on three interim changes and the 1 September HV deadline is the forcing function. No room for another migration.

2. **Keep feature-tagging permanently in the Graph-Team surface** (rejected).
   - Pros: no migration needed.
   - Cons: perpetuates the domain mismatch; continues to tax every Party-side change with an InRisk-concern impact check; leaves the [[graph-team]] owning a substrate for a business concern it doesn't represent.

3. **Leave in place for Phase 1; migrate (table + widget component) to InRisk in Phase 2+** (chosen).
   - Pros: zero Phase 1 scope impact; respects the "InRisk does the minimum for Phase 1" pillar; still gets to the clean end-state eventually.
   - Cons: the programme carries the domain mismatch through Phase 1; Phase 2 trigger condition needs to be set explicitly (see [[open-questions#OQ-019]]).

## Decision
**Option 3.** In Phase 2+, the Postgres feature-tagging table migrates from [[graph-team]] infrastructure to [[prebind-team]] / [[inrisk]] infrastructure, and the widget component's maintenance moves to [[prebind-team]] at the same time.

Phase 1: no change — feature-tagging stays on its current Postgres table and widget in their existing locations.

## Consequences

- **Positive**
  - Phase 1 scope stays disciplined; [[prebind-team]] is not asked to absorb this on top of the three interim changes.
  - Long-term ownership aligns with the domain: an InRisk-business concern owned by the InRisk-owning team.
  - Simplifies the post-Phase-1 [[graph-team]] surface — one less non-Party-domain thing to maintain.
  - Underwriting-community relationship (held by Michael Hay, per [[open-questions#OQ-027]]) transfers to the team whose domain the thing actually represents.

- **Negative / trade-offs**
  - The migration itself is a real piece of work in Phase 2+: moving a Postgres table with live readers, and porting or handing over a React widget component. Estimation deferred.
  - Until the migration happens, every Party-side change has to remember that feature-tagging lives on our substrate (mental-model cost).
  - No concrete trigger date or condition is set today — tracked as [[open-questions#OQ-019]]; risk is that without a trigger it perpetually slips.

- **Open risks**
  - Underwriting-community disruption during migration — the widget surface is familiar; a handover needs good change-management.
  - If [[prebind-team]] is building on [[inrisk-engine]] by the time the migration happens, there's a choice to be made between migrating into [[inrisk]] (IR2) or [[inrisk-engine]] directly — deferred.

## Relationship to other decisions
- Reinforces the "**InRisk does the minimum for Phase 1**" pillar (see [[overview]]). Feature-tagging is deliberately _off_ the Phase 1 list.
- Companion to [[strangle-the-graph-via-proxy-events]] in the sense that both are about bounding the Phase 1 surface so the migration ships.
- Does **not** depend on [[inrisk-engine]]'s go-live; migration can happen into [[inrisk]] (IR2) well before InRisk Engine matures. This is explicit — Phase 2+ feature-tagging ownership is not gated on the final-state Party contract.

## Supersedes / superseded by
- Does not supersede any prior ADR. Not yet superseded.

## Related
- [[party-application]] — current (Phase 1) host of the Postgres table and widget component
- [[inrisk]] — Phase 2+ owner of both
- [[prebind-team]] — team accountable for the migration and subsequent maintenance
- [[graph-team]] — Phase 1 owner; hands over in Phase 2+
- [[ownership-matrix]] — migration is tracked as an action; Phase 2 trigger condition pending
- [[open-questions#OQ-019]] — trigger condition / timing

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — locus of the decision (morning session, feature-tagging discussion)
