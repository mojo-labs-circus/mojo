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
| Device | block/char device | — | device files served over 9P | — | Deciding | `vessel-research.md` |
| Symlink | pure naming, no rights of its own | — | union mounts/binds (different mechanism) | — | Open | Armada-sharing idea raised; naming (mount) vs. granting (capability) are two separate acts, not one — needs resolving explicitly |
| FIFO/socket | pipe, socket | Endpoint, Notification | pipe device | message passing | Open | Tentatively = Hail, not designed |
| Hard links (one inode, many names) | real, distinct from symlink | — | — | — | Open | Untouched entirely — does a Docket ever have >1 name? |
| Mount table / VFS / namespace | single global mount table | — | per-process private mounts, 9P remote mounting | — | Open | The genuinely-new thread from 2026-07-08; Plan 9 is the load-bearing precedent here, not Unix |

### Identity / users

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| uid-equivalent | uid, referenced by files and processes, not itself either | — | per-namespace identity | — | Deciding | Captain mapped to this; own research file not yet created |
| gid-equivalent / groups | gid, ambient, cheap | no ambient identity at all — ACLs don't map | — | — | Open | Collectives/shared-Captain case; real tension under a capability model, may dissolve into "N holders of one capability" instead of surviving as a named thing |

### Process / invocation

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Process/execution unit | task_struct, PID, fork/exec/wait | no process at all — TCB+CSpace+VSpace composed by userspace | per-process namespace, but process itself is Unix-shaped | lightweight isolated process, message-passing only | Open | Already ruled not process-shaped; Erlang likely the real precedent, not Unix or seL4 |
| Supervision / fault recovery | parent `wait()`s, exit status | fault delivered as IPC to a handler thread | — | supervisor trees, "let it crash" | Open | Candidate gap — what happens when an invocation fails mid-task |
| Scheduling/priority | scheduler, nice/priority | scheduler is a kernel mechanism, policy set via TCB cap invocation | — | BEAM's own preemptive scheduler | Open | Untouched; may not matter until there's real contention for a Vessel |

### Trust / enforcement

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Access control model | ambient authority (uid/rwx, checked at open time, not revocable after) | capability (holding the token is the authority, no ambient identity) | per-process namespace as implicit scoping | — | Deciding | Direction picked: seL4-flavored, not Unix DAC. Mechanism not yet designed |
| Revocation | none — can't claw back an open fd | Capability Derivation Tree + `Revoke`, kills a whole derived subtree | — | supervisor can kill a whole subtree of processes | Open | Real candidate mechanism (CDT), not yet designed for Mojo |
| Kernel/syscall boundary as arbiter | large syscall table, kernel enforces per-call | ~3 real syscalls, everything else is object-method invocation through Send/Recv | — | — | Open | This is the trust-enforcement layer itself — separate primitive from invocation, not the same thing (corrected this session) |
| Root of trust / initial capability | root account exists at install | root task gets all resources at boot | — | — | Open | Candidate gap — where does Captain/Vessel #1's very first credential come from; Commission is named but the mechanism isn't designed |

### Shell / environment

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Uniform I/O (stdin/stdout/stderr-equivalent) | universal streams | — | — | message passing | Open | Candidate gap — how does an invocation receive input / emit output uniformly |
| Tool/command discovery (PATH-equivalent) | PATH, command resolution | — | namespace-based binding | — | Open | Candidate gap — may just be skill/tool discovery, unmapped so far |

### Time

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Status | Notes |
|---|---|---|---|---|---|---|
| Scheduled/proactive trigger (cron-equivalent) | cron/at | — | — | timers | Open | Candidate gap — "always-on Jarvis" implies proactive behavior, not just reactive-to-request |

### Already checked, not gaps

| Piece | Resolution |
|---|---|
| Devices | Already Vessel (the device-type Docket) — not a separate primitive |
| Naming/discovery (memorable names vs. raw minted UUID) | Already Book's contents-list, same as a Unix dirent — no new mechanism needed |

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
