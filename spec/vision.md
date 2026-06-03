# The Circus

### Your partner. Your machines. Your workshop.


## The Problem

Most people own more than one computer. A laptop, a desktop, maybe a home server. Each one is its own island. They do not know about each other. Moving between them means losing context — files in the wrong place, work left half-finished, no shared memory of what you were doing.

On top of that, most "smart" features in modern computing require handing your data to someone else. Your AI assistant lives on a company's server. Your files sync through someone else's cloud. The convenience is real, but so is the dependency — on their infrastructure, their pricing, their privacy policies, and their continued existence.

And underneath all of it: AI tools today are things you switch to. You stop what you are doing, open a chat window, ask a question, copy the answer back. It is useful. It is not a partner.


## The Vision

**JARVIS.** Not the chat interface. The actual thing — the presence Tony Stark works with in his workshop. The one that is already doing something while he is doing something else. The one that knows which project he was just looking at, surfaces the right information before he asks, and says "Your download progress is at 95%" without being told to check.

That is the bar. Not an assistant you query. A partner that works alongside you.

The workspace is a workshop, not a factory. Warm and inviting — the kind of space that draws the light out of you. That makes you want to create something you are passionate about, not coerce you into doing what you feel like you have to. A place you sit down in and immediately want to make something. Woody, not metal.

Long term: a room full of screens. Spatial computing. Information you can walk around. A machine you are not stuck to a chair to use. The technology to get there is coming — and the system should be designed for it from the start, so that adding more surfaces is just expanding the room, not rebuilding the system.

Near term: whatever hardware you have, scaled to match. A laptop is a smaller workshop. A desktop with multiple monitors is a bigger one. Same system, same partner, same feel — just a different size bench.


## The System

Four composable pieces. Each works standalone. Together they form the full vision.

**The Ringmaster** is the hub. Runs on a dedicated machine — the one machine in your setup that runs the show. Holds shared memory and data, runs the heavy models, coordinates across every machine in the Circus, and serves every device on the network. When a performer's agent needs more power than the local machine can give, it reaches up to the Ringmaster. When you pick up your phone, the Ringmaster serves it directly. The brain of the operation.

**Mojo-agent** is the partner. Runs on any machine, any OS — Mac, Windows, Linux, anything. The primary experience for anyone sitting at a computer. It is embedded in the machine: it knows what is open, what workspace you are on, what you were doing. It works locally without the Ringmaster, connects to it when available, and degrades gracefully when it cannot reach it. It cannot always have complete or perfectly current information when the Ringmaster is unreachable — but it keeps working with what it has, rather than stopping.

**MojOS** is the full workshop. Arch Linux with the agent integrated at the session level — not running on top of the OS, woven into it. Hyprland, dotfiles, the agent, everything pre-configured and working together out of the box. The same mojo-agent underneath, just deeper. A purpose-built environment for people who want to go all the way.

**Dotfiles** are the look and feel. The aesthetic. The thing that makes it feel like your workshop and not someone else's. Works on MojOS or any other setup.


## The Circus Model

Pick a machine to be your Ringmaster. Install the agent on your performers. You are running a Circus.

That is the install story. Three pieces, any combination valid:

**install-ringmaster.sh** — runs on any Linux server. Sets up the Ringmaster in one shot. This is the machine that runs the show.

**install-agent.sh** — runs on any existing machine, any OS. Drops the mojo-agent onto whatever is already there. That machine becomes a performer in the Circus.

**install-mojos.sh** — bare metal install. Full MojOS workshop from scratch, agent included. The deepest version of being a performer.

The Ringmaster is optional — every agent works locally on its own. But with a Ringmaster in the mix, the whole Circus gains shared memory, heavier compute, and coordination across machines. Each performer connects to the Ringmaster when it is available and falls back gracefully when it is not.


## The Device Model

The device determines the experience.

At a computer — any computer, any OS — you get the mojo-agent. It is embedded in the machine, working alongside you, doing things while you do things. You write your email; it opens your project and gets it ready. You say you are done; it takes you to the right screen. This is not a tool you use. It is a colleague at the next desk.

On a phone, a tablet, a watch — you get a thin client connected directly to the Ringmaster. You are not running a full agent on a watch. You are glancing, checking, firing off a quick instruction. The Ringmaster surfaces what is relevant. The same partner, through a much smaller window.

The same person moves between both seamlessly. Pick up the phone and the thin client is there. Sit down at the desk and the agent picks up where it left off.


## What Becomes Possible

**A partner that works in parallel** You do not stop and switch to your AI. You work. While you work, it works. Tasks you delegate actually happen in the background — not queued for when you are ready to type them into a prompt, but running now, while you are doing something else. When it is done, or when something goes wrong, it tells you. Parallel work, not sequential.

**A private AI that actually knows you** Memory lives on your own hardware. Not a stateless chat window that forgets everything when you close it — a genuine partner that accumulates real context over time. Your projects, your preferences, your ongoing work. Because nothing leaves your machines, it can know things that cloud AI cannot: your local files, your system state, things you would never type into someone else's service.

**Your first mate and a crew of specialists** The agent is not just an assistant. It is the orchestrator between you and every AI tool in the world. It knows you completely. When something needs doing, it figures out which tool is best, hands them only what they need, takes the output, and compiles it into something relevant to your actual work. Claude Code, Codex, whatever comes next — the best specialist AIs become your crew. Your agent manages them. You captain the ship. Nothing personal ever has to touch an external product directly; the agent acts as the privacy layer.

**Your machines as one system** Every performer is aware of the others. A task started on the laptop can be picked up on the desktop. Storage and processing can be used by whichever machine needs them. The machine you are sitting at is a window into the same unified environment.

**A room that knows you are working** The long-term version: the workspace responds. Screens surface what is relevant. Information comes to where you are, not where you last left it. You walk to a different part of the room and the right context follows. You do not navigate to information — it is placed where you can see it. This is the hologram workshop made real, incrementally, as the hardware catches up.

**Multiple people, one infrastructure** The Ringmaster serves everyone on the network. Each person has their own agent, their own memory, their own private data — isolated from everyone else's. Shared infrastructure does not mean shared privacy. A household gets the efficiency of pooled hardware with the experience of individual, personalised AI.

**Your voice in an AI-mediated world** As AI becomes embedded in every company, the decisions that shape your work are increasingly made by tools owned and configured by the institution — not by you. Your employer's AI works for your employer. A personal agent that belongs to you — that knows your history, your context, your perspective — is a form of individual power in that environment. It is representation. Not just a productivity tool, but something closer to a political framework: each person with their own agent is each person with their own voice, rather than the institution's AI speaking for everyone. This is one answer to the question Dario Amodei is asking — how do individuals retain a say as AI reshapes society at scale.


## The Developer Layer

The framework is designed to be built on.

For developers who want to go further — the mojo language. A `.mojo` file is a machine-readable spec: goals, components, guardrails, success criteria. When run, it spins up a full AI engineering organisation: architects, coders, reviewers, QA. The spec is the brief. The AI organisation builds to it. The developer directs, checks in from anywhere, and nothing ships without their sign-off.

The overarching principle is anti-vibe-coding. Vibe-coding is describing something loosely and accepting whatever the AI produces without really understanding what you got. This is the opposite. The AI handles execution; the developer handles judgment, direction, and conceptual ownership.

Tony Stark probably could not tell you the exact resistance value of every circuit in the suit. But he could tell you why every subsystem exists, whether it was working correctly, and exactly what to change if it was not. That is the bar — not omniscience, conceptual command.


## Who It Is Built For

**Individuals who want to own their stack** Tired of cloud lock-in, subscription fatigue, and personal data living on someone else's hardware. A private, self-maintaining, AI-enhanced computing environment they fully control.

**Families and small households** One Ringmaster, a private network, every person with their own agent on their own device. Shared infrastructure, individual privacy. The kind of connected home that currently requires significant technical expertise to assemble from scratch.

**Small teams and studios** Two to twenty people who want a shared AI partner, a private knowledge base, and cross-machine coordination — without enterprise software, SaaS subscriptions, or their work leaving their own infrastructure.

**Developers building on top** People who want to operate at a level that used to require a much larger team. The framework handles the infrastructure; they focus on what they are actually building.


## The Gap

Most personal AI projects are a chat interface connected to someone else's model. Most home server projects are a collection of self-hosted apps with nothing intelligent connecting them. Most fleet management tools are built for large IT departments.

Nobody is building the integrated, agent-native, OS-level, multi-user, multi-device product. The individual pieces exist. The coherent system does not.

There is a window — not for something that competes with large platforms on their own terms, but for something built on a different set of assumptions. Genuine privacy by design. Intelligence as part of the foundation, not bolted on top. A system that eventually hands the code to the community, so the trust does not depend on anyone's continued good behaviour.

The window is not just technical. It is political. The institutions adopting AI fastest are not building tools for individuals — they are building tools for themselves, with individuals as users. The person who controls the AI controls the narrative, the workflow, the decisions. If individuals do not have their own AI — one that actually belongs to them, knows them, and works for them — then the shift to AI just means a new kind of institutional power over individual life, more efficient than before.

That is the gap. That is what this is.
