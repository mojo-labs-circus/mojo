# Session 02 — Agent Model

> Defines what an intelligence in a collective is. Generic — not tied to any
> specific implementation, technology, or domain.

---

## Purpose

Design the model of a single intelligence participating in a collective. Not a task
executor or a production unit — an intelligence. What it is, what it carries, how it
relates to the collective, and how it comes and goes.

Every participant in a collective built on this framework is an agent. The model must
be general enough to describe a human, an AI, or anything else that qualifies as an
intelligence capable of collective participation.

---

## Context

From first principles (session 01) we will know what collective intelligence is and
why it forms. This session defines the unit — the individual intelligence that
participates in that collective.

In the Mojo implementation, agents are defined by `.mojo` files and run on the Mojo
runtime. But this session designs the abstract model. The `.mojo` format is one
implementation of it.

---

## Questions to Resolve

**What makes something an agent in this model?**

- What is the minimum an intelligence must be capable of to participate in a
  collective? What properties are non-negotiable?
- How is an agent's role defined — and is a role something the collective assigns
  or something the agent brings?
- What does an agent own within the collective? What is it responsible for that
  no other agent is?
- What is an agent explicitly not — where do its boundaries end?

**What does an agent carry?**

- What context does an agent hold about itself? About the collective? About the goal?
- What is the minimum an agent needs to know to contribute meaningfully?
- How does an agent's understanding of the collective stay current as the collective
  changes?

**Autonomy and authority**

- What decisions can an agent make on its own, without consulting the collective?
- What decisions require the collective's input or a higher authority?
- Who defines that boundary — and can it change as trust develops?
- What happens when an agent acts outside its authority?

**Lifecycle**

- How does an agent enter a collective? What does it need before it can contribute?
- Can agents be persistent (long-running, accumulating knowledge and context) or
  ephemeral (instantiated for a specific contribution, dissolved after)?
  Or both — and when is each appropriate?
- How does an agent leave a collective? What does it hand off? What persists after it?

**Contribution**

- An agent's value is not just task completion. What else does an agent contribute
  to a collective? (Shared knowledge, error correction, perspective, specialisation)
- How does an agent signal to the collective what it knows, what it needs, and
  what it cannot handle alone?

---

## Generalisation Test

The model must describe all of these equally well:

- A human software engineer on a dev team
- An AI agent executing a specific task
- A player on a sports team
- A musician in an ensemble

If the model breaks for any of these, it is not general enough.

---

## Output Required

1. The minimum definition of an agent — necessary and sufficient properties
2. The role model — how roles are defined and assigned
3. The context model — what an agent carries and how it stays current
4. The autonomy model — what agents decide alone vs defer, and how that boundary is set
5. The lifecycle model — entry, persistence vs ephemeral, exit, handoff
6. The contribution model — what an agent gives to the collective beyond task execution

---

## Dependencies

Session 01 — First Principles
