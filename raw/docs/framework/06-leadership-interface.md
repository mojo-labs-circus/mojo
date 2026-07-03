# Session 06 — Leadership Interface

> Defines the role of the top node in a collective and how it interacts with
> the collective it leads. Generic — not tied to any specific person or system.

---

## Purpose

Design what leadership means in a collective built on this framework — and how
the leader participates in, steers, and maintains the collective without
micromanaging it or becoming a bottleneck.

Leadership is not separate from the collective. The leader is a participant with
a specific role: holding the goal, making decisions only they can make, and creating
the conditions for the collective to function. This session defines that role and
its interface with the rest of the collective.

---

## Context

From first principles we understand what role leadership plays in collective
intelligence — when it is necessary, what it provides, and what happens when it
is absent or excessive. From the collective structure we know the leader is the
top node of the graph. This session defines how that node behaves and what it
needs.

---

## Questions to Resolve

**What does leadership provide to a collective?**

- What can only the leader decide? What would be wrong for any other participant
  to decide unilaterally?
- What does the leader provide that the collective cannot generate itself?
  (Direction, legitimacy, final arbitration, coherence across sub-groups?)
- What should the leader never do — and why?

**How the leader stays informed**

- What does the leader need to know about the collective's state at any moment?
- What is the minimum — and what is the maximum before it becomes noise?
- How does information surface to the leader without requiring them to ask for it?
- How does the leader distinguish signal (something requiring their attention)
  from noise (the collective working normally)?

**How the leader steers**

- What are the valid inputs a leader gives to the collective?
  (Goal setting, prioritisation, unblocking, overriding, course correction)
- How does the leader intervene without disrupting the collective's momentum?
- What is the difference between steering and micromanaging — and how is
  that boundary maintained?

**Decision gates**

- What decisions must explicitly pass through the leader before the collective
  can proceed?
- What is the protocol when the collective needs a leader decision urgently?
- How does the collective continue functioning while waiting for a leader decision?
- What is the minimum information a human leader needs to approve or reject a
  decision without having to understand the full collective state?
- Are there decisions that are always gated on human approval regardless of trust
  level — and if so, what defines that category? (Irreversibility? External impact?
  Resource commitment above a threshold?)

**Human-AI approval architecture**

This is a first-class design problem, not an implementation detail. The approval
model determines whether human oversight is usable or a friction tax.

- What is the interaction model when the collective surfaces something for human
  review? Blocking (collective pauses) vs async (collective continues, human
  responds when available)?
- How does the collective prioritise what surfaces to the human leader — and how
  does it avoid creating notification noise that trains the human to ignore signals?
- What does a human approval interface look like in practice? The human cannot
  read every agent's state — what is the minimal, actionable summary?
- What happens if the human is unavailable? Timeout-and-proceed? Escalate to
  a fallback? Hard stop?
- How does the collective distinguish decisions that need human judgment from
  decisions it has been trusted to make autonomously?

**Trust and delegation**

- As trust in the collective develops, how does the leader's role change?
- What can be delegated over time — and what can never be delegated?
- How does the leader know when to tighten control and when to loosen it?
- How is trust earned operationally — what evidence causes a human to extend
  more autonomy to the collective?
- Is trust per-agent, per-sub-group, or per-action-type? Can the collective
  have high trust in one domain and low trust in another simultaneously?

**The leader as participant**

- The leader is not outside the collective. They are a participant with a
  specific role.
- What does the leader contribute to collective intelligence beyond decisions?
- How does the leader model the norms and culture they want the collective
  to embody?

---

## Generalisation Test

The leadership interface must describe all of these:

- A tech lead on a software team
- A coach of a sports team
- A conductor of an orchestra
- A commander of a military unit

What these have in common is the real model. What differs is domain-specific.

---

## Output Required

1. Leadership role definition — what only the leader can do, and what they must not
2. Situational awareness model — what the leader sees, how it gets to them
3. Steering protocol — valid leader inputs and how they enter the collective
4. Decision gate model — what requires leader sign-off, the hard-gated category,
   the protocol for each gate type
5. Human-AI approval architecture — interaction model (blocking vs async),
   unavailability handling, the minimal actionable approval interface
6. Delegation model — what can be delegated, when, how trust is earned, and
   whether trust is per-agent, per-domain, or per-action-type
7. The leader as participant — their contribution beyond decision-making

---

## Dependencies

Session 01 — First Principles
Session 04 — Collective Structure
Session 05 — Communication Model
