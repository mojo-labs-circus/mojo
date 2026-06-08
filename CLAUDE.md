# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Why This Exists

This is a personal project with genuine ambition. Clarke is not trying to start a company or compete with anyone — but he is trying to build something that matters beyond just himself.

The problems being solved:

- **Devices are islands** — machines don't know about each other. Context is lost every time you move between them. Your work is scattered across hardware that has no awareness of the others.
- **Cloud dependency** — AI and personal data living on someone else's servers, under their pricing, their policies, their continued existence. The convenience is real. So is the dependency.
- **AI as a tool, not a partner** — you stop what you are doing, open a chat window, ask a question, copy the answer back. Useful. Not a partner.
- **Individual agency** — institutions are adopting AI for themselves, not for individuals. The person who controls the AI controls the narrative, the workflow, the decisions. If individuals do not have their own AI that actually belongs to them, the shift to AI is just a new kind of institutional power over individual life.
- **No framework for collective intelligence** — no principled model for how groups of intelligences — human and AI — can coordinate toward a shared goal. The pieces exist. The coherent system does not.

This is not a startup. The ambition is real but the motivation is genuine, not commercial.

If someone else solves a piece of this better, use their solution. No ego in the plumbing.

---

## Current Stage — Framework Design

**Dev work is paused.** The project scope has expanded. Mojo is not just a home AI
system — it is an attempt to model and implement collective intelligence: groups of
intelligences (human, AI, or mixed) coordinating toward a shared goal. The family
AI system is the first instance of that framework. The framework is the real thing.

Key concepts established so far:

- A **collective** is the entity — any group of intelligences working toward a shared goal
- **Intelligences** are the members — human and AI alike, defined the same way
- Every collective has a **constitution** — five founding documents that declare what it is
- **Domains** specialise a collective into a predefined category (software dev, research, household, etc.)

Framework planning sessions are ready to run in `docs/framework/`. Ten sessions in
total — the original eight plus two new ones for the constitutional contract and domains.

**Planning sessions:**

```
01-first-principles      ← what collective intelligence is and why it forms
02-agent-model           ← what an intelligence in a collective looks like
03-frontier-interface    ← how local intelligence calls external intelligence
04-org-structure         ← how intelligences organise into a collective
  constitution           ← how a collective formally declares itself
05-communication-model   ← how intelligences share understanding
06-leadership-interface  ← the role of the top node in a collective
07-layers-and-contracts  ← tooling, orchestration, intelligence — separated
  domains                ← how collectives specialise into predefined categories
08-dev-team-instance     ← the framework applied to a software dev team
```

Session output accumulates in `docs/framework/framework.md`. Pre-session briefs for
the two new sessions are in `docs/framework/constitution.md` and `docs/framework/domains.md`.

**Work tracking is in the Obsidian Kanban board** at `~/projects/mojo-labs/kanban.md`.

---

## What This Is

Mojo is two things that fit together.

**1. A framework for collective intelligence.** A general model for how any group of intelligences — human and AI — can coordinate toward a shared goal. Defines what a collective is, how it constitutes itself, how it specialises into a domain. General enough to apply to a software dev team, a household, a research group, a company. The framework is the real contribution.

**2. Mojo Circus — the infrastructure layer.** The platform that makes a collective real across hardware. A group of machines forming a coordinated intelligent system over Tailscale. One machine runs as the Ringmaster (AI server + hub). Others run as performers (local AI agent + local compute). Non-MojOS machines connect via thin clients. Frontier models (Claude, Codex, Gemini) are called as tools — given only what they need, never trusted with the full picture.

Circus is not an instance of the framework — it is what runs framework instances on real hardware.

## Directory Map

This repo is spec and planning only. Component repos are siblings under `~/projects/mojo-labs/`.

```
mojo/                           ← this repo — org-level docs and planning
├── docs/
│   ├── vision/                 ← product vision, pain points, aesthetic
│   ├── framework/              ← framework planning sessions (01-08) + output
│   ├── architecture/           ← component architecture (moves to component repos post-framework)
│   ├── decisions/              ← ADRs (MADR format)
│   ├── collective/             ← collective governance
│   ├── research/               ← exploratory ideas, competitor analysis
│   └── reference/              ← naming conventions, coding standards
└── memory/                     ← persistent session memory for top-level Claude sessions

Sibling repos (all at ~/projects/mojo-labs/<name>/):
  ringmaster/                   ← AI server backend (FastAPI + LangGraph + Ollama)
  performer/                    ← local mojo-agent for performer machines
  mojos/                        ← MojOS install system (bootstrap, install, configure scripts)
  dotfiles/                     ← system dotfiles — deployed by MojOS, drives the aesthetic of MojOS machines
  client-tui/                   ← terminal UI client
  client-web/                   ← web client for non-MojOS users (family on Mac/Windows)
  mojo-sdk/                     ← shared API contracts / client lib for Ringmaster
```

## Component Roles

| Component | Repo | Where it runs | Who uses it |
|---|---|---|---|
| Ringmaster | `../ringmaster/` | ringbaker server | Everyone — the AI hub |
| Performer agent | `../performer/` | pearlybaker, nomadbaker | Clarke only — local AI + proxy to Ringmaster |
| Web client | `../client-web/` | any browser | Family on Mac/Windows, Clarke on any machine |
| TUI client | `../client-tui/` | any terminal | Clarke on MojOS machines |
| MojOS installer | `../mojos/` | bare metal install | Any machine joining the circus |
| Dotfiles | `../dotfiles/` | any MojOS machine | All MojOS machines — aesthetic + environment config |
| SDK | `../mojo-sdk/` | shared library | performer and clients |

## Key Architecture Decisions (Current Assumptions — Subject to Revision Post-Framework)

- **Clients connect to the nearest Jarvis.** Non-MojOS machines connect directly to the Ringmaster. MojOS machines connect to the local agent, which proxies to the Ringmaster when needed.
- **mojo-agent is a separate, thinner codebase** from the Ringmaster — single-user, subset of nodes, smaller models, no multi-user auth.
- **Routing decision lives in the agent's ROUTER node** — determines whether a request is handled locally or forwarded to the Ringmaster (proxy model for Mk1).
- **Private memory never leaves the performer machine.** Only non-private requests are forwarded.
- **AGENT_TOKEN is per machine-user pair** — a performer registers with the Ringmaster under a specific user identity within a specific circus.
- **Model selection is driven by MojOS hardware detection** — `.mojo_config` knows the GPU/CPU profile; both Ringmaster and agent pick models independently.

## Planning Conventions

- Org-level docs (vision, framework, decisions, governance) live in `docs/` in this repo.
- Component architecture docs will move to their own repos once the framework is complete. `docs/architecture/ringmaster/` and `docs/architecture/mojos/` are temporary.
- Writing conventions are in `docs/reference/conventions.md`.
- Architecture decisions are recorded as ADRs in `docs/decisions/` (MADR format).
- Each component repo has its own `CLAUDE.md` for dev session context.
- This file is for project-level orientation only — not dev workflow.
- Open architecture questions belong in the relevant `docs/` file, not in code comments.

## How We Work (Top-Level Claude)

This session handles big-picture coordination: spec, framework design, cross-component
decisions, repo structure. Implementation work happens in component sessions opened
from the component's own directory.

**We are currently in planning sessions.** Read `docs/framework/00-session-guide.md`
before opening any session brief — it explains how to run each session, including
finding videos, talks, and papers before forming conclusions. Then open each
`docs/framework/` brief in order. Run them sequentially. Do not start session N+1
until session N has produced its required output.

**Research is part of the work.** Each session starts with finding real learning
material — YouTube talks, papers, case studies. Passion and depth of exploration
are valued. The anchor is always the output: research feeds it, not replaces it.

**Explain before writing.** Before producing any file or making structural changes,
explain the approach and wait for approval.

**Commits happen after meaningful changes** — spec updates, framework decisions,
architecture decisions, repo migrations. Component-level commits happen within their
own sessions.

**Never add Claude attribution to commit messages.**

**Git uses SSH remotes and `gh` CLI for GitHub operations** — no HTTPS remotes.
Use `mcp__github__*` tools for issues, PRs, and comments — not `gh` CLI.
