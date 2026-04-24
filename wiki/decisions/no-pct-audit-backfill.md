---
type: decision
title: No Jira audit-history backfill into the new PCT
created: 2026-04-22
updated: 2026-04-22
tags: [decision, migration, scope-bound, pct]
application: [party-curation-tool]
owner: graph-team
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: accepted
---

# No Jira audit-history backfill into the new PCT

## Summary
The new [[party-curation-tool]] will **not** carry across the audit-history trail from the current PCT's Jira-backed workflow. SLA reporting, audit analysis, and historical-workflow investigation are **[[data-universe]]'s job** (Snowflake-side joins across old-Jira tables and new-PCT tables). The current PCT's Jira workflow stays available **read-only** after cutover for ad-hoc historical lookup. The old PCT UI switches off one day to the next — no extended dual-running.

This is a deliberate "line in the sand" decision, directly analogous to [[no-historic-client-backfill-into-mdm]] on the MDM side.

## Context
The current PCT sits on top of a Jira workflow — every curation action (create, merge, version bump, rename, D&B mapping, etc.) produces a Jira ticket with a full audit trail (who, when, what changed, what was approved). This trail is used today for:

- SLA reporting ("how long did this curation take?")
- Audit questions after the fact ("why did this party change?")
- Incident investigations ("which curator made this change?")

The new PCT is being built on a completely different backend (open-API spec over MDM) with its own native audit model (revision events on the party version). Two options exist for what to do with the years of Jira audit history:

1. **Backfill** — pull all Jira audit tickets into the new PCT's audit model so the new UI answers historical questions as well.
2. **Leave it behind** — the Jira data already exists and is already analysable from Snowflake; don't duplicate.

The room settled on option 2 in Session 1. The same reasoning applies as for [[no-historic-client-backfill-into-mdm]]: the data is not being lost (Jira persists; Snowflake holds the aggregate); the backfill would require building a mapping layer between two fundamentally different audit shapes; and no consumer in the room could name a workflow that would break without it.

## Options considered

1. **Full Jira audit backfill into the new PCT's audit model.**
   - Pros: one UI answers all historical questions; no two-system cognitive load for curators.
   - Cons: large mapping effort between Jira's ticket-based workflow semantics and the new PCT's revision-event model; ongoing compatibility debt if Jira schema shifts; uncertain value (no consumer has asked).

2. **Partial backfill** (e.g. last 12 months, or high-signal ticket types only).
   - Pros: compromise.
   - Cons: arbitrary cut-off line; curators still have to learn "before date X, look in Jira; after date X, look in PCT"; pays the mapping cost without fully removing the two-system reality.

3. **No backfill. Old Jira stays read-only; Snowflake is the unified audit/SLA store. (chosen.)**
   - Pros: smallest possible surface area; cheapest; ships soonest; mirrors the MDM-side decision; existing Snowflake joins already support the cross-period view analytically.
   - Cons: the new PCT doesn't _natively_ answer "show me this party's curation history before cutover"; anyone who wants a single-pane view has to go to Snowflake.

## Decision
**Option 3.** The new PCT starts its audit history at cutover. The current Jira workflow is preserved read-only post-cutover for ad-hoc lookup. Snowflake is the cross-period audit/SLA store.

Concretely:

- The new PCT surfaces only post-cutover revisions (its own native audit events).
- Jira remains accessible (read-only) to anyone who needs pre-cutover ticket-level detail.
- SLA dashboards and audit analyses are built in Snowflake with joins across old-Jira tables and new-PCT tables.
- The old PCT UI switches off on cutover day; no extended dual-running UI.

## Consequences

- **Positive**
  - No custom mapping layer between Jira's workflow model and the new PCT's revision model.
  - Cutover scope shrinks — no long-running ETL job in the critical path.
  - Consistent principle with [[no-historic-client-backfill-into-mdm]]: each new store starts its history at cutover; historical detail lives where it was originally generated.
  - [[analytics-team]] / Snowflake is already the reporting locus — reinforces the canonical analytical path.

- **Negative / trade-offs**
  - The new PCT doesn't natively show pre-cutover curation history for a party; users need to know to look in Jira.
  - Anyone wanting a uniform view (for an audit pack, say) has to write a Snowflake query — no one-click UI for that.
  - Slightly more onboarding for new curators ("Jira is read-only history; PCT is live curation").

- **Open risks**
  - If a regulatory / audit requirement later emerges that _requires_ a unified PCT-surface audit view including pre-cutover Jira data, we revisit — probably as a targeted ingest into Snowflake (not into PCT).
  - Jira's own lifecycle — if Convex decides to retire the Jira instance, the read-only access contract needs to be preserved (or the data archived in Snowflake).

## Relationship to other decisions
- [[no-historic-client-backfill-into-mdm]] — mirror decision on the MDM side. Same principle: history starts at cutover; historical detail lives where it originated; [[data-universe]] is the cross-period store.
- [[pct-and-mdm-go-live-together]] — this decision is a sub-decision of the "ship PCT + MDM as one release" posture: one of the reasons that posture works is that neither side carries an audit-backfill burden.

## Supersedes / superseded by
- Does not supersede any prior ADR. Not yet superseded.

## Related
- [[party-curation-tool]] — primary application
- [[data-universe]] · [[analytics-team]] — canonical audit/SLA store
- [[dataops-team]] — primary consumer of PCT audit data; user-side stakeholder
- [[graph-team]] — owner / implementer

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — locus of the decision (morning session, PCT migration-considerations segment)
