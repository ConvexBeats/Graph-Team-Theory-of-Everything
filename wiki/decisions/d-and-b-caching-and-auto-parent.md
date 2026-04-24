---
type: decision
title: D&B caching in Dynamo + automatic parent creation
created: 2026-04-22
updated: 2026-04-22
tags: [decision, enrichment, operations]
application: [party-application, party-curation-tool]
owner: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: accepted
project: party-rearch
phase: [phase-1]
---

# D&B caching in Dynamo + automatic parent creation

## Summary
When a party is found in **Dun & Bradstreet** (D&B) during curation, the new MDM will (a) cache the D&B lookup response in **DynamoDB with a TTL** to avoid repeated external calls and (b) **automatically create the draft party and its ultimate-parent entry** in one operation, with full provenance. This replaces the current double-gated flow where the curator has to create the parent via a separate curation ticket.

## Context
Under the current Graph DB pattern, a D&B hit drives a two-step curation: the draft party is created, and then a second curation ticket is raised to create or link the ultimate-parent entry. This is operationally painful for [[dataops-team]] — two tickets, two context switches, for what is one piece of externally-verified structure. It is also unnecessary: D&B already returns the parent relationship; we've been re-asking a curator to confirm what D&B has told us.

Simultaneously, the D&B lookup call is **external, rate-limited, and not free**. The current system calls out on every relevant curation event; there's no local cache. Results are deterministic per company-record, so caching is a clear win.

The new MDM has DynamoDB available natively — using a TTL-keyed cache there is trivial.

## Options considered

1. **Keep the current double-gated curation flow** (not chosen)
   - Pros: status quo; no change-management impact on curators.
   - Cons: operational friction; redundant human step; unnecessary load on the D&B API.
2. **Auto-create the draft party but still require a second ticket for the ultimate-parent** (not chosen)
   - Pros: incremental change; preserves the current parent-creation audit path.
   - Cons: half-fix; curator still has to do the second ticket; no reason to preserve a control that's really just a rubber-stamp given D&B has already authoritatively stated the parent.
3. **Cache D&B responses + auto-create draft + auto-create ultimate-parent with provenance** (chosen)
   - Pros: single-step curation path; lower D&B spend; explicit provenance on auto-created records so they remain auditable; simpler DataOps flow (fewer tickets, fewer mis-linked parents).
   - Cons: records created by this flow need to be visibly marked as enrichment-created (vs human-curated) so that any later corrections aren't accidentally treated as a human-owned choice. MDM and PCT must both render this provenance clearly.

## Decision
**Option 3**. On D&B match:
- The draft party is auto-created in MDM.
- The ultimate-parent is auto-created (if not already present) and linked, in the same atomic operation.
- Both records carry **full provenance** indicating D&B as the source and the lookup timestamp.
- The D&B response is **cached in DynamoDB with a TTL** (TTL duration to be finalised by the enriched team; initial assumption: days-to-weeks scale, not hours).
- On cache hit, no external D&B call is made.
- Multi-level parent chains are **not** in scope — only ultimate-parent, same as today (in-room agreement).

### Refresh loop (Session 1 addition)
Refresh is driven by a **scheduled job** (monthly or quarterly — cadence to be agreed) that re-calls D&B for cached records and, if the returned record differs from the cached one, **spawns a curation revision** on the affected party. This makes enrichment drift an auditable event rather than a silent overwrite. Scheduled-refresh ownership sits with [[joe-worsfold]] as an open action; cadence decision pending.

## Consequences

- **Positive**
  - DataOps curation flow collapses from double-ticket to single-step for D&B-found parties.
  - D&B API spend / rate exposure reduced.
  - Ultimate-parent relationships stay consistent across records that share a parent (one cached response serves all).
  - Clearer provenance story for audit.

- **Negative / trade-offs**
  - Auto-created parent records must be unmistakably marked as enrichment-created — ambiguity here could let human curators overwrite D&B-sourced facts without realising.
  - TTL choice matters: too short and the caching value evaporates; too long and stale parent relationships sit in the tree longer than they should. Needs the enriched team's input.
  - Cache invalidation policy on explicit re-lookup or record merge needs to be specified.

- **Open risks**
  - Need to confirm with the enriched team that the cache-in-Dynamo pattern aligns with whatever they're building on their side ([[joe-worsfold]] open action).
  - UX on [[party-curation-tool]] must make auto-created parents visible and editable, not invisible / locked.

## Supersedes / superseded by
- **Supersedes** the current double-ticket curation pattern for D&B-found parties.
- Not yet superseded.

## Related
- [[party-application]] — where caching and auto-creation happen
- [[party-curation-tool]] — where auto-created records are surfaced with provenance
- [[graph-team]] — owner
- [[dataops-team]] — primary beneficiary of the simpler flow
- "enriched team" (name TBD) — alignment counterparty

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — scheduled-refresh / revision-on-change implementation note
- [[sources/20260422-meeting-transcript-session-2]] — original decision locus
