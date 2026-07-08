# Naming Conventions

*How things in this project get named, so the vocabulary stays coherent instead of
drifting per-document. Applies wherever Mojo needs a proper noun — standing-orders.md
is the current heaviest user, but this isn't specific to it.*

---

## Three registers, not one

Names in this project come from three deliberately separate registers. Knowing which
one a concept belongs to decides how it gets named — mixing them is the actual failure
mode, not using any one of them.

**1. Nautical/naval — for fleet-model entities and actions.** Captain, First mate,
Keel, Flagship, Consort, Tender, Charter, Mercenary, Commission, Enlist, Hail, Flare,
Watch bill. The rule: only a real, attested word or phrase — never an invented
portmanteau, never a forced backronym. The test is whether an actual sailor could have
said it. If no real term fits cleanly, that's a signal to drop into register 2, not to
manufacture one.

**2. Plain functional English — for computed properties, policy mechanisms, and
category labels.** Permission ceiling, the sensitivity judgment, the routing policy,
the six Fleet section names (Roster, Permissions, Records, Communication, Accounting,
Policy). These are deliberately *not* themed. Giving an abstract/computed concept a
cute nautical name would make it read as more concrete or more "real" than it is —
plain English keeps it honest about what it actually is: a rule, not an entity.

**3. Product branding — for shipped software artifacts.** MojOS, mojo-agent,
mojo-package. A separate namespace entirely, not fleet-model vocabulary. These name
things Clarke ships, not things the fleet model describes.

Test before naming anything new: is this a real thing that exists or happens in the
fleet model (register 1, if real nautical language fits) or a rule/computation about
it (register 2)? Is it software Clarke is building (register 3)? Don't reach for
register 1 just because it's more fun — a forced metaphor is worse than plain English.

---

## Known loose ends

Surfaced during the 2026-07-07 standing-orders.md walkthrough (see devlog), not yet
fixed:

- **Muster-ping (standing-orders.md #36)** — welds a real nautical word ("muster") to
  tech jargon ("ping"). The only place two registers got mixed into one word. Worth
  reconsidering as either a pure register-1 term or dropping to plain register-2
  English ("liveness check").
- **"Billet"** — used constantly throughout standing-orders.md (billet table, billeted,
  ephemeral billet) but never gets its own numbered definition. Load-bearing vocabulary
  that's only defined by implication.
- **The mesh (standing-orders.md #31)** — explicitly still unnamed in the document
  itself. The substrate is settled (Tailscale/WireGuard), the register-1 proper noun
  isn't. Open decision, not resolved here — needs a real pass with Clarke rather than
  a guess.

## Standing naming hazard

**Mojo** itself collides with Modular's existing Mojo programming language (already
flagged in ideas.md, under Collectives/framework). Not urgent to resolve, but worth
carrying here since it's the one name in the whole project that isn't ours alone.
