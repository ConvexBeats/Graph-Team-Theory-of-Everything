---
type: decision
title: InRisk cuts over to MDM before High Volume's 1 Sep go-live
created: 2026-05-18
updated: 2026-05-26
tags: [decision, cutover, sequencing, widget, design-system]
application: [inrisk, party-application, high-volume, party-curation-tool]
owner: tech-tooling
sources: [20260513-inrisk-integration-with-party-mdm-follow-up, 20260514-inrisk-high-level-refinement]
source_count: 2
status: accepted
project: party-rearch
phase: [phase-1]
---

# InRisk cuts over to MDM before High Volume's 1 Sep go-live

## Summary
**1 September is a hard date for [[high-volume]]'s go-live.** That gate has not moved — what has changed is the understanding of Phase 1's internal sequencing. [[inrisk]]'s integration with the new MDM is now a **deliverable in its own right that must land in production before HV cuts over**, with a buffer of **at least two weeks** before 1 September (minimum) to prove the integration in production traffic. HV switches on after that point. To make this achievable inside the Phase-1 timeline, [[joe-worsfold]] will publish a **second, design-system-agnostic component library** for [[inrisk]] to consume, so InRisk does not have to absorb Chakra 3 + design-system risk into its codebase under the September gate. The Chakra-3-with-design-system widget remains in place for [[party-curation-tool]] / [[dataops-team]]. This resolves [[open-questions#OQ-005]].

## Context

Through Sessions 1 and 2, the implicit mental model was that the Phase-1 deliverables — MDM, new PCT, [[inrisk]] interim changes, and [[high-volume]]'s integration — would converge on a single cutover near the 1-Sep gate. The 13 May follow-up call ([[sources/20260513-inrisk-integration-with-party-mdm-follow-up]]) made it clear that converging everything on a single date is not viable: [[high-volume]] needs MDM to be a stable source of truth from day one, and the only way to give HV that confidence is for [[inrisk]]'s integration to be live and exercised in production first.

A separate but coupled concern emerged in the same call. [[joe-worsfold]]'s new MDM widget uses **Chakra 3 plus a design system**. [[inrisk]] today uses Chakra 2 components but not as a design system — it has no design-system surface at all, in any of its UI. Layering Chakra 3 + design-system migration onto [[inrisk]] under the September gate is the kind of unbounded styling-stack risk that historically eats schedules. The room reached consensus in-call: don't force the design system into [[inrisk]] now.

These two threads — the cutover sequencing and the styling-stack call — are coupled because both are about **keeping [[inrisk]]'s Phase-1 path narrow enough to ship before HV's gate**. Rolling them into one ADR keeps the record of the cutover-readiness decisions in one place.

### Constraints
- **1 Sep is fixed** — [[high-volume]] production go-live is the programme's external clock; this decision does not move that date.
- **[[inrisk]] is the precondition** — HV consuming MDM is meaningful only if MDM has been operating as the source of truth long enough to be trusted; [[inrisk]] is the consumer that exercises that operation.
- **No parallel-running for party create / search** — confirmed in the source: a single source of truth is required because Phase 1 is designed around a one-way mapper (MDM → old graph). Two-way mapping would create unmanageable data-mess. This re-confirms [[strangle-the-graph-via-proxy-events]]; see that ADR's refinement for the brief dual-write-for-revert nuance during the cutover window.
- **[[high-volume]] does not use a widget** — API-only via Boomi — so it is unaffected by the styling-stack call.

## Options considered

### A) Cutover sequencing

1. **Single cutover for everything (MDM + PCT + InRisk + HV) at 1 Sep** _(rejected)_.
   - Pros: one window; clean.
   - Cons: HV would land on MDM without any prior production exposure of the MDM → consumer contract via [[inrisk]]; failure mode is the worst case — production go-live discovers integration problems for the first time.
2. **HV-first cutover: HV uses MDM while [[inrisk]] stays on Graph until later** _(rejected)_.
   - Pros: meets the 1 Sep gate trivially.
   - Cons: pushes the curation problem further down the line and makes the [[inrisk]] migration substantially harder once HV is already in production. Joe: _"We just move the migration problem further down and make it more complex than if we do in risk first."_ Bad debt.
3. **[[inrisk]]-first cutover: [[inrisk]] integrates with MDM ≥ 2 weeks before 1 Sep, HV switches on at the 1 Sep gate** _(chosen)_.
   - Pros: HV's first day in production is _after_ MDM has been the source of truth for at least a fortnight, exercised by real [[inrisk]] traffic; failure modes surface earlier; HV gets the safer cutover.
   - Cons: [[inrisk]]'s effective deadline is mid-August, tighter than the headline "1 Sep" gate suggested; sprint-planning needs to reflect that.

### B) Widget styling-stack approach for [[inrisk]]

1. **Force Chakra 3 + design system into [[inrisk]] for the new widget** _(rejected)_.
   - Pros: aligned with [[party-curation-tool]] / [[dataops-team]] story; only one widget library to maintain.
   - Cons: [[inrisk]] has no design-system surface today; introducing one alongside a Chakra-3 upgrade under the September gate is unbounded styling-stack risk. John: _"It will almost certainly cause us headaches."_
2. **[[inrisk]] sticks to Chakra 2** _(rejected)_.
   - Pros: incremental.
   - Cons: Joe's new widget is Chakra 3 — a Chakra-2 widget would mean re-implementing it, defeating the point. Also still leaves a future Chakra 3 migration to do for the whole InRisk codebase eventually.
3. **Joe publishes a second component library, design-system-agnostic, matched to [[inrisk]]'s current look; the Chakra-3-with-design-system widget remains the [[party-curation-tool]] / DataOps option** _(chosen)_.
   - Pros: ring-fences InRisk's styling-stack risk; lets [[party-curation-tool]] keep the modern stack; clean contract; HV is unaffected (API-only).
   - Cons: Joe maintains two component libraries. Joe in-call: _"if my problem is just I have to maintain two component libraries, that's easier than the current thing of like… do all this migration."_ Manageable cost.

## Decision

**A: Option 3 — [[inrisk]]-first cutover.** The party-side MDM cutover for [[inrisk]] is targeted for **at least two weeks before 1 September** (concrete date TBC at [[open-questions#OQ-035]]). HV switches on at the 1 Sep gate, consuming MDM after [[inrisk]] traffic has been live on it for the buffer window.

**B: Option 3 — two component libraries.** [[joe-worsfold]] publishes a **second, design-system-agnostic component library** for [[inrisk]] to consume; the Chakra-3-with-design-system widget remains in place for [[party-curation-tool]] / [[dataops-team]]. Joe to manage the two libraries in parallel — explicitly accepted as the easier of the available trade-offs.

**B (refined 2026-05-14): parity-not-enhancement principle.** The InRisk-side widget on the second library is committed to **drop-in replacement** of today's behaviour — same auth / RBAC / session flow, same look-and-feel, same filter parameters (including **TOBA status** for brokers — [[jason-owen]] surfaced this in-call). UX enhancements are explicitly deferred to a separate change, post-cutover. Joe rolled back his original "modern and different" vision in front of the wider InRisk audience: _"my original scope was don't affect in-risk too much, so I just went crazy and was like, no, I want it to be really modern and different, but, yeah, we can roll it back a bit until that."_ [[john-trahearn]]: _"For as much as possible, it's going to be a drop-in replacement for now, and then should we need to change things in the future, that'll be something that comes separately."_ This sharpens part B of the ADR from a styling-stack choice into a full posture on the InRisk widget surface.

This ADR therefore covers: (a) the cutover sequencing across Phase 1's three production consumers (InRisk, then HV; PCT co-ships with MDM under [[pct-and-mdm-go-live-together]]); (b) the styling-stack call (two component libraries); and (c) (refined 2026-05-14) the parity-not-enhancement posture for the InRisk widget itself, capping its Phase-1 surface area to what's needed for drop-in replacement.

## Consequences

### Positive
- **HV's first production day is not also MDM's first production day.** The buffer means real production traffic has exercised the MDM → consumer contract before HV depends on it. Lower-risk go-live for the deliverable on the hard date.
- **[[inrisk]] is not asked to absorb Chakra 3 + design-system migration under a hard deadline.** The styling-stack risk is contained on Joe's side; [[prebind-team]] can integrate the new widget using a library matched to InRisk's current look.
- **[[party-curation-tool]] keeps the modern stack** — Chakra 3 + design system stays the [[dataops-team]] surface; no compromise there.
- **The Phase-1 critical path narrows around [[inrisk]]** — sprint-planning gains clarity: the real deadline for InRisk's MDM integration is mid-August, not 1 September.
- **Existing decisions stay coherent** — [[strangle-the-graph-via-proxy-events]] still governs the proxy-event seam (with the cutover-window dual-write nuance noted there); [[pct-and-mdm-go-live-together]] still co-ships PCT + MDM; [[no-historic-client-backfill-into-mdm]] and [[no-pct-audit-backfill]] are unchanged.

### Negative / trade-offs
- **[[inrisk]]'s effective deadline is mid-August.** Calendar pressure compresses; less slack for InRisk-side discovery. Story-level estimates for the four-to-five-story epic ([[inrisk#Current state]] / [[inrisk-architecture]]) need to be honest about that.
- **Joe maintains two component libraries.** Real but bounded cost; Joe accepted explicitly. Drift between the two is the long-term risk — should be tracked as a maintenance concern but is not a Phase-1 blocker.
- **[[high-volume]] go-live timing is unchanged**, but the value of the buffer evaporates if [[inrisk]]'s cutover slips. Slip risk on InRisk lands more sharply on HV than it did under the "everything converges on 1 Sep" mental model.

### Open risks
- **The concrete InRisk cutover date is not fixed in this ADR** — only the buffer rule ("≥ 2 weeks before 1 Sep, minimum"). Confirming the date is open at [[open-questions#OQ-035]]. Slip propagates to HV's cutover quality.
- **[[inrisk]]'s existing widget integration mechanic** is confirmed as **query-parameter-driven** today (2026-05-14); the new flow is SDK-style (instantiate MDM SDK, pass methods / hooks to components). The full mechanic-alignment / response-shape question is **gated on a widget-integration spike** between [[joe-worsfold]] + [[billy-calladine]] (ex-Striker) + [[alex-sillars]] + [[daria-romanovskaia]] (PreBind-PO slot inherited from [[andrew-turner]] on his 2026-05-29 departure) before Stories 3/4/5 of the Party MDM Integration epic can take low level. Spike timing to be confirmed at the ad-hoc HL on 2026-05-15. Tracked on [[party-rearch-ownership-matrix]].
- **Widget response-shape alignment is open** — Sergiu raised at the 2026-05-13 follow-up: will the new widget's response give InRisk every field it needs, given OpenSearch indexes differently than Dynamo storage? The spike above is the resolution path; Joe expects to adjust the response shape as integration starts. Tracked at [[open-questions#OQ-036]].
- **Parity must include current widget filter parameters** (refined 2026-05-14) — particularly **TOBA status** for the Lloyd's-vs-retail broker workflow split. If the new widget loses filter parameters that today's widget supports, parity is breached and either rework lands inside the cutover window or InRisk has to compensate in application code. [[jason-owen]] surfaced this in-call; commit to feature parity logged by [[joe-worsfold]].

## Relationship to other decisions

- **Refines [[pct-and-mdm-go-live-together]]** — PCT co-ships with MDM. This ADR adds the layer on top that says HV consumes MDM only _after_ [[inrisk]] has been on it for the buffer window.
- **Refines [[strangle-the-graph-via-proxy-events]]** — the proxy-event seam is unchanged in direction (MDM is source of truth); the cutover window includes a brief dual-write to the old graph for revertability, noted on the ADR but not changing its core decision.
- **Resolves [[open-questions#OQ-005]]** — Chakra V2/V3 widget strategy. The resolution is a Party-side fix (Joe maintains two libraries) rather than the InRisk-side fix the OQ's original framing implied. Recorded in the Resolved section of [[open-questions]].
- **Does not depend on [[feature-tagging-moves-to-inrisk]]** — feature tagging is out of scope for Phase 1 and orthogonal to this cutover.
- **Coupled to [[inrisk-cuts-over-before-high-volume#Open risks|OQ-035]]** for the concrete date, and [[open-questions#OQ-036]] for the widget-response shape.

## Supersedes / superseded by
- Supersedes the implicit "single 1-Sep cutover" framing that Sessions 1 and 2 had not contradicted.
- Not yet superseded.

## Related
- [[inrisk]] — primary consumer affected; Phase-1 epic now sized against the InRisk-first deadline.
- [[party-application]] — second component library published from Joe's side; AWS estate confirmation (existing 3-account / 4-env) reduces an integration unknown for [[prebind-team]].
- [[high-volume]] — gating beneficiary of the InRisk-first cutover; unaffected by the styling call.
- [[party-curation-tool]] — keeps the Chakra-3 + design-system widget.
- [[pct-and-mdm-go-live-together]] — companion cutover decision (PCT + MDM coupled).
- [[strangle-the-graph-via-proxy-events]] — proxy-event seam during the cutover window.
- [[party-rearch-phase-1]] — done-state needs to reflect the InRisk-first deadline.
- [[joe-worsfold]] (owner of the second component library) · [[john-trahearn]] (owner of the InRisk-side stories) · [[billy-calladine]] (existing-widget expertise).
- [[open-questions#OQ-005]] (resolved by this ADR) · [[open-questions#OQ-035]] (concrete InRisk cutover date) · [[open-questions#OQ-036]] (widget response-shape alignment).

## Sources
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — the meeting where both halves of this decision were made.
- [[sources/20260514-inrisk-high-level-refinement]] — wider-audience reinforcement; parity-not-enhancement principle for the InRisk widget; TOBA-status filter as a parity requirement; widget-integration spike scoped (Joe + Billy + Alex + [[andrew-turner]] — PreBind-PO slot now [[daria-romanovskaia]] following 2026-05-29 handover) as the gate on Stories 3/4/5.
