---
type: decision
title: UUID system ID with Graph-ID-backed display ID
created: 2026-04-22
updated: 2026-05-27
tags: [decision, ids, identity]
application: [party-application, party-curation-tool]
owner: graph-team
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: accepted
project: party-rearch
phase: [phase-1]
---

# UUID system ID with Graph-ID-backed display ID

## Summary
In the new MDM, the **system ID** for a party is a **UUID** — machine-facing, opaque, internal. A separate **display ID** field carries the legacy **Graph party ID** (the human-readable number) so that users continue to see the identifiers they are used to during and after migration. Legacy Graph IDs remain searchable and are the "what users see" surface of the party; UUIDs are the "what the system routes on" surface.

## Context
Today's Party Graph generates human-readable integer-based IDs that are used across the business. Users, curators, and downstream consumers refer to parties by these numbers in conversation, audit trails, and tickets. Losing those IDs in the migration would cause unnecessary pain:

- Curators suddenly can't find the party they were working on by the number they have in their head.
- Historical tickets and audit records reference the legacy IDs; those have to remain resolvable.
- External systems that cache party IDs (notably InRisk's current snapshots) carry the legacy IDs in their data.

At the same time, the target architecture benefits from having a properly **opaque, non-sequential, globally-unique** primary identifier for system-to-system routing — one that can be generated without a central allocation service, that doesn't leak volume, and that's safe to use in URLs and events.

The classic split — internal UUID for system routing, external display ID for humans — fits.

## Options considered

1. **Keep the legacy Graph ID as the sole primary ID in the new MDM** (not chosen)
   - Pros: zero transition cost; familiar to everyone.
   - Cons: couples the new system to the old ID-generation mechanism (or requires us to replicate it); inherits the sequential / volume-leaking nature of the Graph ID; not ideal as a URL-safe / event-key identifier long-term.
2. **Use UUIDs only; retire the Graph IDs** (not chosen)
   - Pros: cleanest internal model.
   - Cons: forces every human user and every downstream ID-cache to cutover to a new identifier form simultaneously; high change-management cost; breaks historical ticket / audit references.
3. **UUID system ID + legacy Graph ID preserved as `display_id`** (chosen)
   - Pros: system gets the benefits of UUID routing; humans keep their familiar numbers; downstream systems can resolve on either; historical references stay valid.
   - Cons: two IDs to think about; must be consistent about which is which in APIs, events, and UI.

## Decision
**Option 3**. Every party in MDM has:
- **`id` (UUID)** — system-internal, opaque, the routing key for APIs, events, and cross-reference.
- **`display_id` (integer / Graph-format)** — the legacy Graph party ID, or a new Graph-format ID for parties created post-migration. This is what is shown to users and what curators search by.

Both IDs are indexed; lookups accept either. New parties created post-migration still receive a Graph-format display ID so the user-facing identifier scheme doesn't fragment.

## Consequences

- **Positive**
  - Users see no identifier change on cutover.
  - Historical tickets, audit records, and caches that reference legacy Graph IDs continue to resolve.
  - System gets opaque, collision-safe UUIDs for routing — clean for URLs, event keys, cross-system references.
  - Display-ID scheme can be extended or replaced independently of the system ID scheme later if desired.

- **Negative / trade-offs**
  - Every API / event must be unambiguous about which ID it carries; field naming discipline matters.
  - A generation mechanism for new display IDs is still required (Graph-format or successor scheme) — can't be a stub field.
  - Documentation and training for [[dataops-team]] and any other human users must cover the dual-ID model explicitly so it doesn't surprise anyone.

- **Open risks**
  - External systems that currently cache "the party ID" need explicit guidance on which field to cache going forward. Default assumption: cache the UUID for routing, show the display ID to users. InRisk's three interim changes already bake in "store the party ID" — confirm they're storing the UUID, not the display ID.
  - **[[boomi]] is in this cache-the-ID set and has not been formally guided.** Boomi writes [[ntt]] sanctions results back into Party today (_"we update our party with that"_, [[alex-sillars]], [[sources/20260422-meeting-transcript-session-2]]) — the lookup key is presumably the legacy Graph party ID it sees on the inbound event, but this is not confirmed in any source we hold. The legacy Graph ID survives on every MDM party as `display_id`, so Phase-1 cutover should hold transparently **if** Boomi continues to use that ID. Tracked at [[open-questions#OQ-045]].
  - Proxy events to [[analytics-team]] (DU) must continue to carry the identifier shape the DU currently indexes — verify.

## Supersedes / superseded by
- **Supersedes** the implicit "one Party ID, Graph-format" model of the current system.
- Not yet superseded.

## Related
- [[party-application]] — where the dual-ID model lives
- [[party-curation-tool]] — UI surface that renders display ID and uses system ID behind the scenes
- [[inrisk]] — interim-state change #1 (store party ID) — confirm storing UUID
- [[analytics-team]] — proxy event identifier shape must match their current indexing
- [[graph-team]] — owner

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
