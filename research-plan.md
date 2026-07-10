# Research Plan & System Tracker

*How the Mojo System Interface, Mk1, gets worked out, and the living status of
doing it. Distinct from [devlog.md](devlog.md) (the raw, messy, dated reasoning
trail), `dockets-research/*.md` (per-primitive cleaned findings, earned once real
content accumulates), and `msi.md` (the draft standard itself, once its first
section lands — it doesn't exist yet, on purpose). Nothing in this file is true by
virtue of being written down — the Status column is the only thing that means
anything, and most of it says Open. Nothing moves from here into msi.md until it's
confirmed, same discipline as everywhere else in this repo.*

---

## The target

This plan terminates in **msi.md** — the Mojo System Interface, Mk1, one file at
the repo root until it outgrows that. Its structure is fixed now so the tracker
below can map to it one-to-one:

- **§0 Conformance** — RFC-2119 language (MUST/SHOULD/MAY); compliance profiles,
  one per swappable piece: Memory Provider, Kernel, Harness, Client, Router,
  Model endpoint. A piece is compliant against a named profile, never against the
  whole document — that's what makes "hotswappable pieces that are all
  implementations of the standard" testable rather than rhetorical. Written
  during the assembly pass, out of the landed rows, not guessed up front.
- **§1 Base definitions** — vocabulary and primitives: Docket and its types
  (Book, Cargo, Vessel), Captain. ← Part A below.
- **§2 Schemas** — the First mate: the memory shape, permissions travelling with
  the data, provenance, write-back. ← Part B.
- **§3 Contracts** — the seams: enforcement (kernel), the invocation wrapper,
  Hail, reachability, time, routing. ← Part C.
- **§4 Adopted standards** — binding notes for what's consumed rather than
  designed: model APIs, MCP, SKILL.md, A2A. ← Part D.
- **Appendix: Rationale** — non-normative, the POSIX habit worth copying: why
  each decision landed where it did, distilled from `dockets-research/` and the
  devlog.

Tracker Part ↔ spec section is one-to-one on purpose: when every row in a Part
has a landed answer, that section of msi.md exists. Tracker covered = draft
assembled.

## The plan

Walk every piece, one at a time. For each piece, check what real,
already-existing systems did — not to copy one wholesale, but so a decision is
made against actual precedent instead of invented from nothing. Record the
decision and where the reasoning lives, and draft it into msi.md while it's
fresh.

**Posture, corrected 2026-07-10: adopt-or-adapt applies everywhere — including
the kernel and the memory.** Earlier framing said those two seams had "nothing to
adopt," which was overstated. What's true is that no accepted *standard* exists
there. Prior art does — kernel side: Capsicum, WASI, Landlock, kernel.chat's
`agent-os`, OpenFang's capability gates; memory side: Portable Agent Memory,
Engram, the W3C AI Agent Memory Interoperability CG — and every one of those gets
checked adapt-before-build when its row is walked. Mojo designs the residue: what
rows in the tracker actually leave standing after honest checking, not whole pieces claimed
by default.

**Two separate claims, not one, about what the spec is:**

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

**Three layers, the POSIX way (landed 2026-07-09, see
[interoperability.md](interoperability.md)):** primitives that hold
(Docket/Book/Cargo — Part A's job), schemas describing the shape of what's built
from the primitives (the First mate is the flagship schema — a filesystem
standard built on top of the file, Part B), and contracts describing the seams
between parts (Part C). Every row also carries a **Treatment** — adopt, design,
or deliberately leave open — and a row whose honest answer is "adopt X" or
"leave open" still gets walked before its Status moves: those are decisions too.

**Working rules — the Linus lesson (landed 2026-07-10, see devlog).** POSIX
codified fifteen years of already-running Unix; OSI wrote its spec first and lost
to TCP/IP's running code. Mk1 is written to be built against immediately, so:

- **Timebox rule.** A row that resists landing after one real session gets a
  provisional answer recorded plus an explicit open-question flag — never an
  indefinite Deciding. Mk1 is complete-coverage-thin on purpose; depth comes from
  versioned iteration once something runs.
- **Definition of done, per row.** The decision is written well enough that a
  stranger could build or adopt a compliant piece without ever talking to me.
  Anything less isn't Decided.
- **Draft as you go.** When a section's rows land, its msi.md section gets
  drafted the same session, while the reasoning is fresh. The assembly pass at
  the end is a coherence read, not a writing project.
- **Nothing already thought is decided.** The dockets-research files, every
  "direction picked" note, and every Treatment lean in the tracker are *inputs*
  to the walk, not outcomes of it. A row walked honestly can overturn any of
  them — the Docket shape included. Status is the only truth, and only Decided
  means decided.

**The walk — nine legs, dependency-ordered, each one a study-then-decide pass.**
Every leg is real time spent, not a checkbox: read the named precedent properly
first (that's the training — each leg teaches exactly what its decisions need),
then land the rows, then draft the msi.md text the same session. A leg is one or
more sessions; the timebox rule applies per row, not per leg. Session pattern:
each leg opens with a briefing on that leg's named precedents — what they are,
how they work, what they got right and wrong — before any deciding starts; the
study material is already named per leg below, no separate reading list needed.

**Currently on: leg 1 (adopted seams) — not yet started.** *(Updated in place as
legs complete — the quick answer to "where was I" after a gap; the Status column
stays the real truth.)*

1. **Adopted seams (Part D).** The question: what exactly does adopting MCP,
   SKILL.md, A2A, and the model APIs bind Mojo to? Study: each spec's actual
   documents — what version gets pinned, what surface a compliant piece must
   implement. Cheap, mostly confirmation, and it puts the first real section
   (§4) into msi.md — the momentum leg. One flag carried forward: how A2A
   composes with Hail gets revisited in leg 7.
2. **Captain (Identity).** The question: what is a Captain, minimally — the
   thing every Docket's ownership field will point at? Study: what a Unix uid
   actually is mechanically (referenced by files and processes, itself neither),
   Plan 9's per-namespace identity. Small on purpose, and it comes before
   File-side so ownership fields have something defined to point to. The gid row
   may dissolve into the capability layer — a provisional answer here is fine.
3. **File-side (the primitives).** The long leg. The question: what is a Docket,
   concretely — identity, types, naming, namespace, history over time? Study:
   inode/dirent/VFS mechanics for real, Plan 9's per-process namespaces and 9P,
   git's object model, Venti. The dockets-research files are input to this pass,
   not its outcome — every earlier call gets re-derived, not inherited.
   Permission-shaped holes (symlink's naming-vs-granting, the Billet split, the
   permission ceiling) are left open *deliberately* and flagged — leg 4 exists
   to close them. Drafts §1.
4. **Trust/enforcement (the kernel contract).** The question: what does
   authority look like as data, and how is it enforced from ordinary userspace
   on a real POSIX system? Study: seL4's capability model and CDT properly —
   this is the conceptual training the rest of the standard leans on — then the
   portable-enforcement candidates (WASI's handed-capabilities model, Landlock,
   the UCAN/Biscuit/macaroon token family), then source-level checks on
   kernel.chat and OpenFang. Re-closes every hole leg 3 flagged. Hardest
   conceptual leg, scheduled early for exactly that reason.
5. **First mate schema (Part B).** The question: what is a remembered fact,
   shaped — with the permissions travelling in it? Study: Portable Agent Memory,
   Engram, the W3C CG drafts, checked against the primitives and capability
   model now in hand from legs 3–4. The standard's core deliverable; the
   reused-shape catalog gets verified object by object here. Drafts §2.
6. **Process/invocation (the wrapper).** The question: what is a compliant
   harness handed at invocation, and what must it give back? Study: Erlang/OTP
   supervision for real (the likely process model), Agent libOS's
   `AgentProcess`, the exec/argv/env analogy. Depends on 4 (what's handed is
   capabilities) and 5 (the identity fragment's shape).
7. **Connection (Hail).** The question: what does "one identity across N
   machines" actually require of the seam — ordering, conflict, partition?
   Study: 9P, Erlang distribution, Zircon's capability-passing channels, and the
   CRDT-vs-git merge question honestly. Revisits leg 1's A2A composition flag.
8. **Reachability + Time.** The question: what must a window into the First
   mate implement, and how does a standing instruction fire and reach the
   Captain? Study: Matrix/XMPP's one-identity-many-clients model, cron/systemd
   timers, RRULE. Depends on 5 (what a client is handed is fragment-shaped) and
   7 (windows reach the First mate over Hail).
9. **Routing/dispatch.** The question: task + roster + constraints in, selection
   out — what's the contract? Shortest leg, last on purpose: everything it
   routes between now has a defined shape. Drafts the last piece of §3.

Then the **assembly pass**: write §0's conformance profiles out of what actually
landed, run a cross-row contradiction check, read msi.md end to end once for
coherence — and the Mk1 draft is done. That's this phase's finish line;
[roadmap.md](roadmap.md)'s Next (the first system, released runnable) starts
there.

This order is for attention, not for what Mk1 needs — Mk1 needs a first-pass
answer in every row below. A working system needs some answer for every piece to
run at all, even a rough one.

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

The four fixed precedent columns appear only in Part A, where OS-level precedent
is the whole point. Parts B–D carry a Precedent candidates column instead — their
real precedent is a mix of these systems, the secondary list, and the agent-era
prior art, and four mostly-empty columns were hiding that.

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
  **Caveat, 2026-07-10: FreeBSD-only.** No other POSIX system ships it (the
  Linux port died years ago), so it can never be the adopted mechanism — it's
  precedent for the *shape* of the answer, nothing more. Consequence for the
  design: the portable enforcement mechanism has to come from what an
  invocation is handed (WASI-style — what you weren't given isn't there), with
  per-OS sandboxes (Landlock on Linux, Capsicum on FreeBSD, macOS's sandbox)
  as optional hardening backends underneath; the contract can't depend on any
  one of them existing.
- **WASI / Wasmtime capabilities** — the component model's capability-based
  host functions: no ambient filesystem or network access at all, only
  pre-opened directories/handles explicitly passed in at instantiation.
  Added 2026-07-08, same reasoning as Capsicum — a real, currently-shipping
  system built on exactly Mojo's own constraint (sandboxed, capability-scoped,
  runs as ordinary userspace software), not a research artifact.
- **Landlock** — Linux's own unprivileged sandboxing LSM: a process voluntarily
  restricts its own filesystem/network access, no root required, shipping in
  mainline kernels. Added 2026-07-10, same family as Capsicum/WASI — capability
  discipline on the exact platform Mojo's runtime contract targets. Unverified
  depth; check when the enforcement rows are walked.
- **Capability token systems — UCAN, Biscuit, macaroons** — portable, signed,
  attenuable authority tokens designed to be verified offline and delegated
  with restrictions attached. Added 2026-07-10, unverified: candidate precedent
  for the policy language (permissions travelling *with* the data) and for
  capability handoff over the wire, which no OS-level precedent covers in
  portable-document form.
- **CRDTs (Automerge, Yjs)** — merge-without-coordination data structures,
  shipping and production-real. Added 2026-07-10, unverified: candidate
  precedent for Fleet coherence, alongside git (explicit-merge, human-visible)
  rather than replacing it — the two represent opposite answers to the same
  row.
- **Matrix / XMPP** — federated messaging's "one identity, many simultaneous
  clients/devices" model. Added 2026-07-10, unverified: the closest real
  precedent found for Reachability's client contract — every device a window
  into the same identity, none of them *being* the identity.
- **C2PA / Sigstore** — shipping provenance standards (content credentials;
  keyless signing + transparency logs). Added 2026-07-10, unverified: candidate
  precedent for the signing/provenance row beyond raw Ed25519 signatures.
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

Status: **Open** (not yet worked through against precedent), **Deciding**
(actively being worked, not yet landed), **Decided** (landed, reasoning recorded
elsewhere, drafted into msi.md — link/pointer goes in Notes).

Treatment: **Adopt** (consume something that already exists), **Design** (Mojo's
own work), **Leave open** (deliberately unstandardized — the market's problem on
purpose). A trailing **?** marks the current lean; it's confirmed only when the
row lands, and a row can change treatment when walking it proves the lean wrong.

## Part A — Primitives (→ msi.md §1)

### File-side

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Treatment | Status | Notes |
|---|---|---|---|---|---|---|---|
| Core primitive (inode-equivalent) | inode, type-as-property | Untyped→retype into typed objects | 9P qid | — | Design | Deciding | `docket-research.md`; being redone/made concrete this pass |
| Regular file | regular file | Frame (mapped memory) — not yet checked | file (uniform) | — | Design | Deciding | `cargo-research.md` |
| Directory | directory | — | directory (same file type as regular, no split) | — | Design | Deciding | `book-research.md` |
| Device | block/char device | — | device files served over 9P | — | Design | Deciding | `vessel-research.md`. Reopened 2026-07-08 — "not a separate primitive from Vessel" was called before the agent-lit tier existed to check against; redoing that call from scratch |
| Symlink | pure naming, no rights of its own | — | union mounts/binds (different mechanism) | — | Design | Open | Armada-sharing idea raised; naming (mount) vs. granting (capability) are two separate acts, not one — needs resolving explicitly |
| FIFO/socket | pipe, socket | Endpoint, Notification | pipe device | message passing | Design | Open | The Unix-primitive mapping only; Hail itself is its own Connection section below (2026-07-09) now that Fleet coherence hangs off it |
| Hard links (one inode, many names) | real, distinct from symlink | — | — | — | Design | Open | Untouched entirely — does a Docket ever have >1 name? |
| Mount table / VFS / namespace | single global mount table | — | per-process private mounts, 9P remote mounting | — | Design | Open | The genuinely-new thread from 2026-07-08; Plan 9 is the load-bearing precedent here, not Unix |
| Naming/discovery (memorable names vs. raw minted UUID) | dirent (name → inode within a directory) | no naming layer — capabilities aren't globally named | directory entry via 9P, same shape as Unix | registered names (atom → PID), separate from raw PID | Design | Open | Reopened 2026-07-08 — previously marked "not a gap" (Already Book's contents-list, no new mechanism needed) before the agent-lit tier existed to check against; redoing from scratch |
| Content mutation over time (edit/retract/purge) | none — overwritten in place, no retained history | — | Venti/Fossa — content-addressed immutable blocks, whole-fs snapshot archived nightly, old snapshots readable via `/n/dump/<date>` | — | Design | Open | New 2026-07-09, surfaced outside this repo talking through interoperability.md's memory shape. A Docket's content changing over time is currently undesigned — does superseding a claim edit in place (loses the "the agent used to believe X" trail) or write a new immutable object and move a ref (git's model, added above)? Two operations worth keeping distinct rather than conflated: **retraction** (agent decides something's stale, common, cheap, should keep history) vs. **purge** (compliance/secret-leak, rare, deliberate, actual erasure). Plan 9's Venti is real shipped precedent for content-addressed immutable history but is a single-timeline archive, not branchable; git adds the missing piece (mutable refs, branching) but its merge model assumes unambiguous text diffs — reconciling two branches of belief state that both claim to be true and contradict is a semantic merge problem, not a textual one, and nothing here solves that yet. Also this row's problem, added 2026-07-10: concurrent writes on a *single* Vessel — two simultaneous sessions writing back to the same First mate — not just cross-Vessel divergence, which is Fleet coherence's row. Feeds directly into `docket-research.md`'s shape question, not just this row |

### Identity / users

| Piece | Unix | seL4 | Plan 9 | Erlang/OTP | Treatment | Status | Notes |
|---|---|---|---|---|---|---|---|
| uid-equivalent | uid, referenced by files and processes, not itself either | — | per-namespace identity | — | Design | Deciding | Captain mapped to this; own research file not yet created |
| gid-equivalent / groups | gid, ambient, cheap | no ambient identity at all — ACLs don't map | — | — | Design? | Open | Collectives/shared-Captain case; real tension under a capability model, may dissolve into "N holders of one capability" instead of surviving as a named thing |

## Part B — First mate schema (→ msi.md §2)

*Layer: schemas. New section 2026-07-09 — the First mate never had its own rows
the way Docket got File-side. File-side defines the primitive; this section is
what gets built with it — a filesystem standard on top of the file. This is the
one part of the system owed literal data equivalence across any swap, so its
shape is the standard's core deliverable. The substrate is deliberately boring
(agents already keep memory in plain files — that's why this is standardizable at
all); the genuinely new layer is permissions travelling with the data,
provenance, and write-back.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Memory schema (the unit of remembered fact, shaped) | Portable Agent Memory (arXiv 2605.11032); Engram; W3C AI Agent Memory Interop CG; the plain-file conventions agents already use in the wild | Design | Open | The Docket/Book/Cargo primitives specialized for identity; content-mutation row under File-side (git object model, Venti) feeds this directly. The named candidates are real prior art to check adapt-before-build, not competitors to ignore — per the 2026-07-10 posture correction |
| Retrieval / embedding engine | — (engines compete behind the seam) | Leave open? | Open | Likely "deliberately leave open" — engines compete, but what an engine must accept/return has to be in the schema; not walked yet |
| Persona format (voice, judgment, character) | system-prompt conventions in the wild; nothing standardized found | Design | Open | vision.md's character layer; nothing checked yet |
| Policy language (what's allowed, expressed portably) | UCAN, Biscuit, macaroons (attenuable capability tokens — unverified, added 2026-07-10); Cedar | Design | Open | Must round-trip with the enforcement layer's capability model; Trust/enforcement rows are the other half of this |
| Budget representation | Agent libOS's `AgentProcess` carries budgets | Design | Open | Only candidate seen so far |
| Signing / provenance | Ed25519-signed manifests (OpenFang, shipped); C2PA; Sigstore | Design | Open | Who wrote this memory, under what authority |
| Scoped view / mission fragment | Decoy cargo / Manifest (own prior thinking, `cargo-research.md`'s trust-graded access); no external precedent found yet | Design | Open | New 2026-07-10. The *shape* of what a mercenary is handed — the minimum fragment with real values swapped for stand-ins, per vision.md. The shape lives here; who may mint one, and under what authority, is Trust/enforcement's business. Load-bearing for the whole mercenary model and previously had no row anywhere |
| Write-back path (session learning lands in the shared shape) | — | Design | Open | The rule that makes portability real — a summary computed by any harness lands in memory's shape, never a private buffer; incremental-vs-session-end still open (see interoperability.md) |
| Reused-shape catalog | — | Design | Open | New 2026-07-10. interoperability.md claims one shape does almost all the work — a scheduled trigger, a routing policy, the roster, a task's checkpointed state, an audit record are all "just memory objects." That claim gets verified here per object, not asserted: walk each one, confirm the shape actually holds it or record what it additionally needs |

## Part C — Contracts (→ msi.md §3)

### Trust / enforcement

*Layer: contracts. Per [interoperability.md](interoperability.md) (2026-07-09):
this is the kernel — singular per Fleet as an instance, but the contract itself
goes in the standard so better kernels can replace Mojo's reference. It
mediates every consequential access, issues/derives/revokes capabilities, and
enforces that write-back conforms to the First mate's shape; the First mate is
the protected resource, not the kernel itself.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Access control model | Unix: ambient authority (uid/rwx, checked at open time, not revocable after) — the documented road not taken; seL4: capability (holding the token is the authority, no ambient identity); Plan 9: per-process namespace as implicit scoping; Capsicum; WASI; Landlock (added 2026-07-10); kernel.chat `agent-os` (real ocap-token language, enterprise-scoped — added 2026-07-10); OpenFang capability gates (RBAC per its own docs — unverified) | Design | Deciding | Direction picked: seL4-flavored, not Unix DAC. Mechanism not yet designed. Landed 2026-07-08: what's shown to an invocation should scope to currently-held capability (Plan 9 namespace-style), not a full table gated at call time; AgenticOS confirms capability + intent-check are complementary layers, not competing ones — still a principle, not a mechanism. Capsicum and WASI added 2026-07-08 as capability security on real userspace-on-Unix systems; corrected 2026-07-10 — Capsicum is FreeBSD-only, so it's shape-precedent, not an adoptable mechanism: the portable direction is WASI-style handed-capabilities, with per-OS sandboxes (Landlock/Capsicum/macOS sandbox) as hardening backends, never the contract itself |
| Revocation | seL4: Capability Derivation Tree + `Revoke`, kills a whole derived subtree; Erlang: supervisor kills a whole subtree of processes; Unix: none — can't claw back an open fd | Design | Open | Real candidate mechanism (CDT), not yet designed for Mojo. Agent libOS keeps capabilities separate from tool access and centrally mediated, consistent with this direction, but doesn't detail a CDT-equivalent revoke. Capsicum has no revoke primitive either (fd-scoped, close it or don't hand it out again) — check whether that's actually sufficient for Mojo or whether CDT-style subtree revoke is load-bearing |
| Kernel/syscall boundary as arbiter | Unix: large syscall table, kernel enforces per-call; seL4: ~3 real syscalls, everything else is object-method invocation through Send/Recv; Capsicum capability mode; WASI no-ambient-access | Design | Open | This is the trust-enforcement layer itself — separate primitive from invocation, not the same thing (corrected 2026-07-08). Landed 2026-07-08: undeclared capabilities should be unreachable by construction, not just runtime-gated (AgenticOS's compile-time-absence technique) — Capsicum's capability-mode entry (global namespace gone, not just checked; FreeBSD-only, shape-precedent per the 2026-07-10 caveat) and WASI's no-ambient-access-at-all model are real shipping precedent for the same idea; WASI is the one that runs everywhere Mojo does, check it first |
| Root of trust / initial capability | Unix: root account exists at install; seL4: root task gets all resources at boot; WASI: instantiation-time handle-passing (host explicitly hands the guest its starting capabilities, nothing ambient) | Design | Open | Candidate gap — where does Captain/Vessel #1's very first credential come from; Commission is named but the mechanism isn't designed |
| Audit trail | Unix: syslog/auditd (ambient, best-effort); git log (append-only, content-addressed); kernel.chat's audit-trail pitch (its whole enterprise angle) | Design | Open | New 2026-07-10. The kernel mediates every consequential access — what it records about each mediation, and where. Accountability tracing back to a human is a vision.md commitment, and the record itself is presumably memory-shaped (Part B's reused-shape catalog verifies that). Previously implied everywhere, tracked nowhere |

### Process / invocation

Scope landed 2026-07-08: the actual agent runtime — tool execution, LLM
orchestration, the reasoning loop itself — comes from an existing substrate, not
built from scratch here. Mojo isn't designing the agent; it's designing what the
agent lives inside — the trust boundary and supervision contract Mojo wraps
around an arbitrary agent process, and how that stays coherent across every
device someone owns. This section's rows are about that wrapper, not the
runtime's internals — the rows below (and their agent-lit notes) stay useful as
precedent for the wrapper's shape, not as a spec for the runtime Mojo would
otherwise build.

*Layer: contracts. The loop's internals are "deliberately leave open" — the
market's problem on purpose — and the wrapper is "design." Several rows below
are the boundary between those two: each one is a place where either the wrapper
needs a contract, or the spec needs to record explicitly that the inside is left
open. Absorbed the old Shell/environment section 2026-07-10 (uniform I/O,
tool/command discovery) — their spec home is this contract, not a section of
their own.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Process/execution unit | Unix: task_struct, PID, fork/exec/wait; seL4: no process at all — TCB+CSpace+VSpace composed by userspace; Erlang: lightweight isolated process, message-passing only; Agent libOS `AgentProcess` | Design | Open | Already ruled not process-shaped; Erlang likely the real precedent, not Unix or seL4. Agent libOS's `AgentProcess` (state + children + budgets + checkpoints + explicit capabilities) is a concrete candidate shape worth checking against |
| Supervision / fault recovery | Unix: parent `wait()`s, exit status; seL4: fault delivered as IPC to a handler thread; Erlang: supervisor trees, "let it crash" | Design | Open | Candidate gap — what happens when an invocation fails mid-task. Agent libOS's checkpoint fork/restore/commit is a candidate mechanism, closer to this than Unix `wait()` — thin evaluation, one person, not proven at scale |
| Scheduling/priority | Unix: scheduler, nice/priority; seL4: scheduler is a kernel mechanism, policy set via TCB cap invocation; BEAM's own preemptive scheduler | Leave open? | Open | Untouched; may not matter until there's real contention for a Vessel. AIOS's context-interrupt (snapshot/resume mid-generation, like a context switch for a long LLM call) is real working precedent, not just a paper claim — worth checking closely when this row is picked up |
| Planning strategy | — | Leave open? | Open | New 2026-07-09. Likely lands "deliberately leave open" — harness-internal, the market's problem — but that's a decision to record after walking it, not a reason to skip the row |
| Context-management strategy | — | Leave open? | Open | New 2026-07-09. The algorithm is harness-specific on purpose; what the wrapper owns is the write-back obligation (Part B's write-back row is the other half). Incremental vs. session-end write-back still open |
| Error/retry policy | — | Design? | Open | New 2026-07-09. Distinct from supervision above: supervision is what the wrapper does when the loop dies; this is what the loop is allowed/expected to do about its own failures before that — where the contract line sits is exactly the open question |
| Harness/shell boundary | Unix: exec boundary, argv/env | Design | Open | New 2026-07-09. The seam itself: what a compliant harness is handed at invocation (identity fragment, capabilities, task) and what it must give back — the contract that makes harnesses swappable at all |
| Uniform I/O (stdin/stdout/stderr-equivalent) | Unix: universal streams; Erlang: message passing | Design | Open | Candidate gap — how does an invocation receive input / emit output uniformly. Moved from the old Shell/environment section 2026-07-10 |
| Tool/command discovery (PATH-equivalent) | Unix: PATH, command resolution; Plan 9: namespace-based binding; MCP's own tool-listing | Adopt? | Open | Candidate gap — may just be skill/tool discovery via MCP's listing, unmapped so far. AIOS's load-by-name tool manager confirms the shape but isn't novel — hashmap lookup, nothing to take beyond that. Moved from the old Shell/environment section 2026-07-10 |

### Connection (Hail)

*Layer: contracts. Promoted out of a single buried File-side row 2026-07-09 —
Fleet coherence (one First mate behaving as one identity across every Vessel)
hangs off this seam, which the old placement didn't reflect. The live channel
is seam-only and reuses existing transport (Tailscale/WireGuard); the roster of
who's reachable is memory-shaped and lives in the First mate's shape, not here.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Addressing (naming something reachable on another Vessel) | Unix: sockets, host:port — outside the file namespace; Plan 9: 9P — remote resources through the same namespace as local; Erlang: registered names across nodes, location-transparent | Design | Open | Plan 9 is the load-bearing precedent; ties into File-side's mount-table/namespace row |
| Transport / live channel | Unix: sockets; seL4: Endpoints (sync), Notifications (async); Plan 9: 9P over any transport; Erlang: distribution protocol; Tailscale/WireGuard underneath | Adopt? | Open | Seam-only, nothing persists; reuse Tailscale/WireGuard underneath, per the excluded-list note on raw networking |
| Capability handoff over the wire | Unix: fd-passing over Unix domain sockets; seL4: capability transfer via IPC; Zircon: channels pass capabilities in messages; Mach ports; UCAN-style token delegation (added 2026-07-10, unverified) | Design | Open | "Hand another Vessel a capability" — Zircon's channels flagged as the precedent to check; also Mach ports if this turns port-shaped |
| Fleet coherence / consistency | git (explicit merge, human-visible history); Syncthing; CRDTs — Automerge/Yjs (merge-without-coordination; added 2026-07-10, unverified) | Design | Open | What "one identity across N machines" actually requires of the seam — ordering, conflict, partition behavior. Untouched; the hardest row here and the reason the section exists. git and CRDTs are opposite answers to the same question — explicit reconciliation vs. automatic convergence — and the semantic-merge problem flagged in File-side's content-mutation row sits under both |

### Reachability

*Layer: contracts. New section 2026-07-10 — vision.md has always named
reachability as one of the seams a Jarvis-class system needs that an agent alone
never would, and interoperability.md's anatomy puts "how you get reached" in
userspace, but the tracker never had rows for it. The channels themselves are
userspace and competitive (multi-channel delivery is OpenClaw's entire edge —
real specialization worth hotswapping, not standardizing away); what the
standard owes is the client contract: what a window into the First mate must
implement, so that every device is a window into the same one, never a separate
instance.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Channel / client contract (what a window must implement) | Matrix / XMPP — one identity, many simultaneous clients, none of them *being* the identity (added 2026-07-10, unverified); OpenClaw's multi-channel delivery as the userspace layer this contract hosts | Design | Open | New 2026-07-10. vision.md test 1 ("every device is a window into the same one") depends on this seam and nothing tracked it. What a client is handed, what it may cache, what it must never hold authoritatively |
| Proactive delivery (how the system reaches the human) | ntfy / push-notification conventions; Matrix push gateways; plain email as the degenerate case | Design | Open | New 2026-07-10. The output half of Time's trigger row — a standing instruction firing is only useful if the result reaches the Captain somewhere. Which channel, under what urgency policy, is userspace; that a compliant client can receive an unsolicited delivery is the contract |

### Time

*Layer: schema + contract — the standing instruction is just a memory object in
the First mate's shape (Part B's reused-shape catalog verifies that); the
trigger seam reuses cron/systemd rather than being invented.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Scheduled/proactive trigger (cron-equivalent) | cron/at; systemd timers; iCalendar RRULE (recurrence grammar, added 2026-07-10); Erlang timers | Design | Open | Candidate gap — "always-on Jarvis" implies proactive behavior, not just reactive-to-request. Delivery of the result is Reachability's proactive-delivery row |

### Routing / dispatch

*Layer: contracts. Userspace, swappable, explicitly not the kernel — but the
contract has to be in the standard: what goes in is a task description, the
roster of available compliant pieces (Vessels, harnesses, models), and
constraints/budget; what comes out is a selection. Includes the
mercenary/charter escalation call — hard limits like "no mercenaries, ever" are
policy data the router obeys, not properties of the router.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Routing contract (task + roster + constraints in, selection out) | Slurm's scheduling concepts (resource matching, backfill — reference, not adoption, per ideas.md); model-router products in the wild | Design | Open | The seam itself; routers compete on selection quality behind it. Doing the call by hand first; a trained router is a later implementation detail, not part of the contract |

## Part D — Adopted seams (→ msi.md §4)

*Adoptions are decisions too. Each row here gets walked once against what
adopting it actually binds Mojo to — which version is pinned, what a compliant
piece must implement, what if anything Mojo layers on top — and the result is
recorded in §4 as a binding note. This is the lazy-correct posture from
[interoperability.md](interoperability.md), tracked instead of assumed: these
were the settled-adoption claims that previously had no rows at all, which meant
the adoption decisions themselves were never actually walked.*

| Piece | Precedent candidates | Treatment | Status | Notes |
|---|---|---|---|---|
| Model endpoint | OpenAI Chat Completions / Responses; Anthropic Messages API; local servers speaking the same shapes (llama.cpp, Ollama) | Adopt | Open | The most competitive, most agnostic layer that exists — what "compliant model provider" means, so the router can treat local and frontier endpoints as one roster. Mercenary handling is policy (Routing + Trust), not part of this binding |
| Tools | MCP (Model Context Protocol) | Adopt | Open | Multi-lab adoption, already does the job. The binding note records what Mojo assumes of an MCP server (and the kernel's mediation of tool calls sits above it — Trust's rows, not this one) |
| Skills | SKILL.md / agentskills.io | Adopt? | Deciding | Candidate: adopt outright, per interoperability.md — walk it once before marking Decided. Moved from Part B 2026-07-10: the format persists as part of the First mate, but the decision here is an adoption, same kind as MCP |
| Agent-to-agent | A2A (150+ orgs, real adoption) | Adopt | Open | For traffic between agents; how it composes with Hail (which is Vessel-to-Vessel, capability-mediated) is the one real question to walk |

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

- The exact granularity of §0's conformance profiles — written at the assembly
  pass from what actually landed, not guessed now. The six named profiles are
  the working set, not a commitment.
- Backup / disaster recovery — the all-vessels-lost case from
  `book-research.md`, and mojo-package as the portable snapshot. Deferred past
  Mk1 deliberately: it's an operational concern of a system, not a seam of the
  standard, and the shape being plain files means export is copying — but it's
  recorded here so the tracker isn't silently missing it, and if walking the
  First mate schema proves it *does* need a contract (a defined export
  envelope), it gets a row then.
- What implementation looks like — repos, services, SDKs. Blocked on the system
  actually being defined first, not on this file.
