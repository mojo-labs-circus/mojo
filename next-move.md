# Next Move: Restructure

## The Vision

The Circus is a personal AI system built around one idea: **Jarvis as a partner, not a tool.** A presence that lives on your machine, works alongside you, does things while you do things, and surfaces what you need when you need it.

The aesthetic is a workshop, not a factory. Woody, not metal. A space that draws the light out of you — that makes you want to create something you're passionate about. See `spec/aesthetic.md`.

Long term: a room full of screens, spatial computing, ambient information. Near term: whatever hardware you have, scaled to match. A laptop is a smaller workshop. A desktop with multiple monitors is a bigger one. Same system, same feel, different size.

---

## The Four Pieces

Everything is composable and pick-and-choose. Each piece works standalone. Together they form the full vision.

| Piece | What it is |
|---|---|
| **Jarvis** | The AI server hub. Runs on a dedicated machine. Central data store, heavy compute, multi-user coordination. |
| **Mojo-agent** | The local AI partner. Runs on any machine, any OS. The primary experience for anyone at a computer. |
| **MojOS** | Arch Linux with the agent deeply integrated at the session level. The full workshop, purpose-built. |
| **Dotfiles** | The look and feel. The aesthetic. Works on MojOS or any other setup. |

---

## The Device Model

The device determines the experience — not the user.

| Device | Experience | Why |
|---|---|---|
| Computer (any OS) | Mojo-agent | Embedded in the machine, works alongside you, the real partner experience |
| MojOS machine | Mojo-agent (deeper) | Same agent, fully integrated from the OS up — the full workshop |
| Phone / tablet | Thin client | No full compute, glance-and-go, quick instructions |
| Watch | Thin client | Ultra-thin surface, surface what Jarvis wants to show, fire quick commands |

**Agent-first. Server-optional. Thin clients as escape hatches.**

Everyone gets an agent. Jarvis is the power booster — extra compute, shared memory, coordination across machines. If Jarvis goes down, agents still work locally. Nothing stops completely.

---

## How Jarvis Fits In

Jarvis (the server) doesn't change. It's still the hub:

- All shared data and memory lives here
- Runs heavy models the agent can't run locally
- Serves mojo-agents when they need more power
- Also serves thin clients directly for phones, watches, shared machines

Same server. Two types of clients: agents (primary) and thin clients (fallback).

---

## MojOS vs Just the Agent

The difference is commitment level, not capability.

**Mojo-agent** — "I like my setup, I just want the agent on it." Your OS stays yours. The agent runs alongside it. Works on Mac, Windows, any Linux.

**MojOS** — "I want the whole thing, purpose-built." Arch Linux configured from the ground up around the agent. The workshop. Hyprland, dotfiles, the agent, everything pre-integrated and working together.

Same agent codebase underneath. MojOS just goes deeper.

---

## The Install Story

Three installers, any combination valid:

| Installer | What it does |
|---|---|
| `install-jarvis.sh` | Runs on any Linux server. Sets up Ubuntu and Jarvis in one shot. |
| `install-mojos.sh` | Bare metal install. Full MojOS workshop from scratch. |
| `install-agent.sh` | Any existing machine. Drops the mojo-agent onto whatever OS is there. |

Dotfiles are separate — installable independently on any machine.

---

## Proposed Repo Structure

Monorepo for now. Can split later when pieces mature enough to live alone.

```
circus/
├── spec/
│   ├── vision.md
│   ├── aesthetic.md             ← done
│   ├── jarvis/                  ← was spec/ringmaster/
│   ├── agent/                   ← split from spec/mojos/
│   ├── mojos/                   ← OS spec only
│   └── dotfiles/                ← new
├── jarvis/
│   ├── server/                  ← was mojos/ringmaster/
│   └── clients/                 ← was clients/ (thin client surfaces)
│       ├── web/
│       └── tui/
├── agent/                       ← was mojos/agent/
├── mojos/                       ← was mojos/os/
└── dotfiles/                    ← currently lives elsewhere
```

---

## What Needs Moving / Renaming

| From | To | Notes |
|---|---|---|
| `mojos/ringmaster/` | `jarvis/server/` | Core rename |
| `clients/` | `jarvis/clients/` | Thin client surfaces belong to Jarvis |
| `mojos/agent/` | `agent/` | Agent is its own top-level piece |
| `mojos/os/` | `mojos/` | Loses the agent sibling, just the OS now |
| `spec/ringmaster/` | `spec/jarvis/` | Rename to match |
| `spec/mojos/` | Split → `spec/agent/` + `spec/mojos/` | Agent spec ≠ OS spec |

---

## Open Questions

1. **Dotfiles** — currently live in a separate repo. When do they come in here?
2. **Separate repos eventually?** Jarvis in particular might want its own repo when it's a real deployed service. Fine monorepo for now.

---

## Next Actions

1. Execute the restructure above — move dirs, rename spec folders
2. Rewrite `CLAUDE.md` to reflect the new architecture and component names
3. Rewrite `mojos/ringmaster/CLAUDE.md` → will become `jarvis/server/CLAUDE.md` — currently stale
4. Finish Claude Code setup (`claude-setup-plan.md`)
