---
type: analysis
title: Dependency Map
created: 2026-04-22
updated: 2026-04-22
tags: [standing, dependencies]
application: [party-application, party-curation-tool, inrisk, inrisk-engine, high-volume]
sources: []
source_count: 0
status: stub
---

# Dependency Map

Standing analysis: who consumes Party data, how, and how that shape changes between current and target states. Kept in sync on every ingest.

## Summary
Current state: the [[party-application]] supplies full party payloads to consumers, who snapshot-cache locally.
Target state: the [[party-application]] supplies party IDs + versions; consumers resolve details near real-time.

## Current-state dependencies

| Consumer | Depends on | Contract (current) | Notes |
|---|---|---|---|
| [[inrisk]] | [[party-application]] | Full party payload, snapshot-cached locally | Live in production |
| [[party-curation-tool]] | [[party-application]] | Read/write party records; progress versions | Next.js over Jira workflow |
| [[party-curation-tool]] | Dun & Bradstreet (external) | Third-party enrichment / mapping | Details pending |

## Target-state dependencies

| Consumer | Depends on | Contract (target) | Notes |
|---|---|---|---|
| [[inrisk-engine]] | [[party-application]] | Party ID + version, near-real-time resolve | Co-defining the contract |
| [[high-volume]] | [[party-application]] _or_ [[inrisk-engine]] | Unresolved: direct Party or via InRisk Engine APIs | Forcing-function consumer |
| [[party-curation-tool]] | [[party-application]] | Updated write surface aligned to new version semantics | Must keep working through transition |

## Transitional dependencies

- [[party-application]] serves both old-payload and new-ID contracts in parallel for a period (exact length pending).
- [[inrisk]] continues on the old contract until sunset or migration, whichever comes first.

## Open questions
- Does [[high-volume]] consume [[party-application]] directly or via [[inrisk-engine]]?
- Are there other downstream consumers of [[party-application]] not yet listed here?
- Does the [[party-application]] itself have upstream dependencies on any master-data source aside from D&B via [[party-curation-tool]]?
- What auth / identity boundaries sit between these systems?

## Related
- [[overview]] · [[ownership-matrix]] · [[party-application]]
