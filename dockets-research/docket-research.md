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

*Light formalization pass — just what this is and its Unix mapping. The struct-stat/
inode field work is deliberately not folded in yet; that comes later, per type, as
its own ground-up pass, redesigned fresh rather than patched from older reasoning.*

**What it is.** Docket is Mojo's one core primitive, the way an inode is Unix's one
core primitive — type as a property, not several separate things bolted together.
Vessel, Cargo, and Book are its three known types so far; nothing rules out a fourth
turning up once the deeper per-type work (especially permissions) gets underway, but
none has surfaced after several deliberate attempts to find one. A Docket exists by
virtue of being recorded — the record is what constitutes the thing as real inside
Mojo's model, not just a description of something that already existed independently
(the same relationship a device file has to the disk it points at, generalized to
every type).

**Unix file type match.** None directly — Docket sits at the same level "inode"
does, above the type-bit distinction, not as one specific POSIX type itself.

**What "docket-side" persistent data means.** Docket is the whole *file* half of
Unix's two-part core (files and processes) — passive, persistent, addressed by a
stable identity (mechanism not yet redesigned), holding no behavior of its own. The
*process* half — First mate, Task execution, anything that actually runs — is a
separate, still entirely undesigned primitive that acts on Dockets rather than being
one.

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
either, just a Book-type Docket whose contents happen to include other Book-type
Dockets. Recursion runs the other direction too: a directory also freely mixes files
and subdirectories, so a Book-type Docket's contents hold Vessel-type and Cargo-type
members together, not two separate lists — see book-research.md.

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
still to be designed, once Docket itself is solid.

FIFO/socket, from the full POSIX type list, belong here too — a live Hail/session is
exactly this shape, not a Docket at all. Settled, not open.

First mate needs some persistent identity of its own to genuinely be "one continuous
identity, not a separate instance per vessel" — but it shouldn't be a Docket-side
identity. More like a PID/session-token — a different ID space entirely, matching
how PIDs and inode numbers are already unrelated in real Unix. Deferred to the
process-side research, once we get there.
