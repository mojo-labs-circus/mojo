# Research Plan & System Tracker

*How the rest of Mojo's design gets worked out, and the living status of doing it.
Distinct from [devlog.md](devlog.md) (the raw, messy, dated reasoning trail) and
`dockets-research/*.md` (per-primitive cleaned findings, earned once real content
accumulates). Nothing in this file is true by virtue of being written down — the
Status column is the only thing that means anything, and most of it says Open.
Nothing moves from here into a real spec until it's confirmed, same discipline as
everywhere else in this repo.*

---

## The plan

Walk every interface-level piece of a personal sovereign AI system, one at a time.
For each piece, check what real, already-existing systems did — not to copy one
wholesale, but so a decision is made against actual precedent instead of invented
from nothing. Decide Mojo's own answer per piece: borrowed from whichever system
fits best, synthesized from more than one, or genuinely new where nothing fits.
Record the decision and where the reasoning lives. Only once enough pieces are
actually decided, formalize the whole thing into one real spec.

**Two separate claims, not one, about what that spec is:**

1. **Runtime contract — genuinely an extension of Unix.** The implementation has
   to run as ordinary userspace software on any real POSIX system (Linux distros,
   macOS, BSD) — no custom kernel, nothing that requires seL4, Plan 9, or a BEAM VM
   to literally exist underneath it. This part is non-negotiable and it's the
   actual reason "Unix" is the frame at all.
2. **Conceptual model — a synthesis, described in Unix's vocabulary for
   legibility, not restricted to Unix's own mechanisms.** Where a piece's honest
   answer is seL4's capability model or Erlang's supervision trees, the spec says
   so — it doesn't force the concept back into Unix shape just because the
   document reads Unix-first. Legibility to system architects (the intended
   audience) is a real, deliberate goal; forcing every mechanism into borrowed
   Unix nouns when it isn't actually Unix's mechanism is the same mistake
   `naming-conventions.md` already caught once for vocabulary (Keel, Billet) and
   applies here at the level of whole mechanisms, not just names.

**The spec's own structure, landed 2026-07-09 (see
[interoperability.md](interoperability.md)): three layers, the POSIX way.**
Primitives that hold (Docket/Book/Cargo — File-side's job), schemas describing
the shape of what's built from the primitives (the First mate is the flagship
schema — a filesystem standard built on top of the file), and contracts
describing the seams between parts (enforcement, invocation wrapper, Hail,
routing, I/O). Each tracker section below notes which layer it feeds. Every
seam also carries one of three treatments from interoperability.md — adopt,
design, or deliberately leave open — and a row whose honest answer is "adopt X"
or "leave open" still gets walked before its Status moves: those are decisions
too.

**Sequencing, by dependency, not by the order pieces happened to get hit:**

1. File-side (Docket/Vessel/Book/Cargo) — in progress, being redone/made concrete
   through this pass, not done.
2. Identity/user (Captain) — small, mostly unblocks nothing else, close it out
   early.
3. Trust/capability/enforcement layer — high priority: Vessel, Book, and Cargo
   each already have real open threads (Vessel-role mechanism, permission
   ceiling, the Billet identity/role split) explicitly deferred to this layer.
   Designing it re-closes loose ends already sitting in three existing files.
4. First mate schema — depends on 1 and 3: it's what the primitives get shaped
   into, with permissions travelling in the shape itself.
5. Process/invocation (the wrapper around an arbitrary agent) — depends on 3;
   what an invocation can do is defined by what the trust layer allows.
6. Connection (Hail/mesh) — depends on 3 and 5; it's the channel invocations
   and other actors talk through, mediated by capabilities.
7. Routing/dispatch — last; it chooses between pieces the other layers make
   swappable, so it needs them defined first.

This order is for attention, not for what Mk1 needs — Mk1 of the standards
document needs a first-pass answer in every row of the tracker below, including
the shell/environment, time, trust-bootstrap, and fault-handling rows currently
marked as candidate gaps, not just these five. A working system needs some
answer for every piece to run at all, even a rough one; depth comes later, from
iterating the standard and the system together across versions.

Implementation (which repos, which services, which SDKs) is a separate, later
decision, made once the system itself is actually defined — not blocked on or
folded into this file. `mojo` stays the thinking repo; nothing here is code.

## Systems being checked

**Primary — get checked against every relevant piece:**

- **Unix/POSIX** — the runtime target and the default vocabulary. Files,
  processes, users, IPC, the syscall boundary, shell/environment, devices, time.
- **seL4** — capabilities (no ambient authority, unlike Unix's uid-based DAC),
  the Capability Derivation Tree + `Revoke`, Untyped memory/retyping, no baked-in
  "process" concept (only TCB + CSpace + VSpace, composed by userspace),
  Endpoints (sync IPC) and Notifications (async, badge-tagged, lossy).
- **Plan 9** — location-transparent namespace (9P), per-process private mount
  views, "everything is a file" taken further than Unix ever did, including
  network resources. The real precedent for the piece Unix itself never solved:
  addressing something reachable across machines through the same namespace as
  something local.
- **Erlang/OTP (BEAM)** — cheap, ephemeral, isolated processes with no shared
  state, message-passing only, supervisor trees, "let it crash" as the actual
  fault-recovery model. Likely the better precedent for the invocation contract
  specifically — closer to "no persistent identity, needs supervision" than
  either Unix processes or seL4 threads.

**Secondary — pull in only when a specific piece actually needs it, not a full
pass on principle:**

- **Mach** (XNU/macOS's actual kernel underneath the BSD userland) — port-based
  capability-flavored IPC, already running for free on one of the two OS families
  Mojo has to support. Relevant if Hail turns out port-shaped.
- **Zircon** (Fuchsia) — modern, shipping, production capability kernel;
  channels support passing capabilities themselves through a message, relevant if
  Hail needs "hand another Vessel a capability over the wire."
- **EROS/KeyKOS** — capability OSes built around orthogonal persistence (machine
  state persists automatically). Relevant only if "First mate holds no state of
  its own" needs a harder precedent than what's been reasoned out directly.
- **Capsicum** (FreeBSD) — capability mode bolted onto a real, shipping POSIX
  kernel, not a clean-slate microkernel: a process that enters capability mode
  loses the global filesystem/network namespace entirely and can only use
  capability-restricted file descriptors it already held or was explicitly
  handed. Added 2026-07-08 — likely more directly relevant than seL4 or EROS
  to the whole Trust/enforcement section, since it's the real precedent for
  "capability security on an actual userspace-on-Unix system," which is
  Mojo's own runtime-contract constraint, not seL4's clean-slate one.
- **WASI / Wasmtime capabilities** — the component model's capability-based
  host functions: no ambient filesystem or network access at all, only
  pre-opened directories/handles explicitly passed in at instantiation.
  Added 2026-07-08, same reasoning as Capsicum — a real, currently-shipping
  system built on exactly Mojo's own constraint (sandboxed, capability-scoped,
  runs as ordinary userspace software), not a research artifact.
- **Git's object model** — content-addressed, immutable objects (blobs,
  commits) plus mutable refs that point at them; superseding a claim is a new
  commit and a ref move, not an edit in place, and the old object still exists
  until an explicit GC. Added 2026-07-09 — real, load-bearing precedent for
  "retract without editing in place," and for branching (a ref forking from
  shared history, mergeable or discardable later), which Plan 9's Venti (see
  the tracker row below) doesn't have — Venti is a single-timeline archive
  snapshot, not a divergent-branch model.

**Agent-OS literature — contemporaneous, unproven, checked 2026-07-08. Pulled in
only for specific mechanism ideas, not treated as hardened precedent the way the
four primaries are — these are papers/early projects at the same stage as this
project, not decades of production hardening:**

- **AIOS** (arXiv:2403.16971) — the one real, active, shipped system of the
  three: working Python kernel, Rust rewrite in progress, ~6k stars. Its access
  control is ambient/privilege-group, i.e. the Unix-DAC direction already
  rejected here — cited as the documented road not taken, not a direction.
  Relevant for context-interrupt scheduling (snapshot/resume a long LLM call
  mid-generation, like a context switch) and load-by-name tool discovery.
- **AgenticOS** (arXiv:2606.21129) — three-week-old preprint, no code. Argues
  capability-holding and intent-checking are complementary layers, not
  competing ones: capability is the coarse "can this holder touch this
  object," intent-check is a semantic layer on top asking "does this specific
  use match the declared task." Confirms rather than challenges the seL4
  direction already picked. Relevant technique: compiling undeclared
  capabilities out of the address space entirely ("unreachable by
  construction") rather than gating them with a runtime check.
- **Agent libOS** (arXiv:2606.03895) — solo unreviewed preprint, thin
  evaluation (27 tasks), but has real working code proving the core claim
  isn't just a paper claim: tool dispatch is not the trust boundary — a model
  seeing a tool in its table doesn't grant the runtime authority to use it;
  that's checked separately, centrally. Relevant for its `AgentProcess` unit
  (state + children + budgets + checkpoints + explicit capabilities) and
  checkpoint fork/restore/commit as a fault-recovery candidate.

---

## Tracker

Status: **Open** (not yet worked through against these systems), **Deciding**
(actively being worked, not yet landed), **Decided** (landed, reasoning recorded
elsewhere — link/pointer goes in Notes).

### File-side

*Layer: primitives.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Core primitive (inode-equivalent) | inode, type-as-property | Untyped→retype into typed objects | 9P qid | — | Deciding | `docket-research.md`; being redone/made concrete this pass |
| Regular file | regular file | Frame (mapped memory) — not yet checked | file (uniform) | — | Deciding | `cargo-research.md` |
| Directory | directory | — | directory (same file type as regular, no split) | — | Deciding | `book-research.md` |
| Device | block/char device | — | device files served over 9P | — | Deciding | `vessel-research.md`. Reopened 2026-07-08 — "not a separate primitive from Vessel" was called before the agent-lit tier existed to check against; redoing that call from scratch |
| Symlink | pure naming, no rights of its own | — | union mounts/binds (different mechanism) | — | Open | Armada-sharing idea raised; naming (mount) vs. granting (capability) are two separate acts, not one — needs resolving explicitly |
| FIFO/socket | pipe, socket | Endpoint, Notification | pipe device | message passing | Open | The Unix-primitive mapping only; Hail itself promoted to its own Connection section below (2026-07-09) now that Fleet coherence hangs off it |
| Hard links (one inode, many names) | real, distinct from symlink | — | — | — | Open | Untouched entirely — does a Docket ever have >1 name? |
| Mount table / VFS / namespace | single global mount table | — | per-process private mounts, 9P remote mounting | — | Open | The genuinely-new thread from 2026-07-08; Plan 9 is the load-bearing precedent here, not Unix |
| Naming/discovery (memorable names vs. raw minted UUID) | dirent (name → inode within a directory) | no naming layer — capabilities aren't globally named | directory entry via 9P, same shape as Unix | registered names (atom → PID), separate from raw PID | Open | Reopened 2026-07-08 — previously marked "not a gap" (Already Book's contents-list, no new mechanism needed) before the agent-lit tier existed to check against; redoing from scratch |
| Content mutation over time (edit/retract/purge) | none — overwritten in place, no retained history | — | Venti/Fossa — content-addressed immutable blocks, whole-fs snapshot archived nightly, old snapshots readable via `/n/dump/<date>` | — | Open | New 2026-07-09, surfaced outside this repo talking through interoperability.md's memory shape. A Docket's content changing over time is currently undesigned — does superseding a claim edit in place (loses the "the agent used to believe X" trail) or write a new immutable object and move a ref (git's model, added above)? Two operations worth keeping distinct rather than conflated: **retraction** (agent decides something's stale, common, cheap, should keep history) vs. **purge** (compliance/secret-leak, rare, deliberate, actual erasure). Plan 9's Venti is real shipped precedent for content-addressed immutable history but is a single-timeline archive, not branchable; git adds the missing piece (mutable refs, branching) but its merge model assumes unambiguous text diffs — reconciling two branches of belief state that both claim to be true and contradict is a semantic merge problem, not a textual one, and nothing here solves that yet. Feeds directly into `docket-research.md`'s shape question, not just this row |

### Identity / users

*Layer: primitives.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| uid-equivalent | uid, referenced by files and processes, not itself either | — | per-namespace identity | — | Deciding | Captain mapped to this; own research file not yet created |
| gid-equivalent / groups | gid, ambient, cheap | no ambient identity at all — ACLs don't map | — | — | Open | Collectives/shared-Captain case; real tension under a capability model, may dissolve into "N holders of one capability" instead of surviving as a named thing |

### First mate (schema)

*Layer: schemas. New section 2026-07-09 — the First mate never had its own rows
the way Docket got File-side. File-side defines the primitive; this section is
what gets built with it — a filesystem standard on top of the file. This is the
"design" bucket from [interoperability.md](interoperability.md)'s seam
classification: the one part of the system owed literal data equivalence across
any swap, so its shape is the standard's core deliverable. The substrate is
deliberately boring (agents already keep memory in plain files — that's why
this is standardizable at all); the genuinely new layer is permissions
travelling with the data, provenance, and write-back.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Memory schema (the unit of remembered fact, shaped) | — | — | — | — | Open | The Docket/Book/Cargo primitives specialized for identity; content-mutation row under File-side (git object model, Venti) feeds this directly |
| Retrieval / embedding engine | — | — | — | — | Open | Likely "deliberately leave open" — engines compete, but what an engine must accept/return has to be in the schema; not walked yet |
| Persona format (voice, judgment, character) | — | — | — | — | Open | vision.md's character layer; nothing checked yet |
| Policy language (what's allowed, expressed portably) | — | — | — | — | Open | Must round-trip with the enforcement layer's capability model; Trust/enforcement rows are the other half of this |
| Budget representation | — | — | — | — | Open | Agent libOS's `AgentProcess` carries budgets — only candidate seen so far |
| Skill format | — | — | — | — | Deciding | Candidate: adopt SKILL.md / agentskills.io outright, per interoperability.md — walk it once before marking Decided |
| Signing / provenance | — | — | — | — | Open | Who wrote this memory, under what authority; OpenFang's Ed25519-signed manifests are one shipped precedent to check |
| Write-back path (session learning lands in the shared shape) | — | — | — | — | Open | The rule that makes portability real — a summary computed by any harness lands in memory's shape, never a private buffer; incremental-vs-session-end still open (see interoperability.md) |

### Process / invocation

Scope landed 2026-07-08: the actual agent runtime — tool execution, LLM
orchestration, the reasoning loop itself — is likely to come from an existing
substrate (OpenClaw, Hermes, or similar), not be built from scratch here.
Mojo isn't designing the agent; it's designing what the agent lives inside —
the trust boundary and supervision contract Mojo wraps around an arbitrary
agent process, and how that stays coherent across every device someone owns.
This section's rows are about that wrapper, not the runtime's internals — the
rows below (and their agent-lit notes) stay useful as precedent for the
wrapper's shape, not as a spec for the runtime Mojo would otherwise build.

*Layer: contracts. The 2026-07-09 anatomy work
([interoperability.md](interoperability.md)) confirms the scope call above and
names its treatment: the loop's internals are "deliberately leave open" — the
market's problem on purpose — and the wrapper is "design." The four rows added
2026-07-09 are the boundary between those two: each one is a place where either
the wrapper needs a contract, or the spec needs to record explicitly that the
inside is left open.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Process/execution unit | task_struct, PID, fork/exec/wait | no process at all — TCB+CSpace+VSpace composed by userspace | per-process namespace, but process itself is Unix-shaped | lightweight isolated process, message-passing only | Open | Already ruled not process-shaped; Erlang likely the real precedent, not Unix or seL4. Agent libOS's `AgentProcess` (state + children + budgets + checkpoints + explicit capabilities) is a concrete candidate shape worth checking against |
| Supervision / fault recovery | parent `wait()`s, exit status | fault delivered as IPC to a handler thread | — | supervisor trees, "let it crash" | Open | Candidate gap — what happens when an invocation fails mid-task. Agent libOS's checkpoint fork/restore/commit is a candidate mechanism, closer to this than Unix `wait()` — thin evaluation, one person, not proven at scale |
| Scheduling/priority | scheduler, nice/priority | scheduler is a kernel mechanism, policy set via TCB cap invocation | — | BEAM's own preemptive scheduler | Open | Untouched; may not matter until there's real contention for a Vessel. AIOS's context-interrupt (snapshot/resume mid-generation, like a context switch for a long LLM call) is real working precedent, not just a paper claim — worth checking closely when this row is picked up |
| Planning strategy | — | — | — | — | Open | New 2026-07-09. Likely lands "deliberately leave open" — harness-internal, the market's problem — but that's a decision to record after walking it, not a reason to skip the row |
| Context-management strategy | — | — | — | — | Open | New 2026-07-09. The algorithm is harness-specific on purpose; what the wrapper owns is the write-back obligation (First mate section's write-back row is the other half). Incremental vs. session-end write-back still open |
| Error/retry policy | — | — | — | — | Open | New 2026-07-09. Distinct from supervision above: supervision is what the wrapper does when the loop dies; this is what the loop is allowed/expected to do about its own failures before that — where the contract line sits is exactly the open question |
| Harness/shell boundary | exec boundary, argv/env | — | — | — | Open | New 2026-07-09. The seam itself: what a compliant harness is handed at invocation (identity fragment, capabilities, task) and what it must give back — the contract that makes harnesses swappable at all |

### Trust / enforcement

*Layer: contracts. Per [interoperability.md](interoperability.md) (2026-07-09):
this is the kernel — singular per Fleet as an instance, but the contract itself
goes in the standard so better kernels can replace Mojo's reference. It
mediates every consequential access, issues/derives/revokes capabilities, and
enforces that write-back conforms to the First mate's shape; the First mate is
the protected resource, not the kernel itself.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Access control model | ambient authority (uid/rwx, checked at open time, not revocable after) | capability (holding the token is the authority, no ambient identity) | per-process namespace as implicit scoping | — | Deciding | Direction picked: seL4-flavored, not Unix DAC. Mechanism not yet designed. Landed 2026-07-08: what's shown to an invocation should scope to currently-held capability (Plan 9 namespace-style), not a full table gated at call time; AgenticOS confirms capability + intent-check are complementary layers, not competing ones — still a principle, not a mechanism. Capsicum and WASI added 2026-07-08 — likely the real precedent here over seL4, since both are capability security on real userspace-on-Unix systems, not a clean-slate kernel; check closely when this row is picked up |
| Revocation | none — can't claw back an open fd | Capability Derivation Tree + `Revoke`, kills a whole derived subtree | — | supervisor can kill a whole subtree of processes | Open | Real candidate mechanism (CDT), not yet designed for Mojo. Agent libOS keeps capabilities separate from tool access and centrally mediated, consistent with this direction, but doesn't detail a CDT-equivalent revoke. Capsicum has no revoke primitive either (fd-scoped, close it or don't hand it out again) — check whether that's actually sufficient for Mojo or whether CDT-style subtree revoke is load-bearing |
| Kernel/syscall boundary as arbiter | large syscall table, kernel enforces per-call | ~3 real syscalls, everything else is object-method invocation through Send/Recv | — | — | Open | This is the trust-enforcement layer itself — separate primitive from invocation, not the same thing (corrected this session). Landed 2026-07-08: undeclared capabilities should be unreachable by construction, not just runtime-gated (AgenticOS's compile-time-absence technique) — Capsicum's capability-mode entry (global namespace gone, not just checked) and WASI's no-ambient-access-at-all model are real shipping precedent for the same idea, check these first |
| Root of trust / initial capability | root account exists at install | root task gets all resources at boot | — | — | Open | Candidate gap — where does Captain/Vessel #1's very first credential come from; Commission is named but the mechanism isn't designed. WASI's instantiation-time handle-passing (host explicitly hands the guest its starting capabilities, nothing ambient) is worth checking here too |

### Connection (Hail)

*Layer: contracts. Promoted out of a single buried File-side row 2026-07-09 —
Fleet coherence (one First mate behaving as one identity across every Vessel)
hangs off this seam, which the old placement didn't reflect. The live channel
is seam-only and reuses existing transport (Tailscale/WireGuard); the roster of
who's reachable is memory-shaped and lives in the First mate's shape, not here.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Addressing (naming something reachable on another Vessel) | sockets, host:port — outside the file namespace | — | 9P — remote resources through the same namespace as local | registered names across nodes, location-transparent | Open | Plan 9 is the load-bearing precedent; ties into File-side's mount-table/namespace row |
| Transport / live channel | sockets | Endpoints (sync), Notifications (async) | 9P over any transport | message passing, distribution protocol | Open | Seam-only, nothing persists; reuse Tailscale/WireGuard underneath, per the excluded-list note on raw networking |
| Capability handoff over the wire | fd-passing over Unix domain sockets | capability transfer via IPC | — | — | Open | "Hand another Vessel a capability" — Zircon's channels (capabilities passed in messages) flagged as the precedent to check; also Mach ports if this turns port-shaped |
| Fleet coherence / consistency | — | — | — | — | Open | What "one identity across N machines" actually requires of the seam — ordering, conflict, partition behavior. Untouched; the hardest row here and the reason the section exists |

### Routing / dispatch

*Layer: contracts. New section 2026-07-09 — previously existed only as an
unstructured aside in [roadmap.md](roadmap.md)'s Horizon (the escalation
policy). Userspace, swappable, explicitly not the kernel — but the contract has
to be in the standard: what goes in is a task description, the roster of
available compliant pieces (Vessels, harnesses, models), and
constraints/budget; what comes out is a selection. Includes the
mercenary/charter escalation call — hard limits like "no mercenaries, ever" are
policy data the router obeys, not properties of the router.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Routing contract (task + roster + constraints in, selection out) | — | — | — | — | Open | The seam itself; routers compete on selection quality behind it. Doing the call by hand first; a trained router is a later implementation detail, not part of the contract |

### Shell / environment

*Layer: contracts.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Uniform I/O (stdin/stdout/stderr-equivalent) | universal streams | — | — | message passing | Open | Candidate gap — how does an invocation receive input / emit output uniformly |
| Tool/command discovery (PATH-equivalent) | PATH, command resolution | — | namespace-based binding | — | Open | Candidate gap — may just be skill/tool discovery, unmapped so far. AIOS's load-by-name tool manager confirms the shape but isn't novel — hashmap lookup, nothing to take beyond that |

### Time

*Layer: schema + contract — the standing instruction is just a memory object in
the First mate's shape; the trigger seam reuses cron/systemd rather than being
invented.*

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Scheduled/proactive trigger (cron-equivalent) | cron/at | — | — | timers | Open | Candidate gap — "always-on Jarvis" implies proactive behavior, not just reactive-to-request |

### Explicitly excluded — inherited from the real OS underneath

Memory management, device driver internals, the raw networking stack (Tailscale/
WireGuard), boot/init. Listed here so the tracker stays honestly complete rather
than silently missing them, not because they need Mojo-side design.

---

## Naming

The eventual formal spec is called the **Mojo System Interface** — a direct,
honest echo of POSIX's own full name (Portable Operating System Interface), not
a claim of literal POSIX compliance. Plain functional register, not themed,
matching `naming-conventions.md`'s own rule for policy mechanisms. Deliberately
not "MojOS Standard" — that would collide the abstract policy with MojOS, the
concrete NixOS product; the two need separate identities the same way Docket
(the abstract primitive) and mojos (the repo) already do. Chosen 2026-07-08
under explicit delegation ("do what you think"), not independently re-litigated
— flag it if it's wrong.

## Not yet decided, deliberately

- Exactly where the Mojo System Interface document itself lives (own file at
  root, once it exists — not `research-plan.md`, which stays the tracker, not
  the spec).
- What implementation looks like — repos, services, SDKs. Blocked on the system
  actually being defined first, not on this file.
