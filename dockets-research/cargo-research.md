# Cargo — research notes

*Research, not standing-orders.md. Cargo-specific reasoning — what's common to every
Docket type lives in docket-research.md instead. Cleaned up periodically to stay a
current record, not a full session-by-session diary — the full reasoning trail lives
in devlog.md. Formerly named Log, then Hold — see docket-research.md for why the
name moved twice.*

---

## Definition (current)

*Light formalization pass — just what this is, its flavors, its Unix mapping, and
what "docket" actually means here. Struct-stat/inode field work not folded in yet.*

**What it is.** Cargo is the regular-file-type Docket — direct, opaque content, no
dispatch to a driver, no organizing members. Every regular file is potentially
Cargo, not just one curated store: existence doesn't require being promoted to
active (synced, indexed, elevated in sensitivity) — almost all Cargo just sits at a
do-nothing default, the same way a real inode exists whether or not anyone reads
that file again.

**Flavors.** Not a fixed list — genuinely open-ended per fleet, the way real Unix
never has a fixed list of "what files exist." Split into a new instance wherever
sensitivity tier, sync/access behavior, or owning boundary genuinely differs:
episodic memory, semantic memory, Documents, Projects (one per project), Media are
the ones walked through so far. Memory vs. "just a file" isn't a type distinction
either — it's lever settings (indexing, conflict-resolution mode, sync priority) on
the same type, not two different kinds of Cargo.

**Unix file type match.** Regular file.

**What "docket-side" persistent data means here.** The content model is
deliberately unreinvented from POSIX — opaque bytes, no imposed structure. The real
new work sits above that: cross-replica identity (the same Keel mechanism every
Docket type has, just wider in scope), trust-graded access, and conflict resolution
as explicit policy — all things POSIX never had to solve because it never assumed
more than one machine.

---

## Origin — "the brain"

Standing-orders.md already names this concept: "the log" (#25, "the canonical log"
Flagship holds), already planned elsewhere (ideas.md) as plain, git-backed markdown
memory. This research thread formalized it as a genuine Docket *type*, not a
side-concept hanging off Book — and has since generalized much further than that
first pass expected. "Brain" was the wrong word for the general concept (implies a
mind actively there); a knowledge store existing and an AI actively tending it are
two separate facts. A company accumulates real shared knowledge — decisions, docs,
history — whether or not any AI ever touches it. Consequence: a Fleet-flavored
Book's memory always has an active AI tending it (First mate is guaranteed, #1/#2),
but a Collective's doesn't require a dedicated Collective-level AI to exist.

**Distinct from Vessel's own Local ledger (#8)**: the ledger is thin, transient,
operational bookkeeping (uptime, task-run history, a reconciliation buffer synced
upstream and then droppable). Cargo is the rich, curated-or-not, persistent content
store — a genuinely different kind of thing, not a bigger version of the same one.

**Multi-layer, not guaranteed at every level**: Supreme Allied Command keeps its own
knowledge/coordination record, separate from (though informed by) each member
nation's own internal records — not an aggregation of everyone underneath it, a
genuinely separate, parallel store. Same shape expected for a Mojo Collective's own
instance, if one exists.

## Cargo is the regular-file-type Docket, interrogated not just asserted

Walking the full POSIX type list: this type maps onto **regular file** — direct
content, arbitrary read/write, no forwarding to a driver (unlike Vessel/device), no
organizing members (unlike Book/directory). Checked directly, once "does this
actually fit regular file" came up as a real question rather than an assumption:

- **Opacity check**: is the content opaque to the Book-level mechanics the way a
  regular file's bytes are opaque to the OS? Yes — Sync (#35) just moves bytes, it
  doesn't parse internal structure to decide what to replicate. Whatever richer
  structure exists (chunking, embeddings, sections) is imposed by whichever
  mojo-agent instance reads it, the same relationship a database engine has to its
  own file: opaque to the OS, structured to the engine. Chunking/embedding
  interpretation is application-layer work, not a property of the Docket type
  itself.
- **Seekability/append-mostly check**: "log" the English word already connotes
  append-mostly usage (`/var/log/syslog`, itself a regular file opened `O_APPEND`).
  Memory-flavored Cargo is append-mostly with real edits/deletions gated to human
  review (#27) — still a regular file either way, `O_APPEND` is an open flag, not a
  different inode type.
- **The live counterexample that almost broke it**: the real precedent for this
  concept — git-backed markdown — is, physically, *this repo*: several files
  (`vision.md`, `devlog.md`, `roadmap.md`...), not one. Resolved the same way a
  regular file's own data blocks are themselves scattered across many physical disk
  blocks without that making the file directory-shaped — type classification is
  about the presented interface (one coherent addressable thing), not the physical
  substrate underneath.

## Why this isn't just a renamed POSIX regular file

Checked directly, because the risk of just re-deriving POSIX with new names is real.
The **content model is deliberately unreinvented** — opaque byte stream, no imposed
structure, no dispatch, no member organization, identical to POSIX. The real new
work is a policy layer POSIX never had to build because POSIX assumed one machine,
one trust domain:

1. **Cross-replica identity, not cross-machine-node identity.** Wrongly reached for
   Tailscale's keypair model first — that's the *Vessel*-side mechanism (a
   network-reachable node's identity, rationale #3), a different problem. Also
   wrongly reached for git blob hashing — wrong direction, content-derived identity
   changes the moment content changes, backwards from what's needed (identity that
   *survives* edits). Landed correctly: reuses the same general Keel-minting
   mechanism every Docket type already has (mint once, never re-derive,
   content-independent) — just wider in scope, stable across every replica on every
   vessel holding a copy, not just within one filesystem. Real validating precedent:
   Syncthing's own Folder ID — a minted, stable UUID, identical across every device
   syncing that folder, completely content-independent.
2. **Trust-graded access, not binary permission.** Sensitivity judgment (#47),
   Manifest (#28), Decoy cargo (#29) — give a different, scoped version of the same
   content depending on trust, not allow/deny on one fixed version.
3. **Conflict resolution as explicit policy.** Human review for genuine conflicts
   (#27), acknowledged as likely to evolve, not POSIX's non-answer (last-writer-wins
   or advisory locks only).
4. **Location-transparent addressing.** Reachable regardless of which vessel
   currently holds the authoritative copy (wherever Flagship points), not a path
   that only means something on the machine it's rooted on.

**Explicitly not this type's own contribution**: reversibility/undo. That's the
MojOS snapshot layer (rationale #50 — NixOS generations, snapper snapshots,
impermanence) sitting under everything on the box, this type's data included —
inherited, not designed here.

**Mojo sits on top of Unix, literally, not just conceptually**: two separate,
both-true claims worth keeping distinct — (1) Mojo's *abstract* model borrows
POSIX's structure as a design pattern; (2) independently, wherever a Cargo-type
Docket actually gets implemented on real hardware, it's made of literal POSIX
directories and files (a git repo, physically, is nothing but a directory tree).
Not a coincidence — falls straight out of an already-made decision (git-backed
plain markdown for the memory case). Generalized past just memory: every Docket
type, at the implementation layer, bottoms out in actual files and directories on
actual hardware.

## Nix was considered and ruled out as the mechanism, git/Syncthing preferred

Floated: use Nix's own content-addressed store with a pointer to the newest version.
Self-caught as a space-complexity problem: content-addressed + "pointer to newest"
means every edit mints a brand-new store path; old ones are only reclaimed by
garbage collection, which either deletes the history that was wanted or, left
undisabled, accumulates every version forever. Same tension git itself has, which is
exactly why git needs periodic repack/gc rather than keeping every blob raw —
content-addressing and cheap bounded-space versioning don't come free together.

Where Nix tooling *does* genuinely help: the **impermanence module**, already cited
in standing-orders.md rationale #50. Its job — declaring which paths persist against
an otherwise ephemeral-by-default system — is exactly the right place to declare
*where* this type's territory lives on a MojOS host. It doesn't power the
sync/identity mechanics inside those directories (git/Syncthing does that); it marks
the boundary. Foreign-posture hosts (#52) have no NixOS/impermanence at all, so the
actual mechanism can't depend on it to function — only to get extra
declared-boundary convenience on MojOS specifically.

**Resulting boundary test, Nix vs. this type**: if it's reproducible from
declarative config, it's Nix's job (system config, packages, secrets via
`sops-nix`). If it's not reproducible from a declaration, it's this type's job
(memory, projects, documents, media).

## Every regular file is (potentially) Cargo, and how instances split

First framing treated this as one curated instance per fleet ("the log," #25,
singular) — wrong, the same shape "Ownership doesn't gate existence" was wrong for
Vessel: conflating existence with a narrower condition. Every regular file already
has the identity scaffold, whether or not anything beyond that is ever turned on;
almost all files just sit at a do-nothing default. Build artifacts/caches/
`node_modules`/`.git` internals resolve the same way without a special case:
technically this type too, just never promoted past default, because Nix's own
"undeclared is ephemeral" philosophy already explains why nobody bothers.
Consequence: standing-orders.md #25's current wording ("one per fleet") is not
load-bearing and needs correcting — a fleet holds *many* instances of this type.

**Decomposition principle**: split into a new instance wherever one of three things
genuinely differs from its neighbor; keep together where none do — sensitivity tier
(different sensitivity-judgment outcome, different Permission-ceiling exposure),
sync/access behavior (full write-through vs. abridged, append-mostly vs.
arbitrary-overwrite, git-merge-to-human vs. Syncthing-style), or owning boundary
(a specific project vs. the general fleet, a Collective vs. any one member fleet).
Same logic already governing Ownership/gid/Permission-ceiling elsewhere, reapplied
to decide granularity.

**Walked through a personal fleet**: episodic memory (chronological, append-mostly —
literally what `devlog.md` already is) and semantic memory (consolidated,
revise-in-place — literally what `vision.md`/`philosophy.md`/standing-orders.md
already are) split on access-pattern difference. Documents vs. Projects split, and
Projects further splits **per project**, because sensitivity genuinely varies
project to project — this is where "probably lots" gets a principled reason rather
than a hunch. Media/Photos splits on sync behavior, reusing the existing
abridged-copy mechanism (#26). Secrets/credentials isn't this type's problem at all
— `sops-nix` already owns it at the config layer. Downloads is this type too, just
permanently parked at default posture, not excluded from the model.

**Walked through a Collective (Supreme Allied Command)**: a Collective's own record
is separate, not an aggregation of member fleets' stores pulled upward — owning
boundary differs (the Collective itself, not any one member fleet), so member
fleets keep their own private memory/Documents/Projects untouched, and the
Collective gets its own instance(s) for whatever's genuinely collective-level.

**Grouping requires the directory-type Docket, not a new mechanism**: semantic
memory alone is many individual files, not one — the same directory-type Docket
already established for Book, just holding Cargo members instead of (or alongside)
Vessel members. Recursion was already free for Book-of-Books; Book-of-Cargo is the
identical move.

## Memory vs. "just files the agent uses" — resolved as the same thing

Real question: does agent memory need to be structurally distinct from general
files the agent reads/writes? Evidence against a hard split: this very repo already
mixes true memory-shaped content (`devlog.md` — chronological, agent-and-user
co-authored) with settled-knowledge content (`vision.md`/`philosophy.md`) inside
what's otherwise just an ordinary git project, with no separate "memory folder."
Landed on: not structurally different types, just different **lever settings** on
the same underlying type — memory-flavored instances have semantic indexing,
conflict-routed-to-human-review, and sync-priority turned on; ordinary files don't.
Same move as Flagship/Consort/Tender being lever-settings on one Vessel type rather
than separate types. Reinforced by the current real-world trend of agents leaning on
plain filesystem access over heavy vector-DB/RAG stacks — the plain filesystem being
the actual substrate for agent memory is winning as an approach generally.

## Open threads, current state

1. Does memory need splitting further than semantic/episodic — a third
   (procedural?) category from the cognitive-science literature — or is two enough
   for now? Not pressure-tested.
2. What does a Collective's own instance of this type actually contain in practice?
   Deferred, no real Collective to test against yet. Also: Collective-level instances
   may derive in part from "constitution"-style founding-document work discussed
   elsewhere — explicitly parked, not designed here.
3. The lever framework (sensitivity tier, sync/access behavior, conflict-resolution
   mode, indexing) — proposed as the mechanism that makes "every file is this type,
   memory included" coherent, but flagged as feeling off. Next real discussion item.
