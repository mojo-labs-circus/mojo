# Research: AI Architecture Paradigms & Formal Agent Contracts

*Session: 2026-06-05. Feeds into sessions 07 (layers and contracts) and the formal spec language idea from `interesting.md`.*

---

## Thread 1: Where AI Architecture Is Actually Changing

### The core finding

The field isn't making models deterministic. It's architecting around probabilism — defining verifiable contracts at layer boundaries, using constrained decoding for typed communication, and restricting probabilistic action to domains where either (a) verification is possible or (b) imprecision is acceptable.

### Mojo (Modular)

**Source:** Chris Lattner, Hanselminutes EP1037 (Feb 2026); mojolang.org docs.

Lattner's framing: the two-language problem (Python for ergonomics, C++/CUDA for performance) has no principled bridge. Mojo is the bridge — single type-safe, ownership-tracked, hardware-targeting language built on MLIR. Structs are completely static and compile-time bound. Parameters (square brackets) are compile-time constants; arguments (parentheses) are runtime values. This distinction is structural, not stylistic.

**Relevance:** Mojo is infrastructure-level determinism — the reliable substrate on which probabilistic workloads run. The same logic applies up the stack: you need a deterministic layer beneath the probabilistic one, whether that's a language, a schema, or a role contract.

### Neurosymbolic AI — AlphaGeometry

**Source:** Trinh & Luong, DeepMind, *Nature* (Jan 2024).

LLM for fast intuitive construct generation + symbolic deduction engine for rigorous proof steps. Result: 25/30 IMO geometry problems. Pure symbolic: 10. Human gold medalists: 25.9. The LM generates candidates; the symbolic engine verifies. Neither works alone.

**Relevance:** This is the chef/recipe architecture proven at superhuman level. Probabilistic intelligence operating within a deterministic scaffold. The scaffold doesn't constrain creativity — it channels it toward verifiable outputs. The key insight: the LLM doesn't need to be reliable. It needs to generate correct candidates often enough for the verifier to find one.

### Vericoding

**Source:** Bursuc et al., MIT/Beneficial AI Foundation, arXiv:2509.22908 (Sep 2025).

"Vericoding" = LLM generates formally verified code from formal specs. Contrast with "vibe coding" (probabilistic generation, no verification). Benchmark: 12,504 formal specs in Lean, Verus/Rust, Dafny. Current success rates: 27% Lean, 44% Verus/Rust, 82% Dafny. Dafny improved from 68% to 96% in one year. August 2025: Seed-Prover solved 5 of 6 IMO 2025 problems.

**Relevance:** Directly instantiates the determinism bridge. The reliability model is: LLM generates (probabilistic) + verifier checks (deterministic). You don't make the LLM reliable — you make the verification reliable.

### Constrained Decoding / Typed LLM Outputs

**Source:** Geng et al., EPFL/Microsoft, arXiv:2501.10868 (Jan 2025).

JSON Schema constraint at the token-sampling level — invalid tokens are masked during generation. Key findings: constrained decoding speeds up generation by 50% (not slower as assumed), improves downstream accuracy by up to 4%. Frameworks: Guidance, Outlines, XGrammar, Llamacpp, OpenAI, Gemini. Guidance outperforms on efficiency and coverage.

**Relevance:** For collective agent systems, this is the practical answer to typed message passing between agents. The schema is the contract; the LLM's job is to produce something that satisfies it. This is how you get deterministic communication between probabilistic agents without changing what the models compute.

### RLVR and the Verifiability Boundary

**Sources:** Andrej Karpathy, karpathy.bearblog.dev (Dec 2025); Sebastian Raschka, *Ahead of AI* (Mar 2025).

RLVR (Reinforcement Learning from Verifiable Rewards): training LLMs against automatically checkable reward functions (math, code, formal proofs) causes models to develop reliable reasoning strategies in those domains. The verifiability boundary is the capability boundary — wherever you can define a verifiable reward, you can train reliable behaviour. Wherever you can't, you get probabilistic noise.

"Jagged intelligence": LLMs trained with RLVR are superhuman in verifiable domains and unreliable everywhere else. The jagged shape tracks where RLVR was applied.

**Relevance:** Verifiability is the architectural lever. The more of your system's outputs you can make verifiable (through contracts, schemas, formal specs), the more reliable you can make the agents operating in those domains — either through constrained decoding at inference or RLVR at training.

### The Paradigm Shift

**Source:** Karpathy, Dwarkesh Podcast (Oct 2025); Sun et al. multi-agent survey, arXiv:2502.14743 (Feb 2025).

Karpathy verbatim: *"LLM labs will trend to graduate the generally capable college student, but LLM apps will organize, finetune and actually animate teams of them into deployed professionals in specific verticals by supplying private data, sensors, actuators and feedback loops."*

That is the collective intelligence model. The unsolved problem named by the multi-agent survey: hybridization of hierarchical and decentralized coordination — how do you get the benefits of both a central constitution and autonomous member execution? That's session 04 and session 07.

---

## Thread 2: Formal Agent Specification — State of the Field

### What exists

**Google A2A Agent Card** (open-sourced April 2025, Linux Foundation, 50+ enterprise partners)
A JSON document at `/.well-known/agent.json`. Declares identity, natural language skill descriptions, supported modalities, auth requirements, endpoint URL. Solves discovery — a client can find a remote agent and understand what it does before delegating. Gap: natural language skills, no typed I/O schema, no composition rules, no capability restrictions. The spec says agents must remain "opaque." A business card, not a type signature.

**Anthropic MCP** (November 2023, ~97M downloads, adopted by OpenAI/Google/Microsoft)
Types tool invocations: JSON Schema for inputs and outputs, plus a natural language description. Gap: MCP types what tools accept and return, not what agents are or what they're constrained to do. No way to express agent role, composition rules, or capability boundaries. At the tool layer, not the agent layer.

**Oracle Open Agent Spec** (arXiv:2510.04173, October 2025)
Declarative, framework-agnostic config for agents and workflows. Portability across LangGraph, CrewAI, AutoGen (analogy: ONNX for ML models). Covers agent components, flow graphs, conditional routing, I/O routing, composition, nesting. This is the closest existing thing. Gap: it's a workflow portability format, not a contract. The agent body is still natural language. No type system, no inheritance at the role level, no mechanism for a second collective to trust the spec as verified. Solves "run my agent in a different framework" — not "I can trust what this agent claims to do."

**Session Types (NUS/Griffith/Peking, arXiv:2412.06512, ICML 2025 position paper)**
Type-theoretic approach from PL theory: describes the sequence and types of messages over a communication channel; can statically prove that well-typed participants cannot deadlock. Paper argues for integrating LLMs with formal methods (model checking, temporal logic, process algebras) for trustworthy agents. Gap: applies formal methods to the communication protocol layer, working upward. Nobody has built a type system for agent instructions working downward from the role definition. Currently "we should do this," not "here is the language."

**LangGraph / CrewAI / AutoGen / Letta**
All define agents in Python or YAML with natural language role descriptions. CrewAI is the most structured (name, goal, backstory, tools) — still entirely English. No formal layer exists in any of them.

### The gap

The formal agent specification language — a machine-verifiable layer combining role contracts, typed I/O schemas, capability declarations, and composition rules (inheritance, mixins) with a natural language judgment body — does not exist as a coherent, deployed artifact.

The individual pieces exist in isolation: typed schemas (MCP), capability models (ACM capability security), formal methods (session types), portability formats (Oracle Agent Spec). Nobody has assembled them into a coherent spec language for agent intelligence.

**The open territory:**

1. A type system for agent instructions that rejects ambiguity before execution
2. Composition rules that let collectives safely delegate to each other's members with known contracts
3. Treating the spec as a portable contract rather than a runtime config — a deliverable, not just an artifact

---

## What This Means for the Framework

The framework is architecturally aligned with where the research frontier is pointing. But it has an opportunity to go further than the field has gone.

The field has determinism at the infrastructure layer (Mojo), at the protocol layer (MCP, constrained decoding), and at the verification layer (formal provers, RLVR). What nobody has done is bring that determinism to the **agent definition layer** — the place where you declare what a member of a collective is, what it does, and what it promises to produce.

Session 07 (layers and contracts) is the right place to design this. The formal spec language is how you push the deterministic layer further up the stack without losing the probabilistic intelligence that makes the natural language body valuable.
