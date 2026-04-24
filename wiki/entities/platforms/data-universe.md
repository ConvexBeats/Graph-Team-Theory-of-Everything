---
type: entity
title: Data Universe
aliases: [DU, data-universe-platform]
created: 2026-04-22
updated: 2026-04-22
tags: [platform, data, downstream-consumer]
owner: analytics-team
sources: [20260422-meeting-transcript-session-2]
source_count: 1
status: stub
---

# Data Universe

## Summary
The **Data Universe (DU)** is the organisation's central analytics data platform. It is owned and operated by the [[analytics-team]] and is one of the main downstream consumers of events emitted by [[party-application]]. **Snowflake** is the underlying data-storage technology that sits at the core of the DU, but the DU as a concept is broader than Snowflake — it also encompasses the ingestion flows (for example bulk S&P data) and the data-model joins (today against InRisk via client-ID) that make Party data usable for analytics.

Not a builder on this programme, but a first-class consumer whose tolerance for event-shape changes constrains what the new MDM can emit. The [[strangle-the-graph-via-proxy-events]] decision exists specifically to preserve the DU's current consumption contract through the cutover.

## Key facts
- **Kind**: internal analytics / data platform
- **Owning team**: [[analytics-team]]
- **Programme Manager**: [[steve-perry]]
- **Architect**: [[chris-woodward]]
- **Core storage**: Snowflake
- **Primary feeds (today)**: Party events from [[party-application]]; InRisk data from [[inrisk]]; bulk S&P data
- **Primary role in this programme**: downstream consumer of Party events; reactive rather than a builder

## Current shape (today)
- Indexes Party events joined against InRisk data on the client-ID key.
- Receives bulk S&P data which [[party-application]] in turn consumes from the DU (long-term this flow may invert, with Party as source-of-truth).
- Has no current direct dependency on the Neo4j knowledge graph internals inside [[party-application]]; consumes via the published event contract.

## Target-state exposure (from this programme)
- Per [[strangle-the-graph-via-proxy-events]], the DU should not need to change on cutover — proxy events maintain the current consumption contract.
- Any move to a flattened `(party-id, submission-id)` schema vs the current nested-under-client-ID shape requires confirmation from the Analytics Team (tracked in [[open-questions]]).

## Relationships
- **Owned by** [[analytics-team]] (Programme Manager [[steve-perry]]; Architect [[chris-woodward]]; Data & Process Analyst [[paul-rogers]]).
- **Consumes from** [[party-application]] (events today; proxy events post-cutover).
- **Joins** [[inrisk]] data on the client-ID key.
- **Feeds** bulk S&P data into [[party-application]].

## Claims
- [[steve-perry]] is Programme Manager for the Data Universe — user-declared (2026-04-22).
- [[chris-woodward]] is Architect for the Data Universe — user-declared (2026-04-22).
- Snowflake is the data-storage technology within the DU, not the DU itself — user-declared (2026-04-22).
- The [[analytics-team]] "look after the Data Universe, which includes the Snowflake data stores" — user-declared (2026-04-22).

## Open questions
- Is the DU tolerant of a flattened `(party-id, submission-id)` schema vs the current nested-under-client-ID shape? ([[open-questions|OQ-011]])
- Can the long-term S&P flow invert so that [[party-application]] is the source-of-truth rather than the DU?

## Related
- [[analytics-team]] · [[steve-perry]] · [[chris-woodward]] · [[paul-rogers]]
- [[party-application]] — primary data feed
- [[inrisk]] — joined on client-ID today
- [[strangle-the-graph-via-proxy-events]] — preserves the DU's current consumption contract
- [[dependency-map]] · [[ownership-matrix]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]]
