# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The destination
this all points at is [vision.md](vision.md); what the system concretely is, is
[anatomy.md](anatomy.md).*

---

## Now — the anatomy, then the standard's first version

I'm away from my PC until 15 September — the start of second year — working from
the laptop all summer. That's a fact about the hardware, not a deadline.

The project follows a real systems-development lifecycle (locked 2026-07-08, see
[devlog.md](devlog.md)): requirements ([vision.md](vision.md),
[philosophy.md](philosophy.md) — done) → the Mojo System Interface, Mk1 → Mk1
system implementation, built against that standard → iterate, versioning both the
standard and the system as real use teaches what Mk1 got wrong.

**Current phase: the anatomy first, then the MSI Mk1 derived from it.** Landed
2026-07-10: [anatomy.md](anatomy.md) is the whole-system map — every piece,
every seam (a–n), and the one thing that never swaps — written in plain
language so a stranger can read it and build an implementation. It supersedes
the piecemeal picture the tracker grew out of, and it was drawn precisely
because the old research walk kept forcing decisions about pieces before the
whole system was visible.

The immediate work, in order:

1. **Make the anatomy right.** Work it over until every box and seam holds —
   it decides all future work, so it gets the scrutiny first.
2. **Derive the research plan from it.** Rewrite
   [research-plan.md](research-plan.md) seam by seam: what each seam must pin
   down, what real precedent to study for it (Unix, seL4, Plan 9, Erlang/OTP,
   and the agent-era prior art already catalogued there), and the order the
   dependencies force. The current tracker's rows and research notes are
   inputs to that rewrite, not casualties of it. The session rules in
   `.claude/rules/` get rewritten to match at the same time.
3. **Walk the plan and draft msi.md as rows land** — a first-pass,
   complete-coverage answer for every seam, good enough to build against, the
   same way POSIX.1-1988 covered its whole scope thinly rather than perfecting
   one part and leaving the rest blank.

Work already banked stays banked: the adopted-seams study (MCP `2025-11-25`,
A2A 1.0.0, SKILL.md, the OSS model-serving convergence on the OpenAI-compatible
wire shape) is recorded in the devlog and feeds the plan rewrite.

No running software exists during this phase. Mojo's core architecture is a
foundational, cross-cutting layer — the kind where getting the core abstractions
right matters most before real code depends on them, the same bet real standards
bodies and kernel projects make.

No date attached to any of this. Progress is "does the standards document cover
every seam in the anatomy," not a calendar.

Working rules for this phase:

- **[anatomy.md](anatomy.md) is the map; the research plan's Status column is
  the truth about progress.** Nothing is decided by being drawn or written down.
- **Real precedent before invention**, every piece, every time.
- **Plain names in everything public-facing.** The MSI's nouns stay as plain as
  POSIX's (decided 2026-07-10, see [naming-conventions.md](naming-conventions.md)).
- **New wants go to [ideas.md](ideas.md)**, not folded into the standard while
  it's being defined.
- **VM before metal, open source first** carry forward into the Mk1
  implementation phase once it starts — nothing to apply them to yet.

## Next — the first Mojo system

Assemble the system against the Mojo System Interface once it covers every seam —
stitched from existing parts wherever a compliant one exists (an agent runtime
someone else built, models someone else trained, MCP tools, SKILL.md skills),
built only where nothing does (the kernel, the memory layer).
The milestone ends public, not private (decided 2026-07-10): the MSI-1 draft
published plus a release a stranger can actually stand up — install docs,
pinned parts, the hotswap test reproducible by someone who isn't me. Nix — the
package manager on any distro, not NixOS — is the deployment story: parts
pinned, installs reproducible, upgrades rollbackable. That
release going to forums is the proof of concept; a system only I can run
proves nothing to anyone else.
Lived with immediately once it exists — laptop now, PC from 15 September.
That's the system running across more than one machine, which is just the
federation seam already in the anatomy — not a separate multi-machine phase to
design later.
Known candidates as daily use surfaces them: local models pulling real weight
(the PC's GPU decides how much); the memory layer growing beyond plain files when
plain files measurably fall short; polish on the modes and the voice. Uni term
means smaller sessions — the system has to earn its keep as a study environment
too.

## Horizon — iterating past Mk1

Not separate projects after the first system — the same system, matured through
versioned iteration (Mk2, Mk3...) once Mk1 is real to iterate on.

**Renting compute, then the Commons.** One identity, any size of machine under
it — small local jobs on owned hardware, bigger jobs pooled from your own
machines, up to a rented cloud instance when a job needs more than anything
owned gives. Checked the pricing 2026-07-04: confidential/attested rental runs
~30–50% over plain rental for comparable hardware, not the order-of-magnitude
gap expected, so it's buildable once there's a system to build it into, not a
someday thing — and only gets more capable as whatever's rentable keeps scaling
up. Phased as: plain rented GPU plumbing first, to prove the pipeline; swap in
an attested/confidential instance once that works, which is the point "nobody
can see this" becomes true rather than aspirational; anonymity explicitly out of
scope — a provider knowing who rented the box is fine, the workload itself is
the only thing that has to stay unseen (a Tor-style layer is a real idea, just
not this project's). Two things still open: keeping it feeling like the same
assistant across machine sizes (system prompts carrying voice and judgment for
now — cheap, no training needed; a personality LoRA per model family later,
once prompting alone isn't enough — training one being itself a rental-sized
job), and the escalation policy — deciding when a job is actually worth renting
up for, or handing to a hired frontier model, within whatever hard limits get
set (e.g. "no outside models, ever" is a real setting, not a suggestion). Doing
the escalation call by hand for now; worth training a router on it once there's
enough real use to learn from — the routing contract itself is a seam in
[anatomy.md](anatomy.md), not just this aside. The Commons is the far-horizon
version: a whole reciprocal network of attested compute, not just a one-job
rental.

**Collectives.** The formal model — groups with sovereign members, constitutions,
accountable AI participation. The primitives already have to hold for this —
what's deliberately last is the deeper governance policy on top, since it needs
the individual layer real first.

## The essays

A genuinely separate thread from the system itself: the thinking in this repo
written up properly and put in front of people — professors, forums, the
internet. Craft problem first; sovereignty and collectives after. Feedback and
collaborators are the goal, and the essays get dramatically heavier once there's
a working system behind them.
