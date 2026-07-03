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

## Protocol Landscape

Last updated: 2026-06-15. The space moved fast — first pass missed most of this.

### What exists

**Google A2A (Agent-to-Agent)** — Released April 2025, now Linux Foundation stewardship. The most mature agent-to-agent protocol. Uses HTTP, SSE, and JSON-RPC 2.0. Agents advertise capabilities via "Agent Cards" (JSON business cards), exchange structured Task objects, discover each other via well-known URIs or registries. 150+ production orgs (Microsoft, AWS, Salesforce, SAP, IBM). 22k GitHub stars. SDKs in 5 languages. Complementary to MCP — MCP is agent-to-tool, A2A is agent-to-agent task delegation. **Not a collective intelligence model** — no membership semantics, no ownership chains, no consent framework, no humans in the mesh.

**ANP (Agent Network Protocol)** — Open source community project positioning itself as "the HTTP of the agentic internet." DID-based identity, dynamic protocol negotiation between agents, layered architecture (identity → meta-protocol → application). Technical white paper on arXiv (2508.00007). More ambitious than A2A in scope but less mature and adopted.

**IBM ACP (Agent Communication Protocol)** — REST-native, minimal, JSON-RPC over HTTP/WebSockets. Launched March 2025. Merged into A2A under Linux Foundation governance September 2025. No longer separate.

**MCP (Anthropic)** — Agent-to-tool protocol. Orthogonal to what Mojo needs — not peer communication.

**FIPA ACL / KQML (1990s)** — Academic, never production-adopted. Message semantics only, no identity or governance.

### Identity layer — converging on DIDs

Multiple protocols are independently landing on W3C Decentralized Identifiers (DIDs) as the identity primitive:

**Aegis Protocol** (Aug 2025) — Non-spoofable identity via DIDs on Bitcoin's Identity Overlay Network, NIST post-quantum cryptography, zero-knowledge proofs for policy compliance. 0% attack success rate across 20k trials. arXiv 2508.19267.

**DIAP** (Nov 2025) — IPFS/IPNS-bound DIDs, ZK proofs for stateless ownership verification. arXiv 2511.11619.

**IATP** (Microsoft) — Cryptographic trust handshakes using Ed25519 DIDs, <200ms validation overhead. 82.4% of models vulnerable without this kind of protection.

**IETF draft** (Oct 2025) — Agent networks framework using W3C DID/VC as foundation. Single trust domain only. Expires April 2026.

DIDs are becoming the standard identity substrate. Worth building on rather than reinventing.

### Governance and orchestration

**Microsoft Agent Governance Toolkit** (Apr 2026) — Seven-package open source system covering OWASP Agentic Top 10. Policy engine, secure agent-to-agent channels, sandboxing, compliance verification. Policy enforcement only — not collective decision-making or membership governance.

**Cloudflare Mesh** (Apr 2026) — Private networking for AI agents across multicloud. Infrastructure layer, not a collective intelligence model.

**Coral Protocol** (Sep 2025) — "Internet of Agents" — decentralised communication, coordination, trust, and payments. Built on MCP. Solana partnership. Worth watching as the closest to a collective infrastructure play, but no membership model, no human-AI pair semantics.

### Standards bodies

**W3C AI Agent Protocol Community Group** — First meeting June 2025. Early stage, specs expected 2026-2027.

**Agentic AI Foundation (Linux Foundation)** — Launched Dec 2025. Founding members: Anthropic, OpenAI, Block. Stewarding MCP, AGENTS.md, goose, Agent Skills. 170+ orgs. The emerging home for agent standards.

**NIST** (Feb 2026) — AI agent identity and authorization framework. Building on OAuth, OpenID Connect, SCIM for non-human identity management.

**OWASP Agentic Top 10** (Dec 2025) — First formal taxonomy of risks specific to autonomous agents. Goal hijacking, tool misuse, identity abuse, memory poisoning, cascading failures. Useful threat model for Mojo's security design.

### The gap — still real

None of the above cover:

- **Human-AI pairs as a persistent unit** — identity protocols handle agents, not human+AI partnerships with shared persistent identity
- **Collective membership semantics** — how an intelligence joins, participates in, and leaves a collective; what membership means
- **Consent-based data sharing between collectives** — payment protocols handle transactions, not granular revocable consent for data and context
- **Authority escalation as a first-class primitive** — delegation research exists (HDP, arXiv 2604.04522) but no protocol for how authority requests propagate and who holds final veto
- **Ownership chains with meaningful semantics** — blockchain gives audit trails, not ownership inheritance and accountability chains

### Strategic question

A2A and ANP handle transport and task delegation well. DIDs are converging as the identity substrate. Rather than rebuilding these layers, Mojo's protocol work may be better positioned as the **collective intelligence layer on top of** existing infrastructure — using A2A for agent-to-agent transport, DIDs for identity, and adding the layer nobody has: membership, consent, ownership chains, authority escalation. This is worth resolving before the protocol design session.

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
See `academic-field.md` for the full intellectual landscape — the Hybrid Intelligence
field has been converging on this problem since 2019 without building a system.

**"The Accountability Horizon"** (arXiv 2604.07778, Tibebu & Shemtaga, Apr 2026) — Proves an impossibility theorem: once a human-AI collective's compound autonomy (executive × epistemic) exceeds `1 - 1/|cycle_size|`, no framework can satisfy all four accountability axioms simultaneously. Key implications for Mojo:

- The CTO model (AI proposes, humans approve before execution) keeps executive autonomy near zero, staying below the threshold by design
- Giving frontier models limited context (only what they need) formally reduces epistemic autonomy — this is architecturally correct, not just a privacy choice
- The constitutional model operates in a different problem space (constituting identity, not assigning post-outcome responsibility) — the impossibility doesn't directly threaten it
- If the framework is ever applied to formal domains (healthcare, finance), the interaction graph topology needs careful design to avoid small mixed feedback cycles
- See `spec/framework/constitution.md` for the implications on how the constitution should handle accountability

**"Generative Collective Intelligence"** (arXiv 2505.19167, 2025) — AI as cognitive bridge for human groups. Still positions AI as tool amplifying human collectives, not peer member. Confirms the peer-model framing is novel.

**Dellermann et al. — "Hybrid Intelligence" (2019)** — Foundational definition of the field. 1233 citations. Everything in the academic space references this. Establishes that human+AI combined in ongoing systems outperform either alone. Does not attempt a formal collective model or governance structure.

**Beckers & Teubner — "Three Liability Regimes for Artificial Intelligence" (2023)** — Legal theory introducing the "Hybrids" category: human-algorithm associations that act as quasi-organisations and should be treated as collective entities for accountability. The ontological claim is identical to Mojo's — the collective is a kind of entity. Most important prior art in the academic space. No operational system.

**Eccles — "Hybrid Intelligence Teams (HITs)" (2025)** — Oxford Said Business School. "Persistent groups composed of multiple humans and multiple AI agents working interdependently." Names hybrid-specific failure modes (authority confusion, AI consensus illusions, cross-agent drift) that don't appear in human-only or AI-only team theory. Closest academic formulation to Mojo's collective model. Management science only — no system design.

**Gupta et al. — "COHUMAIN" (2025)** — Topics in Cognitive Science. 118 citations. Proposes COllective HUman-MAchine INtelligence as a framework. Rapidly becoming a reference. AI still positioned as tool amplifying human collectives, not peer member.

---

## Content Businesses / Workflow Tools

Not infrastructure — these are courses or SaaS tools built on top of existing cloud AI.

**Cortex OS** (agentic.james, Instagram) — Multi-agent Claude Code orchestration, agent-to-agent comms, scheduled tasks, phone-accessible. Packaged as a $97/month Skool course. Fully cloud-dependent, no private compute, no OS layer, no hardware coordination. Mid-tier overlap on orchestration. Not a competitive threat to the core Mojo vision.

---

## The Genuine Differentiators

In priority order — what Mojo has that nobody else does:

1. **Human + AI as the same ontological type** — peers in the collective, not master/servant. No framework does this. Every existing system has humans outside the collective as supervisors. The academic field (Hybrid Intelligence, HITs, Beckers/Teubner) has been arguing this should exist since 2019. Nobody has built it.

2. **Constitutional collective intelligence framework** — every collective formally declares itself before operating. AgentCity has something constitutional but it's imposed, AI-only, and blockchain-dependent. The constructive answer to the impossibility-theorem problem nobody has built.

3. **The mojo language** — declarative spec language for AI engineering organisations. Fractal subsidiary model (Architect decides which components warrant their own company). CTO-in-the-loop. Write-once-run-anywhere via hardware-aware model resolution. MetaGPT is the closest but is all-AI, procedural, and not hardware-aware.

4. **Household multi-user model** — per-person private memory on shared infrastructure, mesh across multiple machines, graceful degradation. Nobody else combines these. Qira gets the UX right but is cloud-connected and single-user.

---

## What to Watch

- **OpenClaw** — Moving fast, genuinely agentic, self-hosted. If they add a household model and multi-device sync, the gap narrows on Circus specifically
- **Qira** — If Lenovo adds a local mode and multi-user isolation, they have the UX and the distribution
- **mahilo** — Small project but philosophically the closest to the peer model. Worth watching if it gains traction
- The open-source stack (OpenClaw + Ollama + a memory layer) could approximate Circus functionally within 18-24 months without the framework coherence. The framework is the moat.
