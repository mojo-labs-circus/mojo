# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The destination
this all points at is [vision.md](vision.md).*

---

## Now — defining Mojo's standards, Mk1

I'm away from my PC until 15 September — the start of second year — working from
the laptop all summer. That's a fact about the hardware, not a deadline.

The project follows a real systems-development lifecycle (locked 2026-07-08, see
[devlog.md](devlog.md)): requirements ([vision.md](vision.md),
[philosophy.md](philosophy.md) — done) → the Mojo System Interface, Mk1 → Mk1
system implementation, built against that standard → iterate, versioning both the
standard and the system as real use teaches what Mk1 got wrong.

**Current phase: Mk1 of the Mojo System Interface.** The standard is the project
(landed 2026-07-09, see [vision.md](vision.md) and
[interoperability.md](interoperability.md)), structured the POSIX way —
primitives, schemas built on them, contracts at the seams — and Mk1 is a
first-pass, complete-coverage answer for every piece of the system: files, the
first-mate schema, process/invocation, trust/enforcement, connection, routing,
and whatever else turns up. Good enough to build against, the same way
POSIX.1-1988 covered its whole scope thinly rather than perfecting one part and
leaving the rest blank. [research-plan.md](research-plan.md) is the live
tracker: walk each piece, check it against real precedent (Unix, seL4, Plan 9,
Erlang/OTP, others as needed), land on Mojo's own answer — including where the
answer is "adopt what exists" or "deliberately leave open." Restructured
2026-07-10 to terminate in the draft itself: the tracker's parts map one-to-one
onto `msi.md`'s sections, and each part gets drafted into the spec as its rows
land, so covering the tracker *is* assembling the draft.

Next milestone: the first Mojo system, built against the standards document once
it covers every tracked piece to first-pass depth. That system is a distro in
the honest sense — stitched from as many existing compliant parts as can be
found (a harness someone else built, models someone else trained, MCP tools,
SKILL.md skills) around the two pieces Mojo builds itself, the kernel and the
memory — and it doubles as the standard's test suite: every seam is proven by
something real actually plugging into it. What the build actually is — repos,
services, SDKs — gets decided once the system itself is defined.

No running software exists during this phase. Mojo's core architecture is a
foundational, cross-cutting layer — the kind where getting the core abstractions
right matters most before real code depends on them, the same bet real standards
bodies and kernel projects make.

No date attached to any of this. Progress is "does the standards document cover
every piece in research-plan.md's tracker," not a calendar.

Working rules for this phase:

- **research-plan.md's Status column is the actual truth.** Most rows say Open —
  treat them that way.
- **Real precedent before invention**, every piece, every time.
- **New wants go to [ideas.md](ideas.md)**, not folded into the standards document
  while it's being defined.
- **VM before metal, open source first** carry forward into the Mk1
  implementation phase once it starts — nothing to apply them to yet.

## Next — Mojo, Mk1

Assemble the system against the Mojo System Interface once it covers every
tracked piece — stitched from existing parts wherever a compliant one exists,
built only where nothing does (the kernel, the memory).
The milestone ends public, not private (decided 2026-07-10): the MSI-1 draft
published plus a release a stranger can actually stand up — install docs,
pinned parts, the hotswap test reproducible by someone who isn't me. That
release going to forums is the proof of concept; a system only I can run
proves nothing to anyone else.
Lived with immediately once it exists — laptop now, PC from 15 September.
That's Mojo running across more than one machine, which is just Fleet, already
part of the system model — not a separate multi-machine phase to design later.
Known candidates as daily use surfaces them: local models pulling real weight
(the PC's GPU decides how much); the brain growing beyond plain files when plain
files measurably fall short; polish on the modes and the voice. Uni term means
smaller sessions — the system has to earn its keep as a study environment too.

## Horizon — iterating past Mk1

Not separate projects after Mojo — the same system, matured through versioned
iteration (Mk2, Mk3...) once Mk1 is real to iterate on.

**Chartering, then the Commons.** One first mate, many ship classes — a sloop for
small local jobs, bigger hulls pooled from your own fleet, up to a chartered
frigate or galleon when a job needs more than anything owned gives. Checked the
pricing 2026-07-04: confidential/attested rental runs ~30–50% over plain rental
for comparable hardware, not the order-of-magnitude gap expected, so it's
buildable once there's a system to build it into, not a someday thing — and only
gets more capable as whatever's rentable keeps scaling up. Phased as: plain
rented GPU plumbing first, to prove the pipeline; swap in an attested/
confidential instance once that works, which is the point "nobody can see this"
becomes true rather than aspirational; anonymity explicitly out of scope — a
provider knowing who rented the box is fine, the workload itself is the only
thing that has to stay unseen (a Tor-style layer is a real idea, just not this
project's). Two things still open: keeping it feeling like the same first mate
across hulls (system prompts carrying voice and judgment for now — cheap, no
training needed; a personality LoRA per model family later, once prompting alone
isn't enough — MojOS's job either way, training one being itself a
chartering-sized job), and the escalation policy — deciding when a job is
actually worth chartering up, or handing to a mercenary, at all, within whatever
hard limits get set (e.g. "no mercenaries, ever" is a real setting, not a
suggestion). Doing the escalation call by hand for now; worth training a router
on it once there's enough real use to learn from — the routing contract itself
is now a tracked row in [research-plan.md](research-plan.md), not just this
aside. The Commons is the far-horizon
version: a whole reciprocal network of attested compute, not just a one-voyage
charter.

**Collectives.** The formal model — groups with sovereign members, constitutions,
accountable AI participation. The primitives already have to hold for this (see
[research-plan.md](research-plan.md)) — what's deliberately last is the deeper
governance policy on top, since it needs the individual layer real first.

## The essays

A genuinely separate thread from the system itself: the thinking in this repo
written up properly and put in front of people — professors, forums, the
internet. Craft problem first; sovereignty and collectives after. Feedback and
collaborators are the goal, and the essays get dramatically heavier once there's
a working system behind them.
