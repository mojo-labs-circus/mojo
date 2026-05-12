# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

MojOS Circus — a private, self-hosted AI framework. A group of machines forms a coordinated intelligent system over Tailscale. One machine runs as the Ringmaster (AI server + hub). Others run MojOS as performers (local AI agent + local compute). Non-MojOS machines (Mac, Windows, mobile) connect via thin clients directly to the Ringmaster.

**Mk1 goal:** ringbaker running as Ringmaster serving the family via web client. pearlybaker and nomadbaker running MojOS with a local agent connected to ringbaker.

## Directory Map

```
circus/
├── spec/                       ← all planning and spec docs
│   ├── vision.md               ← product vision and pitch
│   ├── ringmaster/             ← Ringmaster architecture specs
│   │   └── ideas/              ← future ideas (skills, ai, clients, memory, infrastructure)
│   └── mojos/                  ← MojOS OS/agent specs
│       └── ideas/              ← exploratory ideas (fleet-sync, fork, multi-circus, etc.)
├── clients/                    ← all clients — OS-agnostic, work on any machine
│   ├── tui/                    ← terminal UI client
│   └── web/                    ← web client for non-MojOS users (family on Mac/Windows)
└── mojos/
    ├── ringmaster/             ← AI server backend (FastAPI + LangGraph + Ollama)
    ├── agent/                  ← local mojo-agent for performer machines
    └── os/                     ← MojOS install system (bootstrap, install, configure scripts)
```

## Component Roles

| Component | Where it runs | Who uses it |
|---|---|---|
| `mojos/ringmaster/` | ringbaker server | Everyone — the AI hub |
| `mojos/agent/` | pearlybaker, nomadbaker | Clarke only — local AI + proxy to Ringmaster |
| `clients/web/` | any browser | Family on Mac/Windows, Clarke on any machine |
| `clients/tui/` | any terminal | Clarke on MojOS machines |
| `mojos/os/` | bare metal install | Any machine joining the circus |

## Key Architecture Decisions

- **Clients connect to the nearest Jarvis.** Non-MojOS machines connect directly to the Ringmaster. MojOS machines connect to the local agent, which proxies to the Ringmaster when needed.
- **mojo-agent is a separate, thinner codebase** from the Ringmaster — single-user, subset of nodes, smaller models, no multi-user auth.
- **Routing decision lives in the agent's ROUTER node** — determines whether a request is handled locally or forwarded to the Ringmaster (proxy model for Mk1).
- **Private memory never leaves the performer machine.** Only non-private requests are forwarded.
- **AGENT_TOKEN is per machine-user pair** — a performer registers with the Ringmaster under a specific user identity within a specific circus.
- **Model selection is driven by MojOS hardware detection** — `.mojo_config` knows the GPU/CPU profile; both Ringmaster and agent pick models independently.

## Planning Conventions

- Spec lives in `spec/` — never inside component directories.
- Each component has its own `CLAUDE.md` for dev session context (e.g. `mojos/ringmaster/CLAUDE.md`).
- This file is for project-level orientation only — not dev workflow.
- Open architecture questions and decisions belong in the relevant `spec/` file, not in code comments.

## How We Work (Top-Level Claude)

This session handles big-picture coordination: spec, cross-component decisions, repo structure, and migrations. Implementation work happens in component sessions (opened from the component's own directory).

**Explain before writing.** Before producing any file or making structural changes, explain the approach and wait for approval.

**Commits happen after meaningful changes** — spec updates, architecture decisions, repo migrations, cross-component changes. Component-level implementation commits happen within their own sessions.

**Never add Claude attribution to commit messages.**

**Git uses SSH remotes and `gh` CLI** — no HTTPS remotes.
