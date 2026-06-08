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
- Minimal context is a design goal, not a default. An agent should receive only
  what its specific task requires — nothing more. Excess context carries costs
  across multiple dimensions — all of them real, not theoretical:
  - **Performance** — agents produce lower-quality output when attention is diluted
    across irrelevant information
  - **Security surface** — larger blast radius if the agent is compromised or
    behaves unexpectedly; more data exposed to an untrusted execution context
  - **Prompt injection** — more context means more opportunity for adversarial
    content embedded within it to hijack the agent's behaviour; a distinct risk
    from general data exposure
  - **Accountability** — broad context makes it hard to isolate why an agent
    produced a given output; fine-grained context makes failures traceable
  - **Reproducibility** — narrow context produces more deterministic outputs;
    broad context introduces variance as the agent attends to different parts
    across runs, making testing and auditing unreliable
  - **Context window exhaustion** — there is a hard ceiling; context spent on
    irrelevant information directly crowds out space for the actual task
  - **Scope creep** — an agent that can see problems outside its task will
    sometimes try to solve them; tunnel vision is a feature, not a limitation
  - **Error propagation** — hallucinations and mistakes in broad context infect
    outputs; when that output becomes another agent's context, errors amplify
  - **Cost and latency** — more tokens, slower processing, higher cost; compounds
    across a collective making many parallel frontier model calls
  How is minimal context enforced? Who decides what a given task requires?

**Autonomy and authority**

- What decisions can an agent make on its own, without consulting the collective?
- What decisions require the collective's input or a higher authority?
- Who defines that boundary — and can it change as trust develops?
- What happens when an agent acts outside its authority?
- For AI agents specifically: how is human authority represented in the model?
  Is a human override the same as any other authority escalation, or is it a
  distinct category? What actions require human sign-off regardless of the
  collective's internal trust structure?

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

**Capability profile**

- Capability tier is a first-class property of an agent's definition — decided
  proactively when the agent is designed, not defaulted at runtime.
- What determines an agent's required capability level? Role complexity? Stakes of
  failure? Breadth of judgment required? Frequency of edge cases?
- How do you match capability to responsibility without over-provisioning? A
  spec-maintenance agent running a frontier model is waste. A security review agent
  running a small model is a liability.
- For AI agents specifically: which model tier is appropriate, and what is the
  reasoning? This must be documented per-agent, not left implicit.
- What happens when an agent's assigned capability is insufficient for an unexpected
  task — escalation, refusal, or substitution?

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
7. The capability model — how an agent's required capability tier is determined and
   declared, including concrete model selection for AI agents and the reasoning behind it

---

## Dependencies

Session 01 — First Principles
