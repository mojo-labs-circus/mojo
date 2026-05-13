# MojOS

### Your computers. Your data. Your AI.

---

## Fragmented, Dependent, Watched

Most people own more than one computer. A laptop, a desktop, maybe a home server or an old machine repurposed for storage. Each one is its own island. They do not know about each other. Moving between them means losing context — files in the wrong place, work left half-finished, no shared memory of what you were doing. If something breaks, you find out when it is already a problem.

On top of that, most "smart" features in modern computing require handing your data to someone else. Your AI assistant lives on a company's server. Your files sync through someone else's cloud. Your smart home runs through a third-party platform. The convenience is real, but so is the dependency — on their infrastructure, their pricing, their privacy policies, and their continued existence.

This project is an attempt to solve both problems at once.


## One System. Every Machine. Yours.

A framework for turning a group of computers into a coordinated, intelligent system. The machines form a private network — owned and controlled by the people using it, not dependent on any external service. One machine acts as the central brain: it holds shared memory, coordinates the others, and runs the AI. The rest connect to it and can think for themselves when they need to.

The system is built for multiple people. Each person on the network gets their own AI assistant — same underlying infrastructure, completely separate identity, memory, and private data. A family shares one central machine but each member's assistant knows only what their assistant should know. A small team shares compute and tooling but each person's context stays theirs. Shared where it helps, private where it matters.

The AI runs across the whole system. It is the same assistant on every device — same personality, same memory, same ongoing context. It knows which computer you are sitting at, what you were doing there, and what is happening across all your machines. When you move from one device to another, it moves with you.

This is not a research tool and it is not trying to solve frontier AI problems. The focus is narrower and, in practice, more useful to most people: coordination and quality of life. Organising information, keeping machines healthy, picking up where you left off. The ten small things that currently fall through the cracks. Conventional tools — job schedulers, file sync, remote access — do what they are good at. The AI handles what those tools cannot.


## What Becomes Possible

**Your first mate — and a crew of the world's best specialists**
The personal agent is not just an assistant. It is the orchestrator that sits between you and every AI tool you use. It knows you completely — your projects, your context, your preferences, all on your own hardware. When something needs doing, it figures out which tool is best for that task, hands them only what they need, takes the output back, and compiles it into something relevant to you. Claude Code, Codex, Cursor, whatever comes next — the best specialist AIs in the world become your crew. Your first mate manages them. You captain the ship. Nothing personal ever has to touch a cloud product; the first mate acts as the privacy layer, giving the scallywags the bare minimum to do the job and nothing more. You get enterprise-grade capability without enterprise-grade exposure.

**A private AI that actually knows you**
Because the AI's memory lives on your own hardware, it accumulates real context over time. Not a stateless chat window that forgets everything the moment you close it — a genuine assistant that remembers your projects, your preferences, your ongoing work. Because nothing leaves your machines, it can know things that cloud AI cannot: your local files, your system state, notes you would never type into someone else's service.

**Your computers working as one**
Every machine in the group is aware of the others. Storage, processing power, and specialist hardware can be used by whichever machine needs them. A task started on your laptop can be picked up on your desktop. The machine you are sitting at becomes a window into the same unified environment, not a separate island.

**Multiple people, one infrastructure**
The central machine serves everyone on the network. Each person has their own assistant, their own memory, and their own private data — isolated from everyone else's. Shared infrastructure does not mean shared privacy. A household or small team gets the efficiency of pooled hardware with the experience of individual, personalised AI.

**Infrastructure that runs itself**
Updates roll out across all machines from one place. The system converges everything to a known good state. When something breaks, it is surfaced immediately with context — not discovered days later. Managing a group of computers no longer requires logging into each one individually.

**A platform others can build on**
The framework is designed to be forked. A company can take it, put their own name on it, customise it for their users, and ship it as their own product — without rebuilding the underlying infrastructure. The framework handles the hard parts; the product layer sits on top.


## The Developer Layer

Technology does not stop. Every generation of tools becomes the foundation the next generation builds on. The internet was a foundation. Open source was a foundation. This is a foundation — and someone has to build what comes next on top of it.

This section is for those people.

MojOS gives everyone a personal agent. But for the developers who want to push the river forward — who look at a new foundation and immediately start thinking about what can be built on top of it — the circus offers something more. A way to operate at a level that used to require a much larger team. A way to build serious software without losing the understanding and ownership that make it genuinely yours.

The mechanism is the mojo language. A `.mojo` file is a machine-readable spec — goals, components, guardrails, success criteria — that when run, spins up a full AI engineering company: architects, coders, reviewers, QA, an ethics team. The spec is the brief. The company builds to it. The developer directs everything, checks in from anywhere, and nothing ships without their sign-off. The same file runs on any machine in the circus, scaling to whatever compute is available.

The overarching principle is anti-vibe-coding. Vibe-coding is describing something loosely and accepting whatever the AI produces without really understanding what you got. This is the opposite. The AI handles execution; the developer handles judgment, direction, and conceptual ownership. The goal is to operate like **Boris Cherny** with a full engineering org behind you — someone who could walk into any part of the system, understand what is happening and why, and have a strong opinion about whether it is right. Not omniscience. Conceptual command.

**Tony Stark** probably could not tell you the exact resistance value of every circuit in the Iron Man suit. But he could tell you why every subsystem exists, whether it was working correctly, and exactly what to change if it was not. That is the bar.

The river keeps flowing. This is for the people who make sure it does.


## Who It Is Built For

**Individuals who want to own their stack**
People who are tired of cloud lock-in, subscription fatigue, and their personal data living on someone else's hardware. A private, self-maintaining, AI-enhanced computing environment they fully control.

**Families and small households**
One central machine, a private network, every person with their own connected device and their own assistant. Shared infrastructure, individual privacy. The kind of connected home that currently requires significant technical expertise to set up from scratch.

**Small teams and studios**
Two to twenty people who want a shared AI assistant, a private knowledge base, and cross-machine coordination — without enterprise software, SaaS subscriptions, or their work leaving their own infrastructure.

**Companies building on top**
Businesses that want to build a branded, private AI product for their customers. The framework handles the infrastructure; the product is theirs.


## The Trust Is Broken. The Gap Is Real.

Something has shifted. People are not just abstractly concerned about privacy — they are genuinely reconsidering their relationship with the large platforms that have mediated their digital lives for the past two decades. The sequence of events is long: data sold without consent, services that changed their terms mid-relationship, products discontinued after acquisition, advertising systems that revealed just how much was being collected and for what purpose.

AI has made this more acute. People are now being asked to hand over their most personal interactions — their questions, their plans, their half-formed thoughts — to the same companies that have already demonstrated they will use that data in ways users did not intend. The promise of convenience is wearing thin.

At the same time, the technology to build an alternative has matured. Tools for running AI models on personal hardware are now accessible. The infrastructure is ready. And yet a search of the landscape in 2026 turns up a telling gap: the individual component pieces exist, but the integrated product does not. A piece published earlier this year asks directly, *"Who is building the agent-native operating system?"* — and finds no clear answer. The closest projects are either academic, or manual assemblies of separate tools with no shared identity, no multi-user support, and no OS-level design. The component-level space is crowded. The coherent, integrated product is not built yet.

There is a window here — not for a product that competes with the large platforms on their own terms, but for something built on a different set of assumptions. One that ships with genuine privacy by design rather than as a marketing claim, and eventually hands the code to the community as open source — so that the trust does not depend on the company's continued good behaviour.


## What Nobody Else Is Building

Most personal AI projects are a chat interface connected to someone else's model. Most home server projects are a collection of self-hosted apps without anything intelligent connecting them. Most fleet management tools are built for large IT departments, not a household or a small team.

This is an operating system level design. The AI and the infrastructure are built together from the start — they share the same identity, update together, and understand each other. The result feels coherent rather than assembled: not an AI app installed on top of Linux, but a platform where intelligence is part of the foundation.

Nobody is building the integrated, OS-level, fleet-native, multi-user product. That is the gap. That is what this is.
