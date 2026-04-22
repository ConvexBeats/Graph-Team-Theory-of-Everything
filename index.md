# Index

_Last updated: 2026-04-22_
_Pages: 0 sources · 5 applications · 4 teams · 0 concepts · 0 decisions · 2 analyses (standing) · 1 overview_
_Status: seeded — awaiting first ingest_

> The LLM reads this first on every query. Entries = link + one-line summary + optional metadata (`src: N` = number of sources, `owner: …`, date, etc.).

## Overview
- [[overview]] — programme thesis and pillars _(draft)_

## Applications
- [[party-application]] — core Party store and routing; re-architecture subject (owner: [[graph-team]], state: current → target)
- [[party-curation-tool]] — PCT; Next.js frontend over Jira workflow for party curation (owner: [[graph-team]], user: [[dataops-team]])
- [[inrisk]] — IR2; live Policy Admin System for prebind, affected consumer (owner: [[prebind-team]])
- [[inrisk-engine]] — InRisk Core; API-first rewrite co-defining target Party contract (owner: [[devx-team]])
- [[high-volume]] — HV; forcing-function Party consumer, in development (owner: **unknown**)

## Entities
### Teams / working units
- [[graph-team]] — owns [[party-application]] and [[party-curation-tool]]; aka "The Party Team" in PCT context
- [[dataops-team]] — primary users of [[party-curation-tool]]
- [[prebind-team]] — owns [[inrisk]]; aka "The Strikers"
- [[devx-team]] — owns [[inrisk-engine]]

## Concepts
_None yet._

## Decisions
_None yet._

## Sources
_None yet._

## Analyses
### Standing (auto-maintained)
- [[dependency-map]] — inter-application dependencies, current vs. target _(stub)_
- [[ownership-matrix]] — workstreams × accountable working units _(stub)_

---

## How to read this file
- **Applications** — one page per software system, current / target / transitional states tracked separately.
- **Entities** — teams, people, orgs, vendors. Teams are `tags: [team]` under this folder.
- **Concepts** — abstract ideas, frameworks, vocabulary (e.g. party-versioning semantics once defined).
- **Decisions** — ADR-style records of architectural decisions.
- **Sources** — one page per ingested raw source (meeting note, spec, diagram, etc.).
- **Analyses** — cross-cutting synthesis. Standing analyses (prefixed above) are kept current on every ingest.
