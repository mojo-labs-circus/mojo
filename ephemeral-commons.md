# Ephemeral Commons (working name)

*One-line version: confidential compute for personal AI — hide what ran, not who
ran it. Full reasoning trail in [devlog.md](devlog.md) (2026-07-04 entry); this
file is the settled shape, for rereading, not the arc of how we got here.*

*Scope: this is **chartering** — the mechanism in [vision.md](vision.md)'s fleet
model by which the first mate borrows a bigger, rented ship class (a frigate or
galleon) for the length of one voyage, when a job needs more than anything owned
(local + Circus) gives. Distinct from mercenaries (frontier models, hired
per-mission, never part of the fleet). The fleet, ship classes, and escalation
policy live in vision.md and [roadmap.md](roadmap.md); this file is the deep-dive on
chartering specifically, and on what it grows into long-term (the Commons — see
vision.md's far-horizon section).*

---

## The problem

To get a personal AI that's actually capable, you either hand a frontier company
your data, or you wait years for home hardware to match datacentre-scale compute.
Neither is good enough. The gap between "what my own machine can do" and "what the
frontier can do" never closes — it just moves forward as models improve. So this
isn't a stopgap until hardware catches up. It's a permanent piece of the
architecture.

## The idea, in the simplest terms

Your local server (memory, coordination, identity — the first mate) stays home,
always. When a job needs more compute than home hardware gives, it borrows a
rented machine, runs an open-source model on it, gets the answer back, and lets
the rented machine go. Same shape as ordinary cloud bursting — rent capacity for
peaks, own your baseline — with one addition: the rented machine runs on
**confidential computing** (a TEE — trusted execution environment), so the
company that owns the hardware cannot see what ran on it. Not "promises not to
look." Cannot, cryptographically, and can prove it (attestation).

Whether the rental is also anonymous (no one can tell it was you) is a separate
concern this project doesn't need to solve. Knowing *who* rented the box is fine
— what has to be true is that nobody, including the host, can see *what ran on
it*. Anonymity is something anyone could bolt on later if they want it; it's not
a phase of this plan.

## The two properties, and why they're not the same thing

- **Confidentiality** — the host can't see the workload, even if they wanted to.
  Solved by attestation (TEE). This is the part that matters most and the part
  that's buildable now.
- **Anonymity** — no one can tell it was you who rented it. Solved by
  alias/crypto/Tor-style payment and routing. Doesn't stop the host from *seeing*
  the workload while it's live — it only hides who's behind it. Not a goal of
  this project; someone could layer it on later, but it's not being built here.

Confusing these two was the biggest wrong turn in getting here. Anonymity alone
doesn't protect your data. Confidentiality does — with or without anonymity on
top. This project only needs confidentiality.

## What this is NOT (the corrections that got us here)

- **Not "rent any GPU anonymously and you're safe."** Without attestation, the
  host can read everything in the clear — trivially, no subpoena needed. Rented
  compute without attestation is not obviously more private than talking to a
  company directly — arguably less, since a random rental host has none of the
  accountability, security practices, or legal exposure a real company has. Plain
  rental is a plumbing step, not a privacy win, until attestation is added.
- **Not a way to keep using Claude/GPT privately.** Confidential computing
  protects you from the *compute host*, not from the *model vendor*. A frontier
  lab's own model runs on their own infrastructure, under their control — TEEs
  don't apply. This gets you a **private substitute** for frontier models (an
  open model you deploy yourself into an attested enclave), not a private way to
  keep using frontier models.
- **Not built to serve concentrated power.** Big finance and big tech already
  have the money to buy Azure/GCP confidential computing or build their own —
  they don't need this, and building for them was never the point. This is for
  individuals and small businesses who don't have that leverage: open source,
  so anyone can read, audit, and run it, not sold as an enterprise contract to
  the handful of companies who already have every resource. That's not a
  market-segmentation call, it's the same reason the whole project exists —
  see [philosophy.md](philosophy.md), "everyone in the room."
- **Not something to build the marketplace for.** Full decentralisation of
  datacentre-level compute is never happening — power, cooling, and capital don't
  bend that way. Don't try to be Akash/io.net/Nosana. Be a privacy-obsessed
  *client* of whatever GPU supply already exists (their marketplace or a
  centralised one), anonymised and attested on the way in. That drops the "you're
  entering a $19B funded category" problem entirely.
- **Not proof that decentralisation defeats government pressure by itself.** If a
  government compels a company (rental host or otherwise) to hand over data, that
  company can only comply if it *has* the data. Attestation is what makes
  compliance technically impossible, not distance or anonymity. That's the actual
  defence against a subpoenaed host or a captured frontier lab — not obscurity.

## What survives, unqualified

- Confidential (attested) compute as the borrowing mechanism — real, buildable,
  not novel infrastructure (Phala/NEAR AI/Spheron already run it), novel only in
  being aimed at an individual's personal AI rather than sold to enterprises.
- Ephemeral (spin up, use, tear down) as *defence in depth*, independent of
  anonymity — TEEs have had real side-channel vulnerabilities, so shortening the
  exposure window of any one instance is good practice regardless. Needs a
  concrete granularity decision (per-session, or after N minutes idle) — "fully
  ephemeral" isn't a spec until that's chosen, and the choice trades off against
  cold-start latency on every use.
- Open models + open harnesses (Ollama running something like Hermes/Qwen/
  DeepSeek) as the software that runs inside the rented, attested box. Already
  solved tooling — no new engineering here.
- The long-term ecosystem shape, now in [vision.md](vision.md) as **The
  Commons**: a network of datacentres run on attestation instead of trust, that
  any personal AI can borrow from and give back, with how much to trust any given
  provider (attested-but-known vs. attested-and-anonymous) left to the owner.
  Far horizon, same tier as Circus and Collectives — not being built yet.

## Phasing (plumbing before privacy claims)

1. **Plumbing:** rent a plain GPU instance, run Ollama + an open model on it,
   tunnel it home. No privacy claim yet — this is just proving the pipeline
   works.
2. **Confidentiality:** swap the plain instance for an attested one (e.g. Phala
   on OpenRouter). This is the point the "nobody can see this" claim becomes
   true instead of aspirational.
3. **Anonymity — not planned.** A Tor-style layer is a real idea someone could
   explore later, but it's not a phase of this project. Providers knowing who
   rented the box is fine; the logs are the only thing that has to stay unseen.
4. **Companies:** a separate track, later — confidentiality without anonymity,
   which is already how this gets sold commercially. Not urgent.

## Why build it (the three-part case)

- **Interesting** — genuine systems/security depth (attestation, orchestration,
  tunnelling), a stronger portfolio piece than another chatbot wrapper.
- **Usable** — fits directly into the Mojo vision as the Commons; closes the
  "still trusting a frontier vendor with stripped context" gap that was
  otherwise unresolved.
- **Helpful, even at a niche scale** — a real tool for privacy-minded technical
  users is a legitimate bar for "helpful to the world," and matches vision.md's
  own stance that early technical users are the bootstrapping strategy, not the
  target state.

## Open, not yet decided

- Where this sits against the frozen 2026-07-03 core (MojOS + mojo-agent v0.1 by
  Sept 15) — ahead of it, alongside it, or after. Not resolved; revisit when the
  summer goals get replanned.
- Ephemeral granularity (session-based vs. idle-timeout teardown).
- Which attested-compute provider to build against first (Phala on OpenRouter is
  the most turnkey today).

## The one concrete next step

Nothing built yet as of 2026-07-04 — everything above is still words. The
smallest real thing: pick one attested-compute provider, write a client that
sends it a prompt and verifies the attestation quote before trusting the
response. A day of work, not a summer. That's the test of whether this becomes
the eighth idea that shipped or the eighth that didn't.
