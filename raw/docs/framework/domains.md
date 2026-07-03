---
title: Domains — Planning
type: framework
status: draft
owner: clarke
created: 2026-06-03
updated: 2026-06-03
dependencies: null
---

# Domains

## The Concept

Every mojo collective has a constitution (the universal layer) and a domain — the specific type of work the collective does. The domain determines what additional document types and conventions sit on top of the constitution.

The constitution is fixed across all collectives. The domain varies.

## Known Domains

### Software Development

The most developed domain. A software development collective uses a full spec system on top of the constitution:

- Component specs — what each software component is, its responsibilities, and its boundaries
- Architecture specs — how components relate and how the system is structured
- Interface specs — contracts between components; what is exposed, in what form
- ADRs (architecture decision records) — software-specific architectural decisions
- PRD (product requirements) — what the software does, for whom, and how success is measured

The writing conventions for this domain are documented in `spec/conventions.md`.

### Household / Family AI

A collective managing a home AI system. Additional documents:

- Member profiles — who each person is, their preferences, needs, and how they interact with the system
- Routines — what the AI does regularly on behalf of members
- Privacy rules — what data stays local, what is shared, what is never processed
- Interaction protocols — how each family member works with the system

### Research Group

A collective conducting research. Additional documents:

- Research plan — what is being investigated and why
- Methodology — how the research is conducted
- Hypothesis tracker — active hypotheses and their current status
- Findings log — confirmed findings and their evidence base

### Creative Team

A collective doing creative work. Additional documents:

- Briefs — what is being made and for whom
- Style guide — creative constraints and aesthetic decisions
- Feedback processes — how work is reviewed and iterated

### Business Unit

A collective running a business function. Additional documents:

- OKRs — objectives and key results
- Process playbooks — how recurring work is done
- Stakeholder maps — who the collective serves and how
- Operational runbooks — how to handle specific recurring situations

## Open Questions

- Is there a formal way to define a new domain — a domain spec that the mojo-agent can read and deploy?
- How does the mojo-agent determine which domain a collective is using?
- Can a collective operate across multiple domains?
- What is the minimum a domain definition must specify to be valid?
- Should domains be versioned separately from the framework?
- Who can define a new domain — any collective, or only the framework itself?

## Framework Session Connection

Domain thinking belongs primarily in:

- **Session 08 (dev-team-instance)** — the software development domain is the first concrete instance and the most fully specified
- A possible new session covering the domain abstraction itself — how domains are defined, how the mojo-agent deploys them, and how new domains are created by collectives

Consider whether the domain abstraction is significant enough to warrant its own session between 07 and 08.
