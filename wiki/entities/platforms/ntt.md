---
type: entity
title: NTT
aliases: []
created: 2026-05-18
updated: 2026-05-18
tags: [platform, vendor, sanctions]
owner: _unknown_
sources: [20260513-inrisk-integration-with-party-mdm-follow-up]
source_count: 1
status: stub
---

# NTT

## Summary
**NTT** is the vendor application that performs **sanctions processing** for Convex. The check determines whether a client may have sanctions placed on them — a gating check that must be carried out **before** any business can be done with that party. NTT's sanctions services are accessed via API. NTT is not owned by an in-scope team in the [[party-rearch]] project; it is a first-class external platform that sits at the edge of the [[sanctions-processing]] flow today via Boomi's orchestration.

## Key facts
- **Kind**: vendor system (external)
- **Owning team**: external — owning team inside Convex TBD ([[open-questions#OQ-032]])
- **Core technology**: API-accessed sanctions-check service
- **Role in programme**: enrichment / compliance source — the sanctions decision is consumed by [[party-application]] and [[inrisk]] today via Boomi orchestration

## Current shape (today)
- Sanctions checks are performed by NTT's services, called via API.
- The orchestration that decides _when_ to fire a sanctions check, _what data_ to send, and _what to do_ with the result currently lives in **Boomi**, not in NTT itself. See [[sanctions-processing]] for the domain framing and the call-chain.
- [[party-application]] emits events that Boomi listens to; Boomi then calls InRisk APIs one submission at a time (no batch endpoint) to gather submission context, and calls NTT to perform the actual check.
- Result is fed back into the deal lifecycle; the precise return path through to [[inrisk]] is part of what the room considers wrong-place today.

## Target-state exposure (from this programme)
- **Phase 1**: no change to NTT itself. Proxy events from MDM continue to drive Boomi, which continues to call NTT. See [[strangle-the-graph-via-proxy-events]] and [[party-rearch-phase-1]].
- **Phase 2+ (open)**: the room's stated direction is to **pull sanctions orchestration out of Boomi** into its own domain / service. NTT continues as the underlying API; ownership of the calling logic is the open question. Tracked at [[open-questions#OQ-032]] and (for the end-state flow itself) [[open-questions#OQ-008]].

## Relationships
- **Called by** Boomi orchestration (today); long-term: a dedicated sanctions service.
- **Consumes** submission and party context (passed through Boomi).
- **Feeds back** a sanctions decision used by [[party-application]] and [[inrisk]].

## Claims
- NTT is the application that performs sanctions processing for Convex — user-declared, 2026-05-18.
- NTT is accessed via API — user-declared, 2026-05-18.
- The aim of sanctions processing is to determine whether a client may have sanctions placed on them; the check is carried out in advance of doing any business — user-declared, 2026-05-18.
- Boomi today calls InRisk one submission at a time to reconstruct context because InRisk does not expose a multi-submission batch endpoint — Joe Worsfold, [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]].
- Audit's eyes are on sanctions this year — Suzanna Whitefield, [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]].

## Open questions
- Owning team inside Convex (and PO / Tech Lead, if any) for NTT integrations — [[open-questions#OQ-032]] (covers the broader sanctions-domain location question; NTT ownership will fall out of that).
- What is the long-term plan for the sanctions-call orchestration once it is moved off Boomi? — [[open-questions#OQ-008]] and [[open-questions#OQ-032]].

## Related
- [[sanctions-processing]] — the domain page; explains the current call-chain and the wrong-place problem.
- [[party-application]] — emits the events that drive sanctions checks today (via proxy events from Phase 1 onward).
- [[inrisk]] — provides submission context to Boomi; consumes the sanctions decision.
- [[andrea-read]] — escalation target for the sanctions-domain ownership conversation.
- [[party-rearch-dependency-map]] — the cross-app view of the sanctions flow.

## Sources
- [[sources/20260513-inrisk-integration-with-party-mdm-follow-up]] — first substantive treatment; surfaced NTT as a first-class platform.
