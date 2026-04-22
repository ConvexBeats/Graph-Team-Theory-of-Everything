---
type: overview
title: Project Overview
created: 2026-04-22
updated: 2026-04-22
tags: [meta, overview]
status: draft
---

# Project Overview

## Summary
This wiki tracks the re-architecture of the [[party-application]] (aka *Graph* / *Party MDM*) — the authoritative store for business parties (brokers, clients, underwriters, etc.) across the insurance-deal lifecycle — and the coordination with adjacent applications and teams required to land it. The programme is framed by one hard milestone: the new contract must be in place before [[high-volume]] goes live.

## Thesis
The [[party-application]] is shifting from delivering **large party payloads that downstream systems snapshot** to delivering **versioned party references (ID + version) that downstream systems resolve near real-time**. The target model is being co-defined with the [[inrisk-engine]] rewrite ([[devx-team]]), not with today's [[inrisk]] shape. Intermediary transitional states will keep current consumers ([[inrisk]] via [[prebind-team]], [[party-curation-tool]] via [[dataops-team]]) operational while the migration happens consumer-by-consumer. Success is measured by [[high-volume]] going live on the new contract without regressing anything that runs today.

## Pillars
1. **Data model shift** — payload → ID + version + on-demand detail fetch. _Primary page: [[party-application]]._
2. **Target-state co-definition** — target Party model defined jointly with [[inrisk-engine]]. _[[graph-team]] + [[devx-team]]._
3. **Consumer continuity** — [[inrisk]] and [[party-curation-tool]] keep working through transitions. _[[prebind-team]], [[dataops-team]]._
4. **Forcing function** — [[high-volume]] go-live is the programme's deadline-shaped constraint.
5. **Ownership clarity** — every workstream has a single accountable working unit. _Tracked in [[ownership-matrix]]._

## Tensions
- **Ownership gap**: no known owner for [[high-volume]] despite it being the forcing-function consumer.
- **Co-definition risk**: target Party model is co-owned by [[graph-team]] and [[devx-team]]. No decision-making protocol is documented yet — risk of stall or contradictory choices.
- **Sequencing risk**: [[inrisk-engine]] is still prototype stage; if its maturity lags the Party re-architecture, the target contract is being designed for something that doesn't yet exist in a testable form.
- **Surface-area risk**: [[party-curation-tool]]'s current UX may not cleanly map onto new version semantics (merges, in-place edits). Could become a hidden scope item.
- **Visibility gap**: the full list of Party consumers isn't known. We have three ([[inrisk]], [[party-curation-tool]], [[high-volume]]); others may exist and would surface as affected parties mid-programme.

## What I'd want to know next
- [[high-volume]] owning team and target go-live date.
- Current Party Application API surface (schemas, endpoints, auth) and the proposed new one.
- [[inrisk-engine]] prototype coverage and critical-path gaps.
- Version semantics: monotonic int? content hash? semver? immutable snapshot-per-version?
- SLA / latency / throughput budgets the new Party contract must meet under [[high-volume]] load.
- How party **roles** (broker vs. client vs. …) are modelled now and in the target.
- Complete inventory of current Party Application consumers — is the list of three comprehensive?
- Decision-making protocol for [[graph-team]] ↔ [[devx-team]] co-defined target model.

## Pending clarifications (project-wide)
- **ELA** and **FAS** — full names of these insurance lines (listed in [[AGENTS#10-project-scope|project scope]]).

## Related
- [[index]] — full catalog
- [[dependency-map]] — current and target inter-application dependencies
- [[ownership-matrix]] — working units × workstreams
