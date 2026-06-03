---
title: Constitutional Contract — Planning
type: framework
status: draft
owner: clarke
created: 2026-06-03
updated: 2026-06-03
dependencies: null
---

# Constitutional Contract

## The Concept

Every collective that forms under the Mojo framework constitutes itself by writing a constitution — the governing contract that defines what the collective is, who its members are, how it operates, and what it is working toward. This is the first act of any collective. It is not a technical document or a software spec. It is the founding agreement.

The analogy holds precisely. Countries, companies, and open source projects all constitute themselves before doing work. The Mojo framework formalises this act for collectives of intelligences.

## Naming

- The entity: **collective**
- The members: **intelligences** (human and AI alike — the physical substrate does not matter)
- The founding document set: **constitution**

Infrastructure — machines, servers, networks — is not an intelligence. Intelligences are the agents and humans that hold roles and responsibilities within the collective. A server that hosts agents is not itself defined in the constitution; the agents running on it are.

## The Five Constitutional Documents

Every collective has five constitutional documents regardless of domain.

**Charter** — what the collective is and why it exists. Written once, rarely changed. The founding statement.

**Intelligence definitions** — one per member. What that intelligence is, what it is responsible for, what it explicitly is not responsible for, and how it relates to others. Humans are defined here the same way AI agents are. No distinction in format.

**Decision records** — numbered, never deleted. Significant decisions the collective has made, what was considered, and why that choice was made. Permanent record of the collective's reasoning over time.

**Protocols** — how intelligences coordinate. What information flows between members, in what form, triggered by what. Universal regardless of domain — works for API contracts, interaction patterns, handoff processes, reporting structures.

**Goals** — what the collective is working toward and how success is measured. Distinct from the charter because goals evolve as the collective progresses; the charter does not.

## Open Questions

- What does each constitutional document look like structurally? Required fields, sections, constraints on length and form?
- How does the mojo-agent scaffold a new constitution? What does it ask, what does it generate?
- How do constitutional documents relate to domain-specific documents?
- Should intelligence definition format differ between human and AI intelligences, or is it uniform?
- How are decision records numbered — per-collective sequence, or shared with domain decision records?
- What triggers a goal update vs a charter amendment?
- Does the constitution have a ratification step — a moment at which it becomes authoritative?

## Framework Session Connection

This belongs primarily in:

- **Session 04 (org-structure)** — how intelligences organise into a collective; the constitution is the formalisation of that organisation
- **Session 07 (layers-and-contracts)** — the constitution is the top-level contract; domain documents sit below it

Consider whether session 04 should be expanded to cover the constitutional contract explicitly, or whether a dedicated session is warranted between 04 and 05.
