# Making the standard

How the piece/seam map actually becomes MSI-1, and how MSI-1 becomes a real
standard other people build against. This is the document AGENTS.md's item 2
called for, sitting between [anatomy.md](anatomy.md) and the eventual msi.md.
Decided 2026-07-14 against real history, not assumption; the evidence, with
sources and flagged unverified items, is
[standards-research.md](standards-research.md).

## The finding this plan is built on

No successful standard published its document before running code existed.
POSIX codified some thirty running Unixes and got its teeth from government
procurement. CRI, CNI, LSP, OCI, and XMPP were extracted from working
systems, and the extraction trigger was always a concrete second party
arriving (rkt for CRI, Red Hat and Codenvy for LSP, appc for OCI). Matrix,
the one genuinely a-priori design, shipped its reference server on
announcement day and still paid years of revisions; its stated method is the
one that fits this project's position exactly: build an implementation
first, prove the protocol works, then spec it. The failure cases are the
same lesson inverted: OSI had committee-designed layers and government
mandates and no running code, and lost to TCP/IP; CORBA required no
reference implementation and published unimplementable specs.

And the real done-gate, everywhere a standards body is serious about it, is
two independent interoperable implementations, per feature (IETF RFC 2026,
W3C Candidate Recommendation exit). A first implementation proves the design
is implementable. Only a second, independent one proves that the document,
not shared tribal knowledge, carries the interface.

## The sequence

1. **Survey to build-ready.** The existence pass (seam-method.md step 1
   across piece-matrix.md) continues at full per-pair rigor, but its finish
   line is "good enough to build against", not "complete". Where the
   counterpart systems agree, that's settled practice. Where they genuinely
   disagree, the pair gets flagged as an invention-risk zone and the build
   decides it. POSIX only ever invented at exactly those spots (termios,
   O_NONBLOCK, pax), never speculatively, and pax is the standing warning
   that even ratified inventions get ignored.
2. **Verify the build inputs before the build plan hardens.** Three checks,
   none assumed: whether OpenFang really contains an extractable kernel and
   identity/memory layer (a hypothesis, not a finding; its internals were
   the undocumented ones in the Harness-row research); whether the first
   harness's interception surface can actually carry the Harness-Kernel
   seam (current intent: a LangGraph-style runtime, not Claude Code or
   Codex CLI); licensing on every adopted part.
3. **Build the first system.** Adopt everything that exists (harness, model
   endpoints, MCP tools, sandbox, most of the rest); build only what
   doesn't: the kernel (a policy daemon plus per-harness shims), the
   identity and memory schemas, the glue. Every seam is a real process
   boundary with a real format crossing it. The system is not an
   implementation of a finished MSI-1; it is the thing MSI-1 gets extracted
   from. And it is deliberately smaller than the anatomy: not all thirteen
   pieces, see Scope and growth below.
4. **Extract the draft.** MSI-1 describes what the running system verifiably
   does at each boundary. A seam never exercised by running code does not go
   in the draft; that's the IETF rule (unimplemented features get removed)
   applied at birth.
5. **Publish both together.** The draft plus a release a stranger can stand
   up, same day, OCI-style. Announced in the register Linux was announced
   in, in the venues where the right people already are: the communities
   around the counterpart systems, which are this project's comp.os.minix,
   an existing audience with a live grievance (everything welded to its own
   harness and format).
6. **Pass the gate.** MSI-1 is done when a stranger builds a compatible
   implementation of a seam from the text alone, without asking me
   anything. Until then it is a draft and says so.

## The demo that carries the publish

The hotswap test: swap the harness, swap the model endpoint, identity and
memory persist, nothing else notices, reproducible by someone who isn't me.
It's this project's "boots and runs bash and gcc", the under-an-hour proof
that the seams are real, and it's the seed of the eventual conformance
suite.

One hard requirement inside it: at least one swap must be to an
independently-built harness. The first harness is one I assemble, and LSP
documents exactly what happens when the only client is your own: the
dominant implementation's behavior silently becomes the spec (UTF-16
offsets, completion fields whose semantics had to be reverse-engineered from
VS Code). Hyrum's law, in production.

## Scope and growth

MSI-1 is minimal on purpose, and the first system is even smaller. Not all
thirteen pieces get built first: Fleet manager is definitely out of the
first build, and which pieces form the minimal base is a build-time
decision, made once it's clear what can actually be adopted, not a paper
decision made now. The test for each piece is the hotswap demo itself: if
the piece disappeared entirely, could the demo still run? If yes, it's not
MSI-1. The floor is fixed, though: the draft has to cover enough
seams that the hotswap demo is specifiable from the text alone. Below that
it's not a standard, it's a README.

Minimal is the property that works, not a loss to manage. POSIX.1-1988 was
just the system interfaces; the shell came in 1992, real-time and threads in
1996, as amendments to a live standard. OCI shipped runtime-spec alone and
added image-spec and distribution-spec as scope earned them. The hourglass
theorem is the theory for why: every operation added to a first version
shrinks the set of systems that can implement it. Deferring a piece isn't
losing it: it's leaving room in the queue for whoever shows up at publish to
help build it.

The full vision still gets across at publish, through three layers that
never mix:

- **The informative layer carries the map.** anatomy.md, the rationale, the
  essays: the whole thirteen-piece picture, federation and the Commons
  included, published as informative material alongside the minimal
  normative spec. The ambition lives in prose; an implementer only has to
  build what's normative.
- **The structure reserves the slots.** MSI is split by implementer role, so
  every unbuilt piece has a named document boundary waiting for it. MSI-1
  names the family's shape (these specs exist now, these are reserved)
  without writing contract text for the reserved ones. Normative text for an
  unexercised seam is the OSI/CORBA move, the one pattern with a documented
  body count.
- **The change rule is the door.** Everything designed but unbuilt lives on
  as proposals, and graduates into the standard only with a working
  implementation attached (the MSC discipline committed to below). The
  future isn't a promise in the spec; it's a visible queue with a known
  door.

This is the growth mechanism, not just the scope discipline. Publish carries
the full backlog already sitting in ideas.md and vision.md alongside the
minimal spec, not vague ambition: named ideas, with a rough sense of where
each stands. Each one moves through visible states as it matures: idea,
researched, prototype exists, multiple independent implementations exist,
candidate document, standardized. The MSC discipline above is what gates the
last step; the states before it are what make the queue legible enough for
someone else to walk in and take a rung. Nobody joins a spec. They join a
runnable thing with a backlog they can see themselves contributing to.

Fleet manager is the worked example: in scope for the standard family, its
own document when it comes, entering normative text only when a system
actually runs across two machines.

MSI-1 doesn't have to be the digital counterpart standard. It has to make
one possible: proving identity and memory survive a harness swap is a
prerequisite for that vision, not the vision delivered whole, and the two
are allowed to arrive on different timelines.

## Document mechanics MSI-1 commits to

- **Normative language**: RFC 2119 keywords (MUST/SHOULD/MAY), used
  sparingly, only where interoperation requires them.
- **Named holes**: POSIX's triad for deliberate gaps. Undefined (invalid
  input, anything goes), unspecified (valid input, implementation need not
  document), implementation-defined (implementation chooses and must
  document its choice). Gaps in MSI-1 are first-class and named, never
  accidental.
- **A rationale companion**: why each seam landed where it did, including
  rejected alternatives, POSIX-Rationale-style. Most of it already exists;
  it's this repo.
- **Structure by implementer role**, Matrix-style: someone building only a
  memory provider reads only that part.
- **The change rule from day one**, Matrix's MSC discipline: no change to
  the standard without a working implementation linked to the proposal.
- **No flag-day breaking changes, ever**: obsolescence markings a full
  version ahead (POSIX's OB codes), receivers ignore unknown fields (LSP),
  breakage scoped and opt-in (Matrix room versions). Which mechanism fits
  which seam is decided when that seam's shape is.

## Who does what

One person can carry: the survey, the first system, the draft, the publish,
and the fast loop after it. The build scope after adoption is genuinely
small: schemas, one small daemon, shims, glue, a demo. Linux 0.01 was 10,239
lines, and Linus had POSIX handed to him; the survey work here is that half
of the job, being done first.

One person cannot carry, by definition: the second implementation, and
everything downstream of it (a conformance suite that's more than the
hotswap test, governance worth the name). The publish step exists to attract
those people, and the mechanics that have actually worked are the same four
everywhere: a runnable thing (under an hour to try), a fast feedback loop (a
patch visible in days, named credit), credible neutrality, and an existing
venue full of the right people. A spec alone has never done it; there is no
counterexample in the record.

No dates on any of this. Progress is "does the running system exercise the
seams, and does the draft describe only what it verifiably does".
