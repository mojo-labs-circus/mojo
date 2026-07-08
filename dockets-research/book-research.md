# Book — research notes

*Research, not standing-orders.md. Book-specific reasoning — what's common to every
Docket type lives in docket-research.md instead. Cleaned up periodically to stay a
current record, not a full session-by-session diary — the full reasoning trail lives
in devlog.md. Formerly named Fleet — Fleet survives as one of this type's two
flavors, not the type's own name; see docket-research.md's naming history.*

---

## Definition (current)

*Light formalization pass — just what this is, its flavors, its Unix mapping, and
what "docket" actually means here. Struct-stat/inode field work not folded in yet.
Roster itself is one of the things the coming deeper pass still needs to properly
work through.*

**What it is.** Book is the directory-type Docket — it organizes a Roster of other
Dockets rather than holding content of its own the way Cargo does. Its Roster is
heterogeneous: Vessel entries and Cargo entries together, one list, not two, the same
way a real directory freely mixes files and subdirectories with no purity
requirement. Recursion is free — a Book whose Roster holds other Books (an Armada or
Collective) is still just Book, at any depth, no third type needed.

**Flavors.** **Fleet** — a Book mostly holding Vessel entries. **Hold** — a Book
mostly holding Cargo entries. Both Derived, not stored — computed by looking at
what's actually in the Roster right now, the same way a real directory never
declares upfront what kind of children it'll contain. Collective/Armada isn't a
third flavor, just Fleet or Hold recursing one level up (a Book of Books).

**Unix file type match.** Directory.

**What "docket-side" persistent data means here.** The Roster/Billet table *is*
this type's actual content, the same way a directory's list of name→inode entries
is its content, not a bolt-on. Concretely, this is generational, plain, git-tracked
data — same substrate-honesty as Cargo, not a special database structure.

---

## Book is the directory-type Docket

Book shares Vessel's identity/permission scaffold (its own Keel, its own uid/gid)
but its operations are organizational (Roster management) rather than running
judgment — exactly the way a directory has a real inode/permissions but supports
`readdir`/`link`/`unlink` instead of `read`/`write`.

## Recursion is free — Armada isn't a third primitive

A directory containing subdirectories is still just "directory," at any depth, no new
type needed. So Armada (and Collective above it) isn't a separate primitive — it's
just a Book-type Docket whose Roster happens to contain other Book-type Dockets
instead of only leaf Vessels. Matches the existing ideas.md line ("a fleet is the
file, an armada is the folder") but goes further: recursion means there's no need for
a *third* container concept even for "an armada of armadas."

## The multi-Captain / no-single-owner case

An atomic (Fleet-flavored) Book has uid = its one Captain, populated for free (#1
guarantees exactly one). A Book whose Roster contains other Books (Armada/Collective)
has no single Captain over the whole — "a Collective has no single owner over the
whole, it's a federation of already-sovereign Fleets." uid doesn't go empty here —
it points at **the Collective's own nominal identity** instead, following the real
Unix service-account pattern (`root`, `daemon`, a dedicated project account). Same
field, same mechanism, just populated differently — not a fork in the primitive.
Full reasoning lives in docket-research.md.

## Roster/Billet table — authoritative location

A directory entry can only reference an inode on the same filesystem (`link()`
across filesystems fails with `EXDEV`) — billet entries only ever reference Keels
within one Book, and Flagship already holds the canonical memory-flavored Cargo, so
the authoritative Roster naturally lives there too. Other vessels can hold
cached/replicated views (same as a Consort's write-through or abridged copy).

## Armada / cross-Book reference (quick note, not yet designed)

Path resolution in Unix walks each directory component in sequence and requires
search (execute) permission on *every* intermediate directory — you can't jump
straight to a deep file even knowing its full path; a missing `x` bit on any
ancestor directory fails the whole lookup (`EACCES`), regardless of permissions on
the target itself. Confirmed as the right shape for Armada: reaching a specific Keel
in another Book goes through the Armada's own structure (Armada → the target Book as
a named entry → that Book's own Roster resolves internally) — not a flat global
lookup across the whole Armada. Full design deferred to when Armada/cross-Book
scoping is actually addressed.

## Book always has exactly one memory-flavored Cargo; a Collective's is not guaranteed

Every Fleet-flavored Book has exactly one Captain + one First mate (#1, #2), so it
has at least one memory-flavored Cargo instance, guaranteed. A Collective is
different — it *can* stand up its own shared coordinator role (and thus its own),
but doesn't have to; it might just be a pure federation with no shared AI at all, in
which case any shared knowledge that accumulates still forms real Cargo, just
without a dedicated AI tending it. Full reasoning (including the correction that a
knowledge store doesn't require an AI to exist) lives in cargo-research.md — this is
just the Book-side half: the guarantee is structural (from #1/#2), a Collective's is
not.

## Roster heterogeneity is required, not a stretch

A Book's Roster isn't a list of Vessels with Cargo bolted on separately — one Roster
holds Vessel entries (Flagship/Consort/Tender/Charter) and Cargo entries (memory,
Documents, Projects) together, same as a real directory mixing files and
subdirectories freely. This is required for internal consistency: a Book already
governs both "your machines" and "your data" today, and there was never a
principled reason for those to live in separate structures. Standing-orders.md's
current wording ("a roster of vessels") is not load-bearing and needs correcting
once drafted for real.

## mojo-package's actual scope

Corrected after an earlier wrong pass characterized mojo-package as narrowly the
Commission-time bootstrap kit — wrong. **mojo-package is the entire portable
mojo-system**, and it's specifically the answer to cases Enlist+Sync+Switch-flagship
can't cover on their own: switching Flagship without live access to the new
candidate yet, the old fleet being unreachable entirely, or wanting a genuine
offsite backup snapshot of a whole digital life. Getting a new server when a Book
*already exists* and is reachable doesn't need mojo-package at all — that's just
Enlist (#20) + Sync (#35) + Switch flagship (#22), composed, no special artifact
required. The narrow Commission-time bootstrap operation still needs its own name,
distinct from mojo-package — genuinely undecided, parked.

Genuine gap, not solved by any of the above: if the old Book is gone entirely —
every vessel lost at once, nothing reachable to Enlist into or Sync from, and no
mojo-package snapshot exists either — neither Commission (assumes nothing ever
existed) nor Enlist (assumes something reachable still does) covers it.

## Open threads, current state

1. Does a Collective's Roster (if it recursively contains other Books) need anything
   beyond what a normal Book's Roster already provides, or does the same Billet
   mechanism scale up cleanly? Not stress-tested yet.
2. How does a Collective's own nominal identity actually get minted in practice?
   Genuinely deferred, no pressure on it yet — nothing today has more than one
   Captain to test it against.
3. Cross-Book/Armada reference resolution — full design deferred, only the
   directory-traversal shape is confirmed so far.
4. The Commission-time bootstrap kit needs its own name, now that mojo-package
   doesn't mean that.
5. The catastrophic-total-loss gap above — no mechanism covers it yet.
