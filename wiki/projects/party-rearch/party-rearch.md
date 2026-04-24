---
type: project
title: Party Application Re-Architecture (party-rearch)
aliases: [party-rearc, party-mdm-rearch, party-application-rearch]
created: 2026-04-22
updated: 2026-04-22
tags: [project]
slug: party-rearch
status: in-flight
applications: [party-application, party-curation-tool, inrisk, inrisk-engine, high-volume, eclipse]
sources: [20260422-meeting-transcript-session-1, 20260422-meeting-transcript-session-2]
source_count: 2
---

# Party Application Re-Architecture

_Project-level thesis. Updated on every ingest. This is the evolving thesis of the programme for this project._

**Phases**: [[party-rearch-phase-1]] (in-flight, 1-Sep gate) · [[party-rearch-phase-2]] (planned — feature-tagging handover, direct Dynamo→DU stream, InRisk Engine final-state contract)

**Links**: [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] · [[open-questions]] (filter by project=party-rearch)

## Thesis

The Party Application is moving from a **payload-snapshot** integration model to a **versioned-reference** model: consumers receive a party ID + version, and resolve details near-real-time against the new MDM. Session 1 gave us the scaffolding to read this change systematically: every Party-consumer contract sits in one of three buckets — **operational** (how parties are created / curated), **data distribution** (who reads Party events), **enrichment** (external sources flowing in). The re-architecture is a coordinated change to all three. See [[contract-buckets]] for the full framework.

This is not a wire-format change in isolation — it is a load-bearing architectural shift that implies:

1. A new backend — **DynamoDB + OpenSearch** — replacing **Neo4j / Cypher**.
2. A new curation tool (**new PCT**) that must co-ship with MDM.
3. A migration path — the **strangler pattern via proxy events** — that emits in the current Graph event shape so the Data Universe and other downstream consumers don't see a breaking change on cutover.
4. A set of **three interim-state changes to InRisk** (party-ID storage, messaging, broker retrieval) so it can operate against the new MDM during Phase 1.
5. A **forcing function** — [[high-volume]] going into production **on 1 September**. This is the programme's external clock.

The re-architecture is explicitly *interim-first*: it ships a Phase 1 that makes the new MDM the source of truth, leaves some legacy shapes in place, and defers a set of end-state items (direct Dynamo-to-DU stream, client-ID removal, simplified sanctions flow, feature-tagging migration, One Re / CUO rules-based grouping). [[inrisk-engine]] targets the end state and goes live once the final-state Party contract is realised; it does **not** participate in the interim.

## Pillars

1. **Strangle the graph via proxy events** — [[strangle-the-graph-via-proxy-events]]. Cutover is made safe by the adapter, not by dual-running. **Invented in Session 1's DU-events segment**; consolidated in Session 2 with Ben Joseph's bidirectional-mappability criterion.
2. **PCT + MDM ship together** — [[pct-and-mdm-go-live-together]]. Dropping PCT would need a messy interim curation path; the value of the re-architecture is partly the UX. Operationally: **one feature flag** switches both the search widget and the PCT UI in a single cutover event.
3. **InRisk does the minimum for Phase 1** — three clean changes, no data-model rewrite. That's [[inrisk-engine]]'s job, on a different clock. Crucially (Session 1), InRisk _wants_ the change — the Tech Leads have flagged the interim path as simplifying their own model, which is the precondition that makes the whole strategy work.
4. **HV is API-only via Boomi** — no widget dependency; not a blocker on the Chakra V2/V3 question.
5. **Line in the sand on historical migration** — [[no-historic-client-backfill-into-mdm]] on the MDM side; [[no-pct-audit-backfill]] on the PCT side. MDM starts version history at cutover; pre-cutover InRisk client-ID-scoped history stays in InRisk / [[data-universe]]. Jira stays read-only; Snowflake is the cross-period audit/SLA store.
6. **End-state items stay on the roadmap, off the critical path** — they're tracked in [[party-rearch-dependency-map]]'s end-state section and are the subject of Phase 2+ ADRs ([[feature-tagging-moves-to-inrisk]]) and open questions, but they don't gate Phase 1.
7. **Phase-1 ownership calls ring-fence the critical path** — [[bulk-migrations-owned-by-mdm-phase-1]] and [[feature-tagging-moves-to-inrisk]] both explicitly _defer_ scope ([[dataops-team]] self-serve UX; InRisk taking feature-tagging) in favour of a clean Phase 1 shipment.

## Tensions

1. **Client ID is a bad abstraction, but unavoidable for Phase 1.** The renewal-cascade behaviour means historical submissions get re-parented to the latest client ID, which breaks DU-side historical fidelity. The right fix — key on `(party-id, version)` with submission as the carrier — is an end-state change. In the interim: tolerate, and tolerate carefully. Proxy events have to reproduce the current nesting, which may require InRisk to expose a reconstruction API if the DU doesn't tolerate a flatter shape. **Pending DU check.**
2. **HV integration shape is not yet specified.** [[high-volume-team]] (main contact [[simon-hulbert]]) is the owning team and 1 September is the fixed go-live; what's still loose is the expected Party-write volume, whether HV creates parties programmatically, and what auto-curation expectations apply. These inputs size the MDM intercept + backfill and the D&B auto-parent behaviour.
3. **PCT widget Chakra V2/V3 strategy is deferred.** In-team attempts to solve it produce three reasonable options (V2, minimal-render, V3-only-when-InRisk-upgrades); the only productive path is an InRisk-in-the-room workshop. Not a Phase-1 blocker on its own.
4. **"Party on requirement vs submission" is a [[prebind-team]] architectural choice, not ours.** It sits on their Requirement → Submission data-flow spine. Flagged here so it doesn't re-surface as a Party-side decision.
5. **InRisk Engine's readiness depends on our end-state being real.** [[devx-team]]'s timeline is coupled to the quality and stability of the final-state Party contract we define through Phase 1.
6. **Squad shape for MDM delivery is not decided.** Session 1 surfaced a real tension: [[graph-team]] is ~13 people; MDM delivery needs deep context which suits a ~3-person focused squad; but a 3-person cell creates a knowledge-bottleneck risk for the other ten. [[open-questions#OQ-017]] — Rory's call; will merit further discussion.
7. **Roles vs. views is the right model, but the feedback loop is new.** Views (e.g. "insured" / "reinsured") live in [[inrisk]] and now need to flow **back** into MDM to keep DU's projection shape stable. That feedback channel does not exist today; specifying it is part of the InRisk-side interim work.

## Known applications in scope

- [[party-application]] — the subject of the programme
- [[party-curation-tool]] — co-ships
- [[inrisk]] — interim-state consumer, 3 required changes
- [[inrisk-engine]] — end-state target only; out of interim scope
- [[high-volume]] — forcing function; API-only consumer

## Known teams in scope

- [[graph-team]] — builder of [[party-application]] + [[party-curation-tool]]
- [[prebind-team]] — [[inrisk]] owner; interim-state changes
- [[devx-team]] — [[inrisk-engine]] owner (final-state only)
- [[dataops-team]] — [[party-curation-tool]] users
- [[data-quality-team]] — business sponsors / owners of [[dataops-team]]
- [[analytics-team]] — Data Universe owners; schema-impact contact on the flattening question
- [[architecture-team]] — architectural review; SME on InRisk broker-retrieval flow
- [[high-volume-team]] — [[high-volume]] owner; forcing-function consumer
- [[tech-tooling]] — portfolio-level oversight

## What I'd want to know next

The register of _all_ unresolved questions lives in [[open-questions]]. The highest-leverage ones, in priority order:

1. **Analytics Team's answer on the flattened schema** ([[paul-rogers]]) — [[open-questions#OQ-002]]. Single biggest Phase-1 branch point.
2. **Graph API consumer audit** ([[joe-worsfold]] · [[billy-calladine]]) — [[open-questions#OQ-004]]. Sizes decommissioning scope; resolves [[open-questions#OQ-001]] ([[eclipse]] retirement) in one stroke.
3. **MDM delivery squad shape** — [[open-questions#OQ-017]]. Rory's call; blocks sprint planning.
4. **HV integration-shape specifics** ([[simon-hulbert]]) — [[open-questions#OQ-013]].
5. **InRisk's Chakra upgrade posture** — [[open-questions#OQ-005]].
6. **Final-state Party contract spec for [[inrisk-engine]]** — [[open-questions#OQ-018]].
7. **D&B scheduled-refresh cadence** — [[open-questions#OQ-011]].
8. **DataOps change-management lead time** — [[open-questions#OQ-014]].

## Clarifications resolved since Pass A

All resolutions are logged with IDs in [[open-questions#Resolved]]. Highlights:

- **ELA / FAS**: Expense and Loss Adjustment / Free Alongside Ship ([[open-questions#Resolved|OQ-R09]]).
- **Team name corrections**: Sergiu → [[graph-team]] Scrum Master ([[open-questions#Resolved|OQ-R02]]); DU Team → [[analytics-team]] ([[open-questions#Resolved|OQ-R03]]); Anton → [[devx-team]] Tech Lead ([[open-questions#Resolved|OQ-R04]]); Hugh → Head of Data Quality on [[data-quality-team]] ([[open-questions#Resolved|OQ-R05]]); John / Kris → [[prebind-team]] ([[open-questions#Resolved|OQ-R06]]); Andrea → [[tech-tooling]] ([[open-questions#Resolved|OQ-R07]]).
- **HV owning team** → [[high-volume-team]] / [[simon-hulbert]] ([[open-questions#Resolved|OQ-R08]]).
- **Session 1 speaker identifications** — all resolved; Billy and Joe each held two IDs ([[open-questions#Resolved|OQ-R10]], [[open-questions#Resolved|OQ-R11]]).
- **Tom Ash / Tomas** → [[tomas-sivo]], BA on [[graph-team]] ([[open-questions#Resolved|OQ-R12]]).
- **"Darius"** — transcription of "InRisk", not a person ([[open-questions#Resolved|OQ-R13]]).
- **"Boyle"** — transcription of "Will" ([[will-bone]]); no third recording exists ([[open-questions#Resolved|OQ-R14]]).

## Related
- [[party-rearch-dependency-map]] · [[party-rearch-ownership-matrix]] · [[open-questions]]
- [[contract-buckets]] — the Session 1 scaffolding framework
- [[strangle-the-graph-via-proxy-events]] · [[pct-and-mdm-go-live-together]] · [[no-historic-client-backfill-into-mdm]] · [[no-pct-audit-backfill]] · [[feature-tagging-moves-to-inrisk]] · [[bulk-migrations-owned-by-mdm-phase-1]] · [[d-and-b-caching-and-auto-parent]] · [[uuid-system-id-with-display-id]]
- [[sources/20260422-meeting-transcript-session-1]] · [[sources/20260422-meeting-transcript-session-2]]
