---
type: analysis
title: Ownership Matrix
created: 2026-04-22
updated: 2026-04-22
tags: [standing, ownership]
application: [party-application, party-curation-tool, inrisk, inrisk-engine, high-volume]
sources: []
source_count: 0
status: stub
---

# Ownership Matrix

Standing analysis: which working unit is accountable for which piece of the re-architecture. Refreshed on every ingest. Surfaces gaps (unowned work) and overlaps (disputed ownership).

## Applications × owners

| Application | Accountable working unit | Primary users / consumers | Notes |
|---|---|---|---|
| [[party-application]] | [[graph-team]] | [[inrisk]], [[party-curation-tool]], [[high-volume]] (planned) | Core re-architecture subject |
| [[party-curation-tool]] | [[graph-team]] | [[dataops-team]] | Must keep working through transitions |
| [[inrisk]] | [[prebind-team]] | — | Affected consumer; eventual retirement |
| [[inrisk-engine]] | [[devx-team]] | (future Party consumer) | Co-defines target contract |
| [[high-volume]] | **unknown** | — | Owner must be identified |

## Workstreams × owners

Populated as discussions land. Every meaningful workstream should live here with a single accountable owner. Conflicts or gaps are explicit.

| Workstream | Accountable | Supporting | Status | Source |
|---|---|---|---|---|
| Target Party data model (co-definition) | [[graph-team]] + [[devx-team]] | — | open | _pending first ingest_ |
| Target Party API contract (ID + version) | [[graph-team]] | [[devx-team]], HV owner | open | _pending_ |
| Transitional dual-contract serving on [[party-application]] | [[graph-team]] | — | open | _pending_ |
| [[inrisk]] migration or sunset plan | [[prebind-team]] | [[graph-team]], [[devx-team]] | open | _pending_ |
| [[high-volume]] Party integration approach | **unknown** | [[graph-team]] | open | _pending_ |
| [[party-curation-tool]] alignment to new contract | [[graph-team]] | [[dataops-team]] | open | _pending_ |

## Open actions

_None filed yet. Meeting-note ingests will populate this table per the schema (§5.1a)._

| Action | Owner | Application | State | Source |
|---|---|---|---|---|
| — | — | — | — | — |

## Gaps & overlaps

- **Gap**: [[high-volume]] has no known owning working unit. Until identified, any HV-scoped workstream has no accountable party.
- **Potential overlap**: the target Party data model is co-owned by [[graph-team]] and [[devx-team]]. Healthy during design; needs a clear decision-making process to avoid stall.

## Related
- [[overview]] · [[dependency-map]]
