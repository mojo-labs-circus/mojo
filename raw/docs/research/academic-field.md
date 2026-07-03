# The Academic Field — Collective Intelligence and Human-AI Systems

> Context for what Mojo is doing and why. Three research communities have been
> independently converging on the same problem since 2019. None of them built a system.
> This document tracks that intellectual lineage.

---

## The Chain of Reasoning

Since 2019, researchers across cognitive science, organisational theory, and legal theory
have been independently arriving at the same conclusion: groups of humans and AI systems
behave like collectives, and existing frameworks — built for either human teams or AI
systems — fail to model what's actually happening.

The academic consensus is forming. The engineering response does not yet exist.

Mojo is that response.

---

## The Field and Its Names

The recognised term is **Hybrid Intelligence** — coined formally by Dellermann et al. in
2019, now with over 1200 citations. It describes systems where humans and AI are combined
in ongoing, interdependent work that produces outcomes neither could achieve alone.

Sub-terms still forming:

- **Hybrid Intelligence Teams (HITs)** — proposed by Eccles (Oxford, 2025) for persistent
  groups of multiple humans and multiple AI agents working interdependently
- **COHUMAIN** (COllective HUman-MAchine INtelligence) — proposed 2025 as a cognitive
  science framework, 118 citations already
- **Digital collective actors / Hybrids** — legal theory term from Beckers and Teubner
  (2023) for human-algorithm associations that should be treated as quasi-organisations

No single term has won yet. The field is coalescing.

---

## What the Research Has Established

**The collective is the right unit of analysis.** Not individual humans, not individual
AI systems, but the group. Intelligence emerges from the interaction, not from any
member. This is the consistent finding across all three research traditions.

**Existing frameworks are insufficient.** Human team theory, human-automation theory,
and multi-agent AI orchestration all fail to model what actually happens in persistent
human-AI groups. Eccles (2025) names specific failure modes that emerge only at the
collective level: authority confusion, AI consensus illusions, information cascades,
cross-agent drift.

**The hybrid should be treated as an entity.** Beckers and Teubner (2023) argue from
legal theory that when humans and algorithms act together as a hybrid network, the
hybrid itself — not the individual participants — is the right unit of accountability.
This is the same ontological claim Mojo is making: the collective is a kind of entity.

**The peer model is novel.** Every existing system positions AI as a tool being operated
by humans, or AI coordinating with other AI. Treating humans and AI as instances of the
same type — peers in a collective, not master and servant — does not appear in the
literature as a built system.

---

## Key Papers

**Dellermann, Ebel, Söllner et al. — "Hybrid Intelligence" (2019)**
Business & Information Systems Engineering. 1233 citations. The foundational definition.
Hybrid Intelligence = human+AI combined in ongoing systems that outperform either alone.
Establishes the vocabulary everything else builds on. Limitation: framed entirely around
performance outcomes, not around modelling the collective as a formal entity.

**Peeters, Van Diggelen, Van Den Bosch — "Hybrid Collective Intelligence in a Human-AI
Society" (2021)**
AI & Society. 316 citations. Argues for a hybrid collective intelligence perspective that
rejects both "AI replaces humans" and "humans are always in control." Humans and AI form
sociotechnical collectives where intelligence emerges from interaction. Limitation:
society-level analysis, no formal entity model, no governance.

**Beckers and Teubner — "Three Liability Regimes for Artificial Intelligence" (2023)**
Introduces the Hybrids category: human-algorithm associations that act as quasi-
organisations and should be treated as collective entities for purposes of accountability.
The three categories are Algorithmic Actants (AI alone), Hybrids (human-AI collectives),
and Crowds (distributed humans). **Most important prior art for Mojo** — the ontological
claim is identical. Limitation: legal/regulatory framing only, no operational model.

**Gupta, Nguyen, Gonzalez — "Fostering Collective Intelligence in Human-AI
Collaboration: Laying the Groundwork for COHUMAIN" (2025)**
Topics in Cognitive Science. 118 citations. Proposes COHUMAIN as a framework for
human-machine collective intelligence. Fast uptake — becoming a reference framework.
Limitation: cognitive science framing, AI still positioned as tool amplifying humans
rather than peer member.

**Eccles — "Hybrid Intelligence Teams (HITs)" (2025)**
Oxford Said Business School. The closest academic formulation to Mojo's collective model.
"Persistent groups composed of multiple humans and multiple AI agents working
interdependently to accomplish knowledge-intensive tasks." Names hybrid-specific failure
modes absent from both human team theory and multi-agent AI. Limitation: management
science theory, not a software system design.

---

## The Gap

What does not exist anywhere in the literature or the commercial landscape:

- A software system that models a human+AI collective as a first-class persistent entity
  with formal membership, defined roles, constitution-level governance, and accountability
  structures
- Any framework that gives AI members standing within a governance system, not just
  as tools being governed
- A formal specification of what a collective IS that any implementation must satisfy
- Self-hosted, sovereignty-focused implementation of any of the above

The academics are arguing the collective should be modelled as an entity. The legal
theorists are arguing the collective should be accountable as an entity. Nobody has
built the system in which it actually is one.

---

## Why Now

The gap exists for structural reasons, not because the problem is unsolvable:

- Academic work describes problems; it does not build production systems
- Commercial companies building in this space (Kimi et al.) are building product features,
  not formal frameworks
- Local AI capable of powering this use case only became viable in 2024-2025
- The engineering community has not been reading this literature

The infrastructure timing is right. The intellectual foundation is laid. The field is
converging fast — the terminology is being coined in 2025, not 2019. The window for
building the reference implementation before the space crowds is open, not closed.

---

## What This Means for the Framework Sessions

Session 01 should engage with this literature directly. The first principles session is
asking why intelligence collaborates and what emerges from it — these papers have partial
answers grounded in evidence. Specifically:

- Peeters et al. and the Hybrid Intelligence framing for why the collective is the
  right unit of analysis
- Eccles on hybrid-specific failure modes — what goes wrong in human-AI groups that
  doesn't go wrong in human-only or AI-only groups
- Beckers/Teubner on the ontological claim — the collective should be an entity

The framework sessions are designing the standard for a class of systems this literature
has been calling for. That context should be explicit in the output.
