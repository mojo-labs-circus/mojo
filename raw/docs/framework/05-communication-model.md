# Session 05 — Communication Model

> Defines how intelligences in a collective share understanding with each other.

---

## Purpose

Design how intelligences communicate within a collective — not just to pass
instructions, but to build and maintain the shared understanding that makes
collective intelligence possible in the first place.

Communication is not a feature of a collective. It is what a collective is made of.
Without it, you have a group of individuals. With it, you have something that can
think and act together.

---

## Context

From first principles we understand what the interface between two intelligences
requires. From the collective structure we know the graph — who exists and how they
relate. This session defines what flows along those edges and how.

---

## Questions to Resolve

**What is communication in a collective?**

- What is the difference between communication and coordination? Are they the same?
- What is "shared context" — and is it a precondition for collective intelligence
  or an emergent product of it?
- What does an intelligence actually need to receive from another to update its
  understanding meaningfully?

**Channels and primitives**

- What are the valid communication channels in a collective?
- When is async communication appropriate vs synchronous?
- What is the right channel for: status updates, decisions, blockers, knowledge
  transfer, escalation?
- How does the collective avoid communication overload — too much noise destroying
  the signal?

**Information flow**

- What information should flow freely across the collective?
- What information should be scoped — available only within a sub-group or
  between specific intelligences?
- Who decides the scope of any given piece of information?
- How does critical information reach every intelligence that needs it, without
  flooding those that don't?

**The leader's view**

- The collective's leader (top node) needs a coherent picture of the collective's
  state without being in every communication.
- How does a leader stay informed without being a bottleneck?
- What surfaces to the top automatically vs what requires explicit escalation?

**Norms and conventions**

- What communication conventions must every participant in the collective follow?
- How are those conventions enforced — by tooling, by social pressure, by both?
- What happens when a participant communicates poorly — too little, too much, or
  at the wrong time?

**Cross-collective communication**

- How do intelligences from different collectives communicate when they need to?
- What is the interface between two separate collectives?

**Knowledge Objects**

The most valuable thing intelligences exchange is not instructions or status — it is *understanding*. This session must produce the Knowledge Object (KO): the framework primitive for encoding and transferring what one intelligence knows about a subject.

A KO is a portable, semantically-tagged representation of an intelligence's understanding. It is tagged to the producing member (human or AI). AI members consume KOs natively. Human members receive rendered views (briefs, presentations). KOs are not a document format — they are the seam between Member A's understanding and Member B's.

Baseline design is in `docs/research/ko-design.md`. Review it before the session. The schema and concepts there are provisional — this session finalises them.

- What fields does a KO need? What is essential vs. optional?
- What semantic roles can a knowledge node play?
- How do KOs reference other KOs — what does a dependency mean?
- What does a receiver-side intelligence need to compute the gap between its understanding and a KO?
- How do KOs evolve as understanding deepens? Versioning model?
- Storage convention — where do KOs live, how are they named?
- When is a KO produced vs. updated vs. superseded?

---

## Generalisation Test

The communication model must hold for:

- A dev team collaborating on a codebase
- A sports team coordinating during play
- A research group sharing findings
- A creative team working toward a shared vision

---

## Output Required

1. Definition of communication in a collective — what it is and what it achieves
2. Channel taxonomy — valid channels, when each is appropriate
3. Information scoping model — what flows where, who controls it
4. Leader view model — how the top node maintains situational awareness
5. Communication norms — what every participant must do, enforced how
6. Cross-collective interface — how separate collectives communicate
7. Knowledge Object schema — finalised fields, semantic roles, storage convention, versioning model, diff-readiness

---

## Dependencies

Session 01 — First Principles
Session 04 — Collective Structure
