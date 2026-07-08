# Book — research notes

*Research, not standing-orders.md. Book-specific reasoning — what's common to every
Docket type lives in docket-research.md instead. Cleaned up periodically to stay a
current record, not a full session-by-session diary — the full reasoning trail lives
in devlog.md. Formerly named Fleet.*

---

## Definition (current)

*Light formalization pass — just what this is and its Unix mapping. Struct-stat/
inode field work not folded in yet.*

**What it is.** Book is the directory-type Docket — it organizes a list of other
Dockets rather than holding content of its own the way Cargo does. That list is
heterogeneous: Vessel entries and Cargo entries together, one list, not two, the
same way a real directory freely mixes files and subdirectories with no purity
requirement. Recursion is free — a Book whose contents include other Books (an
Armada or Collective) is still just Book, at any depth, no third type needed.

Book is really one general structure parameterized by how many of each of three
things it holds — Captains, First mates, and Vessels — not a family of different
structures. The atomic floor case, a **Fleet**, is exactly one Captain and one
First mate, plus however many Vessels. A **Collective** is the exact same structure
scaled up — more Captains, more First mates, more Vessels — either flat or organized
as nested sub-Books, some of which may be atomic Fleets themselves and some of which
may have no individual Captain at all (in which case the owning identity is a
nominal/shared one, not an empty field).

**Hold** is a Book that's mostly Cargo. Grouping Cargo together like this is what
lets it combine into knowledge bases and context pieces that can be handed to
Charters and Mercenaries as a unit.

**Unix file type match.** Directory.

**What "docket-side" persistent data means here.** The list of what a Book contains
*is* this type's actual content, the same way a directory's list of name→inode
entries is its content, not a bolt-on. Concretely, this is generational, plain,
git-tracked data — same substrate-honesty as Cargo, not a special database
structure.

---

## Book is the directory-type Docket

Book shares Vessel's identity/ownership scaffold — its own persistent identity, its
own ownership — but its operations are organizational rather than running judgment,
exactly the way a directory has a real inode/permissions but supports
`readdir`/`link`/`unlink` instead of `read`/`write`.

## Recursion is free — Armada isn't a third primitive

A directory containing subdirectories is still just "directory," at any depth, no new
type needed. So Armada (and Collective above it) isn't a separate primitive — it's
just a Book-type Docket whose contents happen to include other Book-type Dockets
instead of only leaf Vessels. Matches the existing ideas.md line ("a fleet is the
file, an armada is the folder") but goes further: recursion means there's no need for
a *third* container concept even for "an armada of armadas."

## Book always has exactly one memory-flavored Cargo; a Collective's is not guaranteed

Every Fleet has exactly one Captain plus one First mate, so it has at least one
memory-flavored Cargo instance, guaranteed. A Collective is different — it *can*
stand up its own shared coordinator role (and thus its own memory), but doesn't have
to; it might just be a pure federation with no shared AI at all, in which case any
shared knowledge that accumulates still forms real Cargo, just without a dedicated
AI tending it. Full reasoning lives in cargo-research.md — this is just the
Book-side half: the guarantee is structural for a Fleet, not for a Collective.

## mojo-package's actual scope

Corrected after an earlier wrong pass characterized mojo-package as narrowly the
Commission-time bootstrap kit — wrong. **mojo-package is the entire portable
mojo-system**, and it's specifically the answer to cases Enlist+Sync+Switch-flagship
can't cover on their own: switching Flagship without live access to the new
candidate yet, the old fleet being unreachable entirely, or wanting a genuine
offsite backup snapshot of a whole digital life. Getting a new server when a Book
*already exists* and is reachable doesn't need mojo-package at all — that's just
Enlist + Sync + Switch flagship, composed, no special artifact required. The narrow
Commission-time bootstrap operation still needs its own name, distinct from
mojo-package — genuinely undecided, parked.

Genuine gap, not solved by any of the above: if the old Book is gone entirely —
every vessel lost at once, nothing reachable to Enlist into or Sync from, and no
mojo-package snapshot exists either — neither Commission (assumes nothing ever
existed) nor Enlist (assumes something reachable still does) covers it.
