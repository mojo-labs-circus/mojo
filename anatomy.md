# The Anatomy of a Mojo System

*The most concrete statement of what Mojo is. Every other document in this repo
is the why, the plan, or the history; this is the what. A visual rendering of
the same map lives in [anatomy.html](anatomy.html). This is a working map, not a
finished design. The Mojo System Interface (MSI) gets built by pinning down each
seam below, and none of them are pinned yet.*

---

## What a Mojo system is

A Mojo system is a personal AI. A Jarvis: always on, remembers you, reachable
from every device you own, and yours. It is assembled from swappable pieces the
way a Linux system is: distributions, desktops, and shells compete and get
replaced, while your home directory stays yours. The **Mojo System Interface
(MSI)** is the standard that defines each piece and each seam between pieces, so
that anyone can build a better implementation of any piece and you can run
whichever you want.

One rule sits underneath everything: **every piece is a replaceable
implementation, except one. Your data.** The data is the heart and soul of the
system. Package it up, drop it into a different assembly of pieces, and it is
still the same assistant, the way dotfiles make any machine yours.

Every seam carries one of three postures:

- **Defined by the MSI.** The standard's own work; nothing existing covers it.
- **Adopted.** An existing standard, consumed as-is (MCP, SKILL.md, A2A, the
  model wire format).
- **Deliberately not standardized.** Implementations compete there on purpose.

The pieces themselves sort into three lifecycles, and the split matters as
much as the piece list:

- **Runs are ephemeral.** A run is one agent runtime working one task. It is
  killable, several can be live at once, and each one, sub-runs included,
  spawns with its own selection of services.
- **Services are plural.** Model endpoints, tools, memory providers, sandboxes.
  Several of each kind can serve one machine at the same time, and which ones a
  run uses is chosen fresh for every run. Whether a service is a long-running
  process or started on demand is an implementation choice; the binding is what
  is per-run.
- **System control is singular.** Router, kernel, credential broker,
  provenance, fleet manager. Exactly one live instance of each per machine.
  Swapping one is an owner decision, never a per-run choice.

The rule that does the sorting: when two live instances could disagree about a
fact that gates something consequential (a permission, a secret's scope, a
trust tag, who is in the fleet), the piece is singular, because the
disagreement is itself the hole. When several readings of the same data are
safe at once, the piece is plural, and implementations compete side by side.

## The map

```
                         THE OWNER (a human)
            verified in-system by a key-pair/DID-shaped credential
         (a) owner verification ↕      ↕ (b) proactive delivery
  ┌────────────────────────────────────────────────────────────────┐
  │ WINDOWS: every surface the owner talks through                 │
  │ terminal · phone · voice · watch · …    [swappable: Client]    │
  │ a window runs on one of the owner's machines, or on a thin     │
  │ device that runs nothing else                                  │
  ├────────────────────────────────────────────────────────────────┤
  │ PERIPHERALS: every capability the system senses or acts        │
  │ through: camera · location · notifications · screen · …        │
  │ usually the same device as a window, doing a different job     │
  │ [swappable: Peripheral]                                        │
  └────────────────────────────────────────────────────────────────┘
        ↕ (c) client contract       ↕ (o) peripheral contract
  ┌─ ONE OF THE OWNER'S MACHINES: laptop · desktop · server ───────┐
  │                                                                │
  │  RUNS: ephemeral. One agent runtime working one task,          │
  │  killable, several live at once                                │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ AGENT RUNTIME: the think-act loop. Adopted whole; how    │  │
  │  │ it plans, manages context, retries is not standardized,  │  │
  │  │ runtimes compete there    [swappable: Harness]           │  │
  │  │ (h) reads Skills straight from the identity, not a       │  │
  │  │ per-run binding like the services below                  │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │     ↕ bindings, chosen fresh for every run and sub-run:        │
  │       (e) model · (f) tools · (s) memory · (q) sandbox         │
  │                                                                │
  │  SERVICES: plural. Several of each kind can serve one          │
  │  machine at once; which ones a run uses is the router's pick   │
  │  ┌────────────┐ ┌─────────┐ ┌────────────┐ ┌────────────┐      │
  │  │ MODELS:    │ │ TOOLS:  │ │ MEMORY     │ │ SANDBOXES: │      │
  │  │ vLLM,      │ │ MCP     │ │ PROVIDERS: │ │ where a    │      │
  │  │ Ollama,    │ │ servers │ │ views over │ │ permitted  │      │
  │  │ llama.cpp… │ │         │ │ the data   │ │ action     │      │
  │  │            │ │         │ │            │ │ executes   │      │
  │  └────────────┘ └─────────┘ └────────────┘ └────────────┘      │
  │  [swappable: Model endpoint · Memory Provider · Sandbox]       │
  │                                                                │
  │  SYSTEM CONTROL: singular. Exactly one live instance of        │
  │  each per machine, swapped by the owner, never per run         │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ ROUTER: always on; watches the triggers and assembles    │  │
  │  │ every run        (d) run assembly · (g) triggered runs   │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ KERNEL: the enforcement layer; every consequential       │  │
  │  │ action is checked against what the owner allowed:        │  │
  │  │ granted, narrowed, revoked, recorded                     │  │
  │  │           (i) run contract · (j) enforcement contract    │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ CREDENTIAL BROKER: custody of secrets, injected into     │  │
  │  │ exactly the call that needs them    (p) secret custody   │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ PROVENANCE: trust-tags content before it reaches a run   │  │
  │  │                                     (r) content trust    │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ FLEET MANAGER: keeps the owner's machines one system     │  │
  │  │          (k) federation · (n) machine admission          │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │  [swappable: Router · Kernel · Credential Broker ·             │
  │   Provenance · Fleet Manager]                                  │
  │       ↕ (m) data shape: providers derive, never own            │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ THE IDENTITY: the data. Memory · personas · policies ·   │  │
  │  │ budgets · provenance. Plain files. The only thing never  │  │
  │  │ swapped; portable into any other assembly of pieces.     │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │                                                                │
  │  HOST OS: any Linux distribution (macOS later); everything     │
  │  above, the kernel included, is ordinary userspace software    │
  └────────────────────────────────────────────────────────────────┘
       ↕ (k) federation    ↕ (n) machine admission    ↕ (l) A2A
  the owner's other machines:               other people's agents:
  one identity across all of them           different owners, opaque,
                                            no trust extended
```

## The pieces

**The Owner.** One person. Inside the system they are represented by a
verifiable credential, something key-pair or DID-shaped, so any piece can
check that "the owner said so" actually means the owner said so. Every
permission in the system bottoms out here.

**Windows** *(swappable, profile: Client)*. Every surface the owner talks
through. Each is a window into the same one assistant, never a separate copy of
it. A window can run on one of the owner's machines (a terminal, a desktop app)
or on a thin device that runs nothing else (a phone app, a watch). Windows are
implementations too: anyone can build one, and an owner can use any mix at once.

**Peripherals** *(swappable, profile: Peripheral)*. Every capability the system
senses or acts through on the owner's devices: camera, location, contacts,
notifications, screen control. A phone is usually both a window and a
peripheral host, but talking through a device and commanding it are two
different jobs with two different contracts. A peripheral declares what it
offers; every invocation of one is a consequential action and goes through the
kernel.

**Agent runtime** *(the run; swappable, profile: Harness)*. The loop that does
the thinking and acting: an existing agent runtime, adopted whole. It is the
one piece that is born and dies with the run itself. The MSI defines only how
it connects, what it is handed and what it owes back. Its insides (how it
plans, manages context, retries) are deliberately not standardized. Runtimes
compete there. It reads Skills (SKILL.md, learned procedures) straight from
the identity the same way it reads any other data, not as a per-run binding
the way it picks a model or a sandbox: skills aren't chosen per run, they're
just there.

**Model** *(service; swappable, profile: Model endpoint)*. A local or
self-hosted model server such as vLLM, Ollama, or llama.cpp. Any endpoint
speaking the standard wire shape is interchangeable. Endpoints are
modality-typed: chat, embedding, image, and speech are separate endpoints,
separately swappable, and a runtime may use different endpoints for different
steps of the same run, so the best or cheapest model for each job is always in
reach.

**Tools** *(service; consumed format: MCP)*. What a tool may actually touch is
decided by the kernel, not by the tool protocol.

**Memory provider** *(service; swappable, profile: Memory Provider)*. The
software that serves the data: retrieval, indexing, search, the views a runtime
works from. Providers are plural in the strongest sense: because the data stays
in the standard shape, several providers can serve the same identity at the
same time, a vector-heavy one and a grep-style one side by side, and a run is
pointed at whichever suits its task. That works because of one strict rule:
providers derive, they never own. Anything a provider builds from the data
(indexes, embeddings) must be rebuildable from the data alone, and writes land
in the data's standard shape, where concurrent writes reconcile, never inside
any one provider's private state. The relationship is Obsidian and your vault.
Delete the app, keep everything. What a run asks of a provider and what it
gets back is its own seam (s), so a harness can talk to whichever provider it
is handed; whatever path a write takes, it lands in the data's shape, which
is m's business.

**Sandbox** *(service; swappable, profile: Sandbox)*. Where a permitted action
actually executes: a container, an isolated process, another of the owner's
machines, a remote service. Placement, not permission. Which actions are
allowed at all is the kernel's decision; where an allowed action runs, and what
it can see while running, is the sandbox's.

**Router** *(system control; swappable, profile: Router)*. Always on. When
nothing is running, the router is what watches the triggers: the clock, outside
events, idle time. When one fires, or the owner asks for something, it
assembles the run: given the task, the available pieces, and the owner's
constraints and budget, it picks which agent runtime, which model, which memory
provider, which sandbox, on which machine. Every binding is chosen per run, and
a run can spawn sub-runs, each of which gets its own selection, recursively.
Routers compete on selection quality. Hard limits are the owner's policy data,
which a router obeys. They are never properties of the router itself.

**Kernel** *(system control; swappable, profile: Kernel)*. The enforcement
layer. Every consequential action (reading the data, writing it back, touching
a tool or a peripheral that reaches the outside world) passes through here and
is checked against what the owner allowed. It grants, narrows, and revokes
permissions, and records what happened, so "what did it do, and who allowed it"
always has an answer. Each machine runs its own kernel instance so enforcement
keeps working offline, but all of an owner's machines answer to one shared
policy. Like every other piece, the kernel is swappable: the MSI pins the
contract precisely so that better kernels can replace any given one.

**Credential broker** *(system control; swappable, profile: Credential
Broker)*. Custody of the owner's secrets: API keys, tokens, passwords. It
stores them, scopes them, and injects them into the specific call that needs
them, so a real secret never sits in a model's context or a runtime's hands.
The kernel decides whether an action is allowed; the broker makes sure the
secret reaches only that action. One live broker per machine: custody is
exactly the kind of fact two instances must never disagree about.

**Provenance** *(system control; swappable, profile: Provenance)*. Trust-tags
content before it reaches a run: where it came from, whether the owner wrote
it, whether it arrived from the open web. A runtime and a kernel can then treat
a stranger's words differently from the owner's, instead of feeding everything
to the model as if it were equally trusted. One live instance per machine: if a
run could pick its own tagger, untrusted content could be laundered through the
laxest one.

**Fleet manager** *(system control; swappable, profile: Fleet Manager)*. Keeps
the owner's machines one system. It admits a new machine into the owner's set,
revokes one that is no longer trusted, and coordinates the set so one identity
stays coherent across all of it: sync, conflict, partition, and who is driving
a live task. The kernel enforces on one machine; the fleet manager is what
makes N machines answer as one assistant instead of several that share notes.

**The identity** *(not a piece. The data, and the only thing never swapped)*.
Everything that makes the assistant this assistant: its memory of the owner,
its personas, its skills (SKILL.md, learned procedures a runtime reads
straight off the data, consumed format), the owner's policies and budgets, a
record of who wrote what under what authority. The memory is one; the
personas on top of it can be many. Each
persona is a voice plus a scoped view over the same memory, cut by per-unit
permissions the data itself carries, so a work persona and a casual persona are
two characters reading from one brain, not two brains. Permissions travel with
the data itself. It's stored as
plain files, built on a small set of file-like primitives the MSI defines, the
equivalent of files and directories in an OS. It can be packaged up and dropped
into an entirely different assembly of pieces and it is still the same
assistant. This is what makes any Mojo system yours.

Because the data is the one irreplaceable thing in the system, the MSI also has
to pin down how it keeps its own history. A bad write, whether from a buggy
runtime or a wrong conclusion, must be recoverable by rolling the data back,
whatever pieces sit above it and whatever host OS sits below.

**Host OS.** Any Linux distribution (macOS later). Every piece above, the kernel
included, is ordinary software running in OS userspace. No kernel modules, no
custom OS.

**Outward.** The owner's other machines run the same stack and behave as one
assistant, not several that happen to share notes. Keeping that true is the
fleet manager's job, through the federation and machine-admission seams. Other
people's agents are different owners, opaque by design: A2A carries the
conversation and nothing about it extends trust. This is also how one person's
Jarvis speaks to another's.

## The seams

Eleven pieces get conformance profiles: Client, Peripheral, Router, Harness,
Model endpoint, Sandbox, Credential Broker, Provenance, Kernel, Memory
Provider, Fleet Manager. An implementation is compliant against its profile,
never against the whole document. The identity is data, not a piece: nothing to
conform, only a shape to keep.

| Seam | Connects | What the MSI must pin down | Posture |
|---|---|---|---|
| a | Owner ↔ any window | How the human is verified as the owner. The login of the system | MSI |
| b | System → owner | That a compliant window can receive an unsolicited delivery; urgency policy is the owner's data | MSI |
| c | Window ↔ system | What a window is handed, may cache, must never hold authoritatively | MSI |
| d | Task → a run's assembly | Inputs (task, roster, constraints, budget) and output (a full piece-set for a run, chosen again for every sub-run); selection quality is competitive | MSI |
| e | Runtime ↔ model server | The OpenAI-compatible baseline, plus how optional capabilities (structured output, reasoning traces) are declared rather than assumed; endpoints are modality-typed (chat, embedding, image, speech), never assumed chat-only | Adopt + envelope |
| f | Runtime ↔ tools | MCP, pinned version; enforcement stays with the kernel, not the protocol | Adopt |
| g | A trigger → a run | How a run fires with nothing else running: the always-on router watches the clock, outside events, and idle time (downtime maintenance, like consolidating and pruning memory, is just a run); triggers themselves are data | MSI |
| h | Skills → runtime | SKILL.md as the on-disk shape; skills live in the identity and travel with it | Adopt |
| i | Kernel → runtime | What a run is handed (identity fragment, permissions, task), what it owes back (learnings, in the data's shape), and how a live run is interrupted or killed | MSI |
| j | Kernel ↔ everything consequential | The permission model: granting, narrowing, revoking, auditing, and how it works offline | MSI |
| k | Machine ↔ machine | One identity across N machines: ordering, conflict, partition, and live-task handoff (who's driving); the fleet manager's seam | MSI |
| l | System ↔ outside agents | A2A, pinned version; talking to strangers, no trust extended | Adopt |
| m | Providers ↔ the data | The standard data shape; everything derived must be rebuildable from the data alone; how the data keeps history so a bad write can be rolled back; per-unit permissions travel in the data (they are what cuts a persona's view); several providers can serve the same data at once, so concurrent writes reconcile at the data, never inside one provider | MSI |
| n | New machine → owner's set | How a machine is admitted (hardware attestation is one admissible mechanism) and how its trust is later revoked; the fleet manager's seam | MSI |
| o | Runtime ↔ peripherals | What a peripheral declares it offers, how it is invoked, and that every invocation is kernel-checked | MSI |
| p | Broker ↔ anything needing a secret | How secrets are stored, scoped, and injected into exactly the call that needs them, without entering model context; custody, not permission | MSI |
| q | Kernel → sandbox | Where an allowed action executes and what it can see while running; placement is swappable (local container, host, another machine, remote service) | MSI |
| r | External content → a run | How content is trust-tagged before it reaches the model: source, authorship, what vouches for it | MSI |
| s | Runtime ↔ memory provider | What a run asks of a provider (query in, sourced results out) and what every compliant provider must accept and return, so a harness can talk to whichever provider it is handed; whatever path a write takes, it lands in the data's shape, which is m's business | MSI |

## What survives, what swaps

The whole design reduces to one split.

**The identity is owed literal equivalence.** Same memory, same permissions,
same relationship, bit for bit, across any swap of any piece. This is the only
equivalence the system guarantees anywhere.

**Everything else is owed nothing.** A different model thinks differently. A
different runtime plans differently. A different window looks different. No
piece is owed identical output; only the identity is owed identical data. That
freedom is why pieces can compete at all.
