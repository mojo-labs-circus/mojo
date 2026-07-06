# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The destination
this all points at is [vision.md](vision.md).*

---

## Now — MojOS, with Hermes as the first Jarvis

The project reset to build-first in July 2026 (see [devlog.md](devlog.md)). I'm away
from my PC until 15 September — the start of second year — working from the laptop
all summer. That's a fact about the hardware, not a deadline: the laptop is the
summer machine, the PC becomes the true always-on host once I'm back. No
feature-complete date attached to it — progress is "is Jarvis running and getting
used," not a checklist due by a calendar.

**Locked 2026-07-05:** mojo-agent proper isn't being designed yet. The fastest path
to an actually-working Jarvis is running something that already exists — [Hermes
Agent](https://github.com/NousResearch/Hermes) (Nous Research, MIT, self-hosted,
persistent memory, a learning loop that writes and improves its own skills) —
wholesale, wrappers and all, rather than building an agent loop from scratch. Own
mojo-agent gets designed once living with Hermes shows what it actually gets wrong.

The plan, in sequence — basic OS first, agent in fast, then let the agent help build
the rest rather than hand-building MojOS before there's any help with it:

1. Build a minimal MojOS — just enough NixOS flake to boot and run a resident
   service, proven in a VM (`nixos-rebuild build-vm`) before it ever touches metal.
   Not the modes, not the rice yet — the smallest thing Hermes can actually run on.
2. Install Hermes Agent as the first mate on that minimal system, resident and
   always on whenever the machine is.
3. Point it at whatever local memory store Hermes defaults to and start using it —
   don't pre-design the memory architecture; let living with it teach me what
   "editable brain" needs to mean (plain files, Knowledge-object-style structures,
   a vector store with a translation layer for viewing/editing — not decided, and
   doesn't need to be yet).
4. From there, use Jarvis itself to help build out the rest of MojOS — the modes,
   the theming, daily-driving it on the laptop — instead of building all of that by
   hand first and only adding the agent at the end.
5. Live with it. Whatever Hermes doesn't do natively — a confirmation gate before it
   touches the system, delegating to a mercenary like Claude Code and folding the
   result back in — becomes the design brief for mojo-agent proper, built to have
   that functionality natively instead of as wrappers bolted onto someone else's
   harness.

Phone and voice surfaces (Tailscale access, local speech-to-text/text-to-speech) come
whenever the core is worth reaching remotely — not gated to a date.

Circus (multi-machine) and chartering/mercenaries (the pirate-ship compute model)
stay Horizon exactly as before — nothing to wire in until there's a single working
agent worth routing from.

Working rules for this phase:

- **VM before metal.** Nothing touches the real machine until the config is proven
  in `nixos-rebuild build-vm`.
- **Open source first, at every layer.** Stylix, home-manager, existing flake
  patterns for the OS; Hermes over a hand-built agent loop. Reuse before writing.
- **New wants go to [ideas.md](ideas.md)**, not into what's actually being built
  right now.
- **Learning is the point.** Nix fluency and living with an agent are the
  deliverables of this phase, not overhead.

## Next — living with it (autumn 2026)

Driven by what daily use actually surfaces, not planned in detail yet. Known
candidates: local models pulling real weight (the PC's GPU decides how much); the
brain growing beyond plain files when plain files measurably fall short; the
laptop and PC staying coherent as two machines (the first real taste of the
Circus problem); polish on the modes and the voice. Uni term means smaller
sessions — the system has to earn its keep as a study environment too.

## Horizon

**The Circus.** The PC that already runs MojOS becomes the home server; the laptop
joins as the second machine; phones become windows into it. One intelligence across
all machines. Eventually a dedicated home server. The old fleet-era design thinking
lives in git history and gets rethought fresh when this becomes real.

**Chartering, then the Commons.** One first mate, many ship classes — a sloop for
small local jobs, bigger hulls pooled from the Circus, up to a chartered frigate or
galleon when a job needs more than anything owned gives. Checked the pricing
2026-07-04: confidential/attested rental runs ~30–50% over plain rental for
comparable hardware, not the order-of-magnitude gap expected, so it's buildable now,
not a someday thing — and only gets more capable as whatever's rentable keeps
scaling up. Phased as: plain rented GPU plumbing first, to prove the pipeline; swap
in an attested/confidential instance once that works, which is the point "nobody can
see this" becomes true rather than aspirational; anonymity explicitly out of scope —
a provider knowing who rented the box is fine, the workload itself is the only thing
that has to stay unseen (a Tor-style layer is a real idea, just not this project's).
Two things still open: keeping it feeling like the same first mate across hulls
(system prompts carrying voice and judgment for now — cheap, no training needed; a
personality LoRA per model family later, once prompting alone isn't enough — MojOS's
job either way, training one being itself a chartering-sized job), and the escalation
policy — deciding when a job is actually worth chartering up, or handing to a
mercenary, at all, within whatever hard limits get set (e.g. "no mercenaries, ever" is
a real setting, not a suggestion). Doing the escalation call by hand for now; worth
training a router on it once there's enough real use to learn from. The Commons is the
far-horizon version: a whole reciprocal network of attested compute, not just a
one-voyage charter.

**Collectives.** The formal model — groups with sovereign members, constitutions,
accountable AI participation. The deepest part of the vision and deliberately last:
it needs the individual layer to exist first.

**The essays.** The thinking in this repo written up properly and put in front of
people — professors, forums, the internet. Craft problem first; sovereignty and
collectives after. Feedback and collaborators are the goal, and the essays get
dramatically heavier once there's a working system behind them.
