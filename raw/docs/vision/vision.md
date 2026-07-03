# Mojo

## What This Is

Mojo is a framework for collective intelligence. It provides a principled model for how groups of intelligences — human and AI — constitute themselves, coordinate, and work toward shared goals.

**The end goal in one sentence:** Your own personal intelligence running on your own hardware, following you across every device you own — and a protocol that lets any collective add you and your intelligence into their system, on your terms.

The framework is not theory. It produces actionable specifications that any collective can instantiate: a family, a dev team, a company, a research group. Once a collective is constituted under the framework, every member can harness AI within the collective's own structure, directed toward the collective's actual goals.

Any collective that already exists in the real world can remodel itself under Mojo and become significantly more capable — not by changing what it fundamentally is, but by giving every member access to AI that operates within the collective's own structure, toward its own shared goal.

**Mojo Circus** is the infrastructure layer. It connects the machines of a collective into one system — solving the problem that a collective's hardware is always fragmented across islands.

**mojo-labs** is the first collective constituted under the framework. It builds and maintains the framework itself, and by doing so, dogfoods it.

---

## The Future

Every house has a server in it. Not a novelty — infrastructure. As normal as a router: a square box, sits in a cupboard, you forget it's there. That's where your intelligence lives. Always on, always yours, never leaving your home.

The router analogy is deliberate. A router connects you to the internet — you don't think about it, it just works. The home server connects your intelligence to everything: your devices, your collectives, your life. It will probably be marketed and sold the same way. It comes with the house. It's in the listing. One bed, one bath, a server.

Your personal AI lives on that server and grows with you. It learns how you think, how you work, what you value, how you make decisions. Over years it becomes a genuine extension of you — not a tool you use but a counterpart that knows you. The longer you have it the more valuable it becomes, and it's yours in the same way your memories are yours. No company owns it. You take it everywhere.

From that server you can be part of as many collectives as you want — your family, your company, a community, a side project. Each collective gets the version of you you've consented to share. You scope what your AI does in each collective, what it reveals about you, how much authority it has. Your intelligence is the constant thread running through all of it. When you leave a collective you take it with you, intact.

This is when Mojo becomes infrastructure. Not a product people choose — just the way intelligence works.

---

## The Problem

Collectives — groups of intelligences working toward shared goals — are how humans solve hard problems. They already exist everywhere, and they already work.

But coordination is informal and lossy. Goals drift. Context is not shared. Members work in silos. And AI, despite being genuinely powerful, sits outside the collective as a set of disconnected individual tools. Each member queries their own chat window. The collective does not benefit as a collective.

The question Mojo answers: can you model a collective formally enough that AI can operate within it as a first-class participant — directed by the collective's own structure, toward its actual shared goal?

---

## The Individual

The collective is built on top of individuals. Before you can model how intelligences coordinate, you need to model what a well-formed individual intelligence looks like.

A **sovereign individual intelligence** — human or AI — owns its own data, its own context, its own methods. It is not defined by the collectives it belongs to. It exists independently and joins collectives by choice.

For humans, this means a personal AI that works for you specifically: sovereign, local, running on your own hardware, following you across machines. It knows your context and patterns. The data is yours — it does not belong to any collective you happen to be part of.

For AI members, it means a well-specified agent with a clear role, defined scope, and an accountable human owner.

The collective layer does not replace the individual layer. It builds on it. Giving every member a capable, sovereign individual intelligence is the prerequisite — not an afterthought.

What this changes for a person: work becomes stewardship. Your AI handles the ongoing operational work — the grind, the coordination, the routine. You direct it, you own the outcomes, you make the calls it cannot make alone. It surfaces to you when it hits a decision above its threshold — one well-framed message at the right moment, not a queue of notifications. You decide, it goes back to work. You are never bored — everything that reaches you is worth your attention. You are never buried — the things that don't require your judgment never reach you. You own your time back.

---

## The Model

A **collective** is any group of intelligences working toward a shared goal.

Collectives nest. A team is a collective. A company is a collective of collectives. The model is the same at every scale — composable, recursive, consistent.

A collective has **positions** — defined roles with scope, responsibilities, and accountability. Positions exist independently of who fills them. When a member leaves, the position remains. When a member joins, they take a position.

Positions are filled by **members** — human or AI. A human member brings their own judgment, values, and autonomy. An AI member is defined by its specification and role. AI members are not moral patients — they do not have independent interests to protect. What matters is that they are defined correctly and that accountability for their actions is clear.

Every AI member requires a **human owner** — a human member who is accountable for that AI's actions within the collective. Accountability always traces back to a human. A collective with one human and many AI members is valid; that one human is solely accountable for everything the collective does.

A collective must have **at least one human member**. Without a human, there is no one accountable, no one who actually cares about the goal. Accountability must bottom out somewhere real.

---

## Principles

**Shared goal.** A collective exists to achieve something. Without a declared goal it is not a collective — it is just a group. Goals come in two forms: *mission goals* — ambient, persistent, identity-defining (a family's goal is to raise their children well and support each other) — and *project goals* — discrete, time-bound, achievable. Both are valid. What matters is that the goal is declared; the act of declaration aligns the collective.

**Voluntary participation.** Membership is consensual. Human members join with their own values and limits and retain the right to leave. The collective can only demand what members have explicitly agreed to. The individual is sovereign within the collective — including over ideas and methods developed within it. The collective has a claim on work explicitly contributed and on instances entangled with collective data; it has no claim on the thinking behind them. Role contracts can negotiate terms on top of this baseline, but cannot remove it.

**Privacy.** Context, memory, and data stay within the collective. Nothing leaves without explicit consent from the relevant members.

**Multi-collective membership.** A human and their personal AI can belong to multiple collectives simultaneously — as in the real world. Each membership is governed by that collective's constitution. Context from one collective is invisible to another. This is a framework guarantee, not a convention.

**Sovereignty.** The collective governs itself — its structure, its goals, its decisions. No external authority over how it operates.

**Open source.** The Mojo protocol and runtime are open source and will remain so permanently. This is not a business model choice — it is a logical necessity. An AI that shapes how you think over a lifetime, running on closed source software, offers no sovereignty guarantee worth anything. The only way the claim "this works for you and only you" is credible is if anyone can read, audit, and verify it. Closed source Mojo is a contradiction in terms. mojo-labs captures value through hardware, setup, support, and adjacent markets — never by closing the core.

**Transparency.** Members can see what is happening and why. Actions are traceable. Decisions are legible to the collective.

**Accountability.** Every action traces to a member. AI actions trace to their human owner. The accountability chain is never broken.

**Freedom.** The framework enables. It does not constrain what a collective does beyond what is necessary to maintain accountability and protect members.

**Composability.** Collectives nest. The model is the same at every scale. Small pieces connect cleanly. One thing done well.

**Exit.** Members can leave. Departure is graceful — contributions remain, private context is not exposed, AI members leave with their owner. A position's role definition persists — it defines what the collective expects and what context is transferable to the next holder. The previous holder's personal instance of that role leaves with them. Collectives fill gaps; they do not trap members.

---

## The Architecture

Mojo is designed in three distinct layers. The distinction matters because a flaw at any layer propagates to every layer above it permanently.

**Standard** — the formal specification of what a collective is. Minimal contracts and invariants. What must be true of any Mojo-compatible system. Slow to change by design. This is what the framework sessions produce.

**Mechanism** — the abstract interface layer. Not technology choices — the minimal set of interfaces any implementation must satisfy: how members are identified, how knowledge flows between them, how collective state persists, how governance is enforced. Designed after the standard, before any concrete implementation.

**Policy** — a concrete implementation of the mechanism with specific technology choices. Mojo Circus is the first: specific networking, specific AI models, specific infrastructure. Others can build their own. The standard stays open; policy is where implementations differentiate.

This architecture is rooted in Unix design philosophy — specifically the principle of mechanism, not policy. The framework defines how; implementations decide what. The standard is the contribution. The policy proves it works. The ecosystem that builds its own policies on top is the measure of whether the standard was right.

Three principles from Unix govern every design decision in Mojo:

**Do one thing well, then compose.** Each skill, each member role, each component has one job. The power is in composition — small, clean primitives combining into systems greater than their parts. Complexity emerges from simplicity, not from complicated components.

**Minimal standard.** Only what is universal and non-negotiable belongs in the standard. A thing goes in the standard only if it genuinely cannot live anywhere else. Resist adding — every unnecessary thing in the standard is a constraint on every implementation forever.

**Layered correctness.** Each layer must be right before building on it. This is not a productivity principle — it is a correctness guarantee. A flaw in the standard propagates to every policy ever built on it, permanently. Get it right first.

Applying this model to collective intelligence — systems where the components are intelligences with agency, not deterministic programs — is new territory. There is no prior formal specification for this class of system. The framework sessions are designing one.

---

## The Circus

The Circus is the infrastructure layer that makes a collective real across hardware.

Every collective spans machines — laptops, servers, phones, desktops. Without coordination, each machine is an island. The Circus connects them into one system so the collective's intelligence is not fragmented across hardware.

One machine runs as the **Ringmaster** — the hub. It holds shared memory, runs heavy compute, and coordinates the collective's machines. Others connect as **performers** — local agents with local compute, proxying to the Ringmaster when needed. Non-Mojo machines connect as thin clients.

The Circus is not the product. It is the connective tissue that allows the framework to run.

---

## mojo-labs

mojo-labs is Clarke's collective, constituted under the Mojo framework. It starts with one human and a set of AI intelligences, and will grow to include more human members over time.

mojo-labs has multiple goals. Building and maintaining the Mojo framework is one — handled by a dedicated sub-collective within it. By building the framework within the framework, mojo-labs dogfoods it: every design decision gets tested against real use, every gap surfaces as a real problem. Like Linux being maintained by people who use Linux.
