---
type: application
title: InRisk Engine
aliases: [inrisk-core, new-inrisk]
created: 2026-04-22
updated: 2026-04-22
tags: [application, in-scope, final-state-target]
owner: devx-team
state: concept
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: draft
projects: [party-rearch]
---

# InRisk Engine

## Summary
A ground-up rewrite of [[inrisk]] to replace the legacy data model with an **API-first, backend-only** architecture. InRisk Engine is designed and built **on the assumption of the final state of the Party Application**; it does **not** target any interim MDM state. Its go-live is therefore contingent on the final-state Party contract being in place.

## Aliases / previous names
- InRisk Core
- New InRisk

## Purpose
Replace the legacy IR2 with a modern, API-first PAS that consumes party data cleanly via the target Party Application contract (party ID + version, role-generic, near-real-time lookups). Backend-only — does not include a new UI in its initial scope.

## Current state

- **Owner**: [[devx-team]]
- **Status**: concept / selection of prototypes
- **Observed behaviour in this programme**: not building against the interim state; not consuming proxy events in the Graph shape; not a Phase 1 consumer of MDM

## Target state

- **Contract alignment**: designed against the **final-state** Party Application — party ID + version, the three-mode lookup API (`by ID+version`, `by ID+timestamp`, `by ID`), role-generic parties, no client-ID-keyed joins.
- **Backend-only** — no UI rebuild implied.
- **Go-live**: gated on the final-state Party Application being live. Will proceed on that assumption as it builds out.

## Relationship to this programme

- **Not in scope** for the interim-state Party modifications.
- **Is affected**: the "final state" it builds against is being defined by this programme. Clarity on the final-state contract is what unblocks InRisk Engine's go-live planning.
- **Interim consumption**: stays on [[inrisk]] (IR2). There is no dual-interim work expected from [[devx-team]] on the Party side.
- **Session 1 scope call**: [[alex-sillars]] explicitly set this as an in-room convention: _"don't worry about Anton while we're devising any solutions right now"_. This is not a dismissal of InRisk Engine — it's a design-hygiene rule that Phase 1 should not be shaped by InRisk Engine's future requirements. [[antonie-labuschagne]] is a stakeholder on the final-state contract, not a participant in the interim design.

## Change ownership
- **Working unit**: [[devx-team]]
- **DevX Tech Lead**: [[antonie-labuschagne]] ("Anton")
- **Key inbound dependency**: the end-state Party API contract (final schema, event shape, role model) — owned by [[graph-team]] with [[tech-tooling]] oversight
- _Correction note (Pass B): Pass A speculated that Andrea, Bob, John, and Chris were on the InRisk Engine / DevX side. In fact: [[andrea-read]] is [[tech-tooling]]; [[john-trahearn]] and [[kris-mokrzycki]] are [[prebind-team]] (not DevX). **Bob** is not yet identified. These names should not be conflated with [[inrisk-engine]] ownership going forward._

## Pending / unknown

- Final-state Party API contract — details still being shaped by the Phase 1 work, directly feeds InRisk Engine's design.
- Timeline for InRisk Engine to be a first-class Party consumer — depends on final-state delivery, which is post-HV.
- Whether InRisk Engine will need specific design inputs from this programme beyond the eventual final-state contract (e.g. event-subscription models, version-change event semantics).
- DevX team composition beyond [[antonie-labuschagne]].
- Identity of "Bob" (in-session reference).

## Related decisions
- [[strangle-the-graph-via-proxy-events]] — defines the path to the final state InRisk Engine will consume
- [[pct-and-mdm-go-live-together]] — peripheral; confirms Phase 1 scope excludes InRisk Engine

## Related
- [[inrisk]] — predecessor; remains live during the interim
- [[party-application]] — final-state contract provider
- [[devx-team]] — owner · [[antonie-labuschagne]]
- [[party-rearch-ownership-matrix]] · [[party-rearch-dependency-map]]

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — morning; explicit out-of-scope call
- [[sources/20260422-meeting-transcript-session-2]] — afternoon; ownership + final-state framing
