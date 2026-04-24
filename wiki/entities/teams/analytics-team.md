---
type: entity
title: Analytics Team
aliases: [data-universe-team, du-team, data-engineering]
created: 2026-04-22
updated: 2026-04-22
tags: [team, consumer]
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: stub
---

# Analytics Team

## Summary
The team that owns the **Data Universe (DU)** — the downstream data platform (Snowflake-backed) that consumes events emitted by [[party-application]]. Previously referred to in this wiki and in meetings as "the DU team" / "Data Universe Team" — **Analytics Team** is the formal name.

Not a direct counterparty to this programme's build work, but a first-class consumer whose tolerance for event-shape changes constrains what the new MDM can emit. The [[strangle-the-graph-via-proxy-events]] decision exists specifically to keep the Analytics Team's current consumption contract intact through the cutover.

## Key facts
- **Kind**: internal analytics / data engineering team
- **Aliases**: Data Universe Team, DU Team, Data Engineering (colloquial)
- **Owned platform**: **Data Universe** (Snowflake) — downstream data platform consumed across the business
- **Role in programme**: downstream consumer of Party events; not a builder on this programme

## Members (known in this wiki)

| Name | Role |
|---|---|
| [[steve-perry]] | Programme Manager, [[data-universe]] |
| [[chris-woodward]] | Architect, [[data-universe]] |
| [[paul-rogers]] | Data & Process Analyst |

_Other members pending._

## Platforms / systems owned
- **[[data-universe]]** — the central analytics data platform, Snowflake-backed; indexes Party events; joins to InRisk data via client ID today
- **S&P ingestion flow** — bulk S&P data lands in the DU; [[party-application]] then consumes it from there (long-term this flow may invert with Party as source-of-truth)

## Current workstreams (for this programme)
- _None actively owned._ Involvement is **reactive**:
  1. Answer whether the DU can tolerate a flattened `(party-id, submission-id)` schema vs the current nested-under-client-ID shape. _(Owner to respond: likely Paul Rogers or a named contact on this team.)_
  2. Receive proxy events post-cutover — no change expected on their side per [[strangle-the-graph-via-proxy-events]].

## Relationships
- Consumes events from [[graph-team]] (today: Party Graph; after cutover: MDM via proxy events).
- Joins InRisk data produced by [[prebind-team]] to Party on the client-ID key.
- Supplies bulk S&P data that Party currently ingests manually.

## Claims
- Owns the Data Universe; Paul Rogers is a Data & Process Analyst on this team — user-declared (2026-04-22).
- Formerly referred to in this wiki as "Data Universe Team" / "DU Team" — corrected in Pass B (Analytics Team is the formal name).
- Does not appear to index the nested `client-id → submission` structure (inference; _pending confirmation_) — [[sources/20260422-meeting-transcript-session-2]].

## Open questions
- Is the Analytics Team interested in, or resistant to, a future direct Dynamo-to-DU stream replacing proxy events?
- Beyond [[paul-rogers]], [[steve-perry]], and [[chris-woodward]], who else on the team touches Party events?

## Related
- [[data-universe]] · [[steve-perry]] · [[chris-woodward]] · [[paul-rogers]]
- [[party-application]] — primary data feed
- [[strangle-the-graph-via-proxy-events]] — the decision that preserves their current contract
- [[graph-team]] · [[prebind-team]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
