# Interesting — Threads Worth Continuing

## Collective Intelligence & Agents (from Claude Code workflow design, 2026-06-04)

Working out how to design agents and skills in Claude Code, the following emerged as useful framing for the Mojo framework.

### The delegation principle [→ session 04]

The question "when do you use an agent vs a skill?" maps cleanly to:
**when would a real collective delegate this to a specialist member?**

In a company you don't ask "is this task heavy enough to delegate?" — you ask "is there a member whose role covers this?" Delegation follows the role structure of the collective, not the weight of the task. A researcher exists in the collective, so research gets delegated. Simple.

This is more principled than any complexity-based heuristic.

### The two-layer contract [→ session 07]

The agent definition file (the markdown that describes what an agent does) is the **standing role contract** — it defines what this member does and what it returns. The specific invocation adds **instance-specific terms**. Two layers:

1. Role definition = what this kind of member does in general
2. Invocation = what this member is being asked to do right now

Neither party alone defines the contract. The collective sets the shape (role), the moment sets the content (task).

### Melting away the main intelligence [→ session 04, 06]

In a real collective, no single member holds the whole picture. Intelligence is distributed and shared. The challenge in any hub-and-spoke system (Claude Code, most software) is that the main context becomes the "main intelligence" — it decides, delegates, synthesises.

The reframe: treat the main context as the **coordination layer**, not the main intelligence. It doesn't think for the collective — it routes between members and surfaces outputs. The thinking happens in the members.

This changes how you design the coordinator: less "I will do X" and more "X belongs to this member, let me activate them."

### Human members [→ session 02, 04]

The human is a member of the collective too, not just a user. When execution pauses for human review (e.g. plan mode before implementation), that's not "the system waiting for input" — it's **the collective consulting the human member** at a contract boundary. The human member's role in the collective is decision-making authority, not just input.

This makes plan-mode a principled coordination mechanism rather than a safety check.

### Who defines the contract — open question [→ session 07]

Does the collective's constitution define role shapes in advance? Do members negotiate contracts dynamically? Does the coordination layer fill in instance terms?

Tentative: the constitution defines role shapes (what a researcher-member looks like, what it returns). Individual invocations fill in instance content. Neither party alone holds the contract — the collective sets the shape, the moment sets the content.

**This needs more thought.** Particularly: what happens when a member needs to sub-delegate? Who authorises that? Does it require the coordinator, or can members delegate directly to other members?

---

## Accountability and AI Decision-Making [→ session 06]

> "A computer can never be held accountable, therefore a computer must never make a management decision."
> — IBM, management training slide and manual, 1979

This is not a technical observation — it is a philosophical one about the nature
of accountability. And it maps directly onto the human-AI approval architecture
in session 06.

The argument: accountability requires the possibility of consequence. A human who
makes a bad decision can be fired, sued, shamed, imprisoned. A computer cannot.
Therefore, if a decision requires accountability (i.e. someone must answer for it
if it goes wrong), a computer must not make it alone.

What this implies for the framework:

- The hard-gated category of decisions that always require human sign-off may not
  be definable by task type, stakes, or reversibility alone — it may be definable
  by **accountability requirement**. If the decision requires someone to be
  answerable for it, a human must be in the loop.
- This gives the human-AI approval model a principled foundation rather than a
  pragmatic one. The question is not "is this task too important for AI?" but
  "who is accountable if this goes wrong — and are they in the loop?"
- The inverse is also interesting: decisions that require no accountability (fully
  reversible, low-stakes, internal to the collective) are exactly the ones safe
  to delegate fully to AI agents. Accountability as the selection criterion for
  human oversight is cleaner than any complexity-based heuristic.

Worth exploring whether this can be formalised into the decision gate model.

---

### Chefs and recipes — bridging probabilistic and deterministic [→ session 07]

Skills are recipes. Agents are chefs.

A chef doesn't improvise from scratch every time — they follow recipes. This makes outputs more predictable without removing the need for judgment. The chef provides the intelligence (which recipes to use, in what order, how to combine outputs, how to adapt to the specific context). The recipe provides the scaffold that makes execution reliable.

This is the Mojo framework's answer to a fundamental problem: **AI is probabilistic by nature, software is deterministic by requirement.** You can't make AI deterministic — that removes what makes it valuable. You can't accept pure probabilism — that makes outputs unreliable. The solution is to layer them:

- Skills (recipes) provide deterministic structure
- Agents (chefs) provide probabilistic intelligence operating on that structure
- The result is a system that is neither fully probabilistic nor fully deterministic — a principled middle ground

**Practical implication:** agents should not improvise sub-tasks that skills already cover. An agent's workflow should *be* composed of skills where they exist. The agent decides what to do and in what order; the skills handle reliable execution of each step.

### Async coordination and natural wait points [→ session 05]

Members of a collective work asynchronously and wait on each other at natural handoff points. A fry cook doesn't plate the chicken until the saucier finishes the sauce. This isn't a design problem — it's just how collectives work.

This dissolves an apparent tension in AI agent design: agents that need to pause mid-task for human review feel like they're "interrupting" a clean autonomous run. Under the collective model, there's no tension. The human is a member whose contribution is required at certain points before the next step can proceed. Waiting for that contribution is correct behaviour, not a limitation.

**Implication for agent design:** an agent's workflow can and should include explicit wait points where another member (human or AI) needs to fulfill their role before the agent continues. These are contract boundaries, not interruptions. The presentation agent waits for the human member's content review before beginning design — exactly as the fry cook waits for the sauce.

### Keep busy while waiting — concurrency within the collective [→ session 05]

A good fry cook doesn't stand idle waiting for the sauce. They start the next ticket's potatoes. Members of a well-run collective always have other work they can progress while blocked on a dependency.

**Implication for agent design:** agents should identify which sub-tasks are independent of each other and parallelise them. Where a wait point is unavoidable, the agent should look for other work it can begin in parallel — preparation it will need later, independent searches, setup work — so it stays productive.

In practice: fire independent tool calls in parallel rather than sequentially. Before blocking on a human review, start any downstream work that doesn't depend on the review outcome. The goal is to arrive at each wait point with all non-blocked work already done, so the collective's throughput is maximised.

### Time-management as a first-class concern [→ session 05]

The "stay busy while waiting" principle becomes much more powerful if you can estimate how long each wait point will take. If you know a human review will take ~5 minutes, you can select interim tasks that fit that window — not too short (wasteful context-switching), not too long (you'd still be mid-task when the dependency resolves). If you know source-finding takes ~30 seconds, you don't start something that takes 10 minutes in that gap.

This implies that a mature collective intelligence system needs:

1. **Task duration models** — learned or estimated time for each type of work (human review, web search, code generation, etc.)
2. **Dependency graphs** — which tasks are blocked on which, so the scheduler knows what's actually available to work on
3. **A scheduling layer** — given available tasks and estimated durations of active wait points, decide what to work on next to maximise throughput with minimum context-switching

This is essentially a real-time scheduling problem, and algorithms exist for it (shortest-job-first, critical path, etc.) — the interesting question is whether these can be learned/adapted rather than hardcoded, since task durations in an AI collective are highly variable and context-dependent.

**Near-term implication:** even without a full model, agents should be designed to estimate and communicate expected durations at wait points ("this human review should take a few minutes — in the meantime I'll...") so the coordination layer can make better interim decisions.

### A formal specification language for collective intelligence [→ session 07]

**The problem with English as a specification medium.**
Dijkstra argued that natural language is fundamentally unsuitable for precise specification — it's ambiguous, non-composable, unverifiable. You can't reason formally about "use source-finding as your primary tool." There's no way to lint it, test it, or guarantee consistent behaviour. Every English prompt is a one-off artefact with no quality floor.

This creates a real problem for collective intelligence systems: if recipes (skills/agent instructions) are written in English, anyone can write a bad one. Non-technicals don't produce slop because they're stupid — they produce it because the medium gives them no feedback. No type errors, no schema violations, nothing. The system silently degrades.

**The layered solution.**
Two things working together:

1. **A formal layer** for structure and contracts: member roles, tool access declarations, input/output schemas, composition (inheritance, mixins, DRY). This is where you get verifiability and quality floors. The schema rejects ambiguity before it enters the system.

2. **A natural language body** for judgment: the actual intelligence — what to do, how to adapt, how to combine. LLM flexibility earns its keep here. But it operates within the formal scaffold.

Claude Code's agent files already hint at this structure accidentally: frontmatter (name, tools, description) is the formal layer; the markdown body is the NL layer. But the boundary is ad hoc and the formal layer is too thin to enforce anything useful.

**What a Mojo spec language would formalise:** contract shapes, composition rules (real DRY — inheritance, mixins), tool access declarations, wait point definitions, output schemas. The body stays NL for judgment. The skeleton becomes type-safe.

**Specs as deliverables.**
If agent specs are formally typed, they become shareable, portable contracts. You can hand someone a specced agent and they know exactly what comes out the other side — regardless of which model or system runs it. That's an API contract, not just a config file.

This changes what "AI code" means. A formally specced agent is a deliverable with a guaranteed output shape. Two collectives sharing the same spec language can delegate to each other's members with known contracts. You're not selling "AI" — you're selling formally specified intelligence contracts that others can integrate and rely on.

This connects directly to session 07 (layers and contracts) and the probabilistic/deterministic bridge: a formal spec language is how you push determinism further up the stack without losing the intelligence that makes the NL body valuable.

---

---

## Personal Data Sovereignty and Honest Matching (2026-06-05) [→ session 01]

Current systems reward dishonesty. CVs are sales documents. Job descriptions are aspirational fiction. Both sides are performing, and the mismatch is expensive — wrong hires, imposter syndrome, people stuck in roles they sold themselves into.

If you had a real longitudinal record of how you actually work — what you're good at, where you slow down, what problems you solve, how long things actually take you — then "do I fit this role?" becomes a data question. Not a pitch. And because AI can surface patterns you haven't consciously noticed ("you consistently deliver well on greenfield work but slow down in legacy codebases"), the profile knows things about you that you wouldn't think to put on a CV.

**The Brave New World problem.** The line between "accurate personal profile" and "you are your data, permanently categorised" is thin. If the system decides what you're *for* before you've had a chance to become something unexpected, data about your past becomes a ceiling on your future. That's a caste system with better PR.

**Sovereignty is the only thing that prevents this.** If you hold the data and you decide what to share and when, it stays a tool for self-knowledge. The moment it's centralised — a government, a hiring platform, an employer — the architecture inverts and the data controls you.

**The structural problem:** the centralised version is much easier to build and much more profitable than the sovereign version. So the default trajectory is bad. Building sovereign infrastructure is swimming against the current — which is exactly why it matters.

This is one of the arguments for Mojo. Not just convenience — the alternative is someone else owning the picture of who you are.

---

## Sovereignty as a Framework Axiom (2026-06-05) [→ session 01]

Sovereignty can't be a feature or a design preference — it has to be a founding axiom that propagates through every level of the framework. If it's only stated at the top, it gets designed around accidentally as the framework gets applied.

What this means concretely:

**For each individual intelligence** (human or AI): they own their own data. Not the collective. Not the coordinator. The intelligence itself. Joining a collective does not transfer that ownership. Leaving a collective means leaving with everything that was yours.

**For each sub-collective:** the same rule applies recursively. A sub-collective owns what it produces collectively, but each member's individual data remains theirs. A sub-collective cannot extract or retain data from a member beyond what the member explicitly contributes to shared output.

**For the collective as a whole:** the collective constitution must explicitly define what data is shared, what is pooled, and what is never accessible to the collective — and this definition cannot be changed without consent from affected members. A collective has no legitimate claim to data its members didn't explicitly contribute.

**For all patterns:** any framework pattern — communication, delegation, coordination, memory, decision-making — must be evaluated against whether it creates covert data flow. If a pattern requires an intelligence to expose data it didn't choose to share, the pattern is incompatible with the framework at a constitutional level. Not inadvisable. Incompatible.

The test: at any point in the system, you should be able to ask "who owns this data?" and get a clear, unambiguous answer. If the answer is "unclear" or "the collective by default," the design is wrong.

This is not just about privacy. It's about power. Data ownership is decision-making power. A framework that doesn't explicitly protect data ownership will, over time, concentrate power at whatever level accumulates the most data — which is always the coordinator, always the platform, always the centre. The axiom exists to prevent that drift.

---

## The Framework Should Be Unix (2026-06-05) [→ session 01]

Unix gets extraordinary mileage out of one idea applied consistently at every level: everything is a file. Pipes compose the same way regardless of scale. Permissions work the same on a socket as on a directory. No special cases — because the primitive is powerful enough to handle everything if you just keep applying it.

The collective model should work the same way. A directory isn't just a container — it has its own permissions, its own ownership, its own identity. Nesting doesn't change the rules; it applies them recursively. A collective inside a collective works the same as a directory inside a directory — same contracts, same ownership model, just scoped differently.

**The key implication:** if the framework is genuinely recursive, you never need a special case for "what happens when collectives interact?" Two collectives communicating are just two intelligences (who happen to be collectives internally) talking through the standard interface. The internals of the other collective are none of your business — same as the internals of a subdirectory you don't have read access to. The boundary is respected at every level of nesting, automatically.

**Composability falls out for free.** Nesting, forking, merging, federating collectives requires no new framework machinery — the primitive scales. You don't redesign the rules when you go up a level. You apply them again.

The test for whether the framework has achieved this: you should never find yourself writing a special case for a particular scale or nesting depth. If you do, the primitive isn't powerful enough yet — go back and fix the primitive.

---

## Scaling the Model (2026-06-05) [→ session 01]

The recursive + unix-based model is structurally valid at any scale — two people to a society to a global economy. A collective of collectives is just another collective. The primitive doesn't change.

But structural validity and prescriptive usefulness are different things. At small scale (team, household, small org), the framework is fully actionable: shared goals are clear, constitutions are enforceable, accountability is traceable. As scale grows, the hard problems (contested goals, power asymmetries, defection, legitimacy) don't go away — they just become the dominant concern.

**What needs thinking through:**

- At what scale does a collective's constitution stop being enforceable, and what replaces enforcement?
- How do you handle genuinely contested goals (not just unclear goals) within a collective? Does the model assume sufficient alignment as a precondition, or does it provide a mechanism for resolving value conflict?
- Defection becomes rational at large scale in ways it isn't at small scale. Does the framework need explicit defection-resistance built in, or does sovereignty + voluntary association handle it?
- The sovereignty axiom gets harder at scale. How does inter-collective data flow work when you can't rely on direct constitutional enforcement?

The unix analogy might be the key: unix doesn't prevent misuse at scale — it just provides the primitives and enforces them locally. The rest is policy, culture, law. Maybe the framework's job is the same: provide the primitives and let higher-level mechanisms (constitutions, norms, law) handle what the primitive can't.

**Open question:** is there a scale ceiling above which the model remains descriptively correct but needs a different prescriptive layer on top? Or is the primitive genuinely sufficient if applied rigorously enough?

---

### The practical rule (for now)

Until the framework resolves the deeper questions, use this rule:

> Use an agent when a real human collective would assign this to a specialist member or team. Use a skill when the intelligence handles it in its own capacity.

Research → researcher member → agent.
Drafting → handled inline → skill.
Human decision point → human member consultation → plan mode.

---

## Knowledge Base Scope and Sub-Collectives (2026-06-05) [→ session 04, 05]

### The scope problem

A collective intelligence system needs to decide, for each piece of knowledge: is this personal (one intelligence), sub-collective, or collective-wide? This classification determines access and propagation.

Three candidate models:

- **Origin-based**: scope defaults to whoever created it, until explicitly promoted. Agent-local by default, promoted to sub-collective by consensus, further promoted to collective by broader validation.
- **Relevance-based**: the creating agent declares who the knowledge is *for* at write-time. A fact about a shared codebase is collective-scoped even if only one agent discovered it.
- **Usage-based**: starts local, scope expands as more agents query and validate it. Like cache promotion in memory hierarchies.

**Tentative preference: relevance-based with explicit declaration.** Forces the creating agent to reason about scope when context is richest. Usage-based is elegant but creates lag and ambiguity. Origin-based is too conservative by default.

### Sub-collectives as recursive primitives

The Unix framing applies directly. A process group is just a process that happens to contain processes — structurally identical to any other process, just nested. Sub-collectives should work the same way: structurally identical to a collective, just nested. This gives recursive depth for free.

Each collective maintains its own internal knowledge base and **promotes** knowledge upward when it reaches a confidence threshold, or when the parent queries it. Pull-not-push by default — the sub-collective doesn't broadcast, the parent requests.

The interface abstraction: each collective exposes only what it chooses to its parent. Internals are opaque. This is the same contract as a Unix subprocess — the parent sees stdout, not the process's memory.

Sub-collectives can be **ephemeral** (spun up for a task, dissolved when done) or **persistent** (a permanent domain team). The framework shouldn't require a distinction — the lifecycle is just a parameter on the same primitive.

### What crosses the boundary

The key design question for sub-collectives is what data crosses each boundary and in which direction. Candidate rules:

- Knowledge promoted upward must meet a confidence/validation threshold
- Downward flow is explicit task delegation (parent to sub-collective), not data broadcast
- A sub-collective cannot retain data about a member beyond what was explicitly contributed to shared output (connects to the sovereignty axiom)

### Open questions

- Who decides the confidence threshold for promotion? The sub-collective, the coordinator, or the collective constitution?
- If two sub-collectives at the same level share relevant knowledge, do they communicate directly or always route through the parent?
- What happens to a sub-collective's KB when it's dissolved? Promoted, archived, or dropped?

---

## Personal Servers as the Next Wave (2026-06-05) [→ session 03]

The Roman bath analogy: public baths were the infrastructure of their time — shared, centralised, the only option. Then wealth, materials, and plumbing technology matured to the point where private bathrooms became normal. Not because public baths disappeared, but because private became the default and shared became the fallback.

The same transition needs to happen with compute. Right now your data lives in someone else's data centre under terms that can change, at a price that can change, on infrastructure you don't control. The convenience is real but so is the dependency. When your AI lives in the cloud, whoever runs the cloud owns your AI relationship with you. That's not a risk — it's just what centralised architecture produces structurally.

This is the "you'll own nothing and be happy" trajectory. Whether it's deliberate policy or just the natural outcome of economic incentives doesn't matter — the structural result is the same: centralised data equals centralised power equals dependency. Mojo's position is the sovereign countercultural one: accept the tradeoffs (higher cost, more maintenance, lower raw performance than a hyperscaler) in exchange for actual ownership. Your data stays on your hardware. Your AI runs on your machine. Frontier models are called as tools — given only what they need, never trusted with the full picture.

The cost/benefit curve is already moving. Hardware that would have been a data centre six years ago fits in a laptop now. The trend continues. As compute costs drop and local model quality rises, the tradeoffs shrink and the case for personal servers strengthens without any change to the underlying principle.

A secondary insight from this space: if you're running serious local compute anyway, recovering the waste heat is essentially free. Heata (UK startup) puts compute nodes directly on domestic hot water cylinders — waste heat heats your water, no plumbing required. The thermal problem becomes the product. Worth designing for at some point even if Mk1 doesn't implement it.

---

## Who Mojo Is For — A Design Principle (2026-06-05)

Mojo will not be for everyone immediately, and trying to make it so is a mistake.

The PC wasn't for everyone in 1977. It was for technically capable people who were motivated enough to accept real friction in exchange for something they couldn't get any other way. Mass adoption followed as the technology matured, costs dropped, and the tooling improved — not because the original designers compromised the product to reach a broader audience before the fundamentals were solid.

Mojo's current target: technically capable people motivated by autonomy. People who understand the sovereignty argument, are willing to run their own hardware, and can tolerate rough edges in exchange for something real. That is not a limitation — it is correct sequencing.

**This should drive design at every step.** When choosing between powerful/flexible and easy/constrained, default to powerful/flexible. Don't add abstraction layers for non-technical users until there's actually a non-technical user who needs them. Don't apologise for configuration complexity when the user is capable of handling it. Design for the technically capable audience now, and let the tooling mature toward broader accessibility over time — not the other way around.

As the future arrives — better hardware, cheaper compute, more mature local AI tooling — the friction drops and the audience follows naturally. You don't design for that audience today. You build something real that the early adopters can use, and the trajectory does the rest.

---

## Individual Intelligence Design (2026-06-05) [→ session 02]

The vision establishes that sovereign individual intelligence is the foundation — you can't model collective intelligence without first modelling the individual unit. But the design hasn't answered what "good enough" looks like in practice.

Questions that need a dedicated session before the individual layer can be specified:

- What capabilities does an individual intelligence (human AI or AI agent) need before it can meaningfully participate in a collective? What's the minimum bar?
- What does a personal AI need to know about its human — what context, what patterns, what persistent memory?
- How does a personal AI maintain continuity across machines? What's the sync model?
- What's the boundary between personal AI (individual layer) and collective AI (collective layer)? When does a request stay personal vs get escalated to the collective?
- The vision says personal AI is a standalone product. What does that look like without any collective membership? Is it useful on its own, or only useful as part of a collective?

**Needs a design session before the individual layer spec can be written.**

---

## Role Definition and Persistence (2026-06-05) [→ session 04]

The vision establishes that a position's role definition persists when a holder leaves — the role defines what's transferable, the holder's personal instance leaves with them. But the design hasn't answered what this looks like:

- What format does a role definition take? What's in it — responsibilities, tools, context summaries, decision history?
- What's the mechanism for transferring institutional context to a new holder? Who curates what goes in?
- How much of the previous holder's work product belongs to the role (transferable) vs their personal instance (leaves with them)? Who decides on edge cases?
- If a role accumulates AI-generated context over time (summaries, learned patterns, built workflows), does that stay with the role or the holder?

**Needs resolution as part of the org structure and constitution sessions.**

---

## Multi-Collective Trust and Data Isolation (2026-06-05) [→ session 04, 05]

Multi-collective membership is an explicit principle — humans and their personal AIs can belong to multiple collectives, as in the real world. Context isolation between unrelated collectives must be enforced at the framework level, not trusted to application code or AI judgment. Whatever infrastructure runs the collective enforces what the framework defines — this is not a deployment concern, it's a primitive.

The framework models collectives as a tree. Collectives on the same branch share a trust relationship; unrelated collectives (different branches) share no data by default. But the specifics need a dedicated session:

- What exactly makes two collectives "related" in tree terms?
- What is the trust model between branches — is there ever legitimate cross-branch data flow, and if so, under what conditions?
- How does the framework enforce boundary isolation structurally, such that any compliant implementation gets it for free?
- What happens at the root — is there a root collective, or does the tree have multiple independent roots?

**Needs a dedicated design session before the Circus architecture is finalised.**

---

## Open Source as a Standing Team Principle (2026-06-05)

Mojo's dev team should always reach for open source first when good-quality, relevant code exists. Not as a preference — as a standing principle.

MIT-licensed code can be used freely in any project: include the copyright notice, acknowledge the source, done. No legal complexity. The acknowledgements file handles it.

The practical implication: before writing something from scratch, check whether a well-maintained open source project already solves it. If it does, use it. Adapt it. Build on it. The time saved goes into the parts of Mojo that are genuinely novel.

DeerFlow 2.0 (ByteDance, MIT) is a concrete example. The sub-agent architecture — lead agent spawning isolated workers with independent context, progressive skill loading, filesystem-offloaded state — is directly relevant to how Mojo's performer agents should work. Several of those design decisions are worth taking wholesale. The ByteDance provenance requires governance decisions before any use with sensitive data, and InfoQuest search routing should be swapped out for a self-hosted alternative in any privacy-sensitive deployment.

---

## Runtime-Composed Deliverables (2026-06-07) [→ future]

What if a deliverable — a document, a presentation, an explanation — could be composed
at runtime for the specific person receiving it? Not one version sent to everyone, but
a system that understands how the recipient learns best and tailors the content
accordingly:

- Someone who thinks in analogies gets more analogies
- Someone who learns visually gets diagrams and images
- Someone who connects through music gets music-world examples
- Someone deeply technical gets the architecture; someone non-technical gets the story

The content is the same. The surface is adapted. The recipient gets the version of
the explanation that is most likely to make them actually understand — not the version
the author finds easiest to write.

This connects to the two-way probing model: just as AI should probe humans to bring
out their best thinking, a good deliverable should meet the recipient where they are
rather than demanding they adapt to the author's preferred format.

**A future Mojo capability.** Requires a model of the recipient (their learning style,
domain knowledge, preferred register) and a system that can compose from the same
underlying content into different surface forms. The collective's AI is well-placed
to do this — it knows the members' profiles and can infer what will land for a
given person.

If the recipient also uses Mojo, the model already exists — their personal AI knows
them. No profiling system needed. The sender's collective composes the deliverable;
the recipient's AI shapes the surface for them. Two Mojo intelligences coordinating.
This is the network flywheel: the more people who use it, the better every
interaction between them becomes.

---

## Two-Way Probing — How Intelligences Bring the Best Out of Each Other (2026-06-07) [→ session 02, 06]

The best AI users right now are good at prompting — they know how to frame a question,
how to push back, how to draw out what they need. That skill is rare. Most people just
ask once and take whatever comes back.

The model we should be designing toward is different: AI that doesn't just respond but
actively draws out the best thinking from the human. Not waiting for instructions —
probing, asking better questions, saying "you haven't actually answered this yet." The
way a good therapist draws out what a patient already knows but hasn't articulated.
Bidirectional. Both sides working to get the best out of the other.

This is a concrete answer to "augment not replace." You don't replace human judgment —
you help humans access their own best judgment. The output is still human, but it's
better human than they'd have produced alone.

And it generalises. This is why collectives of intelligences are more capable than
individuals: when intelligences work together, they bring the best out of each other.
The two-way probing model is the human-AI version of exactly the same dynamic that
makes a great team better than the sum of its members.

**Needs a session to design what this looks like in practice within the framework.**
What is the AI's role when a human member is thinking through a problem? How does it
know when to probe vs when to execute? What does the protocol look like?

---

## Proprietary Add-Ons to Roles [→ constitution session]

If a member joins a collective and builds a new workflow onto an AI working under
them — can they keep that proprietary while still in the collective?

Tentative framing: two cases apply.

- Extensions built using collective resources (time, compute, collective data) →
  belongs to the collective.
- Techniques/methods developed independently without collective data → belongs to
  the member.

The harder case: keeping something proprietary *while still in the collective*, not
just on exit — like a consultant with proprietary methods. The collective gets the
output, not the technique. Lean: negotiable in the role contract, not a framework
default. Hard constraint: if the workflow is entangled with collective-private data
to function, the member owns the approach but not that specific instance.

**Status:** Unresolved. Needs confirmation/pushback before the constitution session
closes this question.

---

## Deployment Safety on Constrained Hardware (2026-06-05) [→ impl]

Blue-green deployment — running two identical environments and flipping a router between them — is the standard answer to zero-downtime deploys and instant rollback. It works well in cloud environments where compute is elastic and cheap. It doesn't translate directly to personal hardware.

The Ringmaster runs on ringbaker, a home server with limited resources already doing real work. Running two full server instances simultaneously for deployment safety is wasteful. Performers can't run two agents in parallel at all — GPU and RAM are fully committed to the running instance. True blue-green isn't the right tool.

The adapted pattern that applies everywhere in Mojo is **watchdog-rollback rolling updates**: deploy new version → run health check (heartbeat + inference test) → if check fails within a window, auto-revert to previous version. The previous version stays available for rollback but isn't running in parallel consuming resources. Same safety property — automatic revert on failure — without the infrastructure cost.

The principles from blue-green worth keeping:

- Version-negotiate the Ringmaster↔Performer protocol from the start. Each Performer advertises its version on registration. The Ringmaster routes based on what each node supports. This enables mixed-version fleets during updates — critical on hardware you can't update atomically.
- Apply the expand/contract pattern to schema and API changes: add new capability → run both old and new → remove old only after all nodes have updated. Retrofitting this is painful; building it in from the start is cheap.
- Model updates (pulling a new LLM) are a separate layer from code updates. Pull the new model in the background while the old one serves → verify checksum → flip routing config → deprecate old model after a stabilisation period. Ollama supports this natively.
- Treat Performer machines going offline as a first-class case. An update rolled out while a machine is sleeping must survive the wake-up safely. The Ringmaster tracks update state per node and resumes or retries on reconnect.
