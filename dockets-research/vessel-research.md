# Vessel — research notes

*Research, not standing-orders.md. Vessel-specific reasoning — what's common to every
Docket type lives in docket-research.md instead. Cleaned up periodically to stay a
current record, not a full session-by-session diary — the full reasoning trail lives
in devlog.md.*

---

## Definition (current)

*Light formalization pass — just what this is, its flavors, its Unix mapping, and
what "docket" actually means here. Struct-stat/inode field work not folded in yet.*

**What it is.** Vessel is the device-type Docket — a Docket for hardware running
mojo-agent, or an approved equivalent client. A Vessel exists as soon as it has a
Vessel Docket (a minted Keel), full stop — no billet required, not even an initial
one. Its Docket entry is thin routing/identity information; the real substance
(actual compute, actual judgment) lives on the process-side, the same relationship a
device node has to the driver behind it.

**Flavors.** Persistent, owned-pool roles: Flagship, Consort, Tender. Ephemeral,
rented roles: Sealed charter, Open charter. All five are Declared, held externally in
the Book's own Roster/Billet table, not stored on the Vessel Docket itself — same
shape as Unix major/minor numbers routing one device type to many distinct drivers.
Mercenary is explicitly *not* a flavor of Vessel — it holds no Docket, no Keel, no
Roster entry at all, and stays fully outside the model.

**Unix file type match.** Device — block and character merged into one, not split.
Deliberate: vessels are one homogeneous kind of thing, varying in degree (Capability,
Permission ceiling) rather than in kind.

**What "docket-side" persistent data means here.** The Vessel Docket itself (Keel +
Ownership + Capability) is a thin, dumb pointer — it doesn't store or compute the
real behavior, it just identifies and routes to where the real behavior happens
(the process-side). Node existence is separate from current hardware presence, the
same way `/dev/sda` can exist with no disk plugged in.

---

## What the fleet is actually for

Re-derived from Clarke's own framing rather than invented: share resources (compute),
share data (the personal cloud), keep one continuous agent across every device —
partner, not per-device instances. Four functional groups:

- **Resource sharing** — expose real capacity honestly; accept/execute delegated work
  within the granted role.
- **Data sharing** — hold/sync whatever shared data this vessel is meant to hold
  (full or partial); buffer local writes made while disconnected until they
  reconcile upstream.
- **Agent continuity** — present the agent locally (real judgment if Capability
  allows, or a transparent forward to wherever judgment runs); be consistently
  addressable across time.
- **Cross-cutting (trust)** — never exceed the Permission ceiling regardless of
  current billet; speak the mesh's communication protocol at all.

**Local ledger note**: not append-only/permanent — a reconciliation buffer, durable
only until synced upstream, not a monument. Standing-orders.md #8 currently says
"append-only," which smuggled an implementation detail into policy — needs fixing
once this is drafted for real.

## Existence doesn't require a billet — the mechanism behind the current Definition

Billet (#12) behaves like a directory entry: extrinsic, reassignable, held in an
external table (the Roster). Keel (#3) behaves like the inode itself: intrinsic,
established once, never re-derived. A Vessel Docket existing is exactly Keel
existing — nothing more is required, not even an initial billet. `st_nlink` (how
many directory entries point at an inode right now) validates the shape of this: a
file with `nlink=0` isn't deleted on the spot, it stays alive as long as something
still holds it open. A joined-but-currently-unbilleted vessel maps to that
`nlink=0`-but-still-open state — real, still existing, only truly gone on an actual
Removed event. Genuinely-never-joined hardware, by contrast, is outside the model
entirely, like an unallocated disk block.

## Ownership doesn't gate existence

First pass wrongly assumed Ownership (#4) decides whether something can be a Vessel
at all. Wrong: all three existing values (owned/rented-sealed/rented-open) already
presuppose full vessel participation — if rented compute couldn't be a vessel,
"rented-open" wouldn't exist as a *vessel* property in the first place. Ownership
only ever feeds Permission ceiling (#10), after the fact. (Full uid/gid/ownership
mechanism lives in docket-research.md — this is the Vessel-specific consequence of
it.)

Consequences:

- **Charters are real, full vessels for their duration** — identity scoped to the
  rental, same shape as `O_TMPFILE` (an ordinary inode, same kind, auto-reclaimed on
  last-handle-close rather than persisting past it). Not a different kind of vessel.
- **A borrowed/assigned device (a work laptop) is a real, full vessel too**, for as
  long as it's actually being used — the fleet's boundary is "what Clarke works
  with," not "what Clarke owns." Permission ceiling caps it low, same mechanism as
  rented-open, but it participates fully otherwise. **Open**: does Ownership need a
  fourth value for this case, or does it fold into rented-open as-is? Not resolved.
- **Door locks, appliances, other IoT peripherals stay fully outside the model** —
  tools/skills the first mate calls, never attempt to join, unrelated to ownership.
- **Mercenary stays fully outside the model** — not infrastructure worked *on*, an
  external destination instructions are sent *to*.

## Vessel has no internal sub-types

Billet was the initial suspect for a genuine POSIX-style type axis (kind, not
degree) within Vessel — ruled out, it's structurally the directory-entry layer, not
the type layer. Installability/platform-openness looked promising next, but turned
out graded (a locked iPhone partially satisfies the vessel contract, doesn't cleanly
pass/fail), closer to a second derived dial next to Capability (#9) than a genuine
kind-axis. The real type axis was leaf vs. container, one level up at the Docket
level — see docket-research.md. Vessel itself has no internal sub-types.

## Vessel is the device-type Docket

Walking the full POSIX type list (regular, directory, symlink, FIFO, socket, block
device, character device): Vessel maps onto **device** specifically, not "regular
file" or "the generic example." A device file doesn't store its own content —
reading/writing it dispatches to a driver, which does the real work. That's exactly
what a Vessel does: send it a task, it doesn't "store" the task as content, it
dispatches to the real compute underneath (down to zero, for a Tender, which
forwards essentially everything).

**Device mechanics, and the mapping:**

- **Major/minor numbers** — major identifies *which driver* handles a device, minor
  identifies *which specific instance*. Capability (#9) is closer to the major
  number (what general class of behavior this vessel supports); Keel is closer to
  the minor number (which specific instance among similar ones).
- **The device node is a thin, dumb pointer** — real behavior lives in the driver,
  not the file. Maps to: the Vessel Docket (Keel, Ownership, Capability) is thin
  routing/identity info; the real substance (actual judgment) lives on the
  process-side (First mate actually executing), not on the Vessel itself. Validates
  an earlier finding: mojo-agent never talks to another mojo-agent directly, only
  ever queries its own local vessel layer — "the file routes, the driver does the
  work."
- **`ioctl()`** — the escape hatch for device-specific operations that don't fit
  generic read/write. Open question: does Vessel need an equivalent escape hatch for
  hardware-specific operations beyond the generic task-dispatch/communication
  contract? Not designed, just noted as a live parallel.
- **Node existence ≠ hardware currently present** — `/dev/sda` can exist with no
  disk plugged in; opening it just fails. Maps to: Keel persisting through a
  disconnection is separate from whether the vessel is currently online/reachable —
  a queried, live fact (connectivity), not an identity fact.
- **`udev` and persistent naming** — modern Linux dynamically assigns device nodes,
  and critically, maintains *separate*, stable identity names (`/dev/disk/by-uuid/...`)
  that survive a device landing in a different transient slot (`/dev/sdX` letters
  shift based on boot order/plug order). Real, currently-running precedent for
  exactly the Keel design already chosen — mint a persistent identity, don't derive
  it from transient assignment.

**Worked example: the mouse.** A USB mouse is a textbook character device
(`/dev/input/eventN`) — stream-oriented, no seeking, since motion/button events only
make sense as a sequence. The chain: kernel USB/HID/input subsystem decodes raw
hardware into a standard event stream; `udev` creates the node; `EVIOCGBIT` (an
`ioctl`) queries what the device can actually do; the raw `eventN` slot can change
across replugging, but `udev` also maintains a stable `/dev/input/by-id/...` name
tied to the physical hardware's actual identity. Mapped: physical mouse = the real
hardware; the kernel's decode work = the local stack running on a Vessel; the device
node = the Vessel Docket; the raw, shiftable `eventN` = closer to current **Billet**;
the stable `by-id` name = **Keel**, precisely; `EVIOCGBIT` = querying **Capability**.

## Open threads, current state

1. Does Ownership need a fourth value for borrowed/assigned-not-owned (work laptop)?
2. Local ledger (#8) needs re-scoping in standing-orders.md from "append-only record"
   to "reconciliation buffer, durable until synced."
3. Does Vessel need an `ioctl`-equivalent escape hatch for hardware-specific
   operations?
4. Worth a cheap, single-value "last-seen" queried fact, separate from the heavier
   Local ledger? Not resolved — likely to fall out of the struct-stat pass naturally.
