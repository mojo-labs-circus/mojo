# The pieces of a Mojo system

Split out of [anatomy.md](anatomy.md) so the piece list can move on its own
diff whenever seam work exposes a hole in it, without touching the map or the
seam list, the same way [seams.md](seams.md) already holds seam findings on
their own instead of inside one monolithic doc. [anatomy.md](anatomy.md) is
the union of this file and seams.md: the map and the narrative both draw
from here, this file doesn't draw from them.

13 nodes checked pairwise in [piece-matrix.md](piece-matrix.md): the 11
pieces below plus Owner and Identity. Host OS is excluded as substrate, not a
piece.

Ten of the eleven pieces get conformance profiles: Client, Router, Harness,
Model endpoint, Sandbox, Credential broker, Provenance, Kernel, Memory
provider, Fleet manager. An implementation is compliant against its own
profile, never against the whole document. Tools has no profile of its own
(MCP, adopted as-is). The identity is data, not a piece: nothing to conform,
only a shape to keep.

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
Obsidian and your vault: delete the app, keep everything.

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
fleet manager's job. Other people's agents are different owners, opaque by
design: A2A carries the conversation and nothing about it extends trust.
This is also how one person's digital counterpart speaks to another's.
