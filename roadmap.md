# Roadmap

*What's actually happening. Now / next / horizon, revised in place. The destination
this all points at is [vision.md](vision.md).*

---

## Now — MojOS v0.1 + mojo-agent v0.1 (target: 15 September 2026)

The project reset to build-first in July 2026 (see [devlog.md](devlog.md)). I'm away
from my PC until 15 September — the start of second year — working from the laptop
all summer. The goal:

**By the day I'm back at my PC, the first version of an always-on Jarvis exists:
I can work with it, speak to it, and reach it from my phone — running on MojOS,
which is my daily driver.**

The finish line, as testable statements. Core — all of these:

1. My laptop runs MojOS as its daily driver (replacing Arch), built from the
   `mojos` flake and proven in a VM before it ever touched metal.
2. The entire system is declared in the flake — apps, Hyprland, dotfiles — and
   rebuilds with one command.
3. At least two modes exist and switch cleanly (looks, behaviour, and how much of
   the system shows — e.g. coding mode, normal mode for company).
4. mojo-agent is installed by the flake and runs as a resident service — always on
   whenever the machine is.
5. Running `mojo` opens my first mate, which keeps persistent memory about me as
   plain files on my disk.
6. It can control the computer — run commands, open apps, edit files — behind a
   confirmation gate.
7. It can delegate to the crew: hand work to Claude Code or a frontier model and
   bring back the result, keeping my personal context local.

The Jarvis surfaces — both wanted, built after the core holds, in whatever order
momentum favours:

- **Phone:** I can reach it from my phone (over Tailscale — same agent, smaller
  window).
- **Voice:** I can speak to it and it speaks back (local speech-to-text and
  text-to-speech).

Rough shape: **July** — learn Nix properly, build the flake, live in the VM, build
the mode system and a rice worth keeping (the old dotfiles are not coming along).
**August** — MojOS onto the laptop for real, daily-drive it, build the agent core.
**September** — the phone and voice surfaces, hardening. **15 September** — back at
the PC: MojOS goes on it (dual-boot with Windows), the agent deploys there, and the
PC — left on permanently — becomes the true always-on host. That day is the finish
line, not the start of the work.

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

**Collectives.** The formal model — groups with sovereign members, constitutions,
accountable AI participation. The deepest part of the vision and deliberately last:
it needs the individual layer to exist first.

**The essays.** The thinking in this repo written up properly and put in front of
people — professors, forums, the internet. Craft problem first; sovereignty and
collectives after. Feedback and collaborators are the goal, and the essays get
dramatically heavier once there's a working system behind them.
