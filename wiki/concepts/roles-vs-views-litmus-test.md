---
type: concept
title: Roles vs Views — Litmus Test
aliases: [roles-vs-views, litmus-test]
created: 2026-04-22
updated: 2026-04-22
tags: [concept, data-model, heuristic]
sources: [20260422-meeting-transcript-session-1]
source_count: 1
status: draft
---

# Roles vs Views — Litmus Test

## Summary
A sharp, one-question test — introduced by [[joe-worsfold]] in [[sources/20260422-meeting-transcript-session-1]] — for deciding whether a given attribute of a party belongs **on the party** (a **role**, intrinsic, lives in [[party-application]] / MDM) or **on a consumer's projection of the party** (a **view**, ascribed, lives in the consumer's domain and is fed back to MDM only to preserve downstream projection shape).

## The test
> _"If every policy was deleted from this party, would it still be insured? No. Would it still be a broker? Yes."_

- If the answer is **yes** — the attribute persists without any deal context — it is a **role**. Roles belong on the party itself.
- If the answer is **no** — the attribute only exists _because of_ some deal, policy, or contract — it is a **view**. Views belong on the consumer (typically [[inrisk]]) and are pushed back into MDM only to maintain the shape the [[data-universe]] expects.

## Concrete placements (Session 1)

| Attribute | Intrinsic or ascribed? | Where it lives | Why |
|---|---|---|---|
| **Broker** | intrinsic (role) | [[party-application]] | Broker-ness is a property of the organisation regardless of whether any deal currently runs through them. |
| **Client** | intrinsic (role) | [[party-application]] | The party is a known client organisation independent of any specific policy. |
| **Underwriter / reinsured / cover holder / claims counterparty** | intrinsic (role) | [[party-application]] | Each is a business relationship tied to the party's identity, not to any one deal. |
| **Insured** | ascribed (view) | [[inrisk]] | "Insured" is a _role in a policy_. Delete the policy, the status evaporates. InRisk owns it; MDM reflects it only to keep DU projections consistent. |
| **Reinsured (in a given treaty)** | ascribed (view) | [[inrisk]] | Same shape as insured — depends on a specific contract. |

## Mechanics in the target architecture
- **MDM** stores roles using a generic role schema (any role can be attached to any party); this is the model change vs today's per-role-gated path.
- **InRisk** (and other policy-admin systems) emit **view assertions** back to MDM — not to re-own the attribute, but so the DU's projection continues to see the shape it used to. This is a deliberate compromise around the strangler pattern: the DU contract has to be preserved.
- **PCT** surfaces roles on the party record itself; it does **not** surface views, because the curator is not authoritative on policy-bound state.

## Why it matters

1. **It keeps the MDM schema honest.** Without the test, attributes tend to drift onto the party because that's where it's easiest to attach them; the party record then becomes a grab-bag of deal-context fields.
2. **It clarifies ownership boundaries.** Anything that fails the test is by definition owned by the policy-admin consumer, not by [[graph-team]] — which simplifies feature-scoping arguments.
3. **It is a usable design tool in meetings.** It's short, memorable, and gives a clean yes/no on every candidate attribute — which is why it landed in-session and shaped the rest of the Session 1 data-model discussion.

## Caveats / edge cases
- **Dormant brokers / ex-clients.** A party could pass the test (still a broker in principle) but have no active relationship — the wiki treats these as still roles; status/active flags are a separate concern.
- **Temporary brokerage agreements.** If a broker relationship is genuinely policy-bound (rare), it slips from role to view; such cases should be surfaced rather than forced into either bucket.
- **Consumer-specific sub-roles.** Where a consumer needs finer-grained segmentation (e.g. "preferred broker"), those are views even if the underlying "broker" is a role.

## Related
- [[party-application]] — where roles live.
- [[inrisk]] — where views live and are authored.
- [[contract-buckets]] — finer-grained lens _inside_ the operational / data-distribution boundary.
- [[strangle-the-graph-via-proxy-events]] — the decision that forces views to be pushed back to MDM so DU projections are preserved.

## Sources
- [[sources/20260422-meeting-transcript-session-1]] — introduction and walkthrough.
