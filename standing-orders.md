# Standing Orders

*Mojo's own POSIX — the behavioral contract every vessel and fleet must
satisfy, independent of which OS, which hardware, which implementation
actually satisfies it. Different vessels (MojOS, bring-your-own-NixOS,
Foreign) each separately comply with this, the same way different filesystems
all satisfy one kernel interface without knowing about each other. This
document specifies what must be true; how is a separate, later decision — the
actual code lives in the mojos/mojo-agent repos, and may eventually need its
own concrete interface (a "Mojo VFS"), but that gets built only once there's a
real need to shape it, not designed speculatively here.

Named for the real naval term: standing orders are the general, permanent
instructions governing how a fleet operates, as opposed to a one-off specific
order — general policy for how any fleet is constituted, not one crew's own
particular agreement (which is what a pirate ship's Articles actually were,
historically — per-crew, not fleet-wide, and the wrong precedent for what this
document does).

Structure mirrors POSIX deliberately, not by coincidence: an abstract
behavioral contract, kept separate from any one implementation, with rationale
kept separate from the normative spec so it's never mistaken for a
requirement. Vessel is one machine, alone — what's true of it needs no fleet
in view. Fleet is what only exists once vessels are related to each other.
Collective (multiple fleets) builds on top of Fleet and is deliberately out of
scope until Fleet is solid; Shell & Utilities (actual operator commands) is
reserved for once there's a stable contract to build them against.

Numbers are stable identifiers, assigned once, never renumbered even if
content or section order changes later. Unnamed entries keep just their
number until a name is settled. Rationale — the real prior art each concept
maps onto — lives in its own section at the end, cross-referenced by number,
kept out of the normative text so it reads as grounding, not obligation. Full
reasoning trail for every decision here is in [devlog.md](devlog.md); this is
a first draft, expected to change as Fleet gets built and tested for real —
that history lives in git and devlog, not inside the document itself.*

---

## Identity

**1. Captain** — The human, owner and final authority over the fleet.

**2. First mate** — The AI itself — one continuous identity, not a separate instance per vessel.

---

## Vessel

*One machine, alone. Everything here is true of a vessel with no fleet in view — it doesn't need the rest of this document to mean anything.*

### General concepts

Properties split into four kinds, by what causes them to change:

- **Rooted** — established once, retired rather than transferred, never reassigned or re-derived.
- **Declared** — set by a deliberate action, stays until another deliberate action changes it.
- **Queried** — read fresh each time, because it drifts on its own without anyone deciding anything.
- **Derived** — never stored at all; computed on demand by whichever Fleet-level policy is asking.

A device isn't a Vessel (#11) in this model's terms until Fleet billets it —
raw hardware with no role assigned yet is outside the model, not a
vessel-in-waiting. All vessels here are one homogeneous kind of thing, varying
only in degree along the properties below, not in kind the way a Unix socket
differs from a regular file. Deliberate simplification, not an oversight — a
real future case (a sensor/peripheral that can't run mojo-agent at all) would
need a genuine `type` axis, but nothing in scope today needs it.

### Definitions

**Rooted**

**3. Keel** — A vessel's persistent identity, established once at first join,
surviving OS reinstalls and every billet reassignment. Retired, not
transferred, if the vessel is ever Removed (#21) — if hardware changes hands,
it re-joins fresh under its new owner, not carrying the old identity in.

**Declared**

**4. Ownership** — Owned, rented-sealed, or rented-open. Changed only by a
roster-level operation (Commission/Enlist/Remove-family), never drifts on its
own.

**5. Head or headless** — Whether a human directly interacts with this
vessel. An install-time choice (whether the desktop layer is present at all),
changed only by a deliberate reconfigure. Not forced by role except for
Charter (#16/#17), which is always headless — there's no physical seat on a
rented instance.

**Queried**

**6. OS posture** — MojOS (the full recommended bundle), bring-your-own-NixOS
(the mojo-agent module alone, imported into an existing flake), or Foreign
(Nix as package manager only, wrapped as that OS's native service). Read
fresh, not cached — a vessel can move between these without any roster event
occurring.

**7. Hardware inventory** — Raw compute/memory/GPU/storage specs, read live.
Not the same as Capability (#9) — the inventory is fact, capability is a
judgment computed from it.

**8. The local ledger** — A vessel's own append-only record of what it's
done: connectivity/uptime events, task-run history, and any writes pending
reconciliation because the log (#25) was unreachable when they happened.
Authored locally, read by others — the vessel is the source of truth for its
own history, nobody else independently tracks it.

**Derived**

**9. Capability** — Whether this vessel can be Flagship-eligible, hold a full
write-through copy of the log versus only an abridged one, etc. — computed
from Hardware inventory (#7) at decision time, never stored.

**10. Permission ceiling** — The most this vessel could ever be trusted with,
regardless of its current billet — computed from Ownership (#4) and, where it
applies, attestation status. A rented-open vessel can never legitimately hold
genuinely sensitive data no matter what role it's assigned; that ceiling is
fixed by what it is, not by what it's currently doing.

---

## Fleet

*What only exists once vessels are related to each other. Content held by a
vessel splits the same way: originated content (the local ledger, #8) is
standalone, authored by that vessel about itself; reflected content (below,
under Records) is never standalone — it's always a view onto something
shared, and only means anything in relation to it.*

### General concepts

Six functional parts: **Roster** (who's in the fleet and what role each
holds), **Permissions** (what each role is actually allowed to do, bounded by
each vessel's own ceiling), **Records** (the shapes shared data can take),
**Communication** (how vessels actually reach each other), **Accounting** (a
fleet-wide view built by collating what every vessel already recorded
locally, not a separately maintained ledger), **Policy** (how work actually
gets assigned and resourced).

A Fleet is exactly one Captain (#1) + one First mate (#2) + a roster of
vessels. Everything above Fleet — Collective, multiple fleets joining for
shared purpose — builds on top of this and is out of scope until this is
solid (see Reserved, below).

### Definitions

**Roster**

**11. Vessel** — A device once it holds a billet (#12). The generic term for
anything capable of running mojo-agent and currently doing so under some
role.

**12. The billet table** — The current, generational record of which vessel
holds which role. Generational like a NixOS system config: each change is a
new generation, previous ones remain inspectable and reversible, not a single
overwritten snapshot. Holds persistent entries (Flagship/Consort/Tender —
reassignable pointers over the owned pool) and ephemeral entries (Charter —
TTL-bound, created and destroyed with the rental). Mercenary holds no entry at
all.

**13. Flagship** — The billet held by exactly one vessel: the mandatory,
always-on anchor holding the canonical log (#25). Reassignment is
primary-replica promotion — pick a Consort already holding a full
write-through copy of the log, flip which one accepts writes, demote the old
one. Not forced headless — a Flagship can be sat at.

**14. Consort** — An owned vessel billeted to run independently, holding its
own copy of the log (full write-through or abridged, per its own Capability).

**15. Tender** — An owned vessel billeted with no independent capability — a
pure access surface into the AI running elsewhere.

**16. Sealed charter** — A temporary rented billet running
attested/confidential compute — the provider cannot see what ran on it,
provably.

**17. Open charter** — A temporary rented billet running plain, non-attested
infrastructure — the provider could see what ran on it.

**18. Mercenary** — A third-party frontier/proprietary model. Holds no
billet, no Keel, no roster entry — an external mount, referenced only by the
routing policy (#48) per call, never a member of the fleet.

**19. Commission** — Billets the very first vessel, once, when no
roster/log exists yet. Bootstraps #25/#12/#38 from nothing; the new vessel
becomes Flagship near-by-necessity, since nothing else exists yet to hold
that billet.

**20. Enlist** — Billets any vessel after the first. Always has an existing
Flagship to sync from — syncs the abridged log (#26), or a full copy capacity
permitting, rather than bootstrapping from empty. Billeted Consort or Tender
per eligibility and choice.

**21. Remove** — Drops a billet entry and revokes mesh membership. If the
vessel being removed holds Flagship, Switch flagship (#22) must complete
first — no valid state has zero Flagship, even momentarily.

**22. Switch flagship** — Reassigns the Flagship billet. Requires the
candidate already hold a full write-through copy of the log; if not, wait for
it to catch up first. Near-zero-downtime once the candidate qualifies.

**23. Chartering** — Allocates an ephemeral billet for a task's duration: a
fresh temporary vessel, not a repoint of an existing one. Joins the mesh as a
temporary member; teardown removes both the billet and mesh membership
automatically once the task ends.

**Permissions**

**24. Permission grant** — What a vessel is actually allowed to do right
now, given its current billet — bounded above by its own Permission ceiling
(#10), never exceeding it. Flagship alone may write the log (#25) directly;
Sealed charter may hold real sensitive data and Hail (#34) by default, Open
charter and Mercenary may not. At least one capability (Hailing) is a
per-instance override decided at charter time rather than a fixed fact of the
billet type — the general shape (deployment-time configuration of what a
given instance may do) is expected to generalize past this one case.

**Records**

**25. The log** — The single canonical, ever-accumulating record of the
user's memory/context/preferences — one per fleet, held wherever the Flagship
billet currently points. Single-writer by design — CAP theorem forces this:
with more than one accepted writer you need consensus or accept permanent
disagreement risk, so there's exactly one canonical writer instead.

**26. The abridged log** — A partial copy of #25, held by a Consort lacking
capacity for the full record, kept continuously synced by pull (#35), not
push.

**27. Beyond hailing range** — A vessel unable to reach the Flagship records
to its own local ledger (#8) instead of writing to #25 directly — merged back
in once contact is restored. Not a pure CRDT merge — real edits/deletions are
a deliberate feature (memory review/veto), so genuine conflicts are possible
and route to human review.

**28. The manifest** — A genuine, scoped-down subset of #25, prepared
upfront for a specific task. Which billet type it's sent to is decided by the
sensitivity judgment (#47), not fixed to any one tier.

**29. Decoy cargo** — Context with real values replaced by placeholder
values — no genuine sensitive data present, regardless of scope.

**30. The full hold** — The entirety of #25 transferred in full rather than
a subset. Only ever paired with Sealed charter (#16).

**Communication**

**31. The mesh** — The private, encrypted network connecting every vessel:
owned ones persistently, each Charter as a temporary member for the rental's
lifetime only. Mercenary never joins — reached over the open internet
instead. Not yet named.

**32. Delegate** — Hand a task (#39) and its context to another vessel.
Symmetric — any vessel to any vessel, not Flagship-exclusive.

**33. Report** — Return results to whoever delegated the task. Written to
the reporting vessel's own local ledger (#8), not a central store — the watch
bill (#38) collates these, it doesn't receive them directly.

**34. Hail** — A vessel holding a partial copy of the log queries the
Flagship for additional pieces mid-task, beyond what it started with.

**35. Sync** — Pull-based replication of the log into every abridged copy —
periodic while connected, immediate on reconnect. Matches the git-clone model
already governing #14/#27: nobody pushes into a clone, a consort pulls when
it wants fresh state.

**36. Muster-ping** — An on-demand liveness check — who's actually reachable
right now, distinct from who's a persistent roster member.

**37. Flare** — A payload-less broadcast: "something changed, pull now."
Distinct from the sustained exchange of #32–#36 — collapses the latency of
periodic pull for anyone currently connected, without the Flagship tracking
subscriber state. Correctness never depends on a flare arriving; periodic
Sync (#35) is the actual floor.

**Accounting**

**38. The watch bill** — Not a stored ledger — a fleet-wide view built by
collating every vessel's own local ledger (#8) at query time. Necessarily
partial when a vessel is currently unreachable, same tradeoff as #27.
Append-only by default, carrying the same human-review/redaction capability
as the log (#25), for the same reason: activity history can itself be
sensitive.

**Policy**

**39. Task** — A discrete unit of work the AI performs, resourced according
to one of the following.

**40. Single-device resourcing** — A task handled entirely by whichever
vessel the user is currently using.

**41. Flagship-directed resourcing** — A task directed to the Flagship for
its superior or more continuously-available compute.

**42. Combined-owned resourcing** — A task resourced by combining multiple
owned vessels' memory/compute, splitting one job across them.

**43. Distributed-owned resourcing** — A task resourced by distributing
several smaller jobs across multiple owned vessels in parallel, without
combining any single job's resources.

**44. Charter-resourced** — A task resourced by a Sealed or Open charter.
Context is a Manifest (#28) or Decoy cargo (#29) per the sensitivity judgment
(#47); genuinely sensitive data may only pair with Sealed charter. Sealed
charter may also Hail (#34).

**45. Mercenary-resourced** — A task requiring Mercenary (#18). Context is a
Manifest or Decoy cargo per #47; owned or charter compute remains preferred
regardless of sensitivity; never granted roster or record access.

**46. Checkpoint/resume** — A task periodically saves its own state; if
interrupted, it resumes from the last checkpoint elsewhere rather than
restarting or requiring true live migration. Same shape as #27, applied to
in-flight task state instead of log writes — deliberately the cheap answer,
not full live process migration, which stayed research-grade even in mature
prior art for good reason.

**47. The sensitivity judgment** — The decision of whether a task's context
is a Manifest (#28) or Decoy cargo (#29), based on how sensitive the data
actually is — independent of billet type, except genuinely sensitive data may
only pair with Sealed charter.

**48. The routing policy** — The policy deciding which of #40–#46 a given
Task (#39) receives.

---

## Deployable artifacts

*Concrete implementations of the OS posture axis (#6) — what a vessel
actually runs, tied to which of the three postures it's declared.*

**49. mojo-agent** — A reproducible, declarative package containing the AI's
full software/configuration alone, deployable identically onto any vessel.

**50. MojOS** — A NixOS-based system layer implementing the full recommended
posture: config changes are atomic/reversible generations, data changes are
per-action snapshots, anything undeclared is ephemeral by default.

**51. The bring-your-own-NixOS module** — mojo-agent (#49) packaged as a
plain NixOS module, importable into an existing flake without adopting the
rest of MojOS's opinionated bundle.

**52. The Foreign wrapper** — mojo-agent (#49) built via Nix as a package
manager only, wrapped as the host OS's own native service (systemd unit,
launchd plist, or equivalent).

**53. mojo-package** — The combination of #49, #25, #12, and #38 — deployed
once, only by Commission (#19), onto a brand-new Flagship. Not any vessel's
steady state, which is just mojo-agent running with the log/roster/ledger
already resident.

---

## Reserved: Collective

Multiple sovereign fleets, each still one Captain and one First mate, joining
for shared purpose without any fleet losing individual ownership —
recursive, a Collective's members may be Fleets, other Collectives, or
standalone task-scoped agents belonging to no single Fleet. Deliberately not
built out here — everything above depends on Fleet being solid first. See
[ideas.md](ideas.md) for the standing thinking.

## Reserved: Shell & Utilities

The actual operator-facing commands for running a Mojo-compliant system. Not
started — comes after the contract above is stable enough to build tools
against.

---

## Rationale

*Real prior art each concept maps onto. Informative, not normative — nothing
here is a requirement, only grounding for why a definition takes the shape it
does.*

**1.** Root of trust — a PKI root certificate (self-signed, structurally
trusted, not authenticated by anything else), not Unix root/superuser. No
login/account concept needed — exactly one Captain per fleet by design, the
same reason Flagship is singular, an accountability invariant that's also the
real boundary where Collective picks up rather than a gap in this model.

**2.** Process migration, from distributed OS research (Plan 9/Sprite/Amoeba
lineage) — one logical identity that keeps its identity across physical
relocation. Modern concrete version: Erlang/Elixir's actor model, a
location-transparent address regardless of which physical node executes it.

**3.** An inode number — stable across renames and directory moves, distinct
from the path currently pointing at it. Concretely, this is what a Tailscale
node's identity already has to be — a keypair, not a hostname/IP — for the
mesh to survive a reinstall or rename at all. Keel doesn't invent this
mechanism, it names what already has to exist.

**4.** The standard vendor-trust ladder used in any real security model:
fully controlled infrastructure, contracted-but-external, and adversarial
third party — the same three-tier shape independent of the specific domain.

**5.** On MojOS hosts, a NixOS module import toggle — presence or absence of
the desktop-layer module over an identical base system. On Foreign hosts,
just whether the wrapper autostarts a UI/tray component or runs silently.

**6.** Nix (the package manager, distinct from NixOS) already runs
identically on NixOS, other Linux distros, and macOS — the same reason POSIX
itself can be satisfied by wildly different kernels underneath.

**7.** `/proc/cpuinfo`, `/proc/meminfo` — read live, not cached, real Linux
precedent for exactly this kind of hardware query.

**8.** Unix `wtmp`/`utmp`, which already mixes boot events and session
events in one local log format, generalized into a write-ahead log.
Concretely the same shape as local git commits not yet pushed — #35's own
parallel, recognized as the same substrate rather than a second mechanism.

**9.** Inference over raw specs, not a stored fact — the same relationship a
scheduler's resource-matching logic has to `/proc`'s raw counters: the data
is fact, eligibility is a computed judgment.

**10.** Closer to a capability-based or MAC system (SELinux-context-style)
than classic Unix rwx bits — those are stored, static data; this is computed
fresh from Ownership and attestation status.

**11.** A generic compute host/node abstraction, the same idea used
throughout virtualization and cloud computing.

**12.** NixOS generations, applied one level up from a single machine's
system config to fleet-wide membership: atomic, reversible, an inspectable
history instead of one overwritten snapshot.

**13.** The primary node in a primary-replica database replication topology
— exactly one node accepts writes, everything else holds a replica or reads
from it. Simpler than Raft-style leader election, since there's no scenario
yet where multiple candidates compete for the role.

**14.** Git's own distributed model — every clone is a fully independent,
fully functional repository, capable of every operation completely offline,
reconciling via push/pull/merge on reconnect. Formally, "disconnected
operation" in distributed filesystem research (Coda).

**15.** A thin client — no independent state or processing, relays
input/output to a remote host doing the real work. Classic dumb terminal,
modern remote-desktop client.

**16.** A Trusted Execution Environment (TEE) — SGX, SEV-SNP, or
TrustZone/CCA — hardware-enforced enclaves the operator cryptographically
cannot read. "Provably" is remote attestation specifically: a live
challenge-response protocol run each session, not a cached fact.

**17.** Ordinary cloud virtualization, no TEE layer — security rests on
contractual/reputational trust rather than any cryptographic guarantee.

**18.** A fully external, opaque third-party API dependency, outside the
trust boundary entirely — standard mitigation is data minimization at the
boundary, which #29/#47 already formalize.

**19.** A golden image — a complete, ready-to-boot template used
specifically to bring up a new instance from scratch, distinct from
describing an instance's ongoing runtime state.

**20.** Tailscale's persistent-membership half (a device joins the tailnet
once, stays a member), paired with a replica catching up to the primary
before it's fully trusted.

**21.** Standard node decommissioning in any replicated system: revoke
membership, and if the departing node was primary, failover must complete
first — never leaving zero writers.

**22.** The same primary-replica promotion as #13, applied as the actual
mechanism for reassignment rather than just describing the steady state.

**23.** Ephemeral/pre-authenticated nodes, a real Tailscale feature (auth
keys that auto-expire and self-remove) — an exact match for a temporary
vessel joining and leaving automatically.

**24.** Role-based access control (RBAC), bounded by a separate, more
fundamental trust ceiling — the same layering as a filesystem mounted
read-only overriding even a file's own writable bit.

**25.** Single source of truth (SSOT), paired with #13's primary-node
parallel: Flagship is the *where*, the log is the *what*.

**26.** Filtered/partial replication — concretely git's sparse-checkout /
partial clone: sync only specific paths rather than the whole repository.

**27.** Not a pure CRDT — real edits/deletions mean this is genuine
git-merge-shaped reconciliation: most changes merge cleanly, real conflicts
route to human review, consistent with a human already expected to read and
veto entries anyway.

**28.** RAG-style scoped retrieval for the lightweight case; for broader task
needs, the same architecture as #26, just triggered by task scope instead of
device capacity. Principle of least privilege governs both.

**29.** Tokenization — sensitive values swapped for non-exploitable
stand-ins, the real mapping kept elsewhere. Standard in payment/healthcare
data handling.

**30.** The real RAG-vs-fine-tuning distinction — fine-tuning needs the
whole corpus exported wholesale, since training must see the full
distribution, not just what's relevant to one query.

**31.** Tailscale (WireGuard mesh) — the concrete substrate is settled, only
the proper noun in the main text is still open.

**32.** An RPC / actor-mailbox cast — handing work to a remote process, the
same shape as #2's Erlang parallel one layer up.

**33.** The reply half of the same actor-mailbox exchange — written locally
first (#8), collated later (#38), not delivered directly to a central store.

**34.** Demand paging from OS memory management — a process doesn't need its
whole address space resident upfront, a fault triggers fetching the specific
piece needed. Application-layer version: cache-aside.

**35.** `git fetch`/`pull`, exactly — nobody pushes into a clone, a consort
pulls fresh state on its own schedule and immediately on reconnect.

**36.** Bluetooth's "Connected" vs. just "Paired," or `/proc`'s live process
list one layer up — a live scan over a persistent membership list.

**37.** WebSub-style pub-sub cache invalidation — notify and fetch
deliberately kept as two separate mechanisms, so the publisher never tracks
subscriber state or count.

**38.** Event sourcing, generalized across machines — each vessel's local
ledger is its own event log; "current state" (the fleet-wide view) is a live
collation over all of them, not a separately maintained structure. Closer to
Prometheus's own federation model (each node exposes its own state, a central
query merges on read) than to a push-based central log.

**39.** The base unit any scheduler operates on — the same abstraction
Slurm/HTCondor use for a submitted job, without adopting either as real
infrastructure (checked and rejected for other reasons: daemon-only, wrong
interaction model for chartering/mercenaries).

**40.** The trivial case in any scheduler — local execution, no resource
negotiation needed.

**41.** Offloading to a more capable, always-available node — the same shape
as any thin-client-to-server compute offload pattern.

**42.** The shape exo (Apache-2.0, peer-to-peer auto-discovery, splits a
model across heterogeneous devices by real-time topology/bandwidth) already
has right — blocked today by Linux GPU support only existing via Apple
Silicon/MLX, worth revisiting once that lands.

**43.** Backfill-style parallel job scheduling — the concept worth borrowing
from Slurm/HTCondor without adopting the software itself.

**44.** The sealed/open TEE distinction already established at #16/#17,
applied at the task-routing level rather than the install level.

**45.** Data minimization at an untrusted boundary — the standard mitigation
for any external, opaque API dependency, same as #18.

**46.** The cheap, well-precedented alternative to true live process
migration — HPC preemption/requeue patterns (checkpoint, requeue, resume
elsewhere) rather than CRIU-style live migration, which stayed research-grade
even in mature prior art (Sprite, LOCUS, MOSIX) for good reason: fragile even
when it works.

**47.** Principle of least privilege / data classification — the same
judgment any data-loss-prevention system makes before choosing an egress
path.

**48.** A scheduler's placement policy — the same conceptual slot
Slurm/HTCondor's resource-matching fills, kept as reference for the actual
algorithm without adopting the software.

**49.** A microkernel — a small, trusted core with no dependency on any
particular driver to function, thin swappable host-specific integration
layers plugged in around it. Concretely, the actual packaging mechanism: a
Nix derivation/flake.

**50.** Three real mechanisms, not one, already being built in the mojos
repo: NixOS generations for system config, snapper snapshots for data
(triggered per-agent-action), impermanence as the floor under both.
Irreversible actions with external side effects are a separate, unsolved
problem, expected to live in mojo-agent's own harness eventually.

**51.** The same relationship VFS has to a filesystem driver — the module
*is* the driver, pluggable into any NixOS configuration, without requiring
the rest of MojOS's opinionated bundle.

**52.** The same microkernel parallel as #49 — the wrapper is just the
host-specific integration layer: a systemd unit or launchd plist around an
identical core.

**53.** A golden image, same parallel as #19 applied to the artifact rather
than the operation — a complete, ready-to-boot template for bringing up a new
instance, not a description of steady-state.
