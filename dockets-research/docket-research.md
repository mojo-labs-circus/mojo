# Docket — research notes

*Research, not standing-orders.md. This is where the umbrella primitive itself gets
worked out — whatever's common across every type (Vessel, Book, Cargo, and whatever
else turns up), not the specifics of any one of them. Type-specific reasoning lives
in vessel-research.md, book-research.md, cargo-research.md. Cleaned up periodically
to stay a current record, not a full session-by-session diary — the full reasoning
trail for anything trimmed here still lives in devlog.md. Nothing here moves into a
real spec until it's confirmed and drafted properly, on purpose, at the end.*

---

## Definition (current)

*Light formalization pass — just what this is, its flavors, its Unix mapping, and
what "docket" actually means here. The struct-stat/inode field work is deliberately
not folded in yet; that comes later, per type, as its own pass.*

**What it is.** Docket is Mojo's one core primitive, the way an inode is Unix's one
core primitive — type as a property, not several separate things bolted together.
Vessel, Cargo, and Book are its three known types so far; nothing rules out a fourth
turning up once the deeper per-type work (especially permissions) gets underway, but
none has surfaced after several deliberate attempts to find one. A Docket exists by
virtue of being recorded — the record is what constitutes the thing as real inside
Mojo's model, not just a description of something that already existed independently
(the same relationship a device file has to the disk it points at, generalized to
every type).

**Flavors.** Not applicable at the umbrella level — flavor-naming is a per-type
concept (Vessel's Flagship/Consort/Tender, Book's Fleet/Hold), always external to the
type's own core identity record, never stored on the Docket itself. Real precedent:
Unix major/minor numbers work exactly this way — one kernel type (character device),
many functionally distinct flavors, routed by an external index, not a bit in
`st_mode`.

**Unix file type match.** None directly — Docket sits at the same level "inode"
does, above the type-bit distinction, not as one specific POSIX type itself.

**What "docket-side" persistent data means.** Docket is the whole *file* half of
Unix's two-part core (files and processes) — passive, persistent, addressed by a
stable identity (Keel), holding no behavior of its own. The *process* half — First
mate, Task execution, anything that actually runs — is a separate, still entirely
undesigned primitive that acts on Dockets rather than being one.

---

## Why POSIX at all

Wanted the real precedent, not a vague gesture at "everything is a file" — including
where Unix's own choices were arguably mistakes (recycled inode numbers, no
birth-time for decades), worth learning from rather than copying blind.

## One primitive, not several

Unix technically has *one* primitive at the kernel level, not several: there's no
separate "directory" object distinct from "file" — one thing (an inode) with type as
a property, and directory is one value of that property, same scaffold as every
other type. So Mojo's core primitive is **Docket**, singular. Vessel, Book, and
Cargo aren't separate primitives sitting beside it — they're *types* of Docket, the
way regular-file/directory/device are types of the one Unix file object. Recursion is
free the same way it's free in Unix: a directory containing subdirectories is still
just "directory," at any depth — so Armada/Collective was never a third primitive
either, just a Book-type Docket whose Roster happens to contain other Book-type
Dockets. Recursion runs the other direction too: a directory also freely mixes files
and subdirectories, so a Book-type Docket's Roster holds Vessel-type and Cargo-type
members together, not two separate lists — see book-research.md.

## Keel — identity, minting, persistence

- `st_ino` is unique only within one device/filesystem (`st_dev`), assigned by the
  filesystem itself, pure identity — no type/owner/name info bundled in. Keel is the
  direct analogue: a Book mints a UUID at first-join (mirrors a filesystem allocating
  an inode number from its own bookkeeping, not from reading the disk's hardware);
  the device persists a local copy.
- **Scoping** (`st_dev`): Keel is unique within one Book. Matters once Armada/
  Collective exists — not live today.
- **Persistence**: survives OS reinstall *and* any component-level hardware change
  (GPU swap, etc.) — automatic, because Keel is classified Rooted ("never
  re-derived"). No causal path from Hardware inventory (Queried, drifts on its own)
  to Keel. Only reset trigger: Removed / ownership changing hands.
- **Why minting beats deriving**: a hardware-fingerprint identity (even a solid one
  like the DMI board UUID) is tied to whichever component it was derived from and
  breaks when that component's replaced. A minted UUID has no such dependency —
  survives a full Ship-of-Theseus rebuild, because identity was never about the
  atoms, it's about the continuous relationship.
- **Never recycled** — falls out of UUID space size rather than needing separate
  enforcement (Unix recycles inode numbers because the table is small and scarce; a
  128-bit space isn't scarce, so there's no pressure to reuse in the first place).
- **Existing on-machine identifiers surveyed as reuse candidates — none work as the
  primary mechanism, viable only as optional recovery aids**: `/etc/machine-id`
  (regenerates on reinstall, disqualifying itself), DMI/SMBIOS `product_uuid`
  (firmware-level but unreliable across OEMs, absent on most ARM SBCs), TPM
  Endorsement Key (real, thematically relevant to Sealed-charter attestation, but not
  universal). Conclusion stands: mint at first-join, persist a local copy, treat any
  of the above as a recovery signal only.
- **Birth time** — worth adding explicitly. Classic POSIX had no true creation/birth
  timestamp (`st_ctime` is metadata-change time, commonly misread as creation time) —
  bolted on decades later (`statx`'s `stx_btime`, macOS's `st_birthtime`). Don't
  repeat that omission — build a first-join timestamp into Keel from day one.

## Ownership handoff between Captains — a genuine `EXDEV`, not a `chown`

`chown` only works within one filesystem — same inode, new owner metadata, never
crosses devices; a file crossing filesystems gets a brand-new inode on the
destination (`link()` fails with `EXDEV` for exactly this reason), never the old
number, even for identical content. Since Keel is Book-scoped, a Captain-to-Captain
ownership handoff is a cross-device move, not an in-place `chown` — new Captain, new
Book, new Keel there. Confirms standing-orders.md's existing "retired, not
transferred... re-joins fresh under its new owner" is correct as written, not an
arbitrary caution — it falls directly out of Keel's own scoping rule.

This doesn't block "rent out my own hardware, get real reciprocal cloud compute" —
that was never a `chown` case. Loaning a vessel out while staying its owner is just a
Charter: Ownership never changes, Keel never retires, home Book never changes — a
different Book holds a bounded, ephemeral billet on it. Already the shape of the
Ephemeral Commons idea in roadmap.md/ideas.md, arrived at here from the file-model
side.

## Ownership, sharing, and containment — three separate things

- **uid (ownership)** — the individual Captain who owns this Docket. Free today:
  exactly one Captain per (atomic) Book. Points at the Captain as an individual, not
  derived indirectly via "look up the Book, then its Captain" — ownership of
  hardware predates and outlives billeting. Quietly implies Captain may eventually
  need to be its own addressable identity, separate from Book-the-container — not
  designed now, just not foreclosed.
- **Containment (which Book currently hosts/references a given Docket)** — **not**
  a uid/gid job at all. This is the Billet (#12), a directory entry, not ownership
  metadata. Real Unix `gid` has zero relationship to which directory a file lives in.
- **gid (shared rights)** — a named set of Captains sharing standing rights over a
  resource, independent of ownership and containment both. In real Unix this is
  never actually empty — every file has a populated gid from creation, defaulting on
  an ordinary single-user system to a private group containing just that one user
  (the Debian/Ubuntu "user private group" convention). Same shape here: gid is
  always populated, trivially, as the singleton group of just the owning Captain
  today — and real personal devices already work this way informally (a person's own
  machines already form a natural "just me" group, just not formalized). Grows into
  a real multi-member group only once a Collective actually forms one.
- **Permission bits (owner/group/other)** — still open, and load-bearing: the actual
  mechanism that makes group-sharing *do* anything. Real Unix doesn't achieve
  "everyone on the team has equal rights" via an empty owner — it's gid + generous
  group permission bits, with uid often pointing at something functionally
  incidental (a service account). Part of the coming struct-stat pass.
- **Collective's own nominal identity** — validated by real precedent, not invented:
  Unix system/service accounts (`root`, `daemon`, a project's dedicated account a
  team `chown`s a shared resource to) are exactly this — an identity for a
  role/entity, deliberately stable across whichever humans currently operate it.
  Explicitly *not* the `nobody` account (opposite intent). A Collective is the same
  shape First mate (#2) already is — "one continuous identity," surviving membership
  turnover the way a service account survives staff turnover. How it actually gets
  minted is real future work, genuinely deferred.

## Billet table (the Roster) location

Authoritative copy lives on Flagship. Reasoning: a directory entry can only
reference an inode on the same filesystem (`link()` across filesystems fails with
`EXDEV`) — billet entries only ever reference Keels within one Book, and Flagship
already holds the canonical memory-flavored Cargo, so the authoritative Roster
naturally lives there too. Other vessels can hold cached/replicated views.

## Docket vs. the process-side — the split that resolved the "is AI a type" question

Unix isn't really "everything is a file" without qualification — that slogan
specifically covers files, devices, IPC objects. **Processes are a completely
separate primitive**: different kernel structure (`task_struct`, not inode),
different identity system (PID, not inode number), different lifecycle (runs,
executes, terminates vs. persists until deleted), different verbs (`fork`/`exec`/
`wait` vs. `open`/`read`/`write`/`close`). A process *uses* files, isn't one.

First mate (the AI) has exactly this shape — it *runs on* Vessels and *reads/writes*
Cargo, it doesn't sit statically holding data the way Docket types do. So AI/agent
is **not** a fourth Docket type — it belongs on a separate process-side primitive,
still to be designed, once Docket itself is solid. Validating detail: standing-
orders.md already keeps Captain (#1) and First mate (#2) in their own **Identity**
section, before the Vessel/Fleet sections even start — the document already had this
right without anyone having reasoned through why yet.

FIFO/socket, from the full POSIX type list, belong here too — a live Hail/session is
exactly this shape, not a Docket at all. Settled, not open.

**Open thread, not designed yet:** First mate needs some persistent identity of its
own to genuinely be "one continuous identity, not a separate instance per vessel" —
but it shouldn't be a Keel (that's Docket's, the file-side's, identity system). More
like a PID/session-token — a different ID space entirely, matching how PIDs and
inode numbers are already unrelated in real Unix. Deferred to the process-side
research, once we get there.

## Naming history

Two separate renaming threads, kept together since they informed each other.

**The umbrella: Command → Docket.** Command was the first candidate — genuinely
attested at both scales in real naval usage (an officer given "command" of a single
ship; "Fleet Command"/"Pacific Command" as a whole organization), and validated
further against the Collective case via "Allied Command"/"Supreme Allied Command" (a
real NATO structure, no single owner, arguably a better match than Fleet Command).
It held up fine through Vessel and Book, but strained once Cargo entered the family:
Command's justification was specifically about *authority/organizational scope*, and
nobody "has command of" a markdown file the way they have command of a ship — a real
mismatch, not the kind of stretch POSIX's "file" survived (file was already a loose,
low-commitment word before Unix took it; Command was chosen *because* it fit
narrowly and precisely, which makes it more brittle under new scope, not less).

Reopened from there, widened deliberately: Charge (custody/care) felt wrong on the
same instinct as Command; Holding (a real "mixed portfolio" precedent) risked
colliding with Hold, already spoken for as Book's Cargo-heavy flavor; Complement and
Effects didn't stretch across all three types cleanly. Landed on the
registration/record theme instead of the authority/custody theme — what actually
unifies Vessel/Book/Cargo isn't who's in charge of them, it's that they persist, are
addressed by a stable identity, and are *constituted* by being recorded (a Vessel is
a Vessel as soon as it has a Vessel Docket, full stop — existence tracks the record,
more directly than Unix's own files ever needed it to). Papers and Register both fit
that theme well; **Docket** won specifically because it avoids Papers' natural
collective/plural awkwardness for a single instance ("the devlog.md docket" reads
cleanly; "the devlog.md papers" doesn't), and because a docket's defining property —
carrying its own identity independent of wherever it's currently filed — is exactly
what Keel needs, which a page (position-indexed, no independent identity of its own)
structurally lacks.

**The regular-file type: Log → Hold → Cargo.** "Log" carried too much pre-existing
CS baggage (syslog, audit log, write-ahead log) and sat in a different metaphorical
register than Manifest (#28) and Decoy cargo (#29), which were already real
shipping-cargo terms — renamed to **Hold**, unifying the register (cargo, manifest,
decoy cargo, full hold (#30) all cohere under one shipping metaphor). Then, once
every regular file became potentially an instance of this type rather than one
curated store, "Hold" stopped fitting an individual file — a cargo hold is a ship's
whole storage bay, not one crate in it. Renamed again to **Cargo**, the individual
unit; Hold was freed up to become a flavor-name for the directory type instead.
Manifest and Decoy cargo both read *more* literally after each rename, not less —
real evidence each move was right, not just non-colliding.

**The directory type: Fleet → Book, with Fleet and Hold as its flavors.** Fleet
originally named the whole directory-type Docket, but was found to be doing double
duty once Hold became a real flavor-name too — both the structural umbrella and one
of its own two flavors, the exact problem Vessel avoids by staying cleanly separate
from Flagship/Consort/Tender. Compartment and Bay were both considered as neutral
replacements; landed on **Book** instead — genuinely strong nautical pedigree (log
book, muster book, a ship's account book), single-word, and the parent-child
relationship to Docket reads immediately in plain English without requiring anyone
to already know what a docket book technically is.

## Open threads, current state

1. Permission bits (`st_mode`'s owner/group/other) — real design work, load-bearing
   for group-sharing generally, part of the coming struct-stat pass.
2. Symlink-equivalent — checked twice now (once against the full POSIX type list,
   once again deliberately hunting for a fourth Docket type once Book/Cargo were
   both in place), no mapping found either time. Not forced.
3. Process-side primitive itself — not started. Do it once Docket (and its types)
   are solid, using the process/file split as the reference the same way the file
   model is the reference here.
4. The lever framework for Cargo (sensitivity tier, sync/access behavior,
   conflict-resolution mode, indexing) — proposed in cargo-research.md as the
   mechanism that makes "every file is Cargo, memory included" coherent, flagged as
   feeling off, not yet resolved. Next real discussion item.
