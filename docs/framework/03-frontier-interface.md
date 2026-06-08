# Session 03 — Frontier Interface

> Defines how a local intelligence calls, controls, and integrates external
> intelligence. The boundary between what the collective owns and what it borrows.

---

## Purpose

Design the protocol by which a local agent delegates to an external intelligence
(a frontier model — Claude, Codex, Gemini, or any future equivalent). This is not
about AI integration. It is about how a self-sovereign collective intelligence uses
external intelligence as a resource — without ceding control, leaking context, or
becoming dependent on it.

The local collective holds all state, all memory, all identity. Frontier models are
powerful but external. They are consulted, not trusted. They contribute capability,
not authority.

---

## Context

Singular AI intelligence — frontier models — already exists. What this framework
adds is the collective layer: local agents that hold shared context, coordinate
toward a goal, and decide when and how to consult external intelligence to augment
their own.

Think of it like a team consulting an external expert. The expert is smart and
capable. But the team briefs the expert only on what is relevant, retains ownership
of the decision, and integrates the expert's input into their own understanding.
The expert does not join the collective. They are consulted and released.

---

## Questions to Resolve

**The consultation protocol**

- What is the interface contract between a local agent and a frontier model?
  What is always included in a call? What is never included?
- How does a local agent construct the minimal context a frontier model needs
  to contribute usefully — no more, no less?
- How does the result come back, and how does the local agent validate, interpret,
  and integrate it into the collective's understanding?
- Who decides when to consult a frontier model — the individual agent, the team
  lead, or the collective?

**Privacy and sovereignty**

- What categories of the collective's knowledge must never leave the local system?
- Who decides what is safe to share — the calling agent, a collective policy, or both?
- How is this enforced structurally, not just by convention?

**Efficiency**

- How does the system choose the right external intelligence for the right
  contribution (cost, capability, latency, trust)?
- How does it avoid redundant consultations — caching, deduplication, batching?
- How does it minimise the context passed without losing the quality of the
  contribution?

**Model agnosticism**

- The protocol must not be tied to any specific frontier model.
- What is the abstraction layer that makes swapping Claude for Codex for Gemini
  transparent to the calling agent?

**Control and authority**

- A frontier model is not a member of the collective. It has no persistent state,
  no role, no stake in the goal. It produces a contribution. The local agent
  decides what to do with it.
- How is this boundary maintained operationally?
- What happens when an external contribution conflicts with the collective's
  existing understanding or decisions?

---

## Output Required

1. The consultation contract — inputs, outputs, guarantees for every frontier call
2. The context construction protocol — how a local agent builds a minimal brief
3. The privacy and sovereignty model — categories, enforcement mechanism
4. The efficiency model — selection, caching, batching principles
5. The abstraction layer — what makes the protocol model-agnostic

---

## Dependencies

Session 01 — First Principles
Session 02 — Agent Model
