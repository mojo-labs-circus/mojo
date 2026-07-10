# The Anatomy of a Mojo System

*The most concrete statement of what Mojo is. Every other document in this repo is
the why, the plan, or the history; this is the what. A visual rendering of the same
map lives in [anatomy.html](anatomy.html). This is a working map, not a finished
design — the Mojo System Interface (MSI) is built by pinning down each seam below,
and none of them are pinned yet.*

---

## What a Mojo system is

A Mojo system is a personal AI — a Jarvis: always on, remembers you, reachable from
every device you own, and yours. It is assembled from swappable pieces the way a
Linux system is: distributions, desktops, and shells compete and get replaced,
while your home directory stays yours. The **Mojo System Interface (MSI)** is the
standard that defines each piece and each seam between pieces, so that anyone can
build a better implementation of any piece and you can run whichever you want.

One rule sits underneath everything: **every piece is a replaceable implementation,
except one — your data.** The data is the heart and soul of the system: package it
up, drop it into a different assembly of pieces, and it is still the same
assistant — the way dotfiles make any machine yours.

Every seam carries one of three postures:

- **Defined by the MSI** — the standard's own work; nothing existing covers it.
- **Adopted** — an existing standard, consumed as-is (MCP, SKILL.md, A2A, the
  model wire format).
- **Deliberately not standardized** — implementations compete there on purpose.

## The map

```
                         THE OWNER — a human
            verified in-system by a key-pair/DID-shaped identity
         (a) owner verification ↕      ↕ (b) proactive delivery
  ┌────────────────────────────────────────────────────────────────┐
  │ WINDOWS — every surface the owner talks through                │
  │ terminal · phone · voice · watch · …    [swappable: Client]    │
  │ a window runs on one of the owner's machines, or on a thin     │
  │ device that runs nothing else                                  │
  └────────────────────────────────────────────────────────────────┘
                       ↕ (c) client contract
  ┌─ ONE OF THE OWNER'S MACHINES — laptop · desktop · server ──────┐
  │                                                                │
  │  THE RUNTIME PIECES — assembled per run                        │
  │  ┌────────┐    ┌──────────────────────────────────────────┐    │
  │  │ ROUTER │───▶│ AGENT RUNTIME — the think-act loop,      │    │
  │  └────────┘(d) │ adopted whole; internals not standardized│    │
  │  [swappable:   │   (e) MODEL — local server (vLLM,        │    │
  │   Router]      │       Ollama, llama.cpp, …)              │    │
  │                │   (f) TOOLS — MCP servers                │    │
  │                │   (h) SKILLS — SKILL.md, live in the data│    │
  │                └──────────────────────────────────────────┘    │
  │                 [swappable: Harness, Model endpoint]           │
  │       ↕ (i) run contract        ↕ (g) scheduled runs           │
  │  ══════ KERNEL — the enforcement layer  [swappable: Kernel] ══ │
  │         every consequential action is checked against what     │
  │         the owner allowed — granted, narrowed, revoked,        │
  │         and recorded                                           │
  │       ↕ (j) enforcement contract                               │
  │  MEMORY PROVIDER — serves the data: retrieval, indexing,       │
  │         views    [swappable: Memory Provider]                  │
  │       ↕ (m) data shape                                         │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ THE IDENTITY — the data. Memory · persona · policies ·   │  │
  │  │ budgets · provenance. Plain files. The only thing never  │  │
  │  │ swapped; portable into any other assembly of pieces.     │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │                                                                │
  │  HOST OS — any Linux distribution (macOS later); everything    │
  │  above, the kernel included, is ordinary userspace software    │
  └────────────────────────────────────────────────────────────────┘
       ↕ (k) federation    ↕ (n) machine admission    ↕ (l) A2A
  the owner's other machines —              other people's agents —
  one identity across all of them           different owners, opaque,
                                            no trust extended
```

## The pieces

**The Owner.** One person. Inside the system they are represented by a verifiable
identity — something key-pair or DID-shaped — so any piece can check that "the
owner said so" actually means the owner said so. Every permission in the system
bottoms out here.

**Windows** *(swappable — profile: Client)*. Every surface the owner talks
through. Each is a window into the same one assistant — never a separate copy of
it. A window can run on one of the owner's machines (a terminal, a desktop app) or
on a thin device that runs nothing else (a phone app, a watch). Windows are
implementations too: anyone can build one, and an owner can use any mix at once.

**Router** *(swappable — profile: Router)*. Given a task, the available pieces,
and the owner's constraints and budget, it selects what runs: which agent runtime,
which model, on which machine. Routers compete on selection quality. Hard limits
are the owner's policy data, which a router obeys — never properties of the router
itself.

**Agent runtime** *(swappable — profile: Harness)*. The loop that does the
thinking-and-acting: an existing agent runtime, adopted whole. The MSI defines
only how it connects — what it is handed and what it owes back. Its insides — how
it plans, manages context, retries — are deliberately not standardized; runtimes
compete there.

**Model** *(swappable — profile: Model endpoint)*. A local or self-hosted model
server — vLLM, Ollama, llama.cpp. Any endpoint speaking the standard wire shape is
interchangeable.

**Tools** *(consumed format — MCP)*. What a tool may actually touch is decided by
the kernel, not by the tool protocol.

**Skills** *(consumed format — SKILL.md)*. Learned procedures. They live in the
data layer and travel with it.

**Kernel** *(swappable — profile: Kernel)*. The enforcement layer. Every
consequential action — reading the data, writing it back, touching a tool that
reaches the outside world — passes through here and is checked against what the
owner allowed. It grants, narrows, and revokes permissions, and records what
happened, so "what did it do, and who allowed it" always has an answer. Each
machine runs its own kernel instance so enforcement keeps working offline, but all
of an owner's machines answer to one shared policy. Like every other piece, the
kernel is swappable: the MSI pins the contract precisely so that better kernels
can replace any given one.

**Memory provider** *(swappable — profile: Memory Provider)*. The software that
serves the data: retrieval, indexing, search, the views a runtime works from.
Providers compete — a better one can be swapped in at any time, because of one
strict rule: the data itself stays in the standard shape, and anything a provider
derives from it (indexes, embeddings) must be rebuildable from the data alone. The
relationship is Obsidian and your vault: delete the app, keep everything.

**The identity** *(not a piece — the data; the only thing never swapped)*.
Everything that makes the assistant *this* assistant: its memory of the owner, its
persona and voice, the owner's policies and budgets, a record of who wrote what
under what authority — permissions travelling with the data itself. Stored as
plain files, built on a small set of file-like primitives the MSI defines — the
equivalent of files and directories in an OS. It can be packaged up and dropped
into an entirely different assembly of pieces and it is still the same assistant.
This is what makes any Mojo system *yours*.

**Host OS.** Any Linux distribution (macOS later). Every piece above, the kernel
included, is ordinary software running in OS userspace. No kernel modules, no
custom OS.

**Outward.** The owner's other machines run the same stack and behave as one
assistant, not several that happen to share notes — that's the federation and
machine-admission seams. Other people's agents are different owners, opaque by
design: A2A carries the conversation and nothing about it extends trust. This is
also how one person's Jarvis speaks to another's.

## The seams

Six pieces get conformance profiles — Client, Router, Harness, Model endpoint,
Kernel, Memory Provider — meaning an implementation is compliant against its
profile, never against the whole document. The identity is data, not a piece:
nothing to conform, only a shape to keep.

| Seam | Connects | What the MSI must pin down | Posture |
|---|---|---|---|
| a | Owner ↔ any window | How the human is verified as the owner — the login of the system | MSI |
| b | System → owner | That a compliant window can receive an unsolicited delivery; urgency policy is the owner's data | MSI |
| c | Window ↔ system | What a window is handed, may cache, must never hold authoritatively | MSI |
| d | Task → selection | Inputs (task, roster, constraints, budget) and output (a selection); selection quality is competitive | MSI |
| e | Runtime ↔ model server | The OpenAI-compatible baseline + how optional capabilities (structured output, reasoning traces) are declared rather than assumed | Adopt + envelope |
| f | Runtime ↔ tools | MCP, pinned version; enforcement stays with the kernel, not the protocol | Adopt |
| g | Clock → a run | How a standing instruction fires with nothing else running; the schedule itself is data | MSI |
| h | Skills → runtime | SKILL.md as the on-disk shape; skills live in the identity and travel with it | Adopt |
| i | Kernel → runtime | What a run is handed (identity fragment, permissions, task) and owes back (learnings, in the data's shape) | MSI |
| j | Kernel ↔ everything consequential | The permission model: granting, narrowing, revoking, auditing — and how it works offline | MSI |
| k | Machine ↔ machine | One identity across N machines: ordering, conflict, partition, and live-task handoff (who's driving) | MSI |
| l | System ↔ outside agents | A2A, pinned version; talking to strangers, no trust extended | Adopt |
| m | Provider ↔ the data | The standard data shape; everything derived must be rebuildable from the data alone | MSI |
| n | New machine → owner's set | How a machine is admitted and how its trust is later revoked | MSI |

## What survives, what swaps

The whole design reduces to one split.

**The identity — owed literal equivalence.** Same memory, same permissions, same
relationship, bit for bit, across any swap of any piece. This is the only
equivalence the system guarantees anywhere.

**Everything else — owed nothing.** A different model thinks differently; a
different runtime plans differently; a different window looks different. No piece
is owed identical *output* — only the identity is owed identical *data*. That
freedom is why pieces can compete at all.
