# Session 00B — Blockchain Research

> Pre-framework session. Run this before session 01. Its output shapes how the
> entire framework is designed — collective topology, constitution implementation,
> data ownership, and the trust model all depend on what blockchain can do for us.

---

## Purpose

Understand what blockchain actually is, what it can and cannot do, and where
specifically it applies to the Mojo framework. This is a research session, not
a design session. The goal is to arrive at session 01 already knowing whether
blockchain is load-bearing infrastructure or an optional add-on — because that
changes how everything else is designed.

Do not design anything here. Find out what is real, what is proven, and what is
hype. The Web3 space has enormous noise. Cut through it.

---

## Why This Runs First

Blockchain is not just an implementation detail. If collectives can be governed
on-chain, the constitutional model changes. If collectives can operate without a
central server, the topology model changes. If data sharing with third parties
can be cryptographically controlled, the frontier interface changes. These are
framework-level decisions, not component-level ones. Getting them wrong early
is expensive.

---

## Core Questions

**What blockchain actually is**

- At its core, a blockchain is a distributed ledger with cryptographic consensus.
  What does that mean precisely — and what properties does it provide that a
  normal database cannot?
- What is a smart contract? What can it do, what can it not do? What are the
  hard limits (no external calls, deterministic execution, no secret state)?
- What is a DAO (Decentralised Autonomous Organisation)? What has worked in
  practice? What has failed, and why?

**Public vs private chains**

- Public chains (Ethereum, Solana): open, trustless, slow, costly. When is this
  worth it?
- Private/permissioned chains (Hyperledger Fabric, Cosmos appchains, a custom
  chain among the collective's own machines): fast, free, controlled. What do
  you lose vs a public chain?
- For Mojo specifically: a collective is a small group of known machines and
  people. Is a private chain among those machines sufficient — and what does it
  give us that a shared database does not?

**The DAO prior art**

The Ethereum/Web3 ecosystem has already built collective governance on-chain.
Study this seriously before designing anything:

- **Aragon** — full DAO framework; organisations as smart contracts
- **Compound Governor / Bravo** — governance with proposals, voting, timelock
- **OpenZeppelin Governor** — modular governance contracts; widely used
- **Gnosis Safe** — multi-signature collective control of assets and actions
- **MolochDAO** — minimal DAO design; intentionally simple; influential

What did these get right? What broke? What would a non-financial DAO (no tokens,
no treasury) look like — because Mojo collectives are not financial entities.

**Performance characteristics**

- What are realistic latency figures for on-chain transactions? Per chain?
- What is the throughput ceiling — transactions per second?
- For Mojo: real-time agent coordination requires sub-100ms. On-chain consensus
  cannot do this. The question is which operations need to be on-chain (governance,
  membership, decisions) and which must stay off-chain (routing, task execution,
  context passing). Where is the right split?

**Data ownership and controlled sharing with third parties**

This is one of the most important research questions in this session.

Currently, when you use a third-party AI service (Anthropic for Claude, OpenAI,
etc.) your prompts and context go to their servers. You have no cryptographic
control over what they see, store, or use. Blockchain potentially changes this.

The model to explore:

- Your data lives encrypted in a content-addressed store (e.g. IPFS or similar).
  The encryption key is yours. The data never moves.
- You deploy a smart contract that governs access to that key. The contract
  encodes exactly who can access what, for how long, under what conditions.
- When you call an external AI service, the contract grants temporary,
  scoped decryption access. The access grant is logged on-chain — immutable,
  timestamped, auditable.
- You can revoke access at any time. The service cannot retain access after
  revocation because the key is controlled by the contract, not by them.
- You have a permanent, tamper-proof record of every third party that accessed
  your data, when, and what they were granted access to.

Questions to resolve:

- Is this technically viable with current tooling? What are the gaps?
- Zero-knowledge proofs (ZKPs) allow you to prove a claim about data without
  revealing the data. Could Mojo use ZKPs to share derived insights with a
  third party (e.g. "this user has X preference") without sharing the raw context?
- What is the performance cost of this model? Is it acceptable for interactive
  AI use?
- Self-sovereign identity (SSI) and self-sovereign data are established research
  areas — find the best work here before designing from scratch. W3C DIDs
  (Decentralised Identifiers) and Verifiable Credentials are the standards.

**Collective state on-chain**

- What parts of collective state are good candidates for on-chain storage?
  (Membership, decisions, constitution, goals — these change slowly and require
  auditability)
- What parts must stay off-chain?
  (Agent context, task outputs, real-time routing state — these change fast and
  are large)
- Content-addressed storage (IPFS, Arweave) for large data; on-chain hash as
  the anchor. How does this pattern work in practice?

---

## Research Directive

**Technical foundations:**

- Ethereum whitepaper — still the clearest explanation of what a blockchain is
  and what smart contracts can do
- Solidity / smart contract development documentation — understand the limits
  (no randomness, no external calls, deterministic, public by default)
- ZKP explainers — start with accessible material (Computerphile has good videos);
  go deeper into zk-SNARKs and zk-STARKs if warranted
- W3C DID specification and Verifiable Credentials specification

**DAO case studies (study failures as hard as successes):**

- The DAO hack (2016) — the most important lesson in smart contract security;
  $60M drained through a reentrancy bug
- MakerDAO — complex on-chain governance at scale; what it gets right
- Nouns DAO — unusual DAO design; interesting governance experiments
- Gitcoin DAO — community governance; real coordination problems encountered
- Any post-mortem on a DAO that failed due to governance failure, not a hack

**Privacy and data sovereignty:**

- "Self-Sovereign Identity" by Christopher Allen — foundational concept
- Tim Berners-Lee's Solid project — data pods, user-controlled data access
- Filecoin / IPFS documentation — content-addressed decentralised storage
- Aztec Network — private smart contracts using ZKPs (closest to what we want)

**YouTube / accessible material:**

- "How does a blockchain work" — look for technical explanations, not hype
- Santa Fe Institute lectures on decentralised systems if available
- Any talk by Vitalik Buterin on DAO governance — unusually clear thinker

---

## Output Required

1. **What blockchain gives us** — a precise list of properties it provides that
   no other technology provides. (Immutability, trustless consensus, programmable
   governance, cryptographic auditability.) Be exact — no hand-waving.

2. **What blockchain cannot do** — equally precise. (Real-time coordination,
   secret state, external calls, large data storage.) Know the walls.

3. **The right chain type for Mojo** — public vs private vs permissioned;
   with reasoning. This is a decision, not a list of options.

4. **The DAO model that fits** — which existing governance framework (or
   combination) maps best onto a Mojo collective? What would need to change?

5. **Data sovereignty model** — a concrete description of how blockchain enables
   controlled data sharing with third parties like Anthropic. What the contract
   looks like, what the access grant looks like, what ZKPs could contribute.
   Flag what is proven vs what is speculative.

6. **The on-chain / off-chain split** — exactly which collective operations
   belong on-chain and which must stay off. This becomes a design input for
   sessions 04 and 07.

7. **Open risks** — smart contract bugs are irreversible. What are the failure
   modes specific to Mojo's use case? What mitigations exist?

---

## Dependencies

Session 00 — Session Guide (read first)

Outputs feed into: sessions 01, 04, 07, and the constitutional contract planning.
