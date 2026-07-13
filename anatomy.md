# The Anatomy of a Mojo System

*The most concrete statement of what Mojo is. Every other document in this repo
is the why, the plan, or the history; this is the what. A visual rendering of
the same map lives in [anatomy.html](anatomy.html). This is a working map, not a
finished design. The Mojo System Interface (MSI) gets built by pinning down each
seam below, and none of them are pinned yet.*

---

## What a Mojo system is

**Digital counterpart:** a persistent AI system that maintains one continuous
identity, memory, and set of permissions with its owner across time, devices,
interfaces, and implementations. The term this document and the MSI are
written against, the same way POSIX had to define "operating system" before it
could define anything else.

A Mojo system is one: always on, remembers you, reachable from every device
you own, and yours. It is assembled from swappable pieces the
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

The pieces themselves sort into four lifecycles, and the split matters as
much as the piece list:

- **Reach is the owner's, not any one machine's.** Clients: terminal, phone,
  voice, watch. Many run at once, and unlike everything below, a client isn't
  bound to one of the owner's machines at all, a phone app can be a thin
  device that runs nothing else. Which clients exist is an owner decision, the
  same as system control, but there is no "exactly one" here: reach is
  supposed to be everywhere the owner is.
- **Runs are ephemeral.** A run is one harness working one task. It is
  killable, several can be live at once, and each one, sub-runs included,
  spawns with its own selection of services.
- **Services are plural.** Model endpoints, tools, memory providers,
  sandboxes. Several of each kind can serve one machine at once, and a run is
  granted a set of each, not handed one: a harness reaches for whichever
  instance a given step actually needs, at whatever granularity the task
  calls for, not fixed once for the whole run. A service decides nothing
  itself, it's the thing chosen among; whether a given pick actually falls
  inside what the run was granted is checked the same way every other
  consequential action is, through the kernel. Whether a service is a
  long-running process or started on demand is an implementation choice;
  what's standardized is the grant, made fresh per run, and the check, made
  per action.
- **System control is singular.** Router, kernel, credential broker,
  provenance, fleet manager. Exactly one live instance of each per machine.
  Swapping one is an owner decision, never a per-run choice.

The rule that does the sorting: when two live instances could disagree about a
fact that gates something consequential (a permission, a secret's scope, a
trust tag, who is in the fleet), the piece is singular, because the
disagreement is itself the hole. When several readings of the same data are
safe at once, the piece is plural, and implementations compete side by side.

A second rule sits inside the first: among the singular pieces, only the
kernel writes. Router, the credential broker, provenance, and the fleet
manager all read the data to do their jobs; none of them author it. The
kernel is the one piece that grants, narrows, and revokes what's allowed, and
records what happened, so every change to what the data permits has exactly
one path in and one name attached to it.

## The map

```
                         THE OWNER (a human)
            verified in-system by a key-pair/DID-shaped credential
         (a) owner verification ↕      ↕ (b) proactive delivery
  ┌────────────────────────────────────────────────────────────────┐
  │ CLIENTS: every surface the owner talks through                 │
  │ terminal · phone · voice · watch · …                           │
  │ a client runs on one of the owner's machines, or on a thin     │
  │ device that runs nothing else                                  │
  └────────────────────────────────────────────────────────────────┘
        ↕ (c) client contract
  ┌─ ONE OF THE OWNER'S MACHINES: laptop · desktop · server ───────┐
  │                                                                │
  │  RUNS: ephemeral. One harness working one task,                │
  │  killable, several live at once                                │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ HARNESS: the think-act loop. Adopted whole; how it       │  │
  │  │ plans, manages context, retries is not standardized,     │  │
  │  │ harnesses compete there                                  │  │
  │  │ (h) reads Skills straight from the identity, not a       │  │
  │  │ per-run binding like the services below                  │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │     ↕ bindings, chosen fresh for every run and sub-run:        │
  │       (e) model · (f) tools · (s) memory · (q) sandbox         │
  │                                                                │
  │  SERVICES: plural. A run is granted a set, not one;            │
  │  which instance an action uses is resolved per action          │
  │  ┌────────────┐ ┌─────────┐ ┌────────────┐ ┌────────────┐      │
  │  │ MODELS:    │ │ TOOLS:  │ │ MEMORY     │ │ SANDBOXES: │      │
  │  │ vLLM,      │ │ MCP     │ │ PROVIDERS: │ │ where a    │      │
  │  │ Ollama,    │ │ servers │ │ views over │ │ permitted  │      │
  │  │ llama.cpp… │ │         │ │ the data   │ │ action     │      │
  │  │            │ │         │ │            │ │ executes   │      │
  │  └────────────┘ └─────────┘ └────────────┘ └────────────┘      │
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

**Client** *(reach)*. The surface through which the owner
reaches the one continuous assistant: a conversation (terminal, app, phone
call, text) or direct access to the identity itself (a notes app, a file
browser, anything that touches the data straight, the way Obsidian touches a
vault). Each is a window into the same one identity, never a separate copy of
it, and holds no session of its own: between runs nothing is live, a client
reads and writes against the identity the way an IMAP client reads a mailbox.
It is live only for the duration of a run addressed to it. A client can run on
any reachable device: one of the owner's own machines (a terminal, a desktop
app), a thin device that runs nothing else (a phone app, a watch), or hardware
the owner is only passing through (a friend's laptop, a work machine).
Ownership of the device doesn't matter, since a client holds nothing
authoritative locally to begin with.

**Harness** *(the run)*. The loop that does the thinking and
acting: an existing agent runtime, adopted whole rather than invented by the
MSI. It is the one piece that is born and dies with the run itself. The MSI
defines only how it connects: what it is handed, and what it owes back. Its
insides (how it plans, retries, and manages its own live context budget as a
run proceeds) are deliberately not standardized; harnesses compete there. It
reads Skills (SKILL.md, learned procedures) straight from the identity the
same way it reads any other data, not as a per-run binding the way it picks a
model or a sandbox: skills aren't chosen per run, they're just there.

**Model endpoint** *(service)*. Wherever inference actually
runs: a self-hosted server (vLLM, Ollama, llama.cpp) on hardware the owner
controls, or a vendor's hosted API. Any endpoint speaking the standard wire
shape is interchangeable. Endpoints are modality-typed: chat, embedding,
image, and speech are separate kinds of endpoint, each with its own grant,
not one roster spanning every modality.

**Tools** *(service; no profile)*. Anything a harness calls out to, software
or physical, in the same shape: a search API and a camera are both just an
MCP server. What a tool may actually touch is decided by the kernel, not by
the tool protocol.

**Memory provider** *(service)*. The standardized front door onto the data: a
run reads and writes through whichever provider it's handed, never touching
the data directly. What it returns, and how, is where the real engineering
lives: retrieval, indexing, search, ranking, whatever view a harness needs.
Providers are plural in the strongest sense: because the data stays in the
standard shape underneath, several can serve the same identity at once, a
vector-heavy one and a grep-style one side by side, different views and
different scopings over the very same data, not just different instances of
the same view. That plurality holds because of one strict rule:
providers derive, they never own. Anything a provider builds from the data
(indexes, embeddings) must be rebuildable from the data alone, and any write
it accepts lands in the data's standard shape, where concurrent writes
reconcile, never inside any one provider's private state. The relationship is
Obsidian and your vault: delete the app, keep everything. What a run asks of
a provider and what it gets back is its own seam (s); whatever path a write
takes, it lands in the data's shape, which is m's business.

**Sandbox** *(service)*. Where a permitted action
actually executes: a container, an isolated process, another of the owner's
machines, a remote service. Placement, not permission: a sandbox is the
boundary an action runs inside and what it can see while there, it decides
nothing itself. Which actions are allowed at all, and whether a given
placement was actually one the run was granted, stays the kernel's call
throughout.

**Router** *(system control)*. Always on. When nothing is running, the router
is what watches the triggers: the clock, outside events, idle time, an
owner's direct request. When one fires, it assembles the run: given the task,
the available pieces, and the owner's constraints and budget, it picks the
harness and, on which machine, grants it a set of each service (model
endpoint, tools, memory provider, sandbox) to draw from. Each machine runs
its own router so triggers keep firing and runs keep assembling while
offline; keeping several machines' routers from stepping on the same trigger
is the fleet manager's job, not the router's own.
Routers compete on selection quality. Hard limits are the owner's policy data,
which a router obeys. They are never properties of the router itself.

**Kernel** *(system control)*. The enforcement layer. Every consequential
action any other piece in the system takes (reading the data, writing it
back, touching a tool that reaches the outside world) passes through here
and is checked against what the owner allowed. It grants, narrows, and revokes permissions, and records what
happened, so "what did it do, and who allowed it" always has an answer. Each
machine runs its own kernel instance so enforcement keeps working offline, but
all of an owner's machines answer to one shared policy.

**Credential broker** *(system control)*. The mediator between wherever the
owner's secrets actually live (an OS keychain, a vault, a hardware token,
whatever the owner picked) and the specific call that needs one. It doesn't
own the storage; it scopes and injects, so a real secret never sits in a
model's context, a harness's hands, or a sandboxed action's own view. The
kernel decides whether an action is allowed; the broker makes sure the secret
reaches only that action. One live broker per machine: custody is exactly the
kind of fact two instances must never disagree about.

**Provenance** *(system control)*. Trust-tags content before it reaches a
run: where it came from, whether the owner wrote it, whether it arrived from
the open web. A harness and a kernel can then treat a stranger's words
differently from the owner's, instead of feeding everything to the model as
if it were equally trusted. One live instance per machine: if a run could
pick its own tagger, untrusted content could be laundered through the laxest
one.

**Fleet manager** *(system control)*. Keeps the owner's machines one system.
It admits a new machine into the owner's set, revokes one that is no longer
trusted, and coordinates the set so one identity stays coherent across all of
it: sync, conflict, partition, and who is driving a live task. The kernel
enforces on one machine; the fleet manager is what makes N machines answer as
one assistant instead of several that share notes. One live instance per
machine, the same as every other system-control piece: each machine still
needs its own instance so the fleet stays coherent even when one machine is
offline.

**The identity** *(not a piece. The data, and the only thing never swapped)*.
Everything that makes the assistant this assistant: its memory of the owner,
its personas, its skills (SKILL.md, learned procedures a harness reads
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
harness or a wrong conclusion, must be recoverable by rolling the data back,
whatever pieces sit above it and whatever host OS sits below.

**Host OS.** Any Linux distribution (macOS later). Every piece above, the kernel
included, is ordinary software running in OS userspace. No kernel modules, no
custom OS.

**Outward.** The owner's other machines run the same stack and behave as one
assistant, not several that happen to share notes. Keeping that true is the
fleet manager's job, through the federation and machine-admission seams. Other
people's agents are different owners, opaque by design: A2A carries the
conversation and nothing about it extends trust. This is also how one person's
digital counterpart speaks to another's.

## The seams

Ten pieces get conformance profiles: Client, Router, Harness, Model endpoint,
Sandbox, Credential Broker, Provenance, Kernel, Memory Provider, Fleet
Manager. An implementation is compliant against its profile,
never against the whole document. The identity is data, not a piece: nothing to
conform, only a shape to keep.

| Seam | Connects | What the MSI must pin down | Posture |
|---|---|---|---|
| a | Owner ↔ any client | How the human is verified as the owner. The login of the system | MSI |
| b | System → owner | That a compliant client can receive an unsolicited delivery; urgency policy is the owner's data | MSI |
| c | Client ↔ system | What a client is handed, may cache, must never hold authoritatively | MSI |
| d | Task → a run's assembly | Inputs (task, roster, constraints, budget) and output (a full piece-set for a run, chosen again for every sub-run); selection quality is competitive | MSI |
| e | Harness ↔ model endpoint | The OpenAI-compatible baseline, plus how optional capabilities (structured output, reasoning traces) are declared rather than assumed; endpoints are modality-typed (chat, embedding, image, speech), never assumed chat-only | Adopt + envelope |
| f | Harness ↔ tools | MCP, pinned version; enforcement stays with the kernel, not the protocol | Adopt |
| g | A trigger → a run | How a run fires with nothing else running: the always-on router watches the clock, outside events, and idle time (downtime maintenance, like consolidating and pruning memory, is just a run); triggers themselves are data | MSI |
| h | Skills → harness | SKILL.md as the on-disk shape; skills live in the identity and travel with it | Adopt |
| i | Kernel → harness | What a run is handed (identity fragment, permissions, task), what it owes back (learnings, in the data's shape), and how a live run is interrupted or killed | MSI |
| j | Kernel ↔ everything consequential | The permission model: granting, narrowing, revoking, auditing, and how it works offline | MSI |
| k | Machine ↔ machine | One identity across N machines: ordering, conflict, partition, and live-task handoff (who's driving); the fleet manager's seam | MSI |
| l | System ↔ outside agents | A2A, pinned version; talking to strangers, no trust extended | Adopt |
| m | Providers ↔ the data | The standard data shape; everything derived must be rebuildable from the data alone; how the data keeps history so a bad write can be rolled back; per-unit permissions travel in the data (they are what cuts a persona's view); several providers can serve the same data at once, so concurrent writes reconcile at the data, never inside one provider | MSI |
| n | New machine → owner's set | How a machine is admitted (hardware attestation is one admissible mechanism) and how its trust is later revoked; the fleet manager's seam | MSI |
| p | Broker ↔ anything needing a secret | How secrets are stored, scoped, and injected into exactly the call that needs them, without entering model context; custody, not permission | MSI |
| q | Kernel → sandbox | Where an allowed action executes and what it can see while running; placement is swappable (local container, host, another machine, remote service) | MSI |
| r | External content → a run | How content is trust-tagged before it reaches the model: source, authorship, what vouches for it | MSI |
| s | Harness ↔ memory provider | What a run asks of a provider (query in, sourced results out) and what every compliant provider must accept and return, so a harness can talk to whichever provider it is handed; whatever path a write takes, it lands in the data's shape, which is m's business | MSI |

## What survives, what swaps

The whole design reduces to one split.

**The identity is owed literal equivalence.** Same memory, same permissions,
same relationship, bit for bit, across any swap of any piece. This is the only
equivalence the system guarantees anywhere.

**Everything else is owed nothing.** A different model thinks differently. A
different harness plans differently. A different client looks different. No
piece is owed identical output; only the identity is owed identical data. That
freedom is why pieces can compete at all.
