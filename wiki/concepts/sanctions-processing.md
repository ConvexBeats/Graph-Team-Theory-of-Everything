---
type: concept
title: Sanctions Processing
created: 2026-05-18
updated: 2026-05-18
tags: [concept, compliance, sanctions, cross-cutting]
sources: [20260513-inrisk-integration-with-party-mdm-follow-up]
source_count: 1
status: draft
---

# Sanctions Processing

## Summary
**Sanctions processing** is the compliance check that determines whether a party (typically a client) may have sanctions placed on it. The check **must be run in advance of doing any business** with that party. The actual check is performed by an external vendor application — **[[ntt]]** — accessed via API. In Convex today, the orchestration around _when_ to call NTT, _what context_ to send, and _what to do_ with the response lives in **Boomi**, sitting on top of [[party-application]]'s event stream. The 2026-05-13 follow-up call surfaced wide consensus that this orchestration is in the **wrong place**: it should be its own domain rather than absorbed into Boomi, [[party-application]], or [[inrisk]]. Phase 1 leaves the flow as-is (proxy events drive Boomi unchanged); Phase 2+ is expected to rework ownership. **Audit is focusing on sanctions this year**, which raises the urgency.

## Definition
Sanctions processing is the regulatory / compliance act of checking, via a sanctions screening service, whether a counterparty appears on relevant sanctions lists. In the Convex insurance-deal lifecycle, this gating check is performed prior to engaging in business with a counterparty, and is run again as parties or submissions change in ways that may affect the result.

The **decision** ("is this party sanctioned?") is owned by the [[ntt]] application. The **orchestration** around that decision — what to call, when to call, how to interpret the result, how to propagate it back to the deal lifecycle — is what this concept covers.

## Mechanics / how it works (today)

```
[[party-application]]  ── (E, every party change, regardless of relevance)  ──►  Boomi
                                                                                  │
                                                                                  │ (sees event; lacks submission context)
                                                                                  ▼
                                                              [[inrisk]] API  ◄── (one-submission-at-a-time calls; no batch endpoint)
                                                                                  │
                                                                                  │ (Boomi reconstructs submission+party context locally;
                                                                                  │  Boomi maintains a cache + idempotency check on top)
                                                                                  ▼
                                                                              [[ntt]] (API: sanctions check)
                                                                                  │
                                                                                  ▼
                                                                          sanctions decision  ──►  back into [[party-application]] / [[inrisk]] deal flow
```

The **pain points** flagged in [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]]:

1. **Boomi fires on every party event**, including ones that are irrelevant to sanctions. This creates noisy, often-redundant checks.
2. **No batch endpoint on [[inrisk]]** for retrieving multiple submissions at once, so Boomi calls InRisk one submission at a time to reconstruct context.
3. **Boomi has absorbed significant orchestration logic** — caching, idempotency checking, context reconstruction — that the rest of the system can't see or easily reason about. Joe Worsfold: _"I don't love the idea of having all this, what is probably very important logic on the Boomi side that none of us can see."_
4. **Cascading false positives**: irrelevant party changes can cascade into sanctions re-fires that downstream consumers (notably [[inrisk]]) feel as noise. Kris Mokrzycki: _"sanctions and firing… bulk, fake, let's say false positive changes that are having cascading effect on us."_
5. **Audit visibility**: Suzanna Whitefield — _"Audit's eyes are going to be on sanctions this year."_ The lack of single-source-of-truth orchestration makes audit harder.

## Phase 1 treatment (this programme)

Sanctions is **explicitly not** a Phase-1 deliverable in [[party-rearch-phase-1]].

- [[party-application]] continues to emit party-change events; under [[strangle-the-graph-via-proxy-events]], MDM emits proxy events in the Graph event shape — Boomi continues to consume them transparently.
- Boomi's existing call-chain (Boomi → InRisk → NTT) is untouched.
- Joe's intent is to **proxy these events initially, and stop firing them unnecessarily over time** — i.e. the Phase-1 strangler covers sanctions transparently, and the unnecessary-event-fire problem is addressed in a later phase.

## End-state direction (Phase 2+, open)

The room's consensus from 2026-05-13:

- The sanctions orchestration **should not live in Boomi**. Boomi is a vendor integration layer; it has absorbed logic that should be visible and owned inside Convex.
- The sanctions orchestration **should not live in [[party-application]] either** — Joe explicitly does not want Party to absorb submission data.
- The sanctions orchestration **should not live in [[inrisk]] either** — Joe doesn't want InRisk to feel responsible for sanctions either, given the domain spans multiple apps.
- The likely destination is a **dedicated sanctions service / domain** with its own ownership. Rory: _"we need to pull out the sanctions logic into its own service that is held independently and we work out who the owner of that is later, right? Because it interacts with multiples of our apps."_
- An end-state sketch from [[sources/20260422-meeting-transcript-session-2]] (now superseded by the broader "wrong place" framing): NTT result lands on the party version directly; Party informs InRisk of affected submissions. The mechanism (who orchestrates) is what's still loose.

This direction is tracked at [[open-questions#OQ-032]] (where sanctions logic should live) and [[open-questions#OQ-008]] (end-state sanctions flow owner / timing).

## Variants
- **NTT check (current)** — the actual sanctions screening, performed by [[ntt]] via API. Stable; not the subject of the rethink.
- **Boomi-orchestrated flow (current)** — the call-chain described above. This is what's in the wrong place.
- **Dedicated sanctions service (proposed, Phase 2+)** — a standalone service owning the orchestration; calls [[ntt]]; consumed by [[party-application]] and [[inrisk]] via a stable contract.

## Examples
- A party is updated in [[party-application]] (a name correction, say) — Boomi sees the event and currently re-runs sanctions checks against all submissions involving that party, even though the change has nothing to do with sanctions data. Cascading false-positive activity downstream.
- An [[inrisk]] submission updates a counterparty — Boomi must reconstruct the submission+party context one InRisk-API-call at a time because no batch endpoint exists. Slow, fragile, opaque to Convex engineers.

## Contradictions / open debates
- _None recorded yet_ — the "wrong place" framing is widely shared in [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] (Joe, Kris, Rory, Suzanna all converge). Joe specifically does not want sanctions absorbed into [[party-application]]; nor into [[inrisk]]; but agrees it doesn't belong on Boomi either. The contested question is _where_ it should live, not whether the current location is wrong.

## Related
- [[ntt]] — the underlying vendor sanctions-check application; not the subject of the rethink, but its calling pattern is.
- [[party-application]] — emits the events that today drive Boomi's sanctions orchestration; future state may host different integration points but should not absorb the orchestration logic.
- [[inrisk]] — currently provides one-submission-at-a-time context to Boomi; absence of a batch endpoint is part of the cost.
- [[strangle-the-graph-via-proxy-events]] — Phase-1 proxy events keep the sanctions flow transparent during cutover.
- [[contract-buckets]] — sanctions is `enrichment`-adjacent in the three-bucket framing.
- [[andrea-read]] — escalation target for the sanctions-domain ownership conversation ([[rory-beattie]] + [[suzanna-whitefield]] to escalate).
- [[party-rearch-dependency-map]] — cross-app view of the sanctions flow.

## Open questions
- [[open-questions#OQ-032]] — sanctions-domain location: where should the orchestration logic that today lives in Boomi actually live? (new)
- [[open-questions#OQ-008]] — end-state sanctions flow: who owns it and when does it land? (carried; sharpened by 2026-05-13)
- [[open-questions#OQ-034]] — identification of **Marty** (sanctions / InRisk-adjacent figure referenced in the source). (new)

## Sources
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — the meeting that surfaced the wrong-place framing, the audit-visibility angle, the Boomi orchestration detail (cache, idempotency, one-at-a-time InRisk calls), and the escalation-to-[[andrea-read]] action.
