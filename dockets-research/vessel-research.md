# Vessel — research notes

*Research, not standing-orders.md. Vessel-specific reasoning — what's common to every
Docket type lives in docket-research.md instead. Cleaned up periodically to stay a
current record, not a full session-by-session diary — the full reasoning trail lives
in devlog.md.*

---

## Definition (current)

*Light formalization pass — just what this is and its Unix mapping. Struct-stat/
inode field work not folded in yet; that's a ground-up pass of its own, still to
come.*

**What it is.** Vessel is the device-type Docket — a Docket for hardware running
mojo-agent, or an approved equivalent client. A Vessel exists as soon as it has a
Vessel Docket (its own persistent identity, mechanism not yet redesigned), full
stop — no initial role or placement required. Its Docket entry is thin
routing/identity information; the real substance (actual compute, actual judgment)
lives on the process-side, the same relationship a device node has to the driver
behind it. Running mojo-agent's full judgment layer is not the bar for being a
Vessel — the actual requirement is lower: running whatever lets it participate in
the fleet at all (identity, permission-respecting, speaks the fleet's communication
protocol). Full local AI judgment is a separate, optional, capability-scaled layer
on top of that floor.

**Unix file type match.** Device — block and character merged into one, not split.
Deliberate: vessels are one homogeneous kind of thing, varying in degree (how much
real judgment/compute it can locally provide, and how much it's trusted) rather than
in kind.

**What "docket-side" persistent data means here.** The Vessel Docket itself
(identity + ownership + however much it can do) is a thin, dumb pointer — it doesn't
store or compute the real behavior, it just identifies and routes to where the real
behavior happens (the process-side). Node existence is separate from current
hardware presence, the same way `/dev/sda` can exist with no disk plugged in.

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
- **Agent continuity** — present the agent locally (real judgment if it has the
  capacity, or a transparent forward to wherever judgment runs); be consistently
  addressable across time.
- **Cross-cutting (trust)** — never exceed its trust ceiling regardless of its
  current role; speak the fleet's communication protocol at all.

The local ledger a vessel keeps isn't append-only or permanent — it's a
reconciliation buffer, durable only until synced upstream, not a monument.

## Ownership doesn't gate existence

Ownership doesn't decide whether something can be a Vessel at all — a rented or
borrowed device is still a real, full vessel for as long as it's actually
participating. Ownership only ever feeds how much it's trusted, after the fact.

Consequences:

- **A rented/chartered vessel is a real, full vessel for its duration** — identity
  scoped to the rental, same shape as `O_TMPFILE` (an ordinary inode, same kind,
  auto-reclaimed on last-handle-close rather than persisting past it). Not a
  different kind of vessel.
- **A borrowed/assigned device (a work laptop) is a real, full vessel too**, for as
  long as it's actually being used — the fleet's boundary is "what Clarke works
  with," not "what Clarke owns." Its trust ceiling gets capped low, same as any
  rented device, but it participates fully otherwise.
- **Door locks, appliances, other IoT peripherals stay fully outside the model** —
  tools/skills the first mate calls, never attempt to join, unrelated to ownership.
- **Mercenary stays fully outside the model** — not infrastructure worked *on*, an
  external destination instructions are sent *to*.

## Vessel has no internal sub-types

Checked twice for a genuine POSIX-style type axis (kind, not degree) within Vessel —
found none. A directory-entry-style role/placement isn't a type axis, it's a
separate placement layer. Installability/platform-openness looked promising next,
but turned out graded (a locked iPhone partially satisfies the vessel contract,
doesn't cleanly pass/fail) — closer to a second derived dial than a genuine
kind-axis. The real type axis is leaf vs. container, one level up at the Docket
level — see docket-research.md. Vessel itself has no internal sub-types.

## Vessel is the device-type Docket

Walking the full POSIX type list (regular, directory, symlink, FIFO, socket, block
device, character device): Vessel maps onto **device** specifically, not "regular
file" or "the generic example." A device file doesn't store its own content —
reading/writing it dispatches to a driver, which does the real work. That's exactly
what a Vessel does: send it a task, it doesn't "store" the task as content, it
dispatches to the real compute underneath (down to zero, for a thin vessel that
forwards essentially everything).

**Device mechanics, and the mapping:**

- **The device node is a thin, dumb pointer** — real behavior lives in the driver,
  not the file. Maps to: the Vessel Docket is thin routing/identity info; the real
  substance (actual judgment) lives on the process-side (First mate actually
  executing), not on the Vessel itself. Validates an earlier finding: mojo-agent
  never talks to another mojo-agent directly, only ever queries its own local vessel
  layer — "the file routes, the driver does the work."
- **Node existence ≠ hardware currently present** — `/dev/sda` can exist with no
  disk plugged in; opening it just fails. Maps to: a Vessel's identity persisting
  through a disconnection is separate from whether the vessel is currently
  online/reachable — a live, queried fact, not an identity fact.
- **`udev` and persistent naming** — modern Linux dynamically assigns device nodes,
  and critically, maintains *separate*, stable identity names (`/dev/disk/by-uuid/...`)
  that survive a device landing in a different transient slot (`/dev/sdX` letters
  shift based on boot order/plug order). Real, currently-running precedent for the
  general principle already chosen for identity: mint a persistent one, don't derive
  it from transient assignment.

**Vessel-role mechanism (Flagship/Consort/Tender/Charter) — deliberately not
designed here.** This is genuinely a distributed-systems concept (who currently
holds the fleet's canonical state, who's thin, who's ephemeral) more than a
filesystem one — real Unix major/minor numbers were tried as a precedent and don't
actually fit (they exist because many device files share one driver; each Vessel has
exactly one driver-equivalent, itself). seL4-style capabilities — an object's
identity held separate from external, reassignable, revocable rights over it — fit
better. Deferred to the permission-bits/capability pass rather than designed in
isolation here.
