---
type: application
title: Party Curation Tool
aliases: [pct]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, frontend]
owner: graph-team
state: current
sources: []
source_count: 0
status: stub
---

# Party Curation Tool

## Summary
The human-facing companion to the [[party-application]]. A Next.js frontend over a Jira workflow that lets users curate party records — fix spellings, merge near-duplicates, map to third-party identifiers (e.g. Dun & Bradstreet), and progress version updates.

## Aliases / previous names
- PCT

## Purpose
Keep the party dataset clean and correctly cross-referenced. Without PCT, the [[party-application]]'s data quality drifts; with PCT, curation work is visible, assignable, and auditable via the Jira workflow underneath.

## Current state
- **Owner (application)**: [[graph-team]]
- **Primary users**: [[dataops-team]]
- **Tech stack**: Next.js frontend; Jira workflow as the backing state machine
- **Production status**: in production
- **Integrations**: third-party enrichment services incl. Dun & Bradstreet (_extent pending_)
- **Upstream dependency**: [[party-application]] (reads and writes party records)

## Target state
> ⚠ Mostly open. PCT must keep working throughout the [[party-application]] re-architecture, so its own target state is driven by whatever changes in Party's contracts.

- Likely needs to speak the new party ID + version contract when writing updates.
- Versioning model may change what "updating a party" means from a UX standpoint (explicit new version vs. in-place edit).
- Merge workflows must remain correct as the version graph grows more structured.

## Transitional states
- Continues operating against current Party contracts while they exist.
- May need to dual-write or route through a shim during the consumer-by-consumer migration.

## Change ownership
- **Working unit**: [[graph-team]]
- **User-side stakeholder**: [[dataops-team]] (operational impact, UX changes)

## Pending / unknown
- Exact catalogue of curation actions supported today (spell-fix, merge, D&B map, version bump, … anything else?).
- How Jira is wired under the Next.js view — issue-per-curation-task?
- Authorisation model: who can merge, who can approve new versions.
- Impact of the new version semantics on the merge workflow.

## Related decisions
_None yet._

## Related
- [[party-application]] — the service it curates
- [[graph-team]] — application owner
- [[dataops-team]] — primary users

## Sources
_None yet._
