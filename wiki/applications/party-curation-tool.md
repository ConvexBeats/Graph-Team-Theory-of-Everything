---
type: application
title: Party Curation Tool
aliases: [pct, new-pct]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope]
owner: graph-team
state: current
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
---

# Party Curation Tool

## Summary
A Next.js-based human interface for curating party data in the [[party-application]]. Today: a front-end on top of a Jira workflow, mapping parties onto third-party services (D&B), resolving spelling, merging near-duplicates, version updates. Target: a rebuilt Next.js UI auto-generated from the new MDM open-API spec, shipping **together** with MDM per [[pct-and-mdm-go-live-together]].

## Aliases / previous names
- PCT
- "Party Curation Tool" (referred to both for the current tool and the rebuild-in-flight; the word "PCT" in this wiki defaults to the new one unless called out)

## Purpose
Human-in-the-loop tooling for:
- Drafting new parties (incl. draft + parent-draft combinations)
- Mapping parties to external references (D&B, S&P)
- Correcting spelling and data errors
- Merging near-duplicate parties
- Version updates
- Managing groupings (One Re, CUO groupings) — manual in Phase 1
- Supporting DataOps curation workflow

## Current state

- **Application owner**: [[graph-team]] ("Party Team" alias)
- **User-side owner**: [[dataops-team]]
- **Production status**: production; Next.js view over a Jira workflow
- **Backing store**: current Neo4j Graph DB (via Party Application's current API surface)

## Target state

- **Tech**: Next.js UI generated from the new MDM **open-API spec**
- **Backing store**: new MDM (Dynamo + OpenSearch)
- **Features**:
  - Better search via OpenSearch (fuzzy matching, misspelling tolerance)
  - Explicit party version control (visible and auditable)
  - Simpler merge flow (with a visible merge history)
  - Clearer provenance on enrichment-created records (D&B, future S&P)
  - Dedicated "tech users" handling for [[high-volume]]-created draft parties (user context differentiated from human DataOps curators)
- **Widget / SDK component** (in-flight design question): an embeddable party-creation / party-lookup component for consumers like [[inrisk]] to use during new-business flows. See the open V2/V3 Chakra question below.

## Transitional states

- The old PCT and new PCT run in parallel for **a 2-day crossover**; no extended dual-operation.
- The new PCT cannot be operationally decoupled from MDM — [[pct-and-mdm-go-live-together]].
- **Single-flag coupled rollout (Session 1)**: the _same_ feature flag that switches the InRisk-embedded party-search widget from old-search to new-search **also** switches old-PCT to new-PCT. Originally Joe considered decoupling these ("one then the other"); agreed to ship them together since they share a domain boundary. One flag, one cutover.

## Audit history — line in the sand

The new PCT will **not** backfill historical Jira audit trails from the current PCT's Jira workflow. SLA reporting, audit analysis, and historical workflow investigation are [[data-universe]]'s job (joins old-Jira tables and new-PCT tables in Snowflake). Jira stays available **read-only** for historical lookup after cutover; the old PCT UI switches off one day to the next.

Formalised as [[no-pct-audit-backfill]]. Mirror of [[no-historic-client-backfill-into-mdm]] on the MDM side.

## Change ownership
- **Working unit**: [[graph-team]]
- **Key contributors**: [[alex-sillars]] (PO), [[joe-worsfold]] (integration surface, widget/SDK contract), [[ben-joseph]] (engineer), [[tomas-sivo]] (Business Analyst — functional requirements)
- **Primary user-side stakeholder**: [[dataops-team]]
- **Related cross-team**: [[prebind-team]] (InRisk widget embedding)
- **Oversight**: [[tech-tooling]]

## Change items recorded in these sessions
- [[pct-and-mdm-go-live-together]] (**accepted**) — and the operational **single-flag coupled rollout** shape confirmed in Session 1
- [[no-pct-audit-backfill]] (**accepted**, new ADR from Session 1 Pass B) — Snowflake is audit/SLA store; Jira stays read-only
- [[bulk-migrations-owned-by-mdm-phase-1]] (**accepted**, new ADR from Session 1 Pass B) — [[graph-team]]-owned CLI in Phase 1; full self-serve UX is Phase 2+ (see [[open-questions#OQ-006]])
- **Party-create timing edge case — acknowledged, not fixed in Phase 1** (Session 1): when InRisk creates a party via the search widget but then never raises a submission, DataOps has no audit trail of _"why does this party exist?"_ Joe's preferred long-term fix — `party.create` sent from InRisk at **submit time** rather than **widget-open time** — is noted but explicitly deferred. Phase 1: accept orphan-party noise as minor.
- **Widget Chakra V2 vs V3 strategy** — deferred; pending InRisk workshop. See [[open-questions#OQ-005]].
  - Options on the table:
    1. Ship a widget on Chakra V2 (matches current InRisk);
    2. Ship a **"Teddy Facts"-style minimal render** version (no Chakra dependency; renders core fields in a very simple way) — keeps things low-risk for now;
    3. Ship only a V3 widget once InRisk upgrades;
  - HV is **not** a blocker on this — HV consumes via Boomi API, not the widget.
  - Phase-1 stance: One Re and CUO groupings remain **manual curation** in PCT; rules-based auto-grouping is Phase 2.

## Pending / unknown
All PCT-facing open items are tracked in [[open-questions]]:
- [[open-questions#OQ-005]] — final Chakra widget strategy (V2 / minimal / V3-only).
- [[open-questions#OQ-014]] — DataOps change-management lead time.
- [[open-questions#OQ-020]] — UX spec for the draft-party + auto-parent D&B flow.
- [[open-questions#OQ-022]] — "tech users" model (HV-created drafts).
- [[open-questions#OQ-006]] — Phase 2+ bulk-migration self-serve UX.

## Related decisions
- [[pct-and-mdm-go-live-together]]
- [[no-pct-audit-backfill]]
- [[bulk-migrations-owned-by-mdm-phase-1]]
- [[d-and-b-caching-and-auto-parent]]

## Related
- [[party-application]] — co-ships
- [[inrisk]] — widget embedder
- [[dataops-team]] — user
- [[graph-team]] — owner
- [[tomas-sivo]] — functional-requirements BA
- [[contract-buckets]] — operational-bucket member
- [[ownership-matrix]] · [[dependency-map]] · [[open-questions]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; single-flag rollout, audit-history line, bulk-migration CLI scope, create-timing edge case
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; co-delivery ADR, widget strategy debate
