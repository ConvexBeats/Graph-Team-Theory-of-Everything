---
type: decision
title: No historic InRisk-client backfill into MDM
created: 2026-04-22
updated: 2026-04-22
tags: [decision, migration, scope-bound]
application: [party-application, inrisk]
owner: graph-team
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: accepted
project: party-rearch
phase: [phase-1]
---

# No historic InRisk-client backfill into MDM

## Summary
MDM will **not** be populated with InRisk's historic per-client-ID snapshots (i.e. "what did this party look like _as viewed by InRisk_ at this specific past moment?"). MDM carries full party **version history** from cutover forward; historical _as-of-date_ queries against past InRisk-client-ID views are served by existing systems — [[data-universe]] (Snowflake) for reporting/analytics; the InRisk database itself for audit-on-demand.

This is a deliberate "line in the sand" decision taken during the morning session on 2026-04-22.

## Context
InRisk today stores — on each client-ID, per submission — a snapshot of the party as it looked at the time the submission was raised. The Graph DB does _not_ currently preserve these historical snapshots; it preserves client IDs and current state. Because MDM natively supports full version history per party, the question arose: do we backfill all of InRisk's historical client-ID snapshots into MDM so that MDM becomes the one-stop historical source of truth?

The answer is no, for four reasons landed in-room:

1. The data **already exists** — in InRisk, and (for the subset DU has ingested) in Snowflake. Nothing is being lost.
2. The backfill would be **large and expensive** — every InRisk client-ID back through history, flattened through a point-in-time projection.
3. MDM does not _need_ that history to do its job. The "what is this party, now and going forward?" question is fully answered without it.
4. Consumers who _do_ need historical-as-of-date answers (e.g. DataOps for audit questions) already get them today by querying InRisk directly. That workflow is unchanged.

Joe's framing in-session: _"I'll have a horizon — they can't pass that — and get that back, unless we do this extra work. But they can't right now."_ Alex agreed: _"if that data already exists in InRisk and the DU, I can't see good reason to include that in MDM."_

## Options considered

1. **Full backfill of InRisk historic client-ID snapshots into MDM.**
   - Pros: MDM becomes single source of truth for all historical party views; fewer systems to query for an audit; future-proofs against InRisk DB decommissioning.
   - Cons: large effort; uncertain value (_"who's actually asking for it? Nobody, today"_); delays cutover; inflates Dynamo cost; risk of mis-projecting past InRisk semantics into MDM's new versioning model.

2. **Partial backfill (e.g. last N years, only live parties).**
   - Pros: compromise on scope.
   - Cons: arbitrary cut-off line still requires consumers to learn "before date X, look over there; after date X, look here"; same two-systems reality, with the added cognitive load of the cut.

3. **No backfill — draw a line; MDM starts history from cutover (chosen).**
   - Pros: smallest-possible surface area; cheapest; ships soonest; any consumer who needs historical answers goes to the system that already holds them (unchanged workflow); preserves InRisk as domain-of-record for past InRisk-flavoured audit questions.
   - Cons: consumers with a novel historical-audit workflow would need to either (a) do a two-source join or (b) ask InRisk or DU. Net new pain: low; the in-session test was _"nobody's asking that question today"_ and no one in the room could name a consumer who would need it.

## Decision
**Option 3**. MDM history starts at cutover. InRisk remains the system of record for its own historical client-ID-scoped snapshots; [[data-universe]] holds any aggregated history it has already ingested; MDM owns go-forward version history.

## Consequences

- **Positive**
  - Cutover scope is smaller — no large ETL job in the critical path.
  - Cost (Dynamo storage, OpenSearch indexing) is bounded by forward volume, not forward + historical volume.
  - MDM's version-semantics are cleaner — no need to retrofit InRisk's point-in-time-snapshot shape into MDM's version model.
  - No ambiguity about authority: for pre-cutover per-client audits, ask InRisk; for post-cutover party state at version, ask MDM; for cross-system analytics, ask [[data-universe]].

- **Negative / trade-offs**
  - Consumers who want a _uniform_ API across past + future have to write a two-source join.
  - If InRisk's DB is later decommissioned or archived, anyone who later needs pre-cutover detail has to rely on whatever DU / archived snapshot remains.
  - Sets a precedent that each new data store decides its own backfill horizon; future consumers need to be told about the cut.

- **Open risks**
  - If a regulatory / audit ask emerges after go-live that _requires_ a unified MDM history including pre-cutover InRisk views, we'd have to revisit — probably as a retrospective backfill into the new shape. Mitigation: MDM retains the client ID as a linkage attribute on each party, so a later backfill remains technically possible.

## Relationship to other decisions
- [[strangle-the-graph-via-proxy-events]] — this decision bounds the **backfill scope** for the migration strategy: the backfill that _is_ needed (current Graph state → MDM, to enable faithful proxy-event emission) is separate from and much smaller than the hypothetical backfill this ADR rejects.
- [[pct-and-mdm-go-live-together]] — PCT's own audit-history-backfill question is analogous (the new PCT will similarly _not_ backfill Jira audit trails; Jira and Snowflake retain that history). That's a separate inline call in Session 1 and a candidate for its own ADR.

## Supersedes / superseded by
- Does not supersede any prior ADR. Not yet superseded.

## Related
- [[party-application]] — primary application affected
- [[inrisk]] — system of record for pre-cutover client-ID-scoped historical snapshots
- [[analytics-team]] · [[data-universe]] — analytics source for pre-cutover historical state
- [[graph-team]] — owner / implementer

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — locus of the decision (morning session, PCT migration-considerations segment)
