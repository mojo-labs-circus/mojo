# docs/ Directory Structure

* Status: accepted
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

All project documentation lived under `spec/` with a flat structure. As the project
grows, different document types (vision, architecture, decisions, research, governance)
need to be distinguishable without reading every file.

## Considered Options

* Keep `spec/` — familiar, already established
* Rename to `docs/` with flat structure — industry-standard name, same organisation
* Rename to `docs/` with typed subdirectories — industry-standard name, explicit
  separation by document type

## Decision Outcome

Chosen option: **`docs/` with typed subdirectories**, because `docs/` is the
universal industry standard (GitHub, most open source projects), and typed
subdirectories make document purpose immediately clear without reading content.

Structure:

```
docs/
├── vision/       ← why this exists, what we're building
├── framework/    ← framework planning sessions and output
├── architecture/ ← component architecture specs
├── decisions/    ← ADRs (MADR format)
├── collective/   ← collective governance
├── research/     ← exploratory ideas and competitor analysis
└── reference/    ← naming conventions, coding standards
```

### Consequences

* Good: document type is clear from path alone.
* Good: `docs/decisions/` aligns with MADR standard placement.
* Good: GitHub renders `docs/` for GitHub Pages if needed later.
* Bad: all internal cross-references from `spec/` paths needed updating.
