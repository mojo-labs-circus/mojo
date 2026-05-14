# Next Move: Restructure

> **NOT READY.** This is a rough brain dump from one conversation. The names, structure, and everything else in here still needs to be properly reworked and agreed on before any of it is acted on.

## The Realisation

What we're building is actually 4 separate but composable projects under one roof — not one monolithic OS. Each is independently useful. Together they form the full vision.

| Project | What it is |
|---|---|
| **MojOS** | Clarke's personal Arch Linux setup — opinionated defaults, his way of running a machine |
| **dotfiles** | Look and feel — shell config, theming, UI |
| **Jarvis** | AI server + clients — the Ringmaster, web app, TUI, future clients. Useful completely standalone |
| **Ring** | Circus integration layer — connects machines together, coordinates a tech stack across a network |

The end product is pick-and-choose across all 4. Someone could run just Jarvis. Someone else could use Ring without Jarvis. Clarke runs all 4.

---

## The Naming Problem

`circus` is currently the repo name AND what we've been calling the integration layer — collision. Proposal:

- **Circus** stays as the umbrella brand/repo name
- The integration layer is called **Ring** — machines join the ring, the Ringmaster runs it. On-brand and distinct.
- The repo stays at `circus` (or gets renamed — open question below)

---

## Proposed Repo Structure

Monorepo for now (easier to manage spec across all 4, can split later):

```
circus/
├── spec/                    ← all planning (keep, expand per project)
│   ├── vision.md
│   ├── jarvis/              ← was ringmaster/
│   ├── ring/                ← was mojos/ (agent + circus integration)
│   ├── mojos/               ← MojOS OS setup
│   └── dotfiles/            ← look and feel
├── jarvis/                  ← AI server + all clients (was mojos/ringmaster + clients/)
│   ├── server/              ← FastAPI + LangGraph + Ollama
│   └── clients/
│       ├── web/
│       └── tui/
├── ring/                    ← circus integration layer (was mojos/agent/)
│   └── agent/               ← per-machine agent
├── mojos/                   ← MojOS install system (was mojos/os/)
│   └── os/
└── dotfiles/                ← look and feel (new — currently lives elsewhere)
```

---

## What Needs Moving / Renaming

| From | To | Notes |
|---|---|---|
| `mojos/ringmaster/` | `jarvis/server/` | Core rename |
| `clients/` | `jarvis/clients/` | Clients belong to Jarvis |
| `mojos/agent/` | `ring/agent/` | Agent is Ring, not MojOS |
| `mojos/os/` | `mojos/os/` | Stays, just loses the agent sibling |
| `spec/ringmaster/` | `spec/jarvis/` | Rename to match |
| `spec/mojos/` | Split into `spec/ring/` and `spec/mojos/` | Agent spec ≠ OS spec |

---

## Open Questions

1. **Repo rename?** Does `circus` stay as the repo/GitHub name or does it get a new name now that `circus` means the umbrella not the integration layer? Probably fine to keep — Circus is the umbrella brand.
2. **dotfiles** — currently live somewhere outside this repo. Do they come in now or later?
3. **Separate repos eventually?** Jarvis in particular might want its own repo when it's a real deployed service. Fine to stay monorepo for now.
4. **Install-time decision** — at MojOS install time, user chooses which of the 4 projects they want. Connects to the fork model (full Clarke stack vs. bare minimum vs. custom). Design this properly before building.

---

## Install-Time Fork Model (future)

At install time you choose your flavour:

- **Full Clarke stack** — all 4 projects, Clarke's exact config. Fork of his personal setup.
- **Base** — just the pieces you want. Pick from the matrix.
- **Custom fork** — someone else's curated combination (e.g. a user-friendly fork Clarke writes later)

This is what makes it composable and extensible like Linux itself.

---

## Before Any of This

Two things to complete first, before any restructure work begins:

1. **Finish Claude Code setup** — execute `claude-setup-plan.md` in full (global CLAUDE.md, settings, project settings, sprint workflow, component CLAUDE.md stubs)
2. **Rewrite `mojos/ringmaster/CLAUDE.md`** — currently stale Jarvis context referencing deleted files. Needs a clean rewrite for the current Circus architecture and actual implementation state before any component session is opened there
