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

**Sequencing, by dependency, not by the order pieces happened to get hit:**

1. File-side (Docket/Vessel/Book/Cargo) — in progress, being redone/made concrete
   through this pass, not done.
2. Identity/user (Captain) — small, mostly unblocks nothing else, close it out
   early.
3. Trust/capability/enforcement layer — high priority: Vessel, Book, and Cargo
   each already have real open threads (Vessel-role mechanism, permission
   ceiling, the Billet identity/role split) explicitly deferred to this layer.
   Designing it re-closes loose ends already sitting in three existing files.
4. Process/invocation (First mate) — depends on 3; what an invocation can do is
   defined by what the trust layer allows.
5. Connection (Hail/mesh) — depends on both 3 and 4; it's the channel invocations
   and other actors talk through, mediated by capabilities.

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

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Core primitive (inode-equivalent) | inode, type-as-property | Untyped→retype into typed objects | 9P qid | — | Deciding | `docket-research.md`; being redone/made concrete this pass |
| Regular file | regular file | Frame (mapped memory) — not yet checked | file (uniform) | — | Deciding | `cargo-research.md` |
| Directory | directory | — | directory (same file type as regular, no split) | — | Deciding | `book-research.md` |
| Device | block/char device | — | device files served over 9P | — | Deciding | `vessel-research.md`. Reopened 2026-07-08 — "not a separate primitive from Vessel" was called before the agent-lit tier existed to check against; redoing that call from scratch |
| Symlink | pure naming, no rights of its own | — | union mounts/binds (different mechanism) | — | Open | Armada-sharing idea raised; naming (mount) vs. granting (capability) are two separate acts, not one — needs resolving explicitly |
| FIFO/socket | pipe, socket | Endpoint, Notification | pipe device | message passing | Open | Tentatively = Hail, not designed |
| Hard links (one inode, many names) | real, distinct from symlink | — | — | — | Open | Untouched entirely — does a Docket ever have >1 name? |
| Mount table / VFS / namespace | single global mount table | — | per-process private mounts, 9P remote mounting | — | Open | The genuinely-new thread from 2026-07-08; Plan 9 is the load-bearing precedent here, not Unix |
| Naming/discovery (memorable names vs. raw minted UUID) | dirent (name → inode within a directory) | no naming layer — capabilities aren't globally named | directory entry via 9P, same shape as Unix | registered names (atom → PID), separate from raw PID | Open | Reopened 2026-07-08 — previously marked "not a gap" (Already Book's contents-list, no new mechanism needed) before the agent-lit tier existed to check against; redoing from scratch |
| Content mutation over time (edit/retract/purge) | none — overwritten in place, no retained history | — | Venti/Fossa — content-addressed immutable blocks, whole-fs snapshot archived nightly, old snapshots readable via `/n/dump/<date>` | — | Open | New 2026-07-09, surfaced outside this repo talking through interoperability.md's memory shape. A Docket's content changing over time is currently undesigned — does superseding a claim edit in place (loses the "the agent used to believe X" trail) or write a new immutable object and move a ref (git's model, added above)? Two operations worth keeping distinct rather than conflated: **retraction** (agent decides something's stale, common, cheap, should keep history) vs. **purge** (compliance/secret-leak, rare, deliberate, actual erasure). Plan 9's Venti is real shipped precedent for content-addressed immutable history but is a single-timeline archive, not branchable; git adds the missing piece (mutable refs, branching) but its merge model assumes unambiguous text diffs — reconciling two branches of belief state that both claim to be true and contradict is a semantic merge problem, not a textual one, and nothing here solves that yet. Feeds directly into `docket-research.md`'s shape question, not just this row |

### Identity / users

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| uid-equivalent | uid, referenced by files and processes, not itself either | — | per-namespace identity | — | Deciding | Captain mapped to this; own research file not yet created |
| gid-equivalent / groups | gid, ambient, cheap | no ambient identity at all — ACLs don't map | — | — | Open | Collectives/shared-Captain case; real tension under a capability model, may dissolve into "N holders of one capability" instead of surviving as a named thing |

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

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Process/execution unit | task_struct, PID, fork/exec/wait | no process at all — TCB+CSpace+VSpace composed by userspace | per-process namespace, but process itself is Unix-shaped | lightweight isolated process, message-passing only | Open | Already ruled not process-shaped; Erlang likely the real precedent, not Unix or seL4. Agent libOS's `AgentProcess` (state + children + budgets + checkpoints + explicit capabilities) is a concrete candidate shape worth checking against |
| Supervision / fault recovery | parent `wait()`s, exit status | fault delivered as IPC to a handler thread | — | supervisor trees, "let it crash" | Open | Candidate gap — what happens when an invocation fails mid-task. Agent libOS's checkpoint fork/restore/commit is a candidate mechanism, closer to this than Unix `wait()` — thin evaluation, one person, not proven at scale |
| Scheduling/priority | scheduler, nice/priority | scheduler is a kernel mechanism, policy set via TCB cap invocation | — | BEAM's own preemptive scheduler | Open | Untouched; may not matter until there's real contention for a Vessel. AIOS's context-interrupt (snapshot/resume mid-generation, like a context switch for a long LLM call) is real working precedent, not just a paper claim — worth checking closely when this row is picked up |

### Trust / enforcement

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Access control model | ambient authority (uid/rwx, checked at open time, not revocable after) | capability (holding the token is the authority, no ambient identity) | per-process namespace as implicit scoping | — | Deciding | Direction picked: seL4-flavored, not Unix DAC. Mechanism not yet designed. Landed 2026-07-08: what's shown to an invocation should scope to currently-held capability (Plan 9 namespace-style), not a full table gated at call time; AgenticOS confirms capability + intent-check are complementary layers, not competing ones — still a principle, not a mechanism. Capsicum and WASI added 2026-07-08 — likely the real precedent here over seL4, since both are capability security on real userspace-on-Unix systems, not a clean-slate kernel; check closely when this row is picked up |
| Revocation | none — can't claw back an open fd | Capability Derivation Tree + `Revoke`, kills a whole derived subtree | — | supervisor can kill a whole subtree of processes | Open | Real candidate mechanism (CDT), not yet designed for Mojo. Agent libOS keeps capabilities separate from tool access and centrally mediated, consistent with this direction, but doesn't detail a CDT-equivalent revoke. Capsicum has no revoke primitive either (fd-scoped, close it or don't hand it out again) — check whether that's actually sufficient for Mojo or whether CDT-style subtree revoke is load-bearing |
| Kernel/syscall boundary as arbiter | large syscall table, kernel enforces per-call | ~3 real syscalls, everything else is object-method invocation through Send/Recv | — | — | Open | This is the trust-enforcement layer itself — separate primitive from invocation, not the same thing (corrected this session). Landed 2026-07-08: undeclared capabilities should be unreachable by construction, not just runtime-gated (AgenticOS's compile-time-absence technique) — Capsicum's capability-mode entry (global namespace gone, not just checked) and WASI's no-ambient-access-at-all model are real shipping precedent for the same idea, check these first |
| Root of trust / initial capability | root account exists at install | root task gets all resources at boot | — | — | Open | Candidate gap — where does Captain/Vessel #1's very first credential come from; Commission is named but the mechanism isn't designed. WASI's instantiation-time handle-passing (host explicitly hands the guest its starting capabilities, nothing ambient) is worth checking here too |

### Shell / environment

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Uniform I/O (stdin/stdout/stderr-equivalent) | universal streams | — | — | message passing | Open | Candidate gap — how does an invocation receive input / emit output uniformly |
| Tool/command discovery (PATH-equivalent) | PATH, command resolution | — | namespace-based binding | — | Open | Candidate gap — may just be skill/tool discovery, unmapped so far. AIOS's load-by-name tool manager confirms the shape but isn't novel — hashmap lookup, nothing to take beyond that |

### Time

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
