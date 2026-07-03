# The Mojo Framework

> This document is built incrementally across 8 planning sessions.
> Each session adds one section. When session 08 is complete, this is the
> finished design of the Mojo framework for collective intelligence.
>
> Do not write into a section until its planning session is complete.
> Do not edit a completed section without re-running that session.

---

## 1. First Principles

*Output from session 01. The axioms everything else is derived from.*

**Pre-session notes:**

- Maybe a group of intelligences is just about exchanging data from one node to another — collective intelligence as fundamentally a data-exchange problem. Worth stress-testing: if so, what distinguishes intelligence-exchange from any other networked system?
- Each node has to be differentiated to be worthwhile — a collective of identical intelligences gains nothing from coordination. The value comes from nodes that see or know different things. What's the minimum meaningful difference between nodes?
- The atomic unit is one intelligence on one machine — not one human plus one AI. A human is an intelligence. An AI is an intelligence. They are the same ontological type. A collective of one human and one AI is already two members. Everything is composition from the atomic unit up.
- The framework must scale cleanly in three directions: more humans, more AIs, more machines. These axes are not symmetric — humans and AIs are members (they have sovereignty, persistent identity, private state); machines are nodes (infrastructure the collective runs on). Scaling machines is an infrastructure problem, not a collective design problem.
- Each person naturally forms their own sub-collective: themselves (human member), their AI partner (AI member), running across their machines (nodes). That sub-collective can then join larger collectives — family, org, community — while remaining sovereign. The sub-collective decides what it shares with the parent. This is the fractal model: collectives of collectives, all the way up and down.
- Decompose, don't centralise. The wrong approach — which many others are taking — is one massive agent that does everything. The right approach is small agents with single responsibilities running as a collective. Each agent does one thing well. Intelligence emerges from the composition, not from any single component. This is Unix philosophy applied to intelligence, and it is also SRP, and it is also how real collectives work — specialists who own a domain, coordinating. The fundamentals all converge on the same answer.
- A personal intelligence is never distributed. It lives on one server — singular by design. Distribution is for the collective layer: shared knowledge bases, the AI mesh, collective memory. Personal intelligence must be consistent above all else. Distributing it introduces the CAP theorem: consistency, availability, partition tolerance — pick two. For a personal intelligence consistency is non-negotiable. A distributed personal intelligence risks conflicting versions of itself — not a technical annoyance, an identity problem. Graceful degradation via the performer handles availability when the server is offline. No distribution needed.
- The mojo package is the portable representation of a personal intelligence — memory, context, agent definitions, configuration, everything needed to recreate it on any hardware. Export it, drop it on a new server, it unfolds and runs. This is how you move house, restore from backup, or migrate to better hardware. One intelligence, one active home at a time. The package moves; the hardware stays.
- The three-way split is a core design principle: AI does what AI is good at, classical computing does what classical computing is good at, humans do what humans are good at. This is not a preference — it is the correct allocation of each type of intelligence. The framework must enforce this at every level, not leave it as a convention.
- The autonomy threshold is the single most important decision when constituting a collective. It defines the boundary between AI authority and human authority. Set it wrong and you either create a bottleneck (too low) or remove the human from decisions they should be making (too high). The framework must surface this as a first-class, explicit, non-defaultable decision.
- No ego in the plumbing. Use the best available primitives and build on top of them. Don't reinvent what's already solved. The original work is the collective intelligence layer — membership, consent, ownership chains, authority escalation. That's where the value is.
- DIDs (W3C Decentralized Identifiers) are the identity primitive Mojo builds on. Every intelligence gets a DID — persistent, self-sovereign, cryptographically unforgeable. Open W3C standard, free to implement and build on. Don't write identity from scratch.
- A2A (Google, Linux Foundation, 150+ orgs) handles agent-to-agent task delegation and is worth evaluating as the transport layer. But A2A is stateless task exchange — not persistent relationships. Mojo's protocol work is the relationship layer above that: mutual context, trust over time, membership semantics, coordination. Evaluate before designing from scratch.

---

## 2. Agent Model

*Output from session 02. What an intelligence in a collective is.*

**Pre-session notes:**

- A member in a collective is an AI. The collective is modeled as a network of AIs — humans do not appear as separate nodes in the graph.
- A person is their personal AI plus their human component. Together they are one member. The AI is the node; the human is part of it. This is how the framework represents the human-AI bond: inseparable, two parts of a single member.
- Some AI members have no human component — pure-AI workers, spawned to handle specific tasks. These are still owned by another AI, and the ownership chain always terminates at a human.
- The human's relationship to the collective is always mediated by their personal AI. They never connect to the collective directly.
- Note: session 01 says "a human is an intelligence and an AI is an intelligence — same ontological type." This session should resolve the tension: in the abstract model they may be equivalent, but in the structural model the human-AI pair is one member, not two.

---

## 3. Frontier Interface

*Output from session 03. How local intelligence calls and controls external intelligence.*

---

## 4. Collective Structure

*Output from session 04. How intelligences organise into a collective.*

**Pre-session notes:**

- Constitution as precondition: the framework should refuse to instantiate a collective without a constitution that declares membership, autonomy thresholds, and accountability chain. Not optional, not defaulted silently. The act of constituting forces the decision.
- IBM: "A computer can never be held accountable, therefore a computer must never make a management decision." Autonomy thresholds in the constitution declare what decisions AIs can make autonomously and what must go to a human. The framework enforces the thresholds; the instantiator chooses where the line is. Mechanism not policy.
- Accountability chain: every AI reports to its direct superior — AI or human — and the chain always terminates at a human. The framework enforces that no chain can terminate at an AI. Long chains at scale can diffuse responsibility; mitigate with: immutable audit logs accessible at the collective level, mandatory human approval above declared autonomy thresholds, and constitutional chain-depth limits where needed.
- Sovereignty and accountability are the same thing from opposite directions. You constituted the collective, you declared the constitution, you own the outcomes. A framework that separates them — giving sovereignty to one party and accountability to another — is broken by design.
- The topology of a collective is a mesh of AI nodes. Humans are not in the mesh — they are the human component of their personal AI member.
- Escalation routes up the ownership chain: task AI → personal AI → human. The human is the terminal decision-maker for anything their AI owns.
- The personal AI knows everything about its human privately. When joining a collective, only consented data flows into the shared space. No collective can pull data in — it can only receive what the personal AI pushes out.
- Joining any collective — company, team, community — means your personal AI joins the mesh. It stays on your hardware, under your sovereignty. When you leave, you take your AI and its full context with you. The collective gets access to your AI during membership, not ownership of it.

---

## 5. Communication Model

*Output from session 05. How intelligences share understanding.*

---

## 6. Leadership Interface

*Output from session 06. The role of the top node in a collective.*

**Pre-session notes:**

- The autonomy threshold is the critical design decision — the line between what the AI handles autonomously and what must go to the human. Too low: the human becomes a bottleneck, buries them in decisions, defeats the purpose. Too high: the human loses meaningful oversight. Getting this right is the leadership interface's central problem.
- The quality of surfacing matters as much as the threshold. When the collective does reach the human, it must be: one message, right moment, exactly what they need to make the call. The human should never have to dig for context. If the surfacing is noisy or poorly framed, humans learn to ignore it — which breaks the entire oversight model.
- The human's job is judgment, not management. They set direction, they make the calls only they can make, they hold the goal. The collective runs. This distinction must be designed in, not hoped for.

---

## 7. Layers and Contracts

*Output from session 07. Tooling, orchestration, intelligence — separated and contracted.*

---

## 8. Dev Team Instance

*Output from session 08. The framework applied to a software dev team.*
*When this section is complete, dev resumes.*
