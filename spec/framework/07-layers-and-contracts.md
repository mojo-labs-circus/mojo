# Session 07 — Layers and Contracts

> Defines the three layers of the framework, what each owns, and the contracts
> between them.

---

## Purpose

Design the operational layers that sit beneath the collective — the infrastructure
that makes collective intelligence reliable, consistent, and efficient. The core
principle: everything that can be deterministic must be deterministic. Intelligence
is expensive and fallible. Reserve it for problems that genuinely require it.

---

## Context

A collective of intelligences needs supporting infrastructure to function. Not
everything the collective does requires intelligence. Routing, enforcing conventions,
triggering the right participant at the right time, checking that outputs meet
defined standards — these are algorithmic problems. Solving them with intelligence
wastes the collective's most valuable resource and introduces unnecessary variability.

Three layers exist. They are strictly separated. Each has a defined contract with
the others.

---

## The Three Layers

```
Tooling        — deterministic. Runs automatically. No intelligence involved.
Orchestration  — logical. Routes, schedules, tracks state. Minimal intelligence.
Intelligence   — judgement. The collective itself. Only what algorithms cannot do.
```

The question to ask at every design decision: which layer does this belong in?
If it can be an algorithm, it must be. If it requires judgement, it goes to
the intelligence layer.

---

## Questions to Resolve

**Tooling layer**

- What belongs unconditionally in the tooling layer?
  (Convention enforcement, CI/CD, automation triggers, notifications, state
  transitions driven by events)
- What are the contracts the tooling layer exposes to orchestration?
- What can the tooling layer never do — what is outside its authority?
- How is the tooling layer maintained? Who owns it, who can change it?

**Orchestration layer**

- What is the orchestration layer responsible for?
  (Routing contributions to the right participant, scheduling, tracking collective
  state, surfacing blockers, triggering the right tooling)
- Where is the line between orchestration and intelligence — what does orchestration
  decide vs what does it defer to the collective?
- How does orchestration represent the state of the collective at any moment?
- What are the contracts orchestration exposes to the intelligence layer?

**Intelligence layer**

- What belongs exclusively in the intelligence layer?
  (Judgement calls, design decisions, novel problems, anything requiring context
  and understanding that an algorithm cannot have)
- How does the intelligence layer consume outputs from orchestration?
- How does it hand results back down — and how does it communicate uncertainty
  or blockers?

**Contracts between layers**

- What can each layer see of the others? What is hidden?
- What can each layer request from the others — and what can it never request?
- How are contract violations detected and handled?
- What happens when a layer fails — how does the collective degrade gracefully?

**The principle in practice**

- How do you test whether something belongs in tooling vs orchestration vs
  intelligence? What is the decision rule?
- Real-world examples: where does each of the following live?
  Branch naming enforcement, deciding which participant picks up a contribution,
  writing the contribution itself, detecting that a convention was violated,
  deciding what to do about it.

---

## Output Required

1. Tooling layer definition — what it owns, its contracts, its limits
2. Orchestration layer definition — what it owns, its contracts, its limits
3. Intelligence layer definition — what it owns, its contracts, its limits
4. Contract specifications — what each layer can request from and expose to others
5. The decision rule — how to classify any given function into the right layer
6. Degradation model — what happens when a layer fails

---

## Dependencies

Session 01 — First Principles
Session 02 — Agent Model
Session 04 — Collective Structure
