---
type: decision
title: PCT and MDM go live together
created: 2026-04-22
updated: 2026-04-22
tags: [decision, delivery, scope]
application: [party-application, party-curation-tool]
owner: graph-team
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
status: accepted
---

# PCT and MDM go live together

## Summary
The new **Party Curation Tool** (the Next.js UI on the new MDM architecture) and the new **MDM** (the target [[party-application]] backend) must be delivered and released together. Decoupling them — shipping MDM first and continuing to curate via the current PCT / Jira workflow with DynamoDB → MDM pushes — is considered operationally untenable and is ruled out.

## Context
A natural-seeming option during planning was: land MDM first, keep the existing PCT running, and feed manual curation into MDM from the current DynamoDB via Jira-triggered pushes. The room examined this and found it creates a layered mess: curation changes would flow through a decaying intermediate path while InRisk's messaging contract is itself changing during the same window. Each change to PCT-driven data would have to be kept compatible with both the old and new contracts; the adapter surface multiplies.

There is also a more pragmatic reason. [[dataops-team]] adoption of the new UI is part of the *value* of this programme — better search, proper version control, simpler curation flows, clearer merge and provenance. Not shipping that UI together with MDM means absorbing the risks of the re-architecture without realising its user-facing benefits.

## Options considered

1. **Ship MDM first; keep old PCT + Jira + DynamoDB → MDM push path alive during interim** (not chosen)
   - Pros: staggered delivery; PCT rollout becomes its own discrete project; lower change-management risk for DataOps.
   - Cons: operating a moving Party contract while simultaneously curating via a decayed path; higher ongoing engineering load on the Graph Team for interim adapters; DataOps doesn't feel the benefit of the re-architecture; InRisk message changes during this window would require yet more compatibility work. Characterised in-room as "essentially not an option" (Rory).
2. **Ship new PCT first on current Party, migrate backend later** (not considered seriously)
   - Pros: could decouple frontend risk from backend risk.
   - Cons: the new PCT is specifically designed to sit on the new open-API spec generated from MDM; re-targeting it at the legacy backend would duplicate integration work, then throw it away.
3. **Ship PCT and MDM together as one release** (chosen)
   - Pros: one release, one change-management event, one contract to support forward; realises the UX benefits that make the adoption conversation easier; avoids transitional-adapter sprawl.
   - Cons: larger single release; DataOps training and communication compresses into one window; scope-manage carefully or risk either side dragging the other.

## Decision
**Option 3**. PCT and MDM ship together.

Message to [[dataops-team]] is not *"would you like the new PCT?"* — it is *"here are the benefits of the new PCT, which is coming as part of this release."* ([[will-bone]]: the messaging handle is the business-value framing; [[hugh-lobban]] (business sponsor, mentioned-not-present) helps enforce that DataOps don't distract the programme with cosmetic change requests.)

### Operational shape (from Session 1)
The _same_ feature flag controls old-search→new-search **and** old-PCT→new-PCT. Joe initially considered decoupling the two ("do one and then the other"); the room agreed to keep them coupled because they share a domain boundary and the cutover is cleaner as one. One flag, one cutover, one rollback surface.

## Consequences

- **Positive**
  - Single release window simplifies coordination with InRisk and DU.
  - DataOps get UX improvements (better search via OpenSearch, explicit version control, simpler merge, clearer provenance) on day one — the benefits are the adoption lever.
  - No interim curation-path adapter to build or maintain.
  - Open-API-generated UI stays in lock-step with backend contract.

- **Negative / trade-offs**
  - DataOps change-management lead time needs to overlap with the development timeline — can't be done after the fact.
  - A delay on either side (UI stability, backend readiness) blocks both.
  - One large release is riskier than two small ones; requires solid go-live checklist and test coverage.

- **Open risks**
  - If DataOps training requires a lead time that pushes beyond the HV-driven deadline, a scope-trim conversation on *what* ships on day one becomes necessary.
  - Chakra V3 vs V2 embedding into InRisk is a live unresolved issue that touches the widget side of PCT — see [[sources/20260422-meeting-transcript-session-2]] §Widget/SDK.

## Supersedes / superseded by
- Supersedes the "PCT delay / DynamoDB interim curation push" option.
- Not yet superseded.

## Related
- [[party-application]] · [[party-curation-tool]] — the two applications co-shipping
- [[dataops-team]] — primary user-side stakeholder
- [[strangle-the-graph-via-proxy-events]] — complementary decision; together they define Phase 1 delivery
- [[graph-team]] — owner

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — single-feature-flag coupled-rollout shape
- [[sources/20260422-meeting-transcript-session-2]] — original decision locus
