---
title: Spec Conventions
type: meta
status: draft
owner: clarke
created: 2026-06-03
updated: 2026-06-03
dependencies: null
---

# Spec Conventions

This document defines how specifications are written, structured, and maintained in this project. It applies to all spec types and to all contributors, human or agent.

## What a Spec Is

A spec is a written agreement about what something is — not how to build it. It captures decisions, their reasoning, and the constraints that shaped them. It is the source of truth that implementations derive from, not a description of an implementation after the fact.

Specs are not documentation. Documentation describes how something works after it is built. Specs establish what should be built and why, before implementation begins. Specs are not task lists, plans, or implementation guides.

## Spec Types

Seven types of spec exist in this project.

**Meta** — defines how specs are written, structured, and maintained. The conventions document is the only meta spec.

**Vision** — describes the product: the problem it solves, what it is, who it is for, and what becomes possible. Written once, rarely changed.

**Framework** — foundational principles that everything else derives from. Produced by the eight framework planning sessions. Each principle is named, evidenced across domains, and carries an implication for the system design.

**Architecture** — describes how the system is structured: components, responsibilities, data flows, and key decisions. Not implementation detail — structure.

**Component** — one spec per component. Defines purpose, responsibilities, what the component does not do, its interfaces, dependencies, and constraints.

**ADR (Architecture Decision Record)** — captures a single architectural decision. Numbered sequentially. Never deleted — only superseded. Records context, options considered, the decision, rationale, and consequences.

**PRD (Product Requirements Document)** — defines what the product does, for whom, and how success is measured. Produced after the framework is complete.

## Metadata

Every spec begins with YAML frontmatter:

```yaml
---
title:
type:          # meta / vision / framework / architecture / component / adr / prd
status:        # draft / review / active / superseded
superseded_by: # ADR only — path to the superseding ADR; omit for all other spec types
owner:         # human owner, or agent-name(human-owner) if produced by a named agent
created:       # ISO date
updated:       # ISO date — update on significant changes only
dependencies:  # list of spec paths this spec builds on — null if none
---
```

`owner` and `dependencies` are required fields. `dependencies` may be null; a null value explicitly states that this spec has no dependencies. `owner` must always trace to a human — computers cannot be held accountable. If a named agent produced the spec, the owner is written as `agent-name(human-owner)`, recording both which agent acted and who is accountable for the content. Direct human authorship is written as the human's name alone.

**Status lifecycle:**

A spec moves through three statuses as it matures. Promotion from `review` to `active` requires sign-off from the human named in the `owner` field — either directly, or as the named human behind an `agent-name(human-owner)` owner.

- `draft` — being written. Not yet authoritative. Read for context only; do not build from a draft spec.
- `review` — criteria met; awaiting owner sign-off. An agent or author marks a spec `review` when: no open questions block implementation, and all dependencies are either `active` or the uncertainty is explicitly noted in the spec body (using the mechanisms defined in the Uncertainty section). The agent self-assesses the criteria; the owner reviews and signs off on the content.
- `active` — owner has signed off. Authoritative. This is the source of truth. Build from active specs. Agents do not promote their own specs to `active` — only the human named as owner can.
- `superseded` — ADR only. This ADR has been replaced by the ADR named in `superseded_by`. Preserved for decision archaeology — never deleted, never promoted back. Read it to understand why the current decision exists.

Specs that are no longer relevant are deleted, with the exception of ADRs — these are never deleted, only superseded. When a spec is deleted because a newer spec replaces it, the replacing spec names the deleted spec in a revision note. Git history is the record of what existed and why it was removed.

## Directory Structure

```
spec/
├── conventions.md       ← this document
├── glossary.md          ← shared glossary of project terms
├── vision.md            ← vision spec
├── framework/           ← framework planning sessions and output
│   ├── framework.md     ← accumulated framework output
│   └── NN-name.md       ← session briefs
├── architecture/        ← system-level architecture specs
├── components/          ← one spec per component
├── adr/                 ← architecture decision records
└── prd/                 ← product requirements
```

## File Naming

All files use kebab-case. ADRs are numbered sequentially with three-digit padding: `001-short-title.md`. All other specs use a descriptive name that matches the title.

## Prose Style

Specs are written in direct, assertable prose. The governing principle: every conclusion or position must state its reason. Descriptive facts about the system stand alone.

**Voice and person:** Active voice, third person throughout. "The router classifies intent" — not "intent is classified" and not "we decided the router classifies intent." Third person is timeless and survives team and contributor changes.

**Tense:** Present tense throughout, including for things not yet built. Specs are written as if describing the system as it is. "The router receives a request." Not "the router will receive." Past tense is reserved for historical context in ADRs. Future tense is used only when explicitly signalling that something is undecided or not yet designed.

**Sentences:** One idea per sentence. Logical connectives — because, therefore, which means — earn their place when the relationship between clauses is the point. Sentences that lose meaning when separated are not split.

**Assertability:** Every conclusion is stated directly as a position: "X because Y." If a claim cannot be stated that way, it is not a conclusion — it belongs in the open questions section, not the body.

**Prose vs bullets:** Prose is the default. Bullets are for genuine enumerations: lists of components, interface definitions, requirement lists, option tables. Explanations, reasoning, and decisions are always prose. A bullet that requires a sub-bullet to explain it is a sentence, not a list item.

**Headers:** Headers separate genuinely distinct sections. Two levels deep is the maximum in most specs. Headers are not used to impose visual structure on content that should be a paragraph.

**Examples:** Examples appear only when a concept would be genuinely ambiguous without one. Examples add length — they earn their place only by resolving ambiguity that prose alone cannot.

**Length:** Specs are as long as necessary, no longer. A spec that includes implementation detail, process notes, or content that belongs elsewhere is too long.

## Formatting

Formatting carries semantic meaning. If removing a formatting choice would lose meaning beyond appearance, it is earning its place. Otherwise, cut it.

**Bold** — reserved for terms being defined inline. Not used for general emphasis.

**Italics** — reserved for titles of external documents or other specs being referenced. Not used for emphasis.

**Inline code** — used for anything that appears literally in the codebase: file paths, command names, config keys, API method names. Not used for conceptual or component names.

**Tables** — used for structured comparisons across consistent attributes: option comparisons, interface definitions, status tables.

**Blockquotes** — no use case in specs.

**Diagrams** — ASCII only. Binary image files are not permitted. ASCII diagrams are portable, diffable, and readable without rendering.

## Code in Specs

Specs describe what something is and why. Code belongs in the codebase. A spec that contains significant code is drifting into implementation territory.

Code is permitted in three cases only:

- **Config or schema examples** where the exact format matters and prose cannot express it
- **Interface contracts** where the spec is defining a boundary other components depend on — an API signature, a data structure shape
- **Short illustrative snippets** where prose genuinely cannot express the concept — rare, and always a signal to question whether the spec is being too prescriptive

More than five to ten lines of code in a spec is a red flag. The code either belongs in the codebase and should be referenced from the spec, or the spec is over-specifying implementation.

## Uncertainty

Uncertainty is always stated explicitly. Two mechanisms:

**Inline** — for minor uncertainty within a section. The inline statement identifies what is uncertain and why: "This boundary is uncertain because the frontier interface session has not yet resolved how context is scoped."

**Open questions section** — for anything that requires resolution before or during implementation. Each question states what needs to be answered and why it matters. Not a dumping ground — only questions that genuinely block or shape a decision.

Uncertainty is never buried in hedged language. "This might be worth considering" tells a reader nothing. "This is uncertain because X has not been decided" is actionable.

## Glossary

All project-specific terms are defined in `spec/glossary.md`. Terms appear in alphabetical order. First use of any glossary term in a spec should reference the glossary.

When a spec introduces a term not yet in the glossary, that term is added to `spec/glossary.md` as part of completing the spec. Definitions are one to two sentences.

## Dependencies and Cross-References

Dependencies — specs that a given spec builds on — are declared in frontmatter. A spec should be readable without loading its dependencies, but any contributor or agent working from a spec should know what else to read for full context.

Cross-references within the body use relative paths: `../framework/framework.md`. ADRs are referenced by short form: `[ADR-001]`.

## Revision Notes

Git history records all changes. Revision notes in the spec itself are reserved for significant changes: a decision reversed, a scope change, a major rewrite. When a significant change occurs, a one-line note is added at the top of the spec body with the date and reason. Micro-edits do not warrant a note.
