# Devlog

*Dated thinking, newest first. Messy is fine — this is where the reasoning trail
lives.*

---

## 2026-07-08 — a from-scratch Unix/SSI conversation, run in a different tool with no knowledge of dockets-research/, converges independently on the same Docket/Vessel/Book/Cargo shape

**Context:** a separate conversation (outside this repo, no access to
dockets-research/) walked Unix's file primitive from first principles toward
"how would you extend this to a distributed system," landing on: files are the
tractable part (Plan 9-style location-transparent namespace, not migration),
processes are the hard part, and for a personal-fleet AI identity you can skip
process migration entirely — mount identity/memory as a namespace instead. Pushed
one level up to a collective: `/collective/shared/*` (knowledge, goals, tasks)
plus `/collective/members/<name>/self/*`, recursive, with real multi-writer
concurrency now a genuine problem it didn't have to solve at the personal-fleet
scale. Landed on an open question — event log vs. lock vs. CRDT for the shared
knowledge write path — and stopped there.

**Checked against dockets-research/ and it holds up, mostly as rediscovery.**
The one-primitive/type-as-property move is `docket-research.md`'s starting point.
The collective's recursive member/shared namespace is exactly Book's already-settled
shape (Fleet as atomic Book, Collective as the same structure scaled up, nesting
free). The open conflict-resolution question is already partially further along
in `cargo-research.md` than where the outside conversation left it — human review
for genuine conflicts, explicit rejection of last-writer-wins — though still not
a finished mechanism, and deliberately left open rather than forced shut here too.
Genuinely new, not yet in dockets-research/: the literal mounting mechanism
(synthetic filesystem / Plan 9 9P framing, `/proc`-style live-served content vs.
flat replicated files) — dockets-research has stayed abstract on the mechanism on
purpose, so this is a real implementation-layer thread worth keeping.

**One real correction landed talking it through.** First mate's identity being
"process-side, PID/session-token-like, not Docket-side" (already in
docket-research.md) doesn't mean the content of the identity lives partly on the
process side the way a real Unix process's live memory does — an LLM invocation
is stateless, pure inference over context, so there's no independent runtime
state to hold anything. The process-side token is a pointer to which Cargo to
load, nothing more; essentially all of what constitutes First mate persists
Docket-side, because there's nowhere else for it to be. Sharpens the existing
text rather than contradicting it — worth carrying into whenever the process-side
primitive actually gets designed.

**Next:** new session, walking through Unix from scratch again on purpose, to
find where the model still needs extending.

## 2026-07-08 — standing-orders.md retired to git history; Keel/Billet/Flavor fully stripped from dockets-research/; the naming discipline hardens to "borrow POSIX verbatim or don't name it at all"; Book/Fleet/Collective resolve to one recursive structure; the five-primitive shape of the whole system gets sketched

**Status:** Standing-orders.md committed as-is (preserving the 53-definition
vibes-era first draft) then removed from the working tree entirely — same
treatment the pre-reset `raw/` corpus got. All four `dockets-research/*.md` files
rewritten: Keel, Billet, the Flavor-as-major/minor claim, the uid/gid section, and
Naming history are gone, along with every standing-orders.md reference and old
ticket number. Roughly 480 lines removed, 190 kept or added across the four files.
Verified by grep — zero hits for Keel/Billet/Roster afterward. No "Open threads"
section survives in any of the four files, on purpose: the actual struct-stat work
regenerates open questions as it goes, a maintained pending-list was judged not
worth the upkeep. Nothing in this session moved into a real spec — same discipline
as always.

**Started the actual struct-stat walk (Cargo's `st_ino` first), and it immediately
forced a much harder question than expected: are we inventing Mojo's own nautical
name for every field, or using POSIX's real names directly?** Checked this against
naming-conventions.md's own three-register system — register 1 (nautical) is
explicitly for real entities/actions in the fleet model, register 2 (plain
functional English) is for computed properties and mechanisms. An identity number
is squarely the second thing, and "Keel" was register drift dressed up as a good
fit: nautical-*sounding*, but never actually describing a role or relationship the
way Captain or Charter do. Landed somewhere sharper than either register, though:
if Mojo is going to literally reuse a real POSIX mechanism unmodified, it doesn't
need a name from *us* at all — it's not our vocabulary, Unix already owns it. Only
the things Mojo actually adds *on top* of the real mechanism need naming, and even
then, plainly, not nautically, since they're still pure scaffolding. Confirmed
explicitly: legibility to anyone who already knows Unix is a real, deliberate goal
here, not just a style preference, and worth holding onto "until Mojo becomes a
real thing."

**Applying that to identity specifically: real `st_ino` cannot be the mechanism,
for a precise structural reason, not a preference.** It's scoped to one
filesystem, gets recycled once freed, and changes the instant something crosses
filesystems (`EXDEV`) — none of which survives what Cargo actually needs (the same
identity, stable, across every replica on every vessel holding a copy). Real
validating precedent, since this is the identical problem already solved in
production: Syncthing doesn't use inode numbers either, for exactly this reason —
it mints its own Folder ID. So Mojo's own minted UUID is a second, separate thing
layered *on top* of the real, untouched inode underneath — not a renamed inode,
not a replacement for it. Concretely, this isn't a new metadata file per Cargo
item; it's one entry in whatever a Book already keeps as its own contents-list,
the same way a real directory's dirent table is centralized, not scattered across
each file. Also fixed a real bug in how this was described before: identity mints
at *creation*, not at *joining a Book* — `O_TMPFILE` proves the two are separable
(a real inode, zero names, still fully valid) — "first-join" language was smuggling
in a two-step story where real inode semantics are one step.

**"Billet" got caught on its own etymology and retired.** A billet is historically
an assigned *living space* — spatial, not functional — and Flagship/Consort/Tender
is a functional role, not a place. Real mismatch, not a naming preference. Went
back to first principles on what actually needs distinguishing: plain containment
(does a Docket belong to this Book, what's it called) turns out to need no name at
all, it's structurally just a dirent, same fate as Keel. The functional-role
question is real and separate, and doesn't map onto real POSIX cleanly either way
— major/minor device numbers were the original comparison and don't actually fit
(they exist because *many* device files share *one* driver; each Vessel has
exactly one driver-equivalent, itself, so there's no many-to-one case for a minor
number to distinguish). seL4 capabilities fit much better: an object's identity
held completely separate from external, reassignable, revocable rights over it,
kept in a separate space rather than on the object itself — which is exactly the
shape of "what role does the fleet currently grant this Vessel." Deliberately not
designed further now; deferred to the actual permission-bits/capability pass
rather than solved in isolation.

**That same role-question decomposed into two genuinely separate facts once
pushed on.** (1) How much real compute/judgment a Vessel can actually provide —
this should be *derived* from the hardware/runtime itself (a `/proc`-style live
fact), not declared by anyone. (2) Whether a Vessel currently holds the fleet's
canonical state (is it Flagship right now) — a real, reassignable designation.
This one isn't purely Book-side either: a Vessel needs its own locally-cached
belief about this too (pushed to it at Switch-flagship time), because requiring a
network round-trip before every canonical write would defeat the entire point of
Flagship being an always-on anchor. That reopens a known, previously-unresolved
gap — a partitioned-away old Flagship could keep believing it's still Flagship
after being demoted elsewhere, the classic fencing-token problem — and this time
it got a real answer instead of staying open: yes, self-healing, an epoch/
generation number bumped at every Switch-flagship, no reliance on anyone noticing.

**Does a Fleet's data have to live only on Flagship? No — it lives across all of
them, git-clone-style.** Authoritative and exclusively-located are different
properties; one canonical copy gets designated, but every vessel can hold a full
or partial replica, the same way every git clone has the full history, not just
the remote. Switch-flagship reassigns which copy is canonical, it doesn't move
data anywhere. A Tender is the exception in degree, not in kind — it may hold none
or a partial replica, but that's a capability difference, not a rule that only
Flagship gets the real data.

**standing-orders.md stopped being referenced at all, not even as an after-the-
fact cross-check.** First proposed keeping it as a corroboration signal once
something new got derived — corrected: agreement with something built without
rigor isn't corroboration, it's coincidence, and checking for it either way still
treats the old document as having authority it never earned. Its only remaining
use is as a record of what was generally wanted, useful later once it's actually
being rewritten for real, not as anything to weigh against while deriving the
mechanism now. Concretely retired from the tree this session (see Status above).

**Does Captain or First mate need a Docket? No, and there's a clean structural
reason.** Real Unix actually has a third identity system beyond inode and PID: the
uid/user-account concept, referenced *by* files (ownership) and *by* processes
(who they run as) without being either one itself. Captain maps directly onto
this. First mate stays process-side, already established as needing its own
PID-like identity, explicitly not a Docket. Roster/contents-list membership is
specifically Vessel/Cargo/Book entries — Captain and First mate are referenced by
a Book, not members of it.

**Re-examined what a Vessel actually requires, and the bar turned out lower than
"runs mojo-agent."** Tender already breaks that literal reading — it's thin,
mostly-forwarding, not running real local judgment. The actual floor is just
running whatever lets it participate at all: identity, permission-respecting,
speaks the fleet's protocol. Full AI judgment is a separate, optional,
capability-scaled layer on top of that floor, not the gate itself. This connects
to something bigger worth holding onto: Mojo should work for deterministic
computing generally, not just AI — the fleet-participation layer is a complete,
useful thing with zero AI in it, and AI sits on top to make it better, which maps
directly onto the `mojos`/`mojo-agent` repo split already in place. Pushed on
this once and refined it: the deterministic mechanism working without AI is a
floor, not a boundary — AI is a legitimate participant in decisions made *through*
that mechanism wherever it's available, including decisions about the fleet's own
management (when to actually switch Flagship, triaging conflicts, routing work),
not confined to sitting on top as an external consumer.

**The real structural finding of the night: Book is one general structure,
parameterized by three counts, not a family of different things.** Captains,
First mates, and Vessels are the three axes. The atomic floor case — a Fleet — is
exactly one Captain and one First mate, plus however many Vessels; it's your
machines and your Jarvis, full stop, not also a separate compositional label for
"any book mostly holding vessels" (a real tension flagged mid-session, resolved
this way by the end). A Collective is the identical structure scaled up — more of
each — flat or organized as nested sub-Books, some of which may be atomic Fleets
and some of which may have no individual Captain at all, in which case the owning
identity is nominal/shared rather than empty. A Hold is a Book that's mostly
Cargo — useful because grouping Cargo together is what lets it combine into
knowledge bases and context pieces, both handed to Charters/Mercenaries as a unit
*and* used directly by the agent day to day. Consequence worth being honest about:
the *structure* here is genuinely free — no new primitive needed for Fleet vs.
Collective vs. Armada, the same way infinite directory nesting is free in real
Unix from one recursive definition. The *behavior* across that structure — a
Collective's nominal identity actually getting minted, trust and permissions
enforced between sovereign Fleets that never fully trust each other — is not
free, and lands in the same still-open bucket as the permission-bits work.

Also corrected in passing: home automation devices are not Vessels, and using one
as an example was a mistake — a smart lock doesn't run mojo-agent or any
fleet-participation client, it's a tool the agent calls into, exactly as already
(and correctly) excluded before this session started.

**A definitions file got proposed, drafted, and deliberately not created yet.**
First pass produced nine candidate terms with real definitions; on review, several
felt "weirdly scoped," which turned out to be a fair instinct — Fleet's dual
meaning above and Captain/First mate's negative-only definitions ("not a Docket,
but...") were both real, nameable problems, not vague unease. Decided: only
genuinely, absolutely concrete, straight (non-hedgy) definitions belong in it, and
nothing is concrete enough yet — the actual struct-stat mechanism work is what
will produce real, confirmed content, not a definitions exercise done in
isolation ahead of it. Scope agreed: root-level, all of Mojo, not scoped to
dockets-research specifically. Not written this session.

**Bigger picture, discussed at length and worth keeping.** The full system needs
five pieces at this level of rigor, not one: the file-side (Docket, underway),
the process-side (First mate, still fully undesigned, possibly the hardest of
the five since neither Unix nor seL4 ever had to model judgment-based execution),
the connection between the two (Hail/the mesh), identity/user (Captain, the third
primitive), and the kernel/trust-enforcement layer underneath all of it
(seL4-flavored, already known to connect directly to the permission-bits work).
Worth naming what's deliberately *not* on that list: memory management, device
drivers, the raw networking stack — correctly excluded because Mojo builds on top
of a real OS and real tools (Tailscale/WireGuard) rather than replacing them,
which is itself a sign the scoping is right, not an oversight.

On process: reaffirmed that locking the load-bearing core before building is a
real, evidence-backed bet this session actually demonstrated (Cargo's cardinality,
mojo-package's real scope, Book's required heterogeneity all would have been
breaking changes against running code if gotten wrong first) — with an honest
caveat that this mode has a natural limit, since even Linux itself wasn't preceded
by this level of upfront rigor and the already-agreed principle is to lock the
core, not litigate every leaf-level decision forever.

On timeline, asked directly and answered honestly rather than dodged: the full
five-primitive vision at tonight's level of rigor is realistically months out,
likely into 2027, not a September thing — tonight alone covered maybe 15-20% of
just the file-side primitive. The narrower thing the existing roadmap actually
promises — one Fleet, dogfoodable as a daily driver — is a much smaller slice that
doesn't need Collectives or the general capability system solved first, but even
that is more likely October/November than September 15 once tonight's real depth
is accounted for, unless scope gets cut harder or design time gets capped sooner
to force the pivot into building.

**One real correction surfaced after the above was already written tonight, worth
keeping distinct rather than silently folding in: the process-side primitive
isn't process-shaped in the Unix sense at all, and treating it that way above was
a category error, not just an open question.** There's no actually-persistent
computation to model — each invocation of First mate is genuinely new,
computationally. What got called "identity continuity" earlier tonight isn't a
process fact, it's entirely a context fact: the reason logging into a laptop
feels continuous isn't that any process survived a reboot, it's that the files
did. That kills the "does Mojo need a PID-equivalent for First mate" framing
directly — it doesn't, because there's no process there to give one to. The same
move dissolves the single-vs-multiple-simultaneous-instances question from
earlier tonight too: sameness is just shared access to the same Cargo, not a new
identity-linking mechanism — five concurrent invocations across five vessels are
trivially "the same First mate" as long as they read and write the same
knowledge base.

What's left of process-side design, properly scoped down after this: a much
smaller **invocation** contract, not a rich process abstraction — spin up with a
task plus context access, do work through tools, write results back, tear down.
"Process" itself may be the wrong word to borrow at all here, same
etymology-mismatch shape as Billet not fitting once its own job narrowed. Two
genuinely open pieces remain, smaller than they looked earlier tonight: what
that invocation contract concretely looks like, and whether truly simultaneous
writes to the same Cargo from different vessels need any lighter real-time
coordination beyond eventual git/PR reconciliation. New question raised and
explicitly parked as future work, not to solve now: should Mojo ever run
multiple simultaneous instances of First mate at once, the way Claude itself
uses subagents.

**Next up, not started tonight:** the actual struct-stat field mechanism work —
identity, ownership, `st_nlink`, permission bits, size, timestamps, `st_rdev`,
birth time — per type, side by side, one field at a time, same collaborative
discipline as tonight (propose, check against Clarke's read, adjust, confirm,
only then move on). The four dockets-research files are now lean enough to
actually receive that work without fighting old vocabulary first.

---

## 2026-07-07 (even later still, continued) — Docket lands as the umbrella name; Book replaces Fleet as the directory-type's own name; commands-research/ renamed to dockets-research/ and cleaned into a real current-state record

**Status:** Still no standing-orders.md edits. `commands-research/` is now
`dockets-research/`, with `command-research.md` → `docket-research.md` and
`fleet-research.md` → `book-research.md` (`cargo-research.md` already had the right
name from last time). Every file in it got a genuine cleanup pass tonight, not just
a rename — superseded reasoning (old wrong turns already corrected, resolved naming
debates, redundant restatements) got cut; anything still load-bearing stayed. These
four files are now meant to function as the actual current-state record of the
file-side of the system, not a session diary — the full chronological reasoning
trail for anything trimmed still lives here in devlog.md, that split is deliberate.

**seL4 checked against the file-side shape directly — it doesn't change it, and the
reason why is worth keeping precisely.** seL4 has no file abstraction at all; any
directory/file interface on top of it is built entirely in userspace (Genode is the
concrete, real, already-working example — a POSIX-like VFS running on seL4). What
changes under a capability-based system isn't the data model (files hold content,
directories organize, devices route to drivers — that shape survives untouched),
it's the *access* model: classic Unix trusts ambient identity (whatever uid a
process runs as), seL4 trusts nothing by default and requires an explicit,
unforgeable capability for every single right. Permission ceiling was already
capability-flavored before this discussion started (rationale #10), so there's
nothing here that would force unwinding any of tonight's or previous sessions'
Docket/Vessel/Book/Cargo work. Corrected framing on sequencing too: this isn't a
separate research detour to schedule "later" — it's literally what "doing
permissions properly" already means once the struct-stat pass reaches `st_mode`.

**Actively tried again to break the three-type claim before locking it — nothing
new turned up.** Checked symlink from three fresh angles: cross-Book aliasing (a
reference to a Docket in a different Book) resolves via directory-traversal through
Armada structure, already decided, not symlink-shaped; role-aliasing ("the current
semantic-memory Docket") resolves via the same reassignable Roster-entry mechanism
Flagship already has, not a distinct pointer type; reference-without-copying (is a
Manifest a pointer rather than real content) — no, a Manifest is genuinely real,
scoped content. Vessel, Cargo, Book stands as the type list, with the explicit
caveat that if a fourth genuinely turns up once the deeper per-type work
(especially permissions) gets underway, that's fine, not something to keep forcing
a search for tonight.

**Naming: the umbrella went through one more real round after Command, landing on
Docket.** Widened past Charge (felt wrong on the same instinct as Command) into
Holding (risked colliding with Hold), Complement, Effects — none stuck across all
three types cleanly. Landed on the registration/record theme instead of the
authority/custody theme Command and Charge both tried: what actually unifies
Vessel/Book/Cargo is that they persist, are addressed by stable identity, and are
*constituted* by being recorded — a Vessel is a Vessel as soon as it has a Vessel
Docket, full stop, no billet required, not even an initial one, sharper than the
"existence vs. currently-billeted" fix from two sessions ago. Register and Papers
both fit that theme; Bills got checked and ruled out directly — the watch bill
(#38) is already a real bill in the true nautical-document sense, a genuine
collision, not just an awkward echo. Papers pulled hard for a while specifically
because of how close it is to "file" itself, but the singular-instance awkwardness
never actually resolved ("the devlog.md papers" still reads wrong for one item).
**Docket** won on a real structural test, not just taste: a docket carries its own
identity independent of wherever it's currently filed, which is exactly what Keel
needs — tested against Page specifically and Page failed this cleanly, since a
page's "identity" is purely positional (page 47 means nothing without naming which
book), the opposite of what's needed.

**The directory-type Docket, Fleet's old job, got its own real name too: Book.**
Same double-duty problem Command almost had, one level down — Fleet was both the
structural umbrella and one of its own two flavors (next to Hold). Compartment and
Bay were both floated as neutral replacements; landed on Book instead — genuinely
strong nautical pedigree in its own right (log book, muster book, a ship's account
book), not just a trimmed-down "Docket Book," single-word, and the parent-child
relationship to Docket reads immediately without requiring anyone to already
recognize a specialized term. Checked it against every scale the type needs to
cover — a Fleet-flavored Book (many Vessel entries, like a muster book), a
Hold-flavored Book (many Cargo entries, like a log book), and a Collective (a Book
of Books) — all read naturally.

**One process correction worth keeping distinct from the naming one**: citations to
standing-orders.md item numbers stay, they're genuinely useful — the only actual
issue was #11 specifically, since this whole multi-session thread has substantially
reworked the Vessel-existence criterion it describes and it's now known-stale
relative to tonight's findings. Not a blanket instruction to stop citing numbers,
just not to lean on that particular one as settled ground right now.

**The cleanup pass itself, concretely**: `docket-research.md` got a single
consolidated "Naming history" section replacing four scattered, partially
contradicting naming threads. `vessel-research.md` lost its now-superseded "first
pass, later corrected" scaffolding and its struct-stat table (deliberately not
rebuilding that yet — comes back properly, per type, as its own dedicated pass).
`book-research.md` got its stale mojo-package paragraph actually fixed this time —
mojo-package is the whole portable system, the disaster-recovery/offline-promotion
answer, not the Commission-time bootstrap kit, which still needs its own separate
name, genuinely undecided. `cargo-research.md` lost redundant restatements between
sections that were saying the same thing twice. Every file kept a "Definition
(current)" section at the top (what it is, its flavors, its Unix type match, what
"docket-side persistent data" means for it specifically) and a trimmed "Open
threads" list with only genuinely open items — resolved debates and superseded
framings removed rather than left to accumulate as noise.

**Next up, explicitly not started tonight**: the struct-stat/inode field
walkthrough, per type, against the now-settled Vessel/Book/Cargo names. Handoff
written separately for this, not filed here — same collaborative discipline as the
whole thread: one field at a time, checked against Clarke's read before moving on,
not a solo pass handed back finished.

**Status:** Still no standing-orders.md edits — same discipline as every session in
this thread. `commands-research/` got real updates (`log-research.md` renamed and
rewritten as `cargo-research.md`; `fleet-research.md` and `command-research.md`
updated), but one honest process note: partway through, a characterization of
mojo-package got written into `fleet-research.md` before it was actually settled,
and had to be walked back a message later — that paragraph is currently stale and
needs a fix pass next time, held rather than corrected in the moment, on explicit
instruction to stop writing files mid-discussion and just talk it through instead.
Worth remembering for next time: write less eagerly, especially once something's
visibly still moving.

**The regular-file-type Command went through its full arc this session: Log → Hold
→ Cargo, each rename for a concrete reason, not cosmetic.** Started by actually
interrogating whether "Log" fits regular file at all rather than asserting it —
checked opacity (is it opaque to Fleet-level mechanics the way a real file is opaque
to the OS — yes, Sync just moves bytes, doesn't parse structure), checked
append-mostly-ness (the word "log" already connotes this, same as `/var/log/syslog`
being an ordinary `O_APPEND` file), and ran into a live counterexample that almost
broke it: the actual precedent for this whole idea — git-backed markdown — is,
physically, *this repo*, several files, not one. Resolved the same way a regular
file's own data blocks are scattered across many physical disk blocks without that
making the file directory-shaped: the type is about the presented interface, not the
physical substrate.

Then the real scope question, which mattered more than the interrogation: is this
just agent memory, or the whole shared-data surface — Documents, Projects, Downloads,
the "personal cloud" idea that vessel-research.md had already named as one of
Vessel's four functional groups but never actually mapped to a Command type. Landed
on: same type, decomposed by a real principle rather than by "curated vs. not" —
split into a new instance wherever sensitivity tier, sync/access behavior, or owning
boundary genuinely differs, keep together where none do. Then generalized further,
correcting the same mistake #11 needed correcting for Vessel: existence isn't the
same as being currently promoted/active. Every regular file already has the identity
scaffold; almost all of them just sit at a do-nothing default, the same way a real
inode exists whether or not anyone reads that file again. Renamed Log → **Hold** —
fixes the CS-overload problem ("log" already means something specific and different
in software) and, more importantly, unifies the register with Manifest/Decoy
cargo/full hold, which were already real shipping-cargo terms sitting awkwardly next
to a logbook metaphor. Then Hold → **Cargo** once "every file is potentially this
type" made "Hold" feel wrong for an individual item — a cargo hold is a whole
storage bay, not one crate in it. Hold got reassigned as a flavor-name for a
Cargo-heavy directory instead of the individual-file type's own name. Manifest and
Decoy cargo both read *more* literally after each rename, not less — a real signal
each move was right, not just non-colliding.

**Checked directly whether this is just POSIX with new names — it isn't, and the
real new work is a genuine policy layer POSIX never had to build.** The content
model (opaque byte stream, no imposed structure, no dispatch) stayed completely
unreinvented on purpose. What's actually new: cross-replica identity (wrongly
reached for Tailscale first — that's Vessel's mechanism, a different problem; wrongly
reached for git blob hashing next — backwards, content-derived identity changes the
moment content changes; landed on reusing the same Keel-minting mechanism every
Command type already has, just wider in scope, validated by Syncthing's own Folder
ID doing exactly this in already-running software); trust-graded access via Manifest/
Decoy cargo/the sensitivity judgment instead of binary rwx; conflict resolution as
explicit policy instead of POSIX's non-answer; location-transparent addressing
instead of a path only meaning something on one machine. Explicitly *not* this
type's own contribution: reversibility. That's the MojOS snapshot layer sitting
under everything on the box — confirmed directly, "the beauty of MojOS is that
everything on the system gains reversibility, including all the mojo files."

**Nix's own content-addressed store was floated as the identity/versioning
mechanism and correctly self-caught as a space-complexity trap** — content-addressed
plus "pointer to newest" mints a brand-new store path on every edit, and old ones
only clear via garbage collection, which either deletes the history that was wanted
or, left alone, keeps every version forever. Same tension git already has, and
exactly why git needs periodic repack rather than keeping every blob raw. Git and
Syncthing stay the real mechanism; Nix's actual right role is narrower and genuinely
useful — the impermanence module (already cited in standing-orders.md's own
rationale for MojOS) is the right place to *declare where this type's territory
lives*, not to power what happens inside it. Resulting boundary test: reproducible
from declarative config is Nix's job (system config, packages, secrets via
`sops-nix`); not reproducible from a declaration is this type's job (memory,
projects, documents, media). Walked back an earlier take on secrets needing
*stricter* Hold policy — wrong, secrets isn't this type's problem at all, `sops-nix`
already owns it at the config layer, same place general system config already lives.

**Fleet's Roster is now understood as genuinely heterogeneous — Vessels and Cargo
together, one list, not two.** Same argument that justified grouping Cargo into
directories in the first place (a real directory mixes files and subdirectories
freely, no purity requirement) applies one level up. Not a stretch — required for
internal consistency, since a Fleet already governs both the machines and the data
today and there was never a principled reason to split those into separate
structures.

**Memory vs. "just files the agent uses" resolved as the same thing, not two
categories.** The evidence was sitting in this very repo the whole time —
`devlog.md` (episodic, chronological) sits next to `vision.md`/`philosophy.md`
(semantic, consolidated) inside one ordinary git project, no separate memory folder
anywhere. Landed on: not structurally different types, different lever settings on
the same type (semantic indexing, conflict-routed-to-human-review, sync-priority on
vs. off) — same move as Flagship/Consort/Tender being lever-settings on one Vessel
type. Reinforced by the live industry trend of agents leaning on plain filesystem
access over heavy vector-DB/RAG stacks — the plain filesystem winning as the actual
memory substrate generally. The lever framework itself got flagged as "feeling
weird" and is still genuinely open, not yet resolved.

**mojo-package's real scope, corrected properly this time.** First pass wrongly
narrowed it to just the Commission-time bootstrap kit. Actual intent: mojo-package is
the entire portable mojo-system — everything — and it's specifically the answer to
cases Enlist+Sync+Switch-flagship can't cover: switching Flagship without live access
to the new candidate yet, or the old fleet being unreachable entirely, or wanting a
real offsite backup snapshot of your whole digital life. The narrow Commission-time
bootstrap operation needs its own, different name — not mojo-package — genuinely
undecided, parked.

**The log's single-writer rule loosened — not to no rule, to a different, arguably
better-fitting one.** #24/#25 currently say Flagship alone may write the log
directly, justified by CAP theorem. Reframed: Flagship isn't the only place a write
can *originate*, it's the only place a write becomes *canonical*. Any vessel can
write locally, always, not just when disconnected — it just isn't canon until
synced. This generalizes #27 ("beyond hailing range") from a disconnection-specific
special case into the thing that's always happening, just faster when connected —
which is exactly how git already works, and Cargo is already git-backed. Pulled the
whole git toolkit in deliberately rather than just commit/push/pull: branch as a real
new mechanism for deliberate tentative/exploratory state that isn't meant to become
canon (yet, or ever); PR as the actual concrete implementation of #27's "route to
human review" rather than an abstraction needing its own invented UI — and PRs
already have a well-trodden path from fully-manual review to policy-gated auto-merge,
a real candidate answer to whether human review has to stay the only mechanism
forever. Floated self-hosting this on Flagship via Gitea, then reconsidered toward
**Forgejo** specifically — Gitea underwent a real 2024 governance controversy (a
for-profit entity taking control of the project without full community buy-in),
which is exactly why Forgejo exists as a community-governed fork. Directly on-theme
for a project this concerned with sovereignty, not a random tool swap.

**Bigger picture, worth stating plainly since it kept happening all session: the
actual v1 build strategy is orchestrating proven open-source tools under one
coherent Mojo policy layer, not building bespoke infrastructure.** Tailscale, git,
Syncthing (and its Folder ID mechanism specifically), Nix's impermanence module,
`sops-nix`, Forgejo — every mechanism question this session resolved to "there's
already a real tool for this." Confirmed as the deliberate strategy, not a string of
individually convenient choices: get a working ecosystem together from existing
proven parts first, fix things as real usage reveals what actually needs replacing.

**Asked to explain Unix at the "files, processes, and the rules connecting them"
level of abstraction, then mapped it honestly onto Mojo — including what's still
completely undesigned.** Files split into naming/addressing, access mediation,
process-to-process connection, and the kernel as trusted enforcer underneath all
three. Mapping: files → Command (this whole session, and the two before it). Naming/
addressing → Keel plus location-transparent addressing. Access mediation →
Ownership/Permission ceiling/the sensitivity judgment. Process-to-process connection
→ Hail/the mesh, barely designed. Processes themselves → First mate/the process-side
primitive, flagged back at the start of this whole thread as "still to be designed"
and genuinely still is — this whole three-session arc has been the *file* half of
Unix only. The kernel-as-trusted-enforcer → nothing named yet, and this connects
directly to the seL4/microkernel research thread already flagged as real future work
two sessions ago, not a new tangent. Real conclusion, worth keeping: Mojo's
*structure* is close to 1:1 with Unix's; its *content* deliberately isn't, and
shouldn't be — every place it diverges (cross-replica identity, trust-graded access,
conflict-as-policy, location-transparent addressing) is exactly where Unix never had
to solve for being distributed, multi-trust-domain, or agent-native, which is the
actual interesting work, not a gap to be embarrassed about.

**Single System Image walked back — a real correction to prior-session thinking, not
a new idea from scratch.** SSI got named as the closest research lineage two sessions
ago and reconfirmed this session, then correctly challenged: SSI's actual defining
promise in the literature is uniform resource presentation everywhere — you can't
tell which physical node you're on. Mojo explicitly doesn't want that; a gaming
setup shouldn't show up on the work laptop, that's a feature of the design, not a gap
in it. What Mojo actually wants is narrower — continuity of identity (First mate
feels like the same partner everywhere) and continuity of specific data (Cargo), not
uniformity of every resource. Closer to **federation** — shared identity/protocol/
data continuity while each member stays genuinely, deliberately locally autonomous —
with Nix flakes/modules being the concrete mechanism that makes "shared declarative
base, selectively divergent instances" actually practical, arguably a more
sophisticated answer than classic SSI research (Sprite/MOSIX) ever cleanly achieved,
not just a difference in taste. The Plan 9/Sprite/Amoeba citation in rationale #2
stays valid for the narrower thing it was actually describing — identity/process
continuity across relocation — it just shouldn't be read as claiming the full SSI
promise.

**Sequencing question, genuinely argued through rather than just agreed to.**
Pushed back on "get to full POSIX-completeness before implementing anything" with a
real historical correction — POSIX itself was extracted *from* already-running,
already-diverged Unix implementations, not written speculatively before any of them
existed, so "finish the formal spec first" isn't actually how the precedent being
invoked came to exist. Also named a real, live tension directly rather than letting
it slide: roadmap.md's current build-first-reset stage explicitly rations meta-work
against a September MojOS v0.1/mojo-agent v0.1 deadline, which is in real tension
with "fully spec everything before writing code." Landed on, confirmed: lock the
load-bearing architectural core — type inventory, identity mechanism, the sync/
trust/conflict model, the stuff this very session proved is expensive to get wrong
after code exists (the Cargo-cardinality fix, mojo-package's actual scope, Fleet's
Roster homogeneity, and the Flagship-local-knowledge question below all would've been
breaking changes against running code) — but don't block all implementation on
finishing every leaf-level naming or decomposition decision, which can converge
iteratively alongside real building, the way POSIX itself converged. Not "produce a
finished formal document" — "get the architecture visible enough to start building
against it."

**One concrete stress test worth keeping: does a Vessel need to locally know it's
Flagship, or is that purely Fleet-side?** Purely Fleet-side, queried fresh every
time, doesn't survive contact with reality — Flagship status has to be checked
before every log write, and requiring a network round-trip before each one would
defeat the entire point of Flagship being an always-on anchor. So yes, a vessel
needs a local cached belief, classified **Declared** from its own point of view
(pushed explicitly at Switch flagship time, same shape Ownership already has, not
silently re-derived). Authoritative source stays the Fleet's Roster. Honest,
unresolved gap named plainly rather than papered over: a genuine network partition
could leave an old Flagship still believing it holds the billet after being demoted
while cut off — the classic fencing-token problem any primary-replica system has to
answer, and nothing here answers it yet.

**Naming, still the most open thread of the whole session, deliberately not forced
to a conclusion tonight.** Command itself got reopened, not just Cargo's own name —
its original justification (officer-has-command-of-a-ship, Fleet Command as an
organization) is precisely an authority/organizational metaphor, and it doesn't
stretch to a stored file the way it stretched to Vessel/Fleet/Collective; unlike
POSIX's "file," which was already a loose, low-commitment word before Unix took it,
Command was chosen specifically for how exactly it fit a narrower scope, which makes
it more brittle now that scope has grown, not less. Charge was tried next and also
felt wrong. Opened wide from there — Holding (real "mixed portfolio" precedent, but
risks colliding with Hold, the flavor-name just landed on), Complement, Effects, and
**Register** all surfaced, with Register currently the leading direction: Unix's own
"file" concept is really about persisting as a stable, named, recorded entity, not
about authority or custody, and Mojo's own model already leans on this harder than
Unix does — #11 ties a Vessel's very existence to having been billeted/recorded at
all. Separately, the directory-type Command (currently "Fleet," now also carrying
"Hold" as its Cargo-heavy flavor) needs its own neutral structural name too, same
double-duty problem the umbrella question has one level down — Compartment and Bay
proposed, neither decided. Explicit next step, picking back up from here. — Vessel worked out against POSIX's file/inode model in real depth; Fleet and a new third type, Log, follow from it; AI resolved as process-side, not a Command type at all

**Status:** No standing-orders.md edits tonight, on purpose, same discipline as the
session before it — this was all discussion, now captured properly in a new
[commands-research/](commands-research/) directory (`command-research.md`,
`vessel-research.md`, `fleet-research.md`, `log-research.md`), which replaces and
retires the earlier `vessel-primitive.md`. Explicitly research, not spec — nothing
here is confirmed enough to fold into standing-orders.md yet, same rule as
everywhere else in this repo. Corrected early in the session, worth stating plainly:
"queued correction" or "agreed" language in past devlog entries does not mean
settled — this session re-litigated several things earlier passes had stated too
confidently, and got some of them wrong a second time before landing right.

**The actual exercise: work out Mojo's own "everything is a file" primitive by
testing it field-by-field against POSIX's real `struct stat`/inode model, not a
vague gesture at the phrase.** Landed on a genuine third-order structural finding
partway through: Unix doesn't really have two primitives (file and directory) — it
has *one* (the inode, with type as a property), and directory is just one value of
that property, same scaffold as every other type. Applying that directly: Mojo's
core primitive is one thing, not "Vessel and Fleet as two separate primitives" —
renamed the umbrella **Command**, tested against real naval usage at both scales
("given command of a destroyer" vs. "Fleet Command," and — the sharpest validation —
"Supreme Allied Command" for the no-single-owner Collective case, a better match
than Fleet Command even is). Vessel and Fleet are its first two known types; a third,
**Log**, emerged over the course of the session and is now believed real.

**Vessel resolved as the device-type Command, not just "the generic example."**
Worked through actual Unix device mechanics — major/minor numbers, the device node
being a thin dumb pointer to a real driver doing the actual work, `ioctl` as the
escape hatch for what doesn't fit generic read/write, node-existence being separate
from current hardware presence, and `udev`'s persistent `by-id`/`by-uuid` naming as
already-running real-world precedent for exactly the Keel design already chosen —
then ran a full concrete example (a USB mouse, `/dev/input/eventN`, `EVIOCGBIT`) to
pressure-test the mapping. Also corrected, more than once: existence (Keel) was
wrongly tied to *currently holding a billet* in #11's current wording — an
unlinked-but-open file is still fully a file, and a joined-but-unbilleted vessel
should be a real, still-existing state, not lumped in with hardware that never
joined at all. Ownership was also wrongly treated as gating vessel-hood at all —
corrected: all three existing Ownership values already presuppose full vessel
participation, so Charters and even a borrowed work laptop are real, full vessels for
their duration (the fleet's boundary is "what Clarke works with," not "what Clarke
owns"), while a door lock and a Mercenary genuinely stay outside the model entirely,
for reasons that have nothing to do with ownership.

**Keel's actual mechanics got nailed down properly**: minted by Fleet at first-join
as a UUID, never derived from hardware (a derived identity breaks on component
replacement; a minted one survives a full rebuild because identity was never about
the atoms); never recycled (falls out of UUID-space size, not a rule to enforce);
survives OS reinstall and any hardware change automatically because it's classified
Rooted; and a Captain-to-Captain ownership handoff is a genuine cross-filesystem
`EXDEV` move, not an in-place `chown` — which is exactly why standing-orders.md's
existing "retired, not transferred" line for ownership handoff was already correct,
without anyone having reasoned through why yet.

**uid/gid worked out properly, after one real wrong turn.** First pass conflated
`gid` with "which Fleet a vessel belongs to" — wrong, that's containment (the
Billet, already fully designed), and real Unix `gid` has nothing to do with which
directory a file lives in. Corrected: `uid` is the owning Captain (free today, a
Fleet has exactly one); `gid` is a named set of Captains sharing standing rights,
independent of both ownership and containment, and — like real Unix's own "user
private group" default — is never actually empty, just trivially a singleton group
today. A Collective's lack of a single owner doesn't mean `uid` goes empty either:
it points at the Collective's own nominal identity, following the real Unix
service-account precedent (`root`, `daemon`, a project's dedicated account), the same
shape First mate (#2) already has one level down. Permission bits (`st_mode`'s
owner/group/other) are flagged as the one piece of real, still-open design work that
actually makes group-sharing do anything — parked for the next pass.

**Log is new, and worth being careful about what it actually is.** Started as "the
brain" (already named in standing-orders.md as #25, "the log," just never formalized
as its own type) and initially over-coupled to AI/identity — corrected: a knowledge
store existing and an AI actively tending it are two separate facts, and a Fleet's
Log is guaranteed (First mate always exists) while a Collective's is not (a
Collective can accumulate real shared knowledge without ever standing up its own
dedicated coordinator AI). Maps to the regular-file-type Command; doesn't
recursively contain other Logs the way Fleet contains Fleets.

**AI (First mate) resolved as not a Command type at all — a separate primitive,
still fully undesigned.** Unix's real split between processes and files (different
kernel structure, different identity system — PID vs. inode — different lifecycle
and verbs entirely) maps directly: First mate runs on Vessels and reads/writes Logs,
it doesn't statically hold data the way Command types do. Validating detail found,
not invented: standing-orders.md already keeps Captain (#1) and First mate (#2) in
their own Identity section, ahead of Vessel and Fleet — already correct, without
anyone having reasoned through why until tonight.

**Also**: added an IoT/"OS for all your stuff" idea to ideas.md, deliberately
grounded in Home Assistant's existing real traction rather than pure speculation —
the differentiator is the agent layer, not reinventing device-protocol integration
Home Assistant already does well.

**Closing discussion, still just talk, nothing filed anywhere yet but worth
remembering for the next research pass.** Physical actuation deliberately
deprioritized rather than researched deeper — Clarke isn't building driverless-car-
grade actuation himself, that's a real bolt-on for someone else later, not core
research; the existing "Reserved: Physical actuation" stub in standing-orders.md
stays exactly as light as it already is. Separately, confirmed the real target for
"avoid Unix's problems" is the seL4/microkernel lineage specifically — the two actual
problems being avoided: monolithic trust (a bug in any driver/service in a monolithic
kernel can compromise the whole system, since everything shares one fully-privileged
blob) and the total absence of formal correctness guarantees in Unix's own security
model (decades of convention and patches, never mathematically proven). seL4 is the
sharp example — a kernel with a complete, machine-checked proof that its
implementation matches its spec, plus proven security properties. Directly relevant
to the permission-ceiling/uid-gid/trust-boundary work already underway, not a
tangent — worth real research time later, unlike actuation. Implementation language
flagged as likely mattering here too: Rust for memory safety came up as a candidate,
since it eliminates whole classes of memory-safety bugs at compile time without
needing a full seL4-style formal proof — a lighter-weight way to get similar-in-
spirit rigor.

Also discussed and worth keeping: Mojo is honestly closer to a **Distributed
Operating System** than a variant of a single-machine one — Plan 9 and Amoeba are
the closest real historical attempts at "one coherent system spread across
machines," and Single System Image (SSI) is already a named research thread once
touched in this project's own history (ideas.md references an "SSI research pass").
But none of that prior art had a genuine persistent AI agent as the primary
interface — that combination (real distributed-OS coherence *and* a real agent
layer) is the actually novel part. On why no massive company has built this
already: real structural reasons, not just a gap nobody noticed — sovereignty/
local-first design is often directly opposed to cloud-dependent revenue models;
the enabling technology (local models good enough to be a First mate) is only a
couple of years old, so even big companies haven't fully reoriented around it yet;
and large orgs are structurally biased toward extending existing product lines over
first-principles rethinks, which is why this kind of thing has historically come
from individuals or small groups instead (Unix's own reaction against Multics,
Linux, Home Assistant already cited above). Real adjacent research does exist
(distributed systems academia, edge computing, personal-agent startups) — it's the
specific combination, not the individual pieces, that's actually rare.

**One last thing worth closing the session on: a real one-line definition of Mojo,
Google-snippet style ("Unix is a foundational, multi-user, and multitasking computer
operating system").** Took a couple of wrong passes — "sovereign, single-owner" was
right for a bare Fleet but wrong once Collectives are in view, since the actual
invariant isn't "always exactly one owner," it's that composing upward doesn't
absorb what's underneath it (though worth remembering Clarke's own caveat: top-level
overrides in a real Collective might get designed in later — this isn't claimed as
an absolute, unbreakable law, just today's default shape). Landed on:

**"Mojo is a distributed, agent-native, recursively sovereign operating system."**

Matches Unix's own three-adjectives-plus-noun cadence on purpose. Clarke's own
caveat on "agent-native," worth remembering precisely: First mate is the *primary*
interface, not the *only* one — Mojo still needs to work as a completely normal OS
underneath it (files, a terminal, ordinary apps), just with the agent as a real,
deeply-integrated first-class citizen rather than a mandatory bottleneck for
everything. "Agent-native" was confirmed as still the right word for that, just
worth being precise it doesn't mean agent-only.

Floated, not yet decided or applied: using this line for the actual GitHub org
description and a `.github` profile README. Checked tonight — the org
(`mojo-labs-circus`) and the `mojo` repo both already exist for real, with a current
live description ("Private self-hosted AI framework — OS and intelligence designed
together. Your hardware, your data, your AI."); no `.github` profile repo exists
yet. Nothing changed there tonight — a real public-facing edit, held for an explicit
decision rather than made in passing.

## 2026-07-07 (later) — naming-conventions.md written, vision.md gets a pointer to standing-orders.md, then the real work: designing the actual service architecture the whole ecosystem runs on

**Status:** [naming-conventions.md](naming-conventions.md) now exists — the three
registers (nautical/real-word, plain functional English, product branding), plus the
three loose ends flagged earlier today (Muster-ping's hybrid register, "billet" never
formally defined, the mesh still unnamed) carried over as open items rather than
resolved. vision.md's fleet paragraph now links to standing-orders.md, framed as the
POSIX-style formal contract behind the narrative, crediting the real design work
across both of yesterday's sessions plus today's, not just today. philosophy.md left
untouched on purpose — confirmed with Clarke that the "why" hasn't changed, all of
this has been "how." roadmap.md deliberately left alone too, on Clarke's call — it's
what gets worked out next, not pre-edited from tonight's conceptual pass.

**The actual subject of the second half of tonight: what services/repos the whole
ecosystem is made of, long-term, ignoring what gets built this summer.** Started from
Clarke's own framing — "mojo is two things mainly: the fleet and the first mate" —
and a real test for what earns its own service rather than just being a named
concept: a genuinely different **trust boundary** or **lifecycle** than what's already
been separated, not mere conceptual distinctness (which would justify splitting
forever). Landed on this after several wrong turns, corrected in-session rather than
carried forward wrong:

- **mojo-agent** — pure judgment. Task reasoning, memory curation, conversation,
  voice/personality. Owns no state that has to outlive it. Never makes a network call
  for anything fleet-internal, and never talks to another mojo-agent directly — only
  ever queries its own local vessel service.
- **The vessel service** — a real, separate thing, different lifecycle than
  mojo-agent (has to survive it being reinstalled or replaced entirely, same as Keel
  surviving OS reinstalls). Owns identity, the billet table, the roster, permission
  ceilings, the communication protocol (Hail/Sync/Flare/Muster-ping/Report), and — on
  whichever vessel holds Flagship — the routing policy. Only vessel-service-to-
  vessel-service traffic ever crosses the mesh; mojo-agent is always a local client of
  its own.
- **The brain (the log, #25) — corrected mid-session, this is *not* part of
  vessel-service, and probably isn't a running service at all.** First instinct was to
  fold it into vessel-service since Sync/Hail/Flare already move it around — wrong,
  caught when asked to justify it. The brain's lifecycle is scoped to the First
  Mate/Captain's continuity (has to survive even hardware being replaced), not to any
  one vessel's Keel (which explicitly retires rather than transfers when a vessel
  changes hands) — a longer-lived, differently-scoped lifecycle than anything
  vessel-service owns, and content-heavy in a way topology data isn't. Given the brain
  is already planned as plain git-backed markdown (ideas.md), it doesn't need an API
  or a query — it's a shared data store mojo-agent reads and writes directly.
  Vessel-service only touches it to replicate it across vessels, most likely just
  using git's own fetch/push as the transport, not reimplementing sync.
- **A "boundary gatekeeper" and an "action-safety-gate" were both proposed as
  separate services, then both collapsed back into existing pieces once actually
  justified against the trust-boundary/lifecycle test:**
  - The gatekeeper (enforcing the sensitivity judgment, #47, before data crosses to a
    mercenary or charter) folds into vessel-service — Permission ceiling (#10) and
    Permission grant (#24) are already fleet-layer facts in standing-orders.md, not AI
    judgment, so vessel-service already holds what's needed to enforce this; it's an
    extension of an existing responsibility, not a new one. Sensitivity gets tagged at
    write time, when mojo-agent is already doing the semantic work of curating memory
    — enforcement at send time is then mechanical, not a fresh judgment call.
    Reasoned twice over, independently: (1) data locality — only vessel-service holds
    the trust-tier facts the rule needs; (2) separation of duties — the entity
    generating/using content can't be the sole judge of what's safe to release, or a
    reasoning mistake or jailbreak just *is* the leak.
  - The action-safety-gate (confirmation/rollback before consequential actions,
    rationale #50's still-unsolved problem) doesn't become a fourth repo either — its
    mechanism is inherently posture-specific (NixOS generations vs. Foreign having no
    equivalent at all), so it's not one artifact deployed identically everywhere the
    way mojo-agent and vessel-service are. It's a contract each OS-integration
    profile — MojOS, the bring-your-own-NixOS module, the Foreign wrapper —
    implements differently, using whatever that posture actually provides.
- **Net structure:** two portable logic repos (mojo-agent, vessel service) + the brain
  (shared data, not a service) + the MojOS/bring-your-own-NixOS/Foreign platform
  family (bundles the first two onto a host, and is where the action-safety contract
  actually gets implemented per posture).
- **A separate, already-existing third substrate surfaced late: shared project files
  across vessels.** Not vessel-service's job (different mechanism) and not the
  brain's job (that's curated memory, not work files) — this is the "Shared `~` across
  the fleet" idea already sitting in ideas.md, Syncthing-based, already flagged there
  as the right existing tool rather than something to build. Three different
  already-adopted substrates doing three different jobs: Tailscale under vessel
  service, git under the brain, Syncthing for anything flagged shared.

**Two concrete end-to-end traces run to pressure-test the model, both worth keeping as
reference scenarios:**

1. **Phone (Tender, ~zero local capability) asks Jarvis to summarize a doc worked on
   earlier at the desk.** Confirmed the core rule: mojo-agent never makes a network
   call and never talks to another mojo-agent directly — every cross-vessel hop is
   vessel-service to vessel-service; mojo-agent only ever queries its own local
   vessel-service, and reads the brain directly rather than through a vessel-service
   query once that correction landed.
2. **Laptop (a normal, capable Consort — not capability-starved) writing code
   interactively, task needs Flagship's bigger model, Clarke wants continuous
   collaboration, not repeated cold requests.** Surfaced three refinements the phone
   case didn't: (a) offloading to Flagship is a *per-task* routing decision
   (Flagship-directed resourcing, #41) — a Consort doesn't get demoted to a Tender
   just because one job needs more than it has; (b) if the project folder is a shared
   directory, Flagship may already hold its own Syncthing-synced copy, so per-turn
   payloads carry only the conversational turn, not file content; (c) the whole
   multi-turn session should be one open **Hail (#34)** channel for the task's
   duration, not a fresh request-response per message — matching ideas.md's own
   existing note that hailing means "an ongoing live connection back home for the
   whole task, not a one-time payload." The extra attack-surface caution ideas.md
   attaches to hailing is specific to Charters (rented, less-trusted compute) and
   doesn't apply here — Consort and Flagship are both owned, both already fully
   trusted with the whole brain by design.

**Open for next time, nothing here has touched a spec file yet:** none of tonight's
service-architecture conclusions are written into standing-orders.md — this was all
discussion. Whenever Clarke picks this back up: decide whether/how "the vessel
service" folds into standing-orders.md's Fleet section and Deployable artifacts
(#49–53), and it still needs an actual name — register 3 (product branding, alongside
MojOS/mojo-agent/mojo-package), not resolved tonight, flagged rather than guessed at.
roadmap.md is still untouched — "how we go from here" stayed at the conceptual-
architecture level tonight rather than converging on an actual roadmap rewrite.

**Also queued for tomorrow — the vessel type-axis question isn't actually closed,
caught right at the end of the session.** Yesterday's conclusion ("no type axis
needed, vessel varies only by Capability") held for the cases actually tested — the
door lock (never becomes a Vessel at all) and the phone (scales down to Tender via
Capability). But that phone case quietly assumed mojo-agent is *installable* on it at
all — root access, a background daemon, a real Nix target. That's not guaranteed:
a stock iPhone, a locked-down smart TV, a games console can have plenty of hardware
Capability and still be structurally unable to install arbitrary software. That's a
genuinely different axis than Capability (#9) — installability/platform-openness, not
"how much can it do" — and it's not resolved. Whether a closed platform needs an
entirely different kind of client (not mojo-agent-the-Nix-package at all) is the open
question, and it could reopen something type-axis-shaped. Start here next time.

**Honest close, Clarke's own read and it's fair:** decent discussion, not much
concrete out of it. Real artifacts from tonight: naming-conventions.md (new file),
one cross-reference added to vision.md. Everything else — the whole vessel
service/mojo-agent/brain/sanitizer architecture, both end-to-end traces, the
installability gap above — is reasoned-through discussion, not yet written into
standing-orders.md or anywhere durable beyond this devlog entry. Nothing built,
no repo started, roadmap.md untouched. That's fine for a thinking session, just
worth being honest that "concrete" and "good discussion" aren't the same thing, and
tonight was mostly the second one.

## 2026-07-07 — Standing Orders walkthrough begins: vessel type axis resolved (turns out we don't need one), physical actuation and the door-lock case settled, then a long practical pass on how mojo-agent actually adapts to hardware and OS

**Status:** standing-orders.md itself wasn't edited this session — this was discussion,
Clarke wants to walk the document himself first and bring specific things back. Two
concrete corrections are now queued for whenever that edit pass happens (see bottom).

**Naming conventions, first pass.** Before Clarke started his own walkthrough, a quick
read of the existing 53 definitions surfaced three separate registers already in use,
none written down anywhere: real nautical/naval words for fleet-model entities and
actions (Captain, Keel, Flagship, Hail, Flare, Watch bill — always a real attested
term, never an invented portmanteau or forced backronym); plain functional English for
computed properties and the six Fleet section labels (Permission ceiling, the
sensitivity judgment, the routing policy); and product branding for Deployable
artifacts (MojOS, mojo-agent), a separate namespace entirely. Flagged three loose ends
in the current draft worth resolving whenever the naming-conventions file actually gets
written: **Muster-ping (#36)** welds a real nautical word to tech jargon, the only place
two registers got mashed into one word; **"billet"** is used constantly but never gets
its own numbered definition, despite being load-bearing vocabulary; **the mesh (#31)**
is explicitly still unnamed in the document itself. The theme/naming-conventions file
itself is deferred — Clarke wants to walk the doc and bring things first.

**The vessel type axis — walked through concrete future fleet members (phone, TV, car,
fridge, a JARVIS-on-everything-Stark-owns framing) and landed on not needing one at
all, which turns out to validate the original draft rather than contradict it.** The
reasoning chain: considered giving Vessel a `type` field (mojo-agent-capable vs.
peripheral) to cover devices like a smart door lock or fridge sensor that can't run
mojo-agent. Then resolved the door-lock case directly — it fails the existing
definition of Vessel (#11, requires running mojo-agent *and* holding a billet), so it
was never a vessel-type-variant to begin with, it's a tool/skill a real vessel calls,
same category as "chefs and recipes" already in ideas.md. The existing sentence in the
document — "a device isn't a Vessel until Fleet billets it, raw hardware with no role
assigned is outside the model" — already covers this; a peripheral is just the
permanent case of that same rule, not a new concept. Then resolved the phone case the
same way: a phone genuinely could run mojo-agent with near-zero Capability (#9),
computed from thin Hardware inventory (#7), and net out as a Tender (#15) — "no
independent capability, a pure access surface" — which is already the exact definition
of a phone-as-pure-proxy, and rationale #15 already cites the real precedent (thin
client / dumb terminal / remote-desktop client). Net conclusion: the original
homogeneity claim in the preamble was right — vessels vary only in degree (Capability),
not in kind — and the specific line predicting a future need for "a genuine type axis"
for sensors/peripherals is actually wrong as written, since peripherals never become
vessels at all. **Correction queued for standing-orders.md:** fix that preamble
sentence so it doesn't tell future-Clarke to build a type axis that isn't needed.

**Fleet and Collective confirmed as the same recursive container shape (directory),
differing only in one real invariant.** Both organize members without owning their
contents (Fleet holds vessels, Collective holds Fleets/other Collectives), same
recursion at any depth, no special-casing per level — this was already implicit from
the earlier session's "directory" framing but Clarke's question made it explicit one
level down. The one thing that still genuinely separates them, not just naming: a
Fleet is bounded by exactly one Captain + one First mate (the accountability
boundary); a Collective has no single owner over the whole — it's a federation of
already-sovereign Fleets. Same container mechanics, different ownership semantics
underneath, like a directory one user owns versus a mount point aggregating several
separately-owned filesystems.

**Physical actuation (door locks, eventually the car/fridge/house JARVIS-style) —
deliberately deferred, not solved, and for real reasons rather than just running out
of time to discuss it.** Unlike every other axis in the document, it has no clean
existing precedent to borrow (data-permission mistakes are recoverable — a bad read
gets logged and revoked; actuation mistakes can be actual safety failures, a much
harder category closer to industrial fail-safe/dead-man's-switch design than RBAC),
and there's no real device integration yet to design the rules against. **Correction
queued:** add a **Reserved: Physical actuation** section to standing-orders.md, same
shape as the existing Reserved: Collective / Reserved: Shell & Utilities stubs — names
the concern, explicitly out of scope until there's a real device and an actual safety
model, not designed today. Not written yet, just agreed.

**Long practical pass on how mojo-agent itself (not the Standing Orders contract, the
actual implementation) adapts to hardware and OS — this is mojo-agent-repo territory,
logged here because the reasoning happened in this session:**

- **One artifact, not per-tier builds.** The harness should talk to inference through
  one endpoint interface that doesn't care whether it's local or remote — probes
  hardware at startup, computes Capability, either spins up a local
  llama.cpp/Ollama-style server and points the client at localhost, or points the same
  client at Flagship over the mesh. Same Nix derivation everywhere; which path runs is
  a runtime branch, not a build variant. Same real precedent already in ideas.md
  (Ollama/llama.cpp already auto-detect hardware and pick backend/quantization,
  including "run nothing locally").
- **Harness scaling is the same mechanism, one layer up** — most of the harness
  (task-routing, Delegate/Report, holding a log copy) simply never activates on a
  Tender because it's never assigned that kind of work. Config/capability-gated, not a
  second build.
- **Phone should still run full mojo-agent, not a separate thin-client app** — identity
  (Keel), mesh join, and the comms protocol all have to live somewhere; forking a
  second lightweight client just to save phone footprint means maintaining two
  systems instead of gating one. The actual cost (binary size, idle resource use on a
  phone) is a packaging problem, not an architecture fork.
- **Package-size optimization: probe-then-fetch, scoped narrowly.** Real precedent
  (pip/CUDA wheels, Ollama's installer: detect hardware, pull only the matching
  backend) — but apply it to the one genuinely heavy dependency (the local-inference
  backend/model runtime), not the whole harness. On MojOS this is nearly free already
  via Nix's own declarative conditional builds (`lib.optionals` gated on a config flag
  known at eval time); a live runtime probe binary is only needed on postures where
  hardware isn't known ahead of build time (Foreign, a phone app).
- **Foreign posture, concretely:** Nix installs on Linux and macOS (+ WSL2, not native
  Windows — decided that's fine, "fuck windows"). Install Nix → build mojo-agent via
  its flake (identical reproducible derivation regardless of host) → wrap it as a
  systemd unit or launchd plist, since a non-NixOS host has no declarative service
  management of its own. `nix-darwin`/`home-manager` are optional extra layers, not
  required for the minimal version.
- **OS-level protection (generations/snapshots/impermanence) is a gradient, not a
  MojOS-or-nothing binary.** bring-your-own-NixOS keeps generations for free (still
  NixOS underneath); snapshots and impermanence are opt-in, disk-layout-dependent, and
  probably absent unless the user specifically set them up. Foreign has none of it at
  the OS level. mojo-agent needs to actually detect which protections exist on the
  vessel it's running on, not assume a blanket level from the OS-posture label —
  connects directly to rationale #50's already-flagged unsolved problem: irreversible
  actions with external side effects need harness-level judgment regardless of what
  the filesystem protects.
- **Environment awareness must be baked into flake config/flags, never left to system
  prompt or model memory** — a load-bearing fact like "do I have snapshots here"
  can't be something the model is trusted to recall reliably; it has to be a
  deterministic value read at startup.
- **v0.1 scope decided: any NixOS, not MojOS-only.** bring-your-own-NixOS is close to
  free *if* mojo-agent's module is kept properly decoupled from MojOS's
  Btrfs/snapper/impermanence-specific bundle from day one — that discipline costs
  nothing now and is expensive to retrofit later, so build it decoupled regardless of
  MojOS. Foreign genuinely requires separate host-integration work (the
  systemd/launchd wrapper) and stays deferred until after v0.1 ships and there's some
  traction — MojOS becomes "any NixOS, plus extra good," not a separate track.
- **Rebuild-to-match, split into two real cases.** Most environment awareness (current
  hardware capability, which protections exist) should be runtime-checked with no
  rebuild ever required — cheap syscalls/procfs reads, microseconds, checked once at
  startup and cached until a real trigger (rebuild completing, reconnect, a
  hardware-change signal) invalidates it, same pattern Ollama/llama.cpp already use.
  Only package-inclusion-level decisions (whether the heavy inference dependency is
  even present) require an actual Nix rebuild — which is itself one of the few
  genuinely safe self-modifying actions mojo-agent could trigger autonomously, since a
  generation is atomic and reversible.
- **mojo-agent improving the user's own setup, two different trust levels.** Nudging
  toward impermanence or other protections on an existing bring-your-own-NixOS box is
  already covered for free — OS posture (#6) is Queried, drifts without a roster event.
  Recommending a full OS switch is a categorically bigger action (real data migration,
  feels irreversible even if technically recoverable) and belongs in the same
  high-confirmation category as other consequential actions, not treated as a casual
  config nudge.

**Corrections queued for the next standing-orders.md edit pass (not yet made):**

1. Fix the preamble line predicting a future vessel `type` axis — not needed, wrong as
   written.
2. Add a **Reserved: Physical actuation** stub, same shape as the other two Reserved
   sections.
3. Naming-conventions file still pending Clarke's own walkthrough — Muster-ping, the
   undefined "billet" term, and the unnamed mesh (#31) are the three known loose ends
   to fold in whenever that happens.

## 2026-07-06 — HANDOFF: first draft of the document actually written (53 definitions), then renamed system-model.md → standing-orders.md; Clarke stepping away, here's what's next

**Status:** [standing-orders.md](standing-orders.md) exists as a real first draft —
Identity, Vessel (Definitions + General concepts), Fleet (Roster/Permissions/
Records/Communication/Accounting/Policy), Deployable artifacts, Reserved stubs for
Collective and Shell & Utilities, and a full Rationale section, 53 numbered items
total. This is the file the previous entry called "still unwritten" — it got
written in this same session, right after that entry, once Clarke confirmed he
wanted the actual draft rather than more discussion first.

**The rename, and why it's not just cosmetic:** the document was first called
"Mojo Policy" while being written (see previous entry) — functional but not a real
name, the way "Portable Operating System Interface" isn't what anyone actually
calls POSIX. Considered and rejected two real nautical alternatives before
landing: pirate Ship's Articles (rejected — historically per-crew, negotiated by
each ship's own crew, the wrong shape for a document that's *general policy for
how any fleet is constituted*); Articles of War (structurally the right shape —
real, standardized, fleet-wide, not per-crew — but carries real court-martial/
capital-discipline baggage that reads wrong for what this is). Landed on
**Standing Orders** — the real naval term for permanent, general instructions
governing how a fleet operates, as opposed to a one-off specific order. Lighter
than Articles of War, still genuinely fleet-wide rather than per-crew, still real
precedent rather than an invented term — matches how every other name in this
document works (Keel, Billet, Flare, Muster roll, Watch bill are all real words,
not acronyms or backronyms, and Standing Orders keeps that pattern rather than
forcing a POSIX-style acronym we don't actually need).

File renamed `system-model.md` → `standing-orders.md`; internal title and preamble
updated to introduce it as "Mojo's own POSIX." Links fixed in this file and in
ideas.md (two stale cross-references there also had old pre-rewrite numbering —
watch bill was `system-model.md #12`, now `standing-orders.md #38`; chartered
instance was `#7/#8`, now `#16/#17` — the full renumbering from the old 33-item
document to the new 53-item one means **any other file that cites a Standing
Orders number by memory should be treated as suspect until checked** — roadmap.md,
vision.md, philosophy.md haven't been audited for this yet).

**Checked whether MPI or Kubernetes' CRI/CNI/CSI could implement fleet control
directly, rather than designing it from scratch — same spirit as the earlier
Slurm/Petals/exo/9P pass.** Both are real, mature, and both solve a different,
lower layer than what Standing Orders addresses. MPI assumes membership is fixed
at job-launch time, no trust tiers, no tolerance for a node disappearing
mid-job — it's for fast messaging between processes that are *already* running
on nodes that are *already* trusted. CRI/CNI/CSI are plugin interfaces *within*
Kubernetes' own control plane, assuming a node has already been admitted and
scheduled by something else. Neither has any concept of "some of these machines
are mine, some are rented, one's a mercenary I don't trust at all, and there's
exactly one accountable human" — which is the actual problem Standing Orders is
for, and part of why nobody's built this already (also checked last entry via
Urbit — the sovereign-personal-AI ambition is real and pursued, the specific
billet/trust-tier mechanism isn't). Worth reusing narrowly later, as
implementation detail rather than fleet infrastructure: MPI as the wire protocol
under Delegate (#32) specifically for Combined-owned resourcing (#42), once
billeting's already resolved and a job is splitting across already-trusted,
already-connected Consorts; CRI only if mojo-agent itself needs to run sandboxed
workloads on one vessel internally.

**Next steps, for whoever picks this up (Clarke, back from work):**

1. Read [standing-orders.md](standing-orders.md) fresh and mark what's wrong or
   missing — it's a first draft Clarke asked to be checked, not a finished one.
2. Known, already-flagged gaps, not forgotten: the mesh (#31) still has no name;
   Permissions (#24) states the principle but isn't a full per-billet ×
   per-operation matrix; Collective and Shell & Utilities are reserved but empty;
   `type` heterogeneity for vessels is a named future seam, not designed.
3. Audit roadmap.md/vision.md/philosophy.md for any reference to the old
   system-model.md numbering (1–33) or the old "Mojo Policy" name — none checked
   yet, and the renumbering makes any such reference wrong until verified.
4. Once the draft is dialed in, Collective is the next real thing to build out
   (deliberately deferred until Fleet is solid) — the recursive shape and the
   Fleet-vs-Collective definition are already settled, nothing inside it is.

## 2026-07-06 — Continuing the tier work: Keel, a real permissions split, generational roster, Collective replacing a fixed Armada tier, and checking whether any of this already exists (short answer: the ambition does, Urbit; the mechanism doesn't)

**Status:** system-model.md is still unwritten as of this entry — every session so far has stayed in discussion because Clarke explicitly wants to build the actual rewrite together, piece by piece, not receive a batch-drafted file. Next real step is starting that write, framed as "Mojo Policy" (see below) — not yet begun.

**Keel** — the tier-1 gap surfaced by asking "what makes this the same vessel across every other change." A persistent per-vessel identity, established once (at first join), surviving OS reinstalls and billet reassignment, retired rather than transferred if the vessel is ever Removed. Named for a ship's keel — traditionally its true birth, separate from the christening/renaming that can happen later and doesn't change what ship it is. Real mechanism, not just metaphor: this is what Tailscale's own node identity already has to be (a keypair, not a hostname/IP) for the mesh to survive a reinstall or rename at all — Keel doesn't invent anything, it names what already has to exist. Technical parallel: an inode number, stable across directory renames, distinct from the path currently pointing at it.

**Permissions turned out to split across both tiers, not sit in one.** Two different questions were being asked as if they were one: *what could this vessel ever be trusted with* (fixed by Ownership + Attestation alone, no fleet context needed — a rented-open box can never legitimately hold genuinely sensitive data regardless of what role you'd assign it) versus *what is it actually allowed to do right now* (a consequence of its current billet, moves automatically if the billet changes). First is tier 1 — a new **Permission ceiling** entry, Derived alongside Capability, computed from Ownership/Attestation rather than hardware. Second is tier 2 — promoted to its own visible **Permissions** subsection under the fleet, not buried in Roster's prose, since Clarke was right that this is close to the whole ecosystem's trust design, not a footnote. Worth getting the technical parallel right too: this isn't classic Unix rwx bits (those are *stored*, static data) — it's closer to a capability-based or MAC system (SELinux-context-style), since the ceiling is computed from context rather than a flag you chmod.

**Roster becomes generational, Nix-style, not just logged.** Connects to reversibility as a value Clarke wants generally: rather than the billet table being one current-state snapshot, it's a sequence of generations — same mechanism #20 already established for one machine's system config (atomic, reversible), just applied one level up to fleet membership instead of packages. A bad Switch-Flagship becomes something you can roll back from, not something you're stuck with.

**Mount points sharpened: the test is identity + fleet participation, not ownership.** Charter isn't a mount despite being rented — it gets a temporary Keel, gets billeted, joins the mesh, participates in signal flags. Mercenary is the only real mount: no Keel, no billet, no mesh membership, a bare external call referenced only by the routing policy. Ownership (tier 1) and mount-vs-member (tier 2) are separate axes, not the same fact viewed twice.

**Content genuinely splits into two different kinds, and one of them was filed at the wrong tier.** *Reflected* content (abridged log, manifest, decoy cargo, full hold) is never standalone — it only means anything as a view onto #10, correctly tier 2. *Originated* content (task history, uptime/connectivity history) is authored by the vessel itself, about itself — genuinely standalone, which means it was wrong to file the watch bill under tier-2 Accounting. Consolidated into one tier-1 concept, **the local ledger** — real `wtmp` precedent, which already mixes boot events and session events in one log format. Then Clarke connected it further: the ledger is also where #14's "records locally instead of writing to #10 directly" behavior actually lives — pending-reconciliation state is a third kind of entry in the same ledger, not a separate mechanism. Real parallel sharpens to a write-ahead log / local git commits not yet pushed — which is #5's already-chosen parallel, just now clearly the same substrate as the ledger rather than a second idea living next to it. Tier-2 Accounting stops being a storage location at all — purely the collation mechanism, a fleet-wide view built by querying every vessel's own ledger.

**Live-state migration (mid-task, moving a running process between vessels) — deliberately not solved, checkpoint/resume instead.** Same honest tradeoff reasoning as Cancel: no execution engine exists yet to migrate anything within, and the mature prior art for true live migration (Sprite, LOCUS, MOSIX) is exactly why that lineage stayed research-grade rather than production-standard — fragile even when it works. Checkpoint/resume (save state periodically, pick up from the last checkpoint if interrupted) is the cheap, well-precedented version Clarke wants instead, and it's not a new mechanism either — same shape as #14, applied to in-flight task state instead of log writes.

**Heterogeneous vessel types — real, confirmed by two independent cases, still deferred.** Sensors/peripherals (vision.md's "connects to everything: your devices, your work, your life" implies something that can't run mojo-agent and holds none of #10 — closer to a Unix device file than a regular file) and, separately, Collective-level role-filling AI agents that aren't anyone's First Mate (already in ideas.md under Collectives, just not connected to this until now). Both real, neither in scope for v0.1 — noted so the eventual `type` axis has a real seam to slot into rather than needing a retrofit.

**Link count isn't always 1 — corrected mid-conversation.** True within one fleet (a vessel holds exactly one current billet in its own roster). Not true across a Collective boundary: Armada's own existing language ("references fleets without owning or absorbing their contents, same as a directory entry pointing at an inode rather than duplicating it") was already describing a hard link and nobody had named it as one until now.

**Armada isn't a fixed Tier 3 — it's "Collective," genuinely recursive.** Real Unix directories nest to arbitrary depth using the same rules at every level, no different name per depth. Clarke confirmed sub-collectives within collectives should be possible, which means a fixed "Tier 3 = Armada" was the wrong shape — there's one recursive concept, a Collective, whose members can be Fleets, other Collectives, or standalone task-scoped agents, to whatever depth is actually needed. Armada is probably just the nautical/colloquial name for a Collective, not architecturally distinct from one. Also sharpened the base case precisely: a Fleet is exactly one Captain + one First Mate (+ vessels); a Collective is multiple Fleets, each still fully sovereign, plus possibly those standalone agents.

**Named what system-model.md itself actually is: our POSIX, not a metaphor for one.** The real Unix precedent for "an abstract behavioral contract with multiple concrete implementations, rationale kept separate from spec" is the VFS layer (inode/file/dentry/superblock operations any filesystem driver implements — ext4 and 9P both satisfy the same interface without knowing about each other) plus POSIX itself (specifies behavior, not implementation; keeps rationale in a non-normative annex, exactly the shape "Technical parallels" already had before this session). Decided: system-model.md *is* our POSIX-equivalent — the contract any vessel/fleet must satisfy, independent of implementation (MojOS / bring-your-own-NixOS / Foreign are three drivers against one interface, which is the actual mechanical reason the OS-posture axis works the way it does). Landed on calling the contract itself **Mojo Policy** and anything satisfying it **Mojo-compliant** — explicit that this is an original spec borrowing Unix's proven *method*, not a claim of literal Unix/POSIX compliance or lineage.

**Checked whether any of this already exists rather than assuming it doesn't.** The broad ambition — personal, sovereign AI, own your own server — is real and actively pursued, most visibly by Urbit (currently marketing itself explicitly as "Sovereign Intelligence," self-hosted personal server/OS/identity system). Worth Clarke actually reading Urbit's Azimuth identity spec directly at some point, as the closest real system that's had years of actual users to find its cracks — not as a template to copy. But the actual mechanism is different: Urbit's ship hierarchy (galaxy/star/planet/moon/comet) is an address-space/sponsorship system, nothing like billeting or a trust ladder. The one real overlap is identity — Azimuth is genuinely close to Keel (a portable identity independent of the physical hardware running it). No existing named system turned up combining role/billet assignment + an owned/rented/mercenary trust ladder + a Unix-VFS-shaped contract the way this one does. Conclusion: the mission has prior art, the specific synthesis doesn't — worth being precise about which claim that actually is.

## 2026-07-06 — The owned-charter question resolves by finding the model was missing a layer: vessels are files, fleets are folders, roles are directory entries not inode properties. Full session, system-model.md rewrite starting next

**Status:** system-model.md is still the pre-session version as this is written — the
rewrite (renumbering from 1, restructured around the tiers below) starts right after
this entry. Everything here is settled and ready to write in, not still open, unless
marked otherwise.

**The actual unlock:** picking up last session's stuck "owned charter" question (a
headless consort that behaves like a charter) by building a real attribute table
across every kind of vessel in the network — ownership, OS, head/headless, state
held, independence, cardinality — and finding that the "owned charter" row was
attribute-for-attribute identical to an ordinary headless consort. The only thing
that had felt different was *how* it gets used (directed via task-routing rather than
sat at), which was never an architectural property to begin with.

That dissolved the immediate question but exposed a bigger one: the model had been
putting "what a vessel intrinsically is" and "what only exists once vessels are
combined" in the same flat list. Explicitly splitting into tiers, Unix-style:

- **Tier 1 — the vessel (a file).** Properties true of one machine alone, no fleet in
  view.
- **Tier 2 — the fleet (a folder).** Properties/mechanisms that only exist because
  multiple vessels are related to each other.
- **Tier 3 — the armada (a folder of folders).** Multiple sovereign fleets. Not built
  out — matches the existing ideas.md framing ("a fleet is the file, an armada is the
  folder") one level up, confirming the recursion is real rather than forced.

**The sharpest consequence of the split: role is not a tier-1 property.** Flagship /
Consort / Tender / Charter were being treated as things a vessel *is*, when they're
really things a vessel gets *assigned* — a directory entry (name → inode) is a
different thing from the inode itself. Named the assignment mechanism **Billet**
(real naval term: an assigned post, reassignable without the thing holding it
changing). Two genuinely different mechanics under one name, not one:

- **Flagship** is a true pointer over the owned pool — reassignment is exactly
  primary-replica promotion (already #4's chosen technical parallel, just not
  previously connected to "here's literally how you change who's flagship"): pick a
  consort already holding a full write-through copy of the log, flip which one
  accepts writes, demote the old one.
- **Consort/Tender** billeting is mostly capability-derived from tier-1 hardware
  eligibility, with a real choice only at the capable end (a laptop *could* be run
  thin).
- **Charter** billeting isn't a repoint at all — a fresh temporary allocation
  (`mktemp`, not `ln -sf`), created and destroyed with the rental. Still occupies a
  billet slot for its lifespan though (a tmp file still has a directory entry while it
  exists) — corrected an earlier draft of this reasoning that had wrongly concluded
  charters sidestep billeting entirely.
- **Mercenary** gets no billet at all, ephemeral or otherwise — not provisioned by the
  user, no lifecycle to track, an external mount referenced only by the routing
  policy per call. The one real "is this a vessel" boundary case, and it isn't one.

Consequence: a device only counts as a vessel once it holds a billet. Raw owned
hardware with no role assigned yet is outside the model, not a vessel-in-waiting.

**Fleet formation is incremental, one vessel at a time — not a bulk declaration.**
Walked into, then partly out of, an over-collapse here: first pass concluded
Commissioning was just "Enlist with nothing to sync from," but that's wrong —
Commissioning bootstraps the log/roster/watch-bill from genuinely nothing (no
mojo-package exists yet to sync), where Enlisting always has an existing flagship's
log to pull from. Real enough difference to keep as two named operations sharing one
step (billet the vessel), not collapse into one. Full flow, flowchart written during
the session (not yet transcribed into system-model.md):

Commission (once, bootstraps everything from empty, first vessel becomes flagship
near-by-necessity) → Enlist (repeatable, syncs from existing flagship, billeted as
Consort/Tender per eligibility + choice) → Remove (flagship case must Switch-Flagship
first — no valid state has zero flagship, not even momentarily) → Switch-Flagship
(requires a caught-up replica candidate, near-zero-downtime cutover) → Charter/
teardown (ephemeral side-loop, doesn't touch the persistent roster).

**Tier 1's actual property set** — went through several wrong drafts before landing
here, each correction from Clarke catching a real category error:

- Attestation/TEE isn't a property every vessel has a value for — it only answers "can
  I verify a third party hasn't tampered with this," a question that doesn't exist for
  owned hardware (you already have root). Not conditionally-null, structurally
  undefined on the owned branch. And even on the rented branch it's not a cached fact
  — real remote attestation is a live per-session cryptographic challenge-response,
  never stored.
- "Hardware capability" isn't a stored property either — the stored fact is the raw
  inventory (CPU/RAM/GPU/storage), and "capability" (flagship-eligible? full-log-
  eligible?) is inference computed from that inventory at decision time, by whichever
  tier-2 policy is asking. Never stored as its own value.
- OS posture isn't fixed (distro-hopping is real) — only Ownership and Head/headless
  survive as genuinely declared, roster-level facts, changed only by a deliberate
  operation (Commission/Enlist/Remove-family, or an explicit reconfigure). Everything
  else drifts under you without any roster event occurring, so it has to be read
  fresh, not cached.

Landed on treating the live-queried stuff as **Plan 9-style — every resource,
including ones on other machines, is a file you mount and read** — which turned out
not to be a new idea to reach for but a confirmation of groundwork already laid: the
project's own SSI research pass (below, same day) already picked 9P/`v9fs` as the
concrete real infrastructure for "unify the fleet's shared records as one namespace,"
rejecting Slurm/Petals/exo for good specific reasons. The vessel-as-file framing
converges on exactly the same answer from a completely different angle, which is a
real consistency check, not a coincidence to wave off.

Final tier-1 shape:

- **Fixed** (roster-level operation required to change): Ownership (owned /
  rented-sealed / rented-open), Head or headless.
- **Live-queried, `/proc`-style** (read fresh, matches #11's already-existing
  on-demand-ping precedent rather than inventing a new mechanism): OS posture,
  hardware inventory, connectivity/uptime.
- **Derived, never stored**: capability, computed from the above by tier 2 at decision
  time.

**A vessel is authoritative for its own history; others read it, they don't track
it.** Real `wtmp` precedent (a machine logs its own boot/connect events locally,
`last` just reads that local record) — extended past uptime to the watch bill itself:
decentralized into one append-only log per vessel, with "the watch bill" as a
fleet-wide view being a runtime collation over those, not a centrally-written ledger.
Safe to decentralize in a way the log (#10) isn't — task-history entries are pure
append, one writer per source, no possible conflict, an actual grow-only CRDT unlike
the log (which explicitly isn't one, per #14's own technical parallel, because edits/
deletions are a deliberate feature). Should carry the same human-review/redaction
capability as the log for the same reason — task history can itself be sensitive
(usage patterns, timing) — append-only by default, deletable only by deliberate human
action. Collation is necessarily partial when some vessels are currently unreachable
— same accepted tradeoff #14/Coda already cover for the log, not a new gap.

**Sync is pull, not push — matches the git parallel already chosen for #5, doesn't
need a new one.** Nobody pushes into a git clone; a consort periodically pulls while
connected, pulls immediately on reconnect. Avoids the flagship tracking subscriber
state or buffering updates for a laptop that's been asleep for three days. But pure
periodic pull is too stale for vessels actively collaborating at the same time — fixed
with a second, much lighter signal-flags operation: **Flare**, a payload-less
pub-sub invalidation broadcast ("something changed, go pull now"), distinct in kind
from the sustained back-and-forth of full signal flags. Real precedent: WebSub-style
cache invalidation, notify and fetch as deliberately separate mechanisms. Scales flat
because the publisher never tracks subscriber count or state — same reason it should
also generalize cleanly to Armada later (an invalidation broadcast + scoped pull), once
Armada has its own answer for what's actually shared across the sovereignty boundary,
which it doesn't yet and isn't being solved now. Correctness never depends on the
flare arriving — periodic pull is the actual floor, flare is a latency optimization
only. The "working at the same time" concurrent-edit case is already the git-merge/
human-review case #14 established, not a new mechanism.

**Open, not yet resolved:**

- The mesh itself still has no name. "Halyards" and "home waters" were both floated
  and both rejected (waters framed it as a place rather than fleet equipment;
  Halyards implied equipment each vessel individually carries rather than a shared
  namespace vessels join — closer, but Clarke didn't want it). Leave unnamed
  (`the mesh`, no proper noun) in the rewrite rather than force a name that isn't
  right yet.
- Cancel (aborting a task mid-flight) — floated twice, parked both times as
  premature, nothing in the model currently promises it as a feature.
- Armada's own shared record (what Flare would actually be invalidating one level up)
  — genuinely later work, roadmap already defers Circus/armada-scale work past the
  current build phase.

## 2026-07-06 — HANDOFF: install-list collapsed from 8 named types to 3 parameterized kinds; new "owned charter" question opened, unsolved, stopping here for the night

**Status for a fresh session picking this up:** [standing-orders.md](standing-orders.md) is
**unchanged** this session — nothing below was settled enough to write in yet.
This entry supersedes the install-list part of the previous handoff (below);
the rest of that entry (naming pass through #21, resolved principles) still
stands, don't re-litigate those.

**What happened:** started working the draft 8-item install list one at a
time as planned, but the real move turned out to be simplifying the list
itself before naming any of it. The 8 items were enumerating every
role × OS × head/headless combination by hand instead of separating "what
kind of install is this" from "what flags does it take." Collapsed to three
install *kinds*, each parameterized rather than hand-listed per combination:

1. **Commissioning** — stands up a brand-new flagship. Always NixOS, always
   headless (forced by role). Deploys the full #21 bundle. Happens once ever
   per fleet, no flags — a singleton event.
2. **Enlisting** *(tentative name)* — a consort joining the fleet. Deploys
   #19, wrapped per OS (`NixOS → MojOS` / `foreign-OS → Jury-rig`). One free
   flag: desktop layer on/off (head/headless) — the only install where this
   is actually a user choice rather than forced by role.
3. **Chartering** *(reusing the existing #7/#8 term as the verb for the
   install event itself)* — a temporary rented instance spinning up. Deploys
   #19 wrapped per whatever the provider supports. Always headless, never
   joins the muster roll (#11), torn down after. Data mode (#15/#17/#18)
   decided separately by the sensitivity judgment (#32), not part of the
   install.

**Foreign-OS wrapper artifact (#22, not yet written into the file):**
tentatively named **Jury-rig** — the real nautical term for keeping a ship
running with an improvised, non-purpose-built repair using whatever's on
hand, as opposed to a proper dockyard-built hull (#20). Liked but not
confirmed as final.

**The actual unsolved problem, worth real thought before resuming:** three
threads opened in the same breath, tangled together, none resolved:

- Is head/headless for Enlisting really "just a flag" on top of one
  identical install, or does it deserve its own build/profile (a genuinely
  different NixOS configuration for a no-desktop server-style box vs. one
  meant to be sat at)? Old #23 (pre-restructure) had modeled it as a
  different profile, not a flag — that tension never actually got resolved,
  just deferred by the collapse-to-3 move.
- Same question on the Commissioning and Chartering-on-NixOS sides:
  floated the idea of a *customized* NixOS build specifically for each of
  those, rather than treating them as MojOS-with-different-flags. Not
  explored at all yet — just named as worth exploring.
- **The big one:** a possible new class, "owned charter" — a headless
  consort that behaves functionally like a charter (task-scoped manifest
  #15 per job, no persistent #13 sync, no independent-offline-operation
  guarantee) rather than a full consort. Surfaced from Clarke's own framing
  ("utilise their entire compute for tasks just like you would with a
  charter") — if this is real, it's not just a flag on Enlisting, it may be
  a fourth install kind or a re-carving of the #5/#6 role boundary itself
  (#5 consort's definition currently requires "independent capability even
  while disconnected" — a headless owned-charter-style consort might not
  need that guarantee at all). This is the thread most likely to actually
  reshape the Roles section, not just the install list — dig into it first
  next session, the other two threads probably fall out of whatever this
  one resolves to.

Nothing named here is final. Pick up by actually sitting with the owned-
charter question before writing anything into system-model.md.

---

## 2026-07-06 — HANDOFF: naming pass through #21 done, "The deployable package" mid-restructure, unsolved

**Status for a fresh session picking this up:** [standing-orders.md](standing-orders.md)'s
one-at-a-time naming pass has #1–21 fully done (name + technical parallel,
written into the file). #22–24 are mid-restructure and NOT yet written — this
is the open problem to keep pulling on, not a finished thing to just transcribe.

**Workflow to keep using:** one definition/item at a time. For each: state the
plain definition, propose a fleet/pirate name, propose a technical parallel
(real prior art), discuss until settled, then write both into system-model.md
immediately — name in the main body next to its definition, technical parallel
in the "Technical parallels" section at the end, same number. Don't batch
multiple items ahead of confirmation.

**The specific unsolved problem:** "The deployable package" section was
conflating two different things — *what artifact exists* vs. *when/how it gets
installed onto a specific target*. Splitting into two: the package section
keeps pure artifacts only (#19 mojo-agent, #20 MojOS, #21 mojo-package —
narrowing #21 to just the bundle itself, no longer describing the flagship-
bootstrap event); a new "Installs" section gets added for the deployment events
themselves. One new artifact still needs adding: a foreign-OS wrapper (#19
wrapped as a non-NixOS host's own native service — systemd unit, launchd plist)
— not written yet.

**Key resolved principles to carry forward, don't re-litigate these:**

- "Headless" (no one sits at it) is fixed *by role* for #4 (flagship, always
  headless) and #7/#8 (charters, always headless) — but for #5 (consort) it's
  NOT fixed by role, it varies by how a given consort is actually used. A
  spare PC nobody sits at is still a consort, just a headless one. This gives
  consorts a real 2×2: NixOS×head, NixOS×headless, foreign-OS×head,
  foreign-OS×headless — all four legitimate.
- The flagship stays NixOS-only, no foreign-OS flagship option — it's the one
  role where real atomic rollback matters most, so that guarantee shouldn't be
  optional there.
- No separate builds per host OS — one build (#19) everywhere, matching the
  microkernel principle already established. Only the *wrapper* differs
  (NixOS module vs. systemd unit vs. launchd plist) and the agent's *runtime
  behavior* differs (via the capability-tier detection principle from earlier
  — full NixOS rollback / partial nix-darwin / no native rollback — the agent
  scales its own caution accordingly). Not a build-time fork.

**Current draft install list (8, not yet in the file, still open to change):**

1. New flagship → mojo-package (#21)
2. New consort, NixOS+head → full MojOS
3. New consort, NixOS+headless → background/pooled compute
4. New consort, foreign-OS+head → existing Mac/Ubuntu, used directly
5. New consort, foreign-OS+headless → existing box, background-only
6. Charter, NixOS-headless
7. Charter, foreign-OS-headless (provider doesn't support NixOS)
8. Charter, full-hold (the #18 fine-tuning case)

Explicitly not solved: none of these 8 have names yet, the foreign-OS wrapper
artifact isn't written, and #21/#22's actual text in system-model.md hasn't
been edited to reflect any of this yet. Pick up right here.

---

## 2026-07-06 — Naming pass: through #16; caught a real gap on hailing-permission along the way

Continuing the one-at-a-time naming pass on [standing-orders.md](standing-orders.md).
Through #16 so far: Vessel, Flagship, Consort, Tender, Sealed charter, Open
charter, Mercenary, The log, The muster roll, The watch bill, The abridged log,
Beyond hailing range, The manifest, Hailing — plus a real technical correction
along the way (#14's reconciliation isn't a conflict-free CRDT, since real
edits/deletions are a deliberate feature via the memory-review/veto UX, not
just append-only growth — it's genuinely git-merge-shaped, conflicts included,
which routes naturally to human review anyway).

One design gap surfaced that's worth its own note: #16 (hailing) as currently
written lets any #7/#8 query the flagship for more context mid-task
unconditionally, but that shouldn't be a blanket yes. Hailing means the charter
holds an open, live connection back to the flagship for the whole task, not a
one-time sealed payload — meaningfully more attack surface than a manifest-only
charter, since a compromised charter with an open channel can keep pulling more
data, where a manifest-only one can only ever leak what it was already given.
Should be a per-charter permission decided at charter time, same kind of call
as the tier-routing policy itself. Landed on the bigger idea this is really an
instance of: mojo-agent deployments (#19) should support real deployment-time
config/flags controlling what a given instance is allowed to do, not just this
one case — captured in [ideas.md](ideas.md), general shape still open.

Going through [standing-orders.md](standing-orders.md) one definition at a time, settling
a name and a technical parallel for each before moving to the next. Captain,
First mate, and Vessel settled so far (Captain: root of trust, not an account —
single-tenant by design, no login concept needed, and deliberately singular per
fleet for the same reason #4 is singular, an accountability invariant that's
also the real boundary where Collectives picks up rather than a gap in this
model. First mate: process migration / Erlang-actor-style location-transparent
identity. Vessel: generic compute host abstraction — had to fix its definition
mid-pass, since "capable of running or hosting the AI" wrongly excluded tenders,
which only ever access the AI running elsewhere).

Once the full naming pass is done, vision.md needs a real rework to actually use
this settled terminology and reflect what system-model.md nailed down more
precisely than vision.md's original prose did (the mandatory-flagship reasoning,
the sensitivity-based data-sharing rule, mojo-package's actual bootstrap-only
composition, MojOS's corrected defining property). Not doing that yet — flagged
here so it doesn't get lost, not attempting it mid-naming-pass.

---

## 2026-07-06 — Checked whether real SSI-family software could implement the model directly: one clear yes (9P), one "not yet" (exo), two no's (Slurm, Petals)

Follow-on to the SSI/microkernel/scheduler parallel below: had it actually
researched whether Slurm, exo, petals, and Plan 9/9P could be adopted as real
infrastructure rather than just kept as conceptual reference, against the
specific pieces of [standing-orders.md](standing-orders.md) each was proposed for.

**Slurm** (proposed for #33, the tier-routing policy) — mature, GPLv2, genuinely
light enough to run on a handful of machines. But it's daemon-only, not
embeddable — you'd run its actual services and submit jobs through its own
CLI. Its whole interaction model is multi-tenant batch scheduling for a shared
research cluster; nothing in it maps onto "charter an ephemeral cloud box" or
"call a mercenary API." HTCondor is the same story. Verdict: keep the
scheduling *concepts* (resource matching, backfill scheduling) as reference —
this is still worth digging into for #33's actual design — but don't adopt the
software itself.

**Petals** (proposed for #28, pooling compute across owned devices) — MIT
licensed, but no release in ~3 years with open issues sitting unaddressed, a
real stagnation signal. Even alive, it was built for internet-scale volunteer
swarms (BitTorrent-style, public contributor pool) — backwards architecture for
"a private handful of my own devices" regardless of maintenance state. Skip.

**exo** (also proposed for #28) — Apache-2.0, genuinely thriving (46k+ stars,
shipping into 2026, a real company behind it), and architecturally almost
exactly right: peer-to-peer auto-discovery, no master/worker, splits a model
across devices by real-time topology/bandwidth. The blocker: GPU acceleration
currently only works on Apple Silicon via MLX; Linux is CPU-only, GPU support
still "under development." On the actual fleet (PCs + a laptop), exo today
would mean CPU-only sharding — probably slower than just running a smaller
model on one GPU alone. Right shape, wrong moment — watch it, don't build on it
yet.

**Plan 9 / 9P** (proposed for #2/#10, the unified-identity/shared-record
property) — this is the one worth actually prototyping. Not "install Plan 9" —
9front is the maintained fork, but it's a whole alternative OS, not something
to bolt onto NixOS. The useful piece is narrower and far more mundane: the 9P
protocol is already in the mainline Linux kernel as `v9fs`, used in production
today for QEMU/VM folder-sharing and WSL. Unifying the fleet's shared records
(#10 the log, #11 the roster) as one namespace over 9P is buildable today with
infrastructure that already ships in the kernel — no museum-piece OS, minimal
new dependency surface. Strongest lead of the four, by a clear margin.

---

## 2026-07-06 — Built the full system model (33 definitions, system-model.md); it turns out to be a Single System Image OS in disguise

Long session working out, in plain language before any metaphor got attached,
every entity/record/rule/deployment-shape/execution-tier the mojo ecosystem
actually needs. Landed the whole thing in a new file, [standing-orders.md](standing-orders.md)
— 33 definitions across six sections (Roles, Records, Context sharing, The
deployable package, Execution tiers, Routing). The naming pass (attaching the
fleet/pirate vocabulary to each number) hasn't happened yet — this entry is the
reasoning that got the model itself locked, not the naming.

Threads that resolved along the way, roughly in order:

**The flagship is mandatory, not optional, and here's why.** CAP theorem forces
the issue: if more than one device can accept writes to the shared record, you
either need consensus (Raft/Paxos-style) or you accept permanent risk of
disagreement. This design sidesteps that entirely by having exactly one canonical
writer — the flagship (#4) — with everything else either a full independent
device (a consort, #5) or a pure access point (a tender, #6). Disconnected
devices append locally and reconcile back in on reconnect (#13/#14), which only
works because there's one authority to reconcile *into*. Consensus algorithms
would only become relevant for a different problem — flagship high-availability/
failover across two physical machines — parked as a future idea in
[ideas.md](ideas.md) under Circus, specifically framed as an accessibility
feature for people without a dedicated always-on machine.

**Two axes were getting tangled and needed separating.** Fleet composition
(what you physically own — flagship/consort/tender, any number, any mix) is not
the same axis as voyage/job sizing (how much resourcing a given task gets —
solo, flagship-routed, pooled, fan-out, chartered, mercenary). Conflating them
was making early naming attempts (e.g. calling a physical laptop a "sloop," the
same word vision.md already uses for job size) collide with themselves.

**Data-sharing rules got more honest.** Originally: attested rental (#7) gets
real data, plain rental (#8) gets placeholder-scrubbed data, fixed by tier. Cor-
rected to: what a task actually needs, genuine (#15) or scrubbed (#17), should be
decided by how sensitive that specific data is (#32, "the sensitivity judgment"
— a new, currently-unsolved piece of judgment, same category of hard problem as
the tier-routing policy itself), not by tier alone. Non-sensitive genuine data
can go anywhere, including mercenaries (#9) — though #7/#8 stay preferred over #9
regardless, mercenaries remain the rare exception. Sensitive data may only ever
pair with #7. Whole-record transfers (#18, e.g. fine-tuning) are the one
exception that isn't sensitivity-gated — always #7, never #8, no matter what.

**mojo-package's actual composition and moment of use got nailed down.** It's

# 19 (the harness) + #10 (the log) + #11 (the fleet roster) together — and

crucially, it's a *bootstrap* artifact, not something in continuous use. A new
flagship needs the whole bundle (there's no other live flagship to sync from
afterward). A new consort just needs the bare harness + MojOS, then syncs its
own partial copy from the existing flagship afterward. A chartered instance
normally just needs the harness plus whatever scoped context the task calls
for — except in the fine-tuning case, which needs the full bundle same as a new
flagship does.

**MojOS's own definition got corrected.** Its headline property isn't "looks
good, nice daily driver" — any OS can claim that. It's that system changes the
agent makes are staged, validated, and atomically reversible, because the whole
system is declared in Nix. Aesthetic/desktop is secondary to that, not the
point of it. Confirmed separately that mojo-agent has to be fully self-
sufficient without MojOS (interface, memory, reasoning all intact on any host,
even macOS) — the one real gap off-MojOS is system-mutation safety, which is a
spectrum (full NixOS rollback > partial nix-darwin rollback capped by Apple's
SIP > no native rollback at all) rather than a binary.

**The last thing that fell out, unprompted, is maybe the most useful:** the
whole model turns out to be a recognized, well-studied shape — a **Single
System Image** distributed OS. Plan 9 from Bell Labs is the closest spiritual
ancestor (every resource accessed the same transparent way regardless of which
physical machine holds it); Kubernetes is the closest modern concrete example
(schedules across heterogeneous nodes, presents one coherent cluster). #19 next
to its host-specific wrappings (#20/#23/#24) is a microkernel-and-drivers
shape, which is exactly why the harness has to be self-sufficient — a
microkernel that depended on one specific driver wouldn't be a microkernel.
And #32/#33 (the sensitivity judgment, the tier-routing policy) are structurally
a mandatory-access-control engine and a scheduler, respectively — both
well-studied problems with decades of prior art to draw on rather than invent
from scratch. Worth treating as a real research lead, not just a nice analogy.

---

## 2026-07-06 — mojo-agent runs on any Unix system via Nix (not NixOS); MojOS's real job is safe system mutation, not looks

Landed on a principle worth following deliberately from here on: mojo-agent isn't
a MojOS-exclusive thing, because Nix (the package manager) and NixOS (the whole
operating system) aren't the same thing. Nix runs standalone on any Linux distro
and on macOS via nix-darwin — a machine doesn't have to *be* NixOS to run a
Nix-packaged agent on it. So mojo-agent has to be self-sufficient by design: it
brings its own interface, memory, and reasoning regardless of host OS, and doesn't
depend on MojOS for anything functionally required (already implied by the
multi-distro idea sitting in [ideas.md](ideas.md)).

That forced a correction to what MojOS's own defining feature actually is. "Looks
good, nice daily driver" isn't it — any OS can claim that. The real headline
property: system changes the agent makes are staged, validated, and *atomically
reversible*, because the whole system is declared in Nix. That's the thing nothing
else gives you by default, and it should lead MojOS's definition, not trail it.

Follow-on: the agent needs to know, at runtime, which safety tier it's actually
running under, and behave differently depending on it — not a blanket "MojOS: can
mutate the system, anywhere else: can't." Three real tiers, each with a genuinely
different rollback guarantee: full NixOS (MojOS) gives real atomic rollback for
anything it touches; nix-darwin on macOS gives partial declarative rollback,
capped by whatever Apple's System Integrity Protection still allows to be managed
that way; plain Nix on Ubuntu/Fedora, or bare macOS without nix-darwin, gives the
agent's own package reproducibility but no native rollback for anything it touches
outside that package's footprint. Rather than disabling system mutation outright
on tiers 2–3 (which would gut the "capability gap" pitch for anyone not running
MojOS), the right shape is probably: confirmation-gate strictness and the agent's
own precautions (e.g. taking its own defensive snapshot before something genuinely
hard to undo) scale up as the safety tier gets thinner. Not built yet, but the
principle — never claim a safety guarantee the current host can't actually back
up, and degrade gracefully instead of turning a capability off entirely — is
worth carrying into every future capability decision, not just this one.

Small correction to the entry directly below: it talks about hardware scaling as
"swapping in a better model," at one point loosely as "a frontier-tier model on a
chartered instance." That conflates two things that have to stay separate —
mercenaries (frontier models) are never part of the hardware-scaling axis at all;
they stay walled off to scoped questions only, full stop. The scaling axis the
entry below is actually reaching for runs through local models and Ephemeral
Commons' chartered *open-source* models only.

---

## 2026-07-06 — Nix as the take-it-with-you layer; scaling should be a model swap, not a harness change

Two things landed talking through sops-nix. First, the mojo-package idea got its
real framing: it's not primarily about Ephemeral Commons, it's about
[vision.md](vision.md)'s "every house has a server" implying you eventually *move*
house and the server underneath you changes — Nix flakes are how the whole AI
(config, secrets via sops-nix, the works) travels to the new box instead of getting
rebuilt from scratch. Ephemeral Commons chartering is just a bonus of the same
packaging: beef up the resource spec in the same flake and it stands up mojo-agent
on rented compute too. Decided sops-nix should encrypt secrets everywhere, own
hardware included, not just the rented case — no downside to treating local as
equally untrusted-by-default.

Second, and the bigger open question: mojo scaling to hardware (a bigger server, or
a chartered rented instance) should ideally only ever mean swapping in a better
model for reasoning — the harness and the deterministic aspects (recipes, the
confirmation-gate pattern, whatever mojo-agent's structural layer ends up being)
shouldn't need to change with it. That's the same escalation-policy problem already
sitting open in [roadmap.md](roadmap.md)'s chartering section (deciding when a job's
worth chartering up), reframed as: get that router right and hardware scaling
becomes free — more compute just means better answers, not a different system. Not
fully sure the harness genuinely never has to change as the model gets bigger
(bigger models can support different kinds of harness, e.g. longer tool-use chains,
more autonomy) — worth stress-testing once there's a real harness to test it
against, not resolved now.

---

## 2026-07-05 — Locked: Hermes wholesale, no deadline, agent-first bootstrap

Talked through yesterday's Pi/OpenClaw/Hermes landing point again and it moved:
building mojo-agent's own core loop on Pi is the wrong first step. The fastest way
to an actually-working Jarvis is to run something that already exists —
[Hermes Agent](https://github.com/NousResearch/Hermes) (self-hosted, MIT, persistent
memory, a learning loop that writes and improves its own skills) — wholesale,
wrappers and all, rather than building a loop from scratch and reimplementing what
Hermes already does. mojo-agent proper doesn't get designed yet; it gets designed
once living with Hermes shows what it actually gets wrong for me. Pi and OpenClaw
stay as reference material for that later design, not the v0.1 substrate.

Two things fell out of that that are v0.1 requirements no longer: the confirmation
gate before the agent touches the system, and delegating to a mercenary (Claude
Code or another frontier model). Not dropped — deferred to whenever mojo-agent
proper gets built, since that's exactly the kind of functionality it should have
natively instead of as a wrapper bolted onto someone else's harness. Also decided:
don't pre-design the memory architecture. Point Hermes at whatever it defaults to
locally and let using it teach me what "editable brain" needs to mean — could end
up plain files, could end up Knowledge-object-style structures, could end up a
vector store with a translation layer for viewing/editing. Not deciding that ahead
of using it.

Also dropped the Sept 15 deadline framing, though not the date itself — I'm away
from my PC until then regardless, laptop is the summer machine, PC becomes the true
always-on host when I'm back. That's a fact about the hardware, not a target to hit
features by. The reason a deadline got written down in the first place (the 2026-07-03
reset entry, below) was a real pattern: months of planning, no runnable code. Keeping
that countdown pressure risked becoming its own kind of over-planning — a checklist
substituting for actually building. Progress now is "is Jarvis running and getting
used," continuously, not seven checkboxes due by a calendar.

Last piece: flipped the build order inside MojOS itself. Original plan was full OS
(modes, rice, daily-driven) before the agent goes in. Better: smallest possible NixOS
flake that boots and runs a resident service, Hermes installed on top of that
immediately, then use Jarvis itself to help build out the rest of MojOS — modes,
theming, daily-driving it — instead of hand-building all of that first and only
getting agent help at the end. Bootstrapping with the thing being built, as early as
the thing can bear weight.

Filed into [roadmap.md](roadmap.md)'s Now section. Circus/chartering/mercenaries
(the pirate-ship compute model) untouched — still Horizon, nothing to wire in until
there's a single working agent worth routing from.
[ephemeral-commons.md](ephemeral-commons.md)'s content is already reflected in
roadmap's Horizon section; keeping the file around for now rather than deleting it.

---

## 2026-07-04 — Candidate harness substrate: Pi, under OpenClaw and Hermes

Closing thread for the day. Went looking for what to actually build mojo-agent's
core loop on, now that Anthropic's own SDK is correctly off the table — building
the first mate's harness on a frontier vendor's proprietary tooling would recreate
exactly the dependency the project exists to avoid, sovereignty claim or not.

Two real, current open-source personal-agent products exist already: **OpenClaw**
(MIT, fast-growing, connects to messaging apps, orgs "own the full harness" per
Nvidia's own writeup) and **Hermes Agent** (Nous Research, MIT, self-hosted, a
built-in learning loop that writes and improves its own skills from experience,
persistent memory). Worth studying both, neither obviously the final answer.

The better find: **Pi** (`earendil-works/pi`) is the substrate *under* OpenClaw, not
a competitor to it — a genuinely minimal agent loop (one write-up: 418 lines,
reportedly outperforms thousand-line frameworks), a unified LLM layer, tools in,
results back, nothing else. OpenClaw adds the product layer — channels, queueing,
persistence — on top of Pi's bare loop. That's a better shape to build mojo-agent's
own core on than either full product: small enough to actually audit (matters for
"provable, not pledged"), unopinionated enough to bend around mojo's own
memory-as-plain-files and fleet/routing model instead of fighting someone else's
assumptions baked in.

Landing point, not a decision: mojo-agent's core probably gets built on something
Pi-shaped, borrowing specific ideas from OpenClaw and Hermes Agent (Hermes's
self-improving skill loop especially) rather than adopting either wholesale. Not
resolved — wants a real side-by-side before committing. The build target underneath
all of it stays simple regardless of which substrate wins: one agent, running on
MojOS, holding the second brain, doing fleet/mercenary routing when a job actually
needs it.

---

## 2026-07-04 — The fleet, chartering, and mercenaries (supersedes "Scallywags" below)

Kept pulling on the pirate metaphor from earlier today and it resolved into
something better than the tier ladder I wrote a few hours ago. Recording the shape,
not the whole back-and-forth.

**Captain and First Mate never fully merge, on purpose.** The long-term goal is that
it feels like one person — the first mate develops me, takes the boring stuff off my
plate, teaches so I stay accountable for what's actually happening rather than just
supervising it. But the seam stays real underneath the feeling: the first mate
defers to me on anything that matters and never substitutes its judgment for mine.
That's not a limitation to smooth away later — it's what keeps "who's accountable
here" answerable even once daily use makes the line invisible. Same instinct as
Collectives' "always traces back to a human," just one layer down.

**"Scallywags" was wrong — it's not a hired outsider, it's a bigger boat.** If it's
literally the first mate's own weights and harness running on rented hardware for a
few hours, that's not a separate trust tier, it's the same first mate wearing a
bigger hull. Collapsed the model to: one fleet, many ship classes — sloop for
something small and local, bigger hulls pooled from owned machines (the Circus), up
to a chartered frigate or galleon when a job needs more than anything owned gives.
Chartering is the verb for renting the bigger hull, same nautical sense as it always
had. Fleet command — the memory, the orchestration, the judgment — always lives at
home, on the ringmaster (or, before there's a dedicated one, whatever single machine
is doing the job) — only the hull doing the actual thinking changes underneath it.

**Mercenaries are the one real outsider**, and the distinction that actually matters:
is this me, temporarily bigger (fleet, any class), or is this someone else's model on
someone else's infrastructure that doesn't get torn down when I'm done with it
(mercenary)? Mercenaries get a fragment of the charter — the minimum context, real
values swapped for stand-ins — report back, never board anything. The defence isn't
"they'll forget," it's "there's less worth remembering" — a frontier vendor keeps
whatever they're handed regardless, so the only real lever is shrinking the fragment.

**Keeping it feeling like the same first mate across ship classes isn't free** — a
bigger model is a genuinely different mind, not just a stronger one, so "same first
mate, bigger hull" is a design target, not something the metaphor grants automatically.
Landed on: a personality LoRA per model family, carrying voice and judgment style —
never memory, which stays in the plain-file brain and gets retrieved fresh regardless
of hull. Conflating the two would mean correcting a fact requires retraining instead
of editing a file, which breaks the whole "brain as plain files, always correctable"
premise. Training and maintaining the LoRA library is MojOS's job — and training one
is itself a chartering-sized job (rare, occasional, worth the escalation), a decent
sign the pieces actually fit rather than being bolted on separately.

Same open question as before, just renamed: the escalation policy (when chartering
up or hiring a mercenary is actually worth it) is still hand-decided for now, still
the thing worth training a router on later. Filed the updated shape into
[vision.md](vision.md) and [roadmap.md](roadmap.md); [ephemeral-commons.md](ephemeral-commons.md)
is now scoped specifically to chartering.

**Addendum, same day:** three corrections after talking it through further. First,
every ship class gets the whole brain, chartered or not — that's the actual
definition of fleet vs. mercenary, not a detail; nothing about renting a bigger hull
should ever mean less trust. Second, the LoRA is the long-term fix, not the plan for
now — short term is just system prompts carrying voice and judgment, cheap and no
training required, upgraded to a LoRA per model family later if prompting alone stops
being enough. Third, the escalation ceiling is mine to set, not just the first mate's
to judge — "never hire a mercenary, full stop" has to be a real, respected setting.
Whatever tools exist, how much they get used is always my call, not a default the
system talks me out of.

---

## 2026-07-04 — Naming the ladder, and folding it into vision.md

Picked this idea back up and pushed it from "a doc that exists" into the actual
vision/roadmap. Landed on a cleaner shape than the original four-tier draft:

Local and Circus were never separate trust tiers — Circus is just more owned
hardware, so it folds entirely into **the first mate**: everything I own outright,
zero exposure. Beyond that, two tiers, named properly instead of left as loose
descriptions: **Scallywags** (rented, ephemeral, optionally attested — hired
dockside, let go after the job) and **Mercenaries**, not "crew," for the frontier
models — a crew boards the ship and has standing access; these get one mission, the
minimum context it needs, and never come aboard. That distinction matters: the whole
point is they can't access anything beyond what's handed to them for that one job.

Checked the pricing on Scallywags for real: confidential/attested GPU rental
(confidential.ai, TEE via Intel TDX + AMD SEV-SNP + Nvidia CC) runs about 30–50% over
plain rental for comparable hardware — H100 $2.00/hr attested vs $1.33–1.55/hr plain.
Expected an order-of-magnitude privacy tax; it isn't one. Changes this from "someday,
if confidential computing gets cheap" to "buildable now, cost is a rounding error
against the value."

The real open question, both times it came up: *when* is escalating tiers actually
worth it — the cost, the exposure, against how much better the answer gets. No
ground truth ahead of time, since you often don't know a higher tier would've done
better until you've run it there. Answer for now: decide by hand. Real answer,
later: train a router on accumulated first-mate decisions (task → tier → was it
worth it) once there's enough real use to learn from — same shape as "the first mate
learns how I think," not a bolt-on. Not a Sept 15 problem. Filed as the open research
question in [roadmap.md](roadmap.md)'s horizon section.

**Addendum, same day:** the first mate owns the budget too, not just the per-job
tier call — which mercenary subscriptions are actually worth keeping, what Scallywag
rental costs over time, tracked with the same discipline as what got seen. And the
expected shape of usage across the ladder should be lopsided on purpose: nearly
everything local, Scallywags for genuine load, mercenaries rare — an exception path,
not a habit.

---

## 2026-07-04 — Scallywags, and the ephemeral compute idea

Two things surfaced today, from watching Claude Code's own subagent tooling and
riffing on it.

First, a language match, not a new idea: the fork-vs-fresh-agent distinction in
Claude Code's Agent tool — a fork inherits full context so you hand it a directive,
a fresh agent gets briefed cold with everything it needs and nothing it doesn't,
and the rule "never delegate understanding" (do the thinking yourself, then dispatch
a scoped task) — is exactly the first-mate/crew relationship already in
[vision.md](vision.md). Good confirmation the metaphor holds up mechanically, not
just poetically.

Second, an actual new idea, chasing "what does the crew look like before I trust
any frontier vendor with my data." Landed on: an ephemeral compute tier — rent
open-source-model hosting anonymously (alias, crypto, VPN/Tor) and tear it down
after, so the coordinator can borrow datacenter-class compute without a company
ever holding the data *or* knowing it was mine. Checked it against reality: the
individual pieces already exist — DePIN GPU marketplaces (Akash, io.net, Nosana,
Clore — a $19B+ category) do anonymous rental, and TEE-based confidential
computing (Phala on OpenRouter, NEAR AI Cloud, Spheron, Nvidia's own Hopper/
Blackwell CC mode) does attested "not even the host can see it" inference. Nobody
combines both, aimed at an individual's privacy rather than enterprise compliance
— that's the actual gap, and it's a positioning gap, not a technology one.

Caught myself almost getting the emphasis backwards: the ephemeral tier is
plumbing, not the differentiator — it's explicitly stateless (rent, use, forget),
while "remembers you" requires the opposite, durable local memory. The first
mate's judgment (what's routine enough for local, what needs the tier, what to
strip before anything leaves) is still the hard, unbuilt, actually-mine part.
But it's also the most novel and most personally interesting thing I've got right
now, and there's a real business shape on top later — not owning datacenters
(wrong shape, capital sink) but the broker/attestation layer over hardware that
already exists, same move every DePIN player already makes.

Real risk, flagged and not resolved: anonymity-as-a-feature inherits Tor's
problem — legitimate, valuable, and permanently associated with whatever the
worst users do with it, which makes payment processors and banks skittish
regardless of the real user base. Would hate to build something that mainly
enables the wrong things. Needs a real answer before this becomes a product, not
just a vibe.

Landed somewhere, in the end. The key unlock: don't build the marketplace. Full
decentralization of datacenter-level compute is never happening — power, cooling,
capital don't bend that way — so stop trying to be Akash. Just be a privacy-
obsessed *client* of whatever GPU supply already exists, centralized or
decentralized, anonymized and attested on the way in. That drops the "entering a
$19B funded category" problem entirely — you're renting from it, not competing in
it.

And the long-term shape holds up: everyone runs their own local server for memory
and coordination, forever, and reaches for the ephemeral tier whenever a job needs
more than home hardware gives. That gap between "what my server can do" and "what
the frontier can do" never closes — it just moves — so this isn't a stopgap until
local hardware catches up, it's the permanent architecture. It's also not a novel
bet: it's cloud bursting, a proven pattern, with a new trust layer bolted on. The
first mate's judgment is still the unbuilt, actually-hard, actually-mine part —
this is the answer to "how do I get the compute without trusting a company," not
to "what makes Mojo worth having." Both still need to exist.

This is the one. Chased some version of this for months without landing it —
this time it's scoped small enough to actually build and big enough to matter.
Where it sits against the frozen 2026-07-03 core (ahead of it, alongside it, or
after) is still open — picking back up when the summer goals get replanned.

**Addendum, same day:** dropped anonymity as a goal entirely. Fine if a provider
knows it was me who rented the box — the only thing that has to stay unseen is
the logs, the actual content. That kills the "Tor for AI compute" framing (Tor
hides *who*; this only ever needed to hide *what*) and the doc's one-liner and
phasing got fixed to match. A Tor-style layer is still a real idea, just not
this project's — someone can bolt it on later if they want it.

---

## 2026-07-04 — Sovereignty, not abstractly

Talked through an ADHD/cannabis pattern with Claude tonight — the kind of thing worth
reasoning through carefully and tracking over time before a prescriber conversation.
Mid-conversation, hit the obvious problem: that reasoning now sits on Anthropic's
servers, and I don't get to decide who else ever sees it. Philosophy.md has said "data
ownership is decision-making power" for a while now; this is what that looks like when
it stops being abstract. Not a reason to stop using frontier models today — mojo-agent
doesn't exist yet, and Claude is still the best contractor available for this — but a
real, personal instance of the exact problem the project exists to fix, not a thought
experiment. Tracking doc lives outside this repo: `~/documents/adhd/`.

## 2026-07-03 — The reset

Stepped back and admitted the pattern: months of framework sessions, docs
restructures, and meta-work arranging thinking — and no runnable code anywhere in the
org. The vision was never the problem. The sequencing was. A first-year with a
manifesto is invisible; a first-year with a manifesto *and a machine that actually
runs it* is a different thing entirely. Proof, not promise — my own rule, finally
applied to myself.

So: reset to build-first. This repo becomes exactly what it should have been — the
thinking repo. Seven flat files (vision, philosophy, roadmap, ideas, devlog, README,
AGENTS), written in my voice, public, kept current. Everything else — the old `raw/`
corpus, the 9-chunk restructure tracker, the framework session briefs — distilled
into these files and deleted; git history keeps every byte if we ever need to dig.

The new goal, frozen: by 15 September — back at my PC for second year — the first
version of an always-on Jarvis exists. MojOS (NixOS-based) as my daily driver on the
laptop I have with me all summer, mojo-agent v0.1 — my first mate — resident inside
it, with phone access and voice as the surfaces once the core holds. The day I'm
back, it all deploys to the PC (dual-boot with Windows), which becomes the true
always-on host. Finish line in [roadmap.md](roadmap.md): July is Nix and the OS in a
VM, August is the laptop for real plus the agent core, September is surfaces and
hardening. The Circus and the collectives stay on the horizon, alive in
[vision.md](vision.md), untouched until the foundation is real.

Also settled today: the essays (craft problem first) come later and get written from
these files; the dotfiles repo's contents dissolve into the mojos flake when the rice
gets rebuilt; and the old Hyprland setup is not being ported — it's being replaced.
