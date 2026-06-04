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

**Task decomposition**

Decomposition is an orchestration responsibility, not an intelligence one. The
orchestration layer should break incoming work into the smallest independently
executable units before routing to any agent. This is not just a performance
optimisation — it is a foundational design principle with three motivations:

- **Performance**: agents perform better with focused, narrow tasks. Tunnel vision
  is a feature. An agent given a large ambiguous task will produce lower-quality
  output than one given a precise, scoped task.
- **Security**: the smaller the task, the smaller the context the agent needs —
  and therefore the smaller the blast radius if that agent is compromised or behaves
  unexpectedly. Minimal context is a security property, not just an efficiency one.
- **Accountability**: fine-grained tasks produce fine-grained audit trails. When
  something goes wrong, the failure is localised to a specific task unit. Blame
  assignment and root cause analysis become tractable.

Questions to resolve:

- What is the minimum viable task unit — when has a task been decomposed enough?
- Who is responsible for decomposition — is it always the orchestration layer, or
  can an intelligence node decompose and re-submit?
- How does the orchestration layer know that a task has been over-decomposed
  (coordination overhead exceeds the gain from focus)?
- How does context scoping work — what does orchestration give each agent, and
  how is it determined? The principle is: no more than the task requires.

**Mathematical model for optimal context size**

The optimum context size for a task is potentially findable mathematically rather
than by intuition. Define:

- `B(c)` — benefit as a function of context size `c`. Increases rapidly as the
  agent receives necessary information, then flattens as context becomes redundant,
  then turns negative as noise, scope creep, and degradation set in. Shape: concave
  (sigmoid or log-like).
- `C(c)` — cost as a function of context size. Token cost is roughly linear.
  Security surface, injection risk, and error propagation are plausibly superlinear
  — interactions between context elements compound. Shape: convex and monotonically
  increasing.
- `V(c) = B(c) - C(c)` — net value. Has a maximum.

The optimum satisfies `V'(c) = 0`, equivalently `B'(c) = C'(c)`: marginal benefit
equals marginal cost. This is a standard result given concavity of B and convexity
of C — existence and uniqueness of the optimum can be proven under mild assumptions,
even if a closed-form solution requires empirical calibration of the function shapes.

Close analogues already exist: the bias-variance tradeoff in ML (too little context
= underfitting, too much = overfitting to noise), and Tishby's Information Bottleneck
(the minimum representation that preserves mutual information with the correct output).

Session 07 should explore whether a rigorous proof of optimum existence can be
established, and what empirical approach would let a specific collective calibrate
`B(c)` and `C(c)` for its task types.

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
6. Decomposition model — minimum task unit definition, who decomposes, context
   scoping per task, and how over-decomposition is detected
7. Degradation model — what happens when a layer fails

---

## Dependencies

Session 01 — First Principles
Session 02 — Agent Model
Session 04 — Collective Structure
