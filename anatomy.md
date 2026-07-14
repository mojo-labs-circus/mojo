# The Anatomy of a Mojo System

*The most concrete statement of what Mojo is. Every other document in this repo
is the why, the plan, or the history; this is the what. A visual rendering of
the same map lives in [anatomy.html](anatomy.html). This is a working map, not a
finished design. The Mojo System Interface (MSI) gets built by finding each real
seam between these pieces and pinning it down; that search runs fresh in
[seam-method.md](seam-method.md) and lands in [seams.md](seams.md), not here.*

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
              owner verification ↕      ↕ proactive delivery
  ┌────────────────────────────────────────────────────────────────┐
  │ CLIENTS: every surface the owner talks through                 │
  │ terminal · phone · voice · watch · …                           │
  │ a client runs on one of the owner's machines, or on a thin     │
  │ device that runs nothing else                                  │
  └────────────────────────────────────────────────────────────────┘
        ↕ client contract
  ┌─ ONE OF THE OWNER'S MACHINES: laptop · desktop · server ───────┐
  │                                                                │
  │  RUNS: ephemeral. One harness working one task,                │
  │  killable, several live at once                                │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ HARNESS: the think-act loop. Adopted whole; how it       │  │
  │  │ plans, manages context, retries is not standardized,     │  │
  │  │ harnesses compete there                                  │  │
  │  │ reads Skills straight from the identity, not a           │  │
  │  │ per-run binding like the services below                  │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │     ↕ bindings, chosen fresh for every run and sub-run:        │
  │       model · tools · memory · sandbox                         │
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
  │  │ every run                 run assembly · triggered runs  │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ KERNEL: the enforcement layer; every consequential       │  │
  │  │ action is checked against what the owner allowed:        │  │
  │  │ granted, narrowed, revoked, recorded                     │  │
  │  │              run contract · enforcement contract         │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ CREDENTIAL BROKER: custody of secrets, injected into     │  │
  │  │ exactly the call that needs them       secret custody    │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ PROVENANCE: trust-tags content before it reaches a run   │  │
  │  │                                        content trust     │  │
  │  ├──────────────────────────────────────────────────────────┤  │
  │  │ FLEET MANAGER: keeps the owner's machines one system     │  │
  │  │             federation · machine admission               │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │       ↕ data shape: providers derive, never own                │
  │  ┌──────────────────────────────────────────────────────────┐  │
  │  │ THE IDENTITY: the data. Memory · personas · policies ·   │  │
  │  │ budgets · provenance. Plain files. The only thing never  │  │
  │  │ swapped; portable into any other assembly of pieces.     │  │
  │  └──────────────────────────────────────────────────────────┘  │
  │                                                                │
  │  HOST OS: any Linux distribution (macOS later); everything     │
  │  above, the kernel included, is ordinary userspace software    │
  └────────────────────────────────────────────────────────────────┘
       ↕ federation    ↕ machine admission    ↕ A2A
  the owner's other machines:               other people's agents:
  one identity across all of them           different owners, opaque,
                                            no trust extended
```

## The pieces

Full descriptions live in [pieces.md](pieces.md), split out on its own so the
piece list can move without touching this file. Thirteen nodes: Owner,
Client, Harness, Model endpoint, Tools, Memory provider, Sandbox, Router,
Kernel, Credential broker, Provenance, Fleet manager, and the Identity (data,
not a piece). Host OS sits underneath all of them as substrate.

## The seams

Not listed here. The old eighteen-letter table assumed every seam was the
same flat shape, and that assumption turned out wrong, so seam-finding now
runs as its own procedure, [seam-method.md](seam-method.md), with results
landing one at a time in [seams.md](seams.md) as they're actually found and
checked, not asserted up front.

## What survives, what swaps

The whole design reduces to one split.

**The identity is owed literal equivalence.** Same memory, same permissions,
same relationship, bit for bit, across any swap of any piece. This is the only
equivalence the system guarantees anywhere.

**Everything else is owed nothing.** A different model thinks differently. A
different harness plans differently. A different client looks different. No
piece is owed identical output; only the identity is owed identical data. That
freedom is why pieces can compete at all.
