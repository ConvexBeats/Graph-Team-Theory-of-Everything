---
type: entity
title: Ben Joseph
aliases: [speaker-9]
created: 2026-04-22
updated: 2026-05-26
tags: [person, engineer]
team: graph-team
sources: [20260422-meeting-transcript-session-2, 20260519-mdm-implementation-strategy]
source_count: 2
status: draft
---

# Ben Joseph

## Summary
Engineer / Developer on the [[graph-team]]. Has deep working knowledge of the current Graph DB event shapes, the backfill mappings, the ultimate-parent / D&B handling, and the current DU consumption pattern — i.e. the very shape of the integration that the [[strangle-the-graph-via-proxy-events]] adapter has to faithfully reproduce. Articulated the core acceptance criterion for the proxy adapter: bidirectional mappability of events ↔ MDM records.

## Key facts
- **Role**: Engineer / Developer
- **Team**: [[graph-team]]
- **Area of deep knowledge**: current Party Graph event shapes, backfill, DU consumption, D&B / parent handling

## Relationships
- Works alongside [[alex-sillars]] and [[joe-worsfold]] on the [[graph-team]].
- Close working relationship with the [[analytics-team]] by virtue of being the engineer who touches the event stream directly.

## Claims
- Engineer / Developer on [[graph-team]] — user-declared.
- Articulated the adapter acceptance criterion: _"if I can take an event currently emitted from Party Graph, map it into MDM, and map the MDM record back to the exact same event, we can go in"_ — [[sources/20260422-meeting-transcript-session-2]].
- Described the current event nesting (requirement + submission under client ID) and the renewal behaviour where client-ID cascades move historical submissions onto the latest client — [[sources/20260422-meeting-transcript-session-2]].
- Scoped the backfill strategy for parent parties (two-pass approach: backfill records, then second batch to resolve parent links via legacy party IDs) — [[sources/20260422-meeting-transcript-session-2]].
- **Pairing with [[joe-worsfold]] on the data-model-proxying + legacy-migration epic.** Joe in [[sources/20260519-mdm-implementation-strategy]]: _"I'm sitting on the epic which is around the data-model proxying and the legacy migration, and Ben's been helping with that one."_ As of 2026-05-19, three iterations of backfill have run; stable alpha targeted for end of week 2026-05-22; subsequent changes will be additive only (no further re-wiping).

## Current actions (open)
_None directly assigned in this session; Ben is primarily in an execution role for the tickets [[alex-sillars]] is creating._

## Open questions
- Specialisation — is Ben full-stack, backend-specialist, data-engineering-leaning? _(In-meeting evidence points to backend/data; confirm.)_

## Related
- [[graph-team]] · [[alex-sillars]] · [[joe-worsfold]]
- [[party-application]] · [[analytics-team]]
- [[strangle-the-graph-via-proxy-events]]

## Sources
- [[sources/20260422-meeting-transcript-session-2]] — bidirectional-mappability criterion; backfill strategy
- [[sources/20260519-mdm-implementation-strategy]] — pairing with Joe on the data-model-proxying + legacy-migration epic
