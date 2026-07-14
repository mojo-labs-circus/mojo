# How a seam gets found

The procedure for going from the (mostly) settled piece list to a (mostly)
settled seam list. Derived, not assumed, from working through what a seam is
actually for. Findings from walking this procedure land in
[seams.md](seams.md), one entry per seam, not in this file.

## What a seam is for

Most interactions inside a system aren't seams. Inside one piece's own
implementation there are plenty of internal calls, and MSI doesn't standardize
any of them; that's implementation-private, exactly what anatomy.md already
says about Harness ("its insides... are deliberately not standardized;
harnesses compete there"). A seam is specifically the outward-facing surface:
the exact spot where something built by a completely different team, who has
never seen this codebase and never will, needs to interact correctly anyway.

The bar a seam has to clear is blind interoperability. Two implementations,
built independently, zero coordination, working correctly together the first
time they meet, and continuing to work after either side gets replaced by a
competitor. TCP/IP does this. POSIX does this. HTTP does this. None of those
are examples of good code; they're examples of a boundary written down
precisely enough that neither side has to trust, know, or read the other
side's internals, only the contract between them. "Every Mojo piece designed
to be outcompeted" only means something real if the boundary a competitor has
to hit is written down to that same standard. A fuzzy seam makes that a lie in
practice, since the only way to build a competing implementation becomes
reverse-engineering whatever the reference implementation happened to do.

## The six problems

Strip away everything a same-process function call gets for free (same
memory, same version, same author, same trust, always running), and six real
problems are left, and only these six:

- **Shape** — do both sides agree on the representation of what crosses.
- **Vocabulary** — do both sides agree on what operations exist and what they
  mean.
- **Discovery** — how does one side even find the other, know it exists, know
  what it currently supports.
- **Versioning** — the interface changes over time, and the two sides don't
  redeploy in lockstep; skew (an old implementation and a new one talking at
  the same moment) is the normal case, not an edge case.
- **Trust** — should this even be allowed, should what's said be believed,
  independent of whether it was understood correctly.
- **Liveness** — is the other side even running right now, and did a call
  that went out actually land; "not currently reachable" and "outcome
  unknown" are both normal states across a seam, not crashes. The second
  half is the sharper one: a call can fail midway, and the caller may never
  learn which side of the operation the failure landed on. That
  indeterminacy is what forces idempotency and retry semantics into a
  contract, and it is not derivable from "the other side may be down".

Underneath all six sits one root fact worth naming: the two sides are
independently administered. Nobody upgrades them together, nobody vouches for
both, nobody can see both ends. Independent administration is why skew is the
normal case (versioning), why the other side can't be assumed benign or
truthful (trust), and half of why discovery has to be live rather than
configured. Deutsch's fallacies call it "there is one administrator";
Conway's law is the same observation from the other end, seams land where
independently-governed parties actually meet.

The framework was checked against real prior art on 2026-07-14 (the eight
fallacies of distributed computing, the end-to-end argument, Parnas's
information-hiding criterion, the hourglass model, Hyrum's law) and held: the
six are contract questions where the fallacies are channel physics,
complementary rather than overlapping. Findings and citations in
[standards-research.md](standards-research.md).

Not every problem matters for every seam. Which ones bite is a consequence of
the seam's structure, not a fixed weight — that's what step 2 below is for.

## The procedure

Prior art runs throughout this procedure, not just at step 5. anatomy.md's
own prose is a starting point, never the whole check: this project
standardizes what real systems already do, not what sounds structurally
clean on paper, so any claim doing real work — especially a structural
shortcut used to settle more than one pair at once, the way "the Owner only
ever reaches Client" settled ten pairs in one move — gets checked against
real systems before it's accepted, not asserted from the doc alone and
rescued with research later only if it feels shaky. The standing prior art:
the five digital-counterpart-shaped systems already used for the piece-list
check (OpenClaw, Hermes, Khoj, Letta, OpenFang) for how real systems shape
this kind of relationship day to day, and the deeper precedent already
archived in research-plan.md (POSIX, seL4, LSP, Kubernetes' CRI/CNI/CSI and
admission/RBAC, OCI, Matrix/XMPP) for the protocol shape once one's needed.

**1. Existence pass.** For every pair of pieces, does a real fact or action
ever need to cross at all? Ground this in anatomy.md's own prose per piece,
then check it against prior art before it's treated as settled, same as
everywhere else in this procedure. Most pairs are a fast no. Lives in
[piece-matrix.md](piece-matrix.md).

**2. Seam-worthy filter.** For every pair with a real crossing: does MSI
actually want independent implementations to interoperate here, or is this
one piece's internal wiring mislabeled as two pieces? In most systems this
filter does real work; in this one it will mostly say yes, since the design
already commits to "everything but the data swaps." Keep the step anyway —
Owner is a person, never swapped, and relationships from Owner may not behave
like the rest.

**3. Structural classification.** For every surviving crossing, answer four
questions:

- Same machine/process, or across machines?
- Cardinality on each side: at the moment this crossing happens, is each
     side exactly one live instance, or an open/plural set the other side
     picks from or broadcasts to? (Not how many vendors build the piece —
     that's always plural by design. How many are live and reachable right
     now from this crossing's seat. Already marked in anatomy.md itself:
     Kernel, Credential broker, Provenance, and Fleet manager are each "one
     live instance per machine"; Model endpoint, Tools, Memory provider, and
     Sandbox are explicitly plural, a roster a run draws from.)
- Does one side hold authority over the shared fact, or are both sides
     peers?
- Does a fact survive after this interaction ends, or does nothing remain
     once it returns? Not "is this crossing important", every seam matters.
     Whether the interaction leaves something behind that outlives it: a
     token issued, a record written, a membership changed. Harness to Model
     leaves nothing, the response is consumed and gone. Harness to Identity
     leaves a changed record. Fleet to Fleet leaves changed shared state.
     This is what step 4's trust and versioning weight actually tracks: a
     fact that outlives the crossing carries stakes a transient one doesn't,
     since something can go stale or turn out to have been wrong after the
     interaction that created it is long over.

**4. Load-bearing problem set.** From those four answers, work out which of
the six problems actually matter here and which basically vanish. Same
machine kills discovery and liveness almost every time. A singular-authority
side talking to a plural/broadcast side makes discovery and vocabulary do
real work. Two peers of equal standing turns "trust" into "is this still a
member" rather than "is this legitimate" or "is this truthful."

**5. Shape match.** Match the load-bearing problem profile against real,
shipped precedent that already solved that exact profile, rather than
inventing one. The taxonomy, all already researched (see research-plan.md's
archived findings for the sourcing):

- **Static shared format, no live conversation** — a shared data shape
     both sides just read and write, no handshake at all (OCI image-spec,
     POSIX base definitions).
- **One boundary, many operations, open caller set, static conformance**
     — a single piece's surface decomposed into named operations anything
     can call (POSIX syscalls).
- **One caller, many independently-built providers, live negotiation once
     per session** — a capability handshake at session start, unknown fields
     ignored for forward compatibility (LSP/DAP).
- **One orchestrator core, many vendored plugins, live negotiation per
     call** — capability RPCs checked on nearly every invocation, not just at
     startup (Kubernetes' CRI/CNI/CSI).
- **Peer replication among equals, deterministic conflict resolution** —
     no single caller or provider, both sides hold copies of shared state and
     converge (Matrix federation, git).
- **Lease and grant, capability-based authorization** — an authority hands
     out a scoped, revocable token; the holder proves the grant, no live
     decision-maker consulted per use (seL4, AWS STS).

A shape match also has to respect the physics of the boundary, not just its
problem profile: latency, bandwidth, and round-trip count are real costs, and
a design that pretends a remote call is local dies the way CORBA-style
distributed objects died. Real seams encode the physics structurally (CRI is
gRPC at pod granularity partly to bound round trips per action; LSP is
asynchronous precisely so the editor never blocks on the server). If the
matched shape implies chattiness the boundary can't afford, that's a wrong
match, back to step 4.

**6. Substitutability stress test.** Swap either side for a hypothetical
competing implementation. Does it still work, blind, against only what's
written down? If not, diagnose which failure it is: the write-up is missing
something real (back to step 4), or the piece boundary itself is wrong (back
to the piece list — this is the loop that already caught seam q and seam l
being two relationships wearing one letter).

**7. Record.** One entry in seams.md: the crossing, the classification, the
load-bearing problems, the matched shape, what actually crosses (named
plainly, checked against whether it's really a new thing, an existing memory
shape per m's "verify per object, never assert" rule, or an entry in
seams.md's cross-cutting object registry, not assumed), open questions,
status.

A registry hit on that last check means this crossing isn't a shape this
seam owns alone: it's one bilateral face of an object other seams also
touch (a Grant issued by one piece, checked by a second, revoked by a
third). The pairwise grid this procedure runs against (piece-matrix.md)
has no way to represent that on its own, one cell per pair, so it has to be
caught here, at record time, by hand. That's what the registry is for, kept
in seams.md, not this file.
