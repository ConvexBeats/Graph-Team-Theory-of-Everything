---
type: application-architecture
title: Party Curation Tool — Current-state Architecture
created: 2026-04-22
updated: 2026-05-26
tags: [architecture, current-state]
application: party-curation-tool
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2, 20260519-mdm-implementation-strategy]
source_count: 3
status: draft
---

# Party Curation Tool — Current-state Architecture

_Living reference for the **as-is** technical shape of [[party-curation-tool]]. Kept current: when a project phase ships, any architecture deltas it introduced are folded into this page and the old claims are moved to **Superseded claims** at the bottom. See AGENTS.md §5.4 for the fold-in workflow._

> **Status: draft.** Current-state PCT scaffolded from Sessions 1 & 2; **new-PCT-under-build** shape added 2026-05-26 from [[joe-worsfold]]'s 2026-05-19 walkthrough ([[sources/20260519-mdm-implementation-strategy]]). Two coexisting realities below: **production PCT today** (Next.js on Jira workflow) and **new PCT under build** (Next.js on the MDM REST API; revisions + filters; mark-for-review / approve / request-changes baked in). Both ship under [[pct-and-mdm-go-live-together]] at the single-flag cutover.

## Summary
PCT is a **Next.js frontend** sitting on top of a curation workflow. **Production today**: workflow is Jira-driven (Jira tickets carry the audit history; data lives in [[party-application]]'s Neo4j). **Under build (Phase-1 target)**: workflow is **revisions-and-filters** — every party change is a revision against MDM (built-in to MDM's versioning), surfaced in PCT with filters in place of Jira's tabs; revision tickets carry **mark-for-review / approve / request-changes** actions baked into the UI. Under [[party-rearch-phase-1]] the Jira-backed workflow is retired live and PCT is re-backed onto the new MDM ([[pct-and-mdm-go-live-together]]). As of 2026-05-19, the new PCT is **fully functional in dev** (cosmetic changes pending); backfill into lower envs is the remaining gap before integration-env exercise.

## Technologies & services

### Production today
- **Frontend**: **Next.js** — confirmed in transcripts.
- **Workflow / ticketing backend**: **Jira** — the curation workflow is Jira-driven today; retired live in Phase 1.
- **Search widget**: UI component with a current/future Chakra-UI-version tension (V2 vs V3) — resolved 2026-05-13 with the two-component-libraries call ([[inrisk-cuts-over-before-high-volume]]); see [[open-questions#OQ-005]].
- **Primary datastore**: reads from / writes to [[party-application]] — the PCT does not have its own authoritative datastore for party data; Jira is the workflow store, not the party store.
- **Audit / history**: Jira audit trail (for workflow events) + [[data-universe]] (Snowflake) for long-horizon analytical queries. Not folded into PCT's own model — see [[no-pct-audit-backfill]] for the explicit boundary.
- **Compute / hosting**: _unknown — pending ingest_
- **Auth**: _unknown — pending ingest_

### New PCT under build (Phase-1 target — fully functional in dev as of 2026-05-19)
From [[joe-worsfold]]'s walkthrough in [[sources/20260519-mdm-implementation-strategy]]:

- **Frontend**: **Next.js** (continues); part of the **MDM monorepo** rather than a separate package.
- **Backend**: **MDM REST API** (auto-generated from code). Jira is gone — the workflow is **revision-based**, backed by MDM's built-in versioning system (every party change is a revision; no Jira ticket layer).
- **UI shape**: **revisions + filters** in place of Jira's tabs. Joe: _"PCT today is obviously a table with tabs for different things, and then when you open the ticket, it's obviously Jira behind it. In the new world … instead of having tabs, you can have filters. And then the actual tickets themselves look different."_
- **Ticket UI**: **mark-for-review / approve / request-changes** baked into the revision UI as first-class actions — _"it'll have the ability to mark for review, approve, or request changes."_
- **Component library**: **Chakra 3 + design system** (the modern stack — Joe's widget on PCT keeps it). PCT is **standalone — no package-dependency clashes**, so it can carry the design system without affecting InRisk. Per [[rory-beattie]] in [[sources/20260519-mdm-implementation-strategy]]: _"curation is standalone — curation doesn't have any package dependency clashes, i.e. design system, whatever else, so we can use Chakra 3 safely for party curation tool, we just can't use it at all for the widget."_
- **Audit / history**: inherits MDM's revision history (built-in), API Gateway access logs (S3-Athena), and Dynamo-stream audit bucket (per [[party-application-architecture]]'s Observability section). Cross-period analytical queries continue to go via [[data-universe]] (Snowflake); no Jira-history backfill ([[no-pct-audit-backfill]]).
- **Functional gap-analysis vs current PCT**: in flight. [[tomas-sivo]] formalised 2026-05-19 as the author of the gap-analysis tickets; the [[graph-team]] squad picks them up in-sprint (per [[joe-worsfold]]: _"we can lean on Tom Ash if it's a case of trying to get the tickets together, and then the rest of the squad can pick up the tickets in sprints"_).
- **DataOps parity confirmation**: open at [[open-questions#OQ-044]] — will DataOps users prefer revisions + filters over the current table + tabs flow, and can they do everything they need?

## Touchpoints

### Inbound API / reads
- User browser sessions — Next.js app; auth mechanism TBC
- _Any programmatic integrations unknown — pending ingest_

### Outbound API / writes
- `→ Jira` — reads and writes curation workflow tickets (current-state; retires in Phase 1)
- `→ [[party-application]]` — reads / writes party data for curation operations; shape / endpoints TBC
- `→ Dun & Bradstreet` lookup — provenance-stamped mappings; see [[d-and-b-caching-and-auto-parent]]
- _Others — pending ingest_

### Event streams

#### Produced
- _unknown — pending ingest (Jira workflow events may or may not be exposed as event streams; likely internal)_

#### Consumed
- _unknown — pending ingest_

### Batch / scheduled integrations
- D&B scheduled refresh loop — refreshed D&B records trigger curation revisions on affected parties; cadence open ([[open-questions#OQ-011]])
- _Others — pending ingest_

## Internal structure
_Populate from raw ingest. The **search widget** is singled out in transcripts as a high-salience component (co-ships with the MDM cutover; Chakra-version tension) — treat it as its own module when populating._

## Observability & SLOs
_Populate from raw ingest._

## Known constraints / rough edges
_From transcripts:_

- **Jira workflow is load-bearing in production today** — curation operations and their audit trail currently require Jira. Phase 1 retires this live (no dual-running) rather than gradually. New-PCT replacement: MDM-backed revisions.
- **No audit backfill** — the new PCT will not carry Jira history across the cutover; Snowflake becomes the cross-period store. This is a deliberate scope call, not a debt. See [[no-pct-audit-backfill]].
- **Search-widget framework split** — PCT uses Joe's **Chakra-3 + design-system widget**; [[inrisk]] uses a **bespoke widget on raw React + close-to-raw CSS** (refined 2026-05-19). PCT being standalone with no package-dependency clashes is what makes carrying the design system safe here, where it isn't safe in InRisk's surface. Closed at [[open-questions#OQ-005]] · refined into [[inrisk-cuts-over-before-high-volume]].
- **Coupled to [[party-application]]'s data model** — PCT is a thin layer above party data; a shift in party data model (as is happening in [[party-rearch]]) forces coordinated change on PCT. Operationally managed via the single-flag coupled rollout ([[pct-and-mdm-go-live-together]]).
- **DataOps parity not yet confirmed** — new-PCT's revisions + filters + baked-in mark-for-review / approve / request-changes flow has not yet been walked with [[dataops-team]] users to confirm it covers everything they need. [[open-questions#OQ-044]].
- **Curation gap-analysis tickets pending** — [[tomas-sivo]] formalised 2026-05-19 as the author; the [[graph-team]] squad picks them up in-sprint. Until the gap analysis lands, the surface area of "what curation features still need to be built into new-PCT" is incomplete.

## Relationships to other architecture pages
- [[party-application-architecture]] — PCT is the curation frontend; the two move together in Phase 1
- [[inrisk-architecture]] — PCT is not a direct integration point for InRisk today, but PCT-driven curation changes propagate downstream via [[party-application]]'s payload events

## Phase-target pages that delta from this one
_Populate as targets are created._

- _(none yet — [[party-rearch-phase-1-party-curation-tool-architecture]] to be created when Phase-1 target detail is available)_

## Superseded claims
_None yet — this page has not yet been through a phase-completion fold-in._

## Related
- [[party-curation-tool]] — identity page (owner, primary user, role)
- [[party-rearch-dependency-map]] — cross-app view of touchpoints
- [[pct-and-mdm-go-live-together]] — the single-flag coupled rollout that governs PCT's Phase-1 transition
- [[no-pct-audit-backfill]] — the audit-history boundary
- [[d-and-b-caching-and-auto-parent]] — D&B integration decisions that PCT participates in

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — audit boundary and bulk-migration CLI scope
- [[sources/20260422-meeting-transcript-session-2]] — Next.js-on-Jira current shape, single-flag rollout, Chakra widget tension
- [[sources/20260519-mdm-implementation-strategy]] — new PCT shape (revisions + filters; mark-for-review / approve / request-changes baked in); PCT keeps Chakra-3 + design system (standalone, no clashes); [[tomas-sivo]] formalised as gap-analysis-ticket author
