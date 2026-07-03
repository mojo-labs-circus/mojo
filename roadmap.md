# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The destination
this all points at is [vision.md](vision.md).*

---

## Now — MojOS v0.1 + mojo-agent v0.1 (target: start of September 2026)

The project reset to build-first in July 2026 (see [devlog.md](devlog.md)). The goal
for the next two months is one real thing, used daily, by me:

**My PC dual-boots Windows and MojOS, and MojOS is my daily driver, with my first
mate running inside it.**

The finish line, as testable statements:

1. My machine dual-boots Windows and MojOS; MojOS is my daily driver.
2. The entire system is declared in the `mojos` flake — apps, Hyprland, dotfiles —
   rebuilds with one command, and the same config boots as a VM for safe iteration.
3. At least two modes exist and switch cleanly (looks, behaviour, and how much of
   the system shows — e.g. coding mode, normal mode for company).
4. mojo-agent is part of the OS — installed by the flake, not by hand.
5. Running `mojo` opens my first mate, which keeps persistent memory about me as
   plain files on my disk.
6. It can control the computer — run commands, open apps, edit files — behind a
   confirmation gate.
7. It can delegate to the crew: hand work to Claude Code or a frontier model and
   bring back the result, keeping my personal context local.

Rough shape: **month one** — learn Nix properly, build the flake, live in the VM,
build the mode system and a rice worth keeping (the old dotfiles are not coming
along). **month two** — dual-boot install on metal, daily-drive it, build agent v0.1
into it, harden the rough edges.

Working rules for this phase:

- **VM before metal.** Nothing touches the real machine until the config is proven
  in `nixos-rebuild build-vm`.
- **Open source first.** Stylix for system-wide theming, home-manager for user
  config, existing flake patterns — reuse before writing.
- **Scope is frozen.** New wants go to [ideas.md](ideas.md), not into the milestone.
- **Learning is the point.** Nix fluency and agent-building skills are deliverables
  of this phase, not overhead.

## Next — living with it (autumn 2026)

Driven by what daily use actually surfaces, not planned in detail yet. Known
candidates: the agent as a resident system service rather than a summoned CLI; local
models pulling real weight (the PC's GPU decides how much); the brain growing beyond
plain files when plain files measurably fall short; voice input; polish on the modes.
Uni term means smaller sessions — the system has to earn its keep as a study
environment too.

## Horizon

**The Circus.** The PC that already runs MojOS becomes the home server; the laptop
joins as the second machine; phones become windows into it. One intelligence across
all machines. Eventually a dedicated home server. The old fleet-era design thinking
lives in git history and gets rethought fresh when this becomes real.

**Collectives.** The formal model — groups with sovereign members, constitutions,
accountable AI participation. The deepest part of the vision and deliberately last:
it needs the individual layer to exist first.

**The essays.** The thinking in this repo written up properly and put in front of
people — professors, forums, the internet. Craft problem first; sovereignty and
collectives after. Feedback and collaborators are the goal, and the essays get
dramatically heavier once there's a working system behind them.
