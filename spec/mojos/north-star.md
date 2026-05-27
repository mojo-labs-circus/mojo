# MojOS — North Star

MojOS is a framework for turning a Linux machine into an agentic OS backed by a personal AI assistant. The OS layer and AI layer are designed together, share a single config source, and update together.

---

## What the Agent Carries

Three things must be consistent across all surfaces:

1. **Identity** — personality, name, preferences
2. **Memory** — what the agent knows
3. **Tasks** — active work and open loops

Private memory never leaves the device it lives on.

---

## Topology Overview

Three roles: Solo, Performer, Ringmaster. See [topology.md](topology.md).

---

## Design Principles

**Single identity across all surfaces.** Hardware and topology change what the agent can do locally — not who the agent is.

**CWD is agent context.** Where you are in the filesystem determines how the agent behaves. No manual mode switching. See [filesystem.md](filesystem.md).

**Intelligence lives where the hardware supports it.** Local inference when hardware allows. Ringmaster handles what local can't. Everything local when offline.

**Data never crosses a boundary without explicit intent.** The active circus is always known. Private memory never leaves the device. User can inspect where every piece of data lives.

**Single source of truth.** `.mojo_config` drives both the OS layer and the AI layer. See [mojo-config.md](mojo-config.md).

**Convergence model.** `mojo-update` brings the machine to current desired state — OS and agent together.

**Conventional tools first.** Cron, SSH, rsync, systemd. The agent uses these tools; it does not replace them. AI reasoning is for intent classification and coordination, not for wrapping shell commands.

**Confirmation gate on destructive actions.** The agent proposes, the user confirms.

---

## What MojOS Is Not

- Not tied to a single distro. The framework is distro-agnostic. Fork choices (e.g. Arch + Hyprland for baker) are not framework requirements.
- Not a cloud product. Everything runs on owned hardware. No SaaS layer.
- Not autonomous. Nothing destructive happens without user confirmation.
- Not public yet. Bootstrap requires auth. The decision to open the repo is deliberate.
