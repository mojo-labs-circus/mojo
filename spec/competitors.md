# Competitors & Landscape

Last updated: 2026-06-04. Keep this current as the space moves fast.

The purpose of this file is twofold: track what others are building so we don't miss something important, and stay honest about what makes Mojo genuinely different vs. what just sounds different.

---

## What Mojo Is Competing On

Three distinct things, which is unusual — most projects only do one:

1. **A general framework for collective intelligence** — constituting and running any group of intelligences (human + AI) toward a shared goal
2. **A self-hosted, privacy-first personal/household AI system** (Circus)
3. **A declarative language for running AI engineering organisations** (mojo language)

No competitor is doing all three. Several are doing one. Knowing which quadrant a competitor is in determines whether they're a threat, a reference point, or irrelevant.

---

## Multi-Agent Frameworks

These build AI-to-AI coordination. None treat humans as members of the collective.

**AutoGen** (Microsoft) — Multi-agent conversations where agents hand off to each other. Humans are outside the loop except as task-givers. Fully cloud/API dependent. No collective identity, no constitution, no household model.

**CrewAI** — Role-based AI agent teams. Fixed pipelines, not composable collectives. Humans are supervisors. No persistent memory or cross-device model.

**LangGraph** — Graph-based agent orchestration. Infrastructure-level, not a framework for constituting collectives. Human nodes exist but as controllers, not members.

**MetaGPT** — Simulates a software company with AI-only roles (PM, engineer, etc.). Closest to the mojo language concept. Key differences: all-AI, procedural not declarative, no fractal subsidiary model, no CTO-in-the-loop model, no write-once-run-anywhere on local hardware.

**CAMEL** — "Society of Mind" role-playing agents. All AI. Humans only at prompt boundary.

**mahilo** (github.com/wjayesh/mahilo) — The most adjacent framework found. Multiple humans each have a dedicated AI agent, and agents share a coordination layer. But: humans are still supervisors, not members of a declared collective. No constitution, no formal collective identity, no peer ontology. Self-hostable.

**Google A2A protocol** — Peer-to-peer AI-to-AI collaboration without central oversight. Infrastructure, not a collective intelligence model. No humans in the collective.

**How Mojo differs:** Humans and AI are instances of the same "intelligence" type — peers in the collective, not controllers of it. Every collective has a constitution that formally declares what it is before it operates. The framework is general (applies to any collective), not hardcoded to a software company or fixed pipeline.

---

## Self-Hosted Personal AI

These run locally. None have a household model or collective intelligence framework.

**Ollama + Open WebUI** — The de facto local AI stack. Inference server + chat UI. No agents, no multi-user isolation, no cross-device context, no ambient intelligence. Infrastructure Mojo builds on top of, not a competitor to the system layer.

**Jan.ai** — Single-user desktop app, fully offline. No multi-device, no agents, no household model.

**OpenClaw** — Fastest-growing project in this space (250k GitHub stars ~60 days as of mid-2026). Heartbeat daemon, calls Claude/GPT as reasoning engines, proactively acts on your behalf. Genuinely agentic. But: single-user, no mesh across machines, no household model, no shared infrastructure, no collective framework. Worth watching — closest to Circus in spirit.

**Screenpipe** — Ambient context capture (screen + audio → local SQLite, queryable by AI). Local, private, MCP-compatible. Single-user, single machine. Solves one piece of what Circus does (context from your actual work), not the system.

**OpenJarvis** (Stanford Scaling Intelligence Lab) — Five-layer architecture for AI on personal devices, hardware-aware model selection. No multi-device sync in documented design, no household model.

**Nevo** — Self-hosted AI on a Mac Studio, 40+ specialised sub-agents. Single-machine, single-user, personal business automation focus.

**Home Assistant + Ollama** — Household AI for smart home. Multi-user in the sense of serving a household, but no per-person private memory, no ambient context from individual devices, no agent mesh. Smart home automation, not general intelligence.

**How Mojo differs:** Per-person private memory on shared infrastructure. Mesh across multiple machines (performer agents + Ringmaster hub). Graceful degradation when hub is offline. Ambient context from each person's actual work, not just smart home sensors. And underneath all of it, a collective intelligence framework that the household is an instance of.

---

## Commercial / Big Tech

These have resources but wrong assumptions.

**Lenovo Qira** (announced CES 2026) — Closest commercial UX to what Circus aims for. Ambient intelligence flowing across PC, phone, tablet, wearables. Context-aware, offline-capable, multi-device. Proactive, not just reactive. But: cloud-connected (data goes to Lenovo's servers), single-user, no agent framework, no collective intelligence model. Good proof the UX is right. Wrong architecture.

**Apple Intelligence** — On-device processing, cross-device sync via encrypted handoff, Private Cloud Compute for heavier tasks. Most privacy-serious of the big-tech offerings. But: cloud-dependent for anything non-trivial, Apple ecosystem locked, single-user, no household coordination, no user agency over the underlying model.

**How Mojo differs:** Actually local. Multi-user with genuine isolation. Not ecosystem-locked. And the system is user-owned — the infrastructure, the models, the memory.

---

## Constitutional / Governance Models

These have thought about constituting AI collectives formally.

**AgentCity** (arXiv 2604.07007) — Uses smart contracts as the literal constitution for AI collectives. Separation-of-powers structure. Closest thing to Mojo's constitutional model in the academic space. But: blockchain-based enterprise infrastructure, AI-only collective (humans are the enforcement layer, not members), top-down imposed rather than collectively declared, no household use case.

**Anthropic's Collective Constitutional AI** (2023) — ~1000 people collectively authored principles for a model's behaviour. Philosophically interesting but different problem: governing AI behaviour, not constituting a collective.

**"From Craft to Constitution" paper** (arXiv 2510.13857) — Governance-first agent engineering: agents should have declared principles before implementations. Close in spirit, but operates at individual agent level, not collective level.

**How Mojo differs:** The constitution is declared by the collective itself, not imposed by an external authority. It constitutes the collective's identity and purpose, not just rules for a single agent's behaviour. Humans are members of the collective, not external governors.

---

## Research to Watch

Not competitors but papers that directly constrain or inform the framework design.

**"The Accountability Horizon"** (arXiv 2604.07778, Tibebu & Shemtaga, Apr 2026) — Proves an impossibility theorem: once a human-AI collective's compound autonomy (executive × epistemic) exceeds `1 - 1/|cycle_size|`, no framework can satisfy all four accountability axioms simultaneously. Key implications for Mojo:

- The CTO model (AI proposes, humans approve before execution) keeps executive autonomy near zero, staying below the threshold by design
- Giving frontier models limited context (only what they need) formally reduces epistemic autonomy — this is architecturally correct, not just a privacy choice
- The constitutional model operates in a different problem space (constituting identity, not assigning post-outcome responsibility) — the impossibility doesn't directly threaten it
- If the framework is ever applied to formal domains (healthcare, finance), the interaction graph topology needs careful design to avoid small mixed feedback cycles
- See `spec/framework/constitution.md` for the implications on how the constitution should handle accountability

**"Generative Collective Intelligence"** (arXiv 2505.19167, 2025) — AI as cognitive bridge for human groups. Still positions AI as tool amplifying human collectives, not peer member. Confirms the peer-model framing is novel.

---

## The Genuine Differentiators

In priority order — what Mojo has that nobody else does:

1. **Human + AI as the same ontological type** — peers in the collective, not master/servant. No framework does this. Every existing system has humans outside the collective as supervisors.

2. **Constitutional collective intelligence framework** — every collective formally declares itself before operating. AgentCity has something constitutional but it's imposed, AI-only, and blockchain-dependent. The constructive answer to the impossibility-theorem problem nobody has built.

3. **The mojo language** — declarative spec language for AI engineering organisations. Fractal subsidiary model (Architect decides which components warrant their own company). CTO-in-the-loop. Write-once-run-anywhere via hardware-aware model resolution. MetaGPT is the closest but is all-AI, procedural, and not hardware-aware.

4. **Household multi-user model** — per-person private memory on shared infrastructure, mesh across multiple machines, graceful degradation. Nobody else combines these. Qira gets the UX right but is cloud-connected and single-user.

---

## What to Watch

- **OpenClaw** — Moving fast, genuinely agentic, self-hosted. If they add a household model and multi-device sync, the gap narrows on Circus specifically
- **Qira** — If Lenovo adds a local mode and multi-user isolation, they have the UX and the distribution
- **mahilo** — Small project but philosophically the closest to the peer model. Worth watching if it gains traction
- The open-source stack (OpenClaw + Ollama + a memory layer) could approximate Circus functionally within 18-24 months without the framework coherence. The framework is the moat.
