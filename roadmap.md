# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The
destination this all points at is [vision.md](vision.md); what the system
concretely is, is [anatomy.md](anatomy.md).*

---

## Now: the survey to build-ready, the first system, the draft extracted from it

The project follows a real systems-development lifecycle (locked 2026-07-08,
ordering revised 2026-07-14, see [devlog.md](devlog.md)): requirements
([vision.md](vision.md), [philosophy.md](philosophy.md), done) → the seam
survey and the first system, grown together → the MSI-1 draft, extracted from
what actually runs → iterate, versioning both the standard and the system as
real use teaches what Mk1 got wrong.

The 2026-07-14 revision inverted the old ordering, which had the finished
standards document gating any build. That was the OSI shape, and the history
in [standards-research.md](standards-research.md) is unambiguous about it: no
successful standard published its document before running code existed; the
spec gets extracted from or validated by a working system, never the reverse.
[making-the-standard.md](making-the-standard.md) is the full plan. The short
version, in order but overlapping on purpose:

1. **Finish the existence pass to build-ready, not to completeness.**
   [seam-method.md](seam-method.md) step 1 across
   [piece-matrix.md](piece-matrix.md), same per-pair rigor against real
   systems, new finish line. Where the counterpart systems agree, that's
   settled practice, write it down. Where they genuinely disagree, flag the
   pair as an invention-risk zone (POSIX's termios cases) and let the build
   decide it, not paper.
2. **Verify the build inputs.** OpenFang as candidate source for the kernel
   and the identity/memory layer (a hypothesis to check, not a finding; its
   internals were the undocumented ones in the Harness-row research). The
   first harness, a LangGraph-style runtime rather than Claude Code or Codex
   CLI, and whether its interception surface can actually carry the
   Harness-Kernel seam. A licensing pass over every adopted part.
3. **Build the first system.** Stitched from existing parts wherever a real
   one exists (harness, model endpoints, MCP tools, sandbox), built only
   where nothing does: the kernel (a policy daemon plus per-harness shims),
   the identity and memory schemas, the glue. Deliberately not all thirteen
   pieces: Fleet manager is out of the first build, and which pieces make
   the minimal base is decided during the build, once it's clear what can
   actually be adopted (see
   [making-the-standard.md](making-the-standard.md)'s Scope and growth).
   Every seam a real process boundary with a real format crossing it. The demo that matters is the
   hotswap test: swap the harness, swap the model endpoint, identity and
   memory persist, nothing else notices. At least one swap has to be to an
   independently-built harness, or the draft silently becomes "whatever my
   harness does", LSP's documented failure.
4. **Extract the MSI-1 draft from what runs.** The draft describes what the
   system verifiably does at each boundary; a seam never exercised by
   running code doesn't go in it. Document mechanics (normative language,
   named holes, rationale, change rule) per
   [making-the-standard.md](making-the-standard.md).
5. **Publish both together.** The draft plus a release a stranger can stand
   up, same day, announced where the right people already are (the
   communities around the counterpart systems). Nix (the package manager on
   any distro, not NixOS) is the deployment story: parts pinned, installs
   reproducible, upgrades rollbackable. Then the actual recruiting
   mechanism: fast merge loop, named credit, visible iteration.

No dates on any of this; the old September framing is deliberately gone.
Progress is "does the running system exercise the seams, and does the draft
describe only what it verifiably does", not a calendar.

Working rules for this phase:

- **[anatomy.md](anatomy.md) is the map; [piece-matrix.md](piece-matrix.md)
  and [seams.md](seams.md) are the truth about seam progress.** Nothing is
  decided by being drawn or written down.
- **Real precedent before invention.** Every piece, every seam, every time.
- **Plain names in everything public-facing** (decided 2026-07-10, see
  [naming-conventions.md](naming-conventions.md)).
- **New wants go to [ideas.md](ideas.md)**, not folded into the standard
  while it's being defined.
- **VM before metal, open source first** for the build as it starts.

## Next: the second implementation

MSI-1 is not done when the document is finished. It's done when a stranger
builds a compatible implementation of a seam from the text alone, without
asking me anything. That's the two-implementation gate the serious standards
bodies encode (IETF's RFC 2026, W3C's Candidate Recommendation exit), and it
can't be reached solo, by definition. Everything in Now is aimed at
attracting that person. Until the gate is passed, MSI-1 stays labeled draft,
honestly, the way IETF drafts do.

Lived with as soon as it runs: the system as my daily driver, which is what
surfaces the next candidates (local models pulling real weight, the memory
layer growing past plain files when they measurably fall short, polish on the
modes and the voice). More than one machine under one identity is the
federation seam already in the anatomy, exercised whenever a second machine
is actually there, not a separate multi-machine phase to design later.

## Horizon: iterating past Mk1

Not separate projects after the first system. The same system, matured through
versioned iteration (Mk2, Mk3...) once Mk1 is real to iterate on.

**Renting compute, then the Commons.** One identity, any size of machine under
it: small local jobs on owned hardware, bigger jobs pooled from your own
machines, up to a rented cloud instance when a job needs more than anything
owned gives. Checked the pricing 2026-07-04: confidential/attested rental runs
about 30–50% over plain rental for comparable hardware, not the
order-of-magnitude gap expected, so it's buildable once there's a system to
build it into, not a someday thing. It only gets more capable as whatever's
rentable keeps scaling up. Phased as: plain rented GPU plumbing first, to prove
the pipeline; swap in an attested/confidential instance once that works, which
is the point "nobody can see this" becomes true rather than aspirational.
Anonymity is explicitly out of scope. A provider knowing who rented the box is
fine; the workload itself is the only thing that has to stay unseen (a
Tor-style layer is a real idea, just not this project's). Two things still
open: keeping it feeling like the same assistant across machine sizes (system
prompts carrying voice and judgment for now, cheap, no training needed; a
personality LoRA per model family later, once prompting alone isn't enough,
training one being itself a rental-sized job), and the escalation policy,
deciding when a job is actually worth renting up for, or handing to a hired
frontier model, within whatever hard limits get set ("no outside models, ever"
is a real setting, not a suggestion). Doing the escalation call by hand for
now. Worth training a router on it once there's enough real use to learn from;
the routing contract itself is a seam in [anatomy.md](anatomy.md), not just
this aside. The Commons is the far-horizon version: a whole reciprocal network
of attested compute, not just a one-job rental.

**Collectives.** The formal model: groups with sovereign members,
constitutions, accountable AI participation. The primitives already have to
hold for this. What's deliberately last is the deeper governance policy on top,
since it needs the individual layer real first.

## The essays

A genuinely separate thread from the system itself: the thinking in this repo
written up properly and put in front of people. Professors, forums, the
internet. Craft problem first; sovereignty and collectives after. Feedback and
collaborators are the goal, and the essays get dramatically heavier once
there's a working system behind them.
