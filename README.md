# Mojo

> A framework for collective intelligence — and its first instance.

Mojo is two things.

**The framework** — a general model for how any group of intelligences, human and AI, can coordinate toward a shared goal. Defines what a collective is, how it constitutes itself, and how it specialises into a domain (software development, a household, a research group, a company).

**Mojo Circus** — the first instance of that framework. A framework for turning a group of computers into a coordinated, intelligent system. One machine acts as the Ringmaster — the AI hub, shared memory, and coordination centre. The rest run MojOS as performers — each with a local agent, local AI, and a private connection to the Ringmaster. Built for multiple people: shared infrastructure, completely separate identity and memory per user.

The OS and AI layers are designed together from the start. They share a single config source, update together, and understand each other. Not an AI app installed on top of Linux — a platform where intelligence is part of the foundation.

---

## Current Stage

Framework design. Dev work is paused until the framework is complete. Planning sessions are ready to run in `spec/framework/`. See `next-move.md` for the full sequence.

---

## Repo Structure

This repo is spec and planning only. Component code lives in sibling repos under `~/projects/mojo-labs/`.

```
~/projects/mojo-labs/
├── mojo/           ← this repo — spec, planning, memory
├── ringmaster/     ← AI server backend (FastAPI + LangGraph + Ollama)
├── performer/      ← local mojo-agent for performer machines
├── mojos/          ← MojOS install system
├── client-tui/     ← terminal UI client
├── client-web/     ← web client for non-MojOS users
└── mojo-sdk/       ← shared API contracts / client lib
```

## Components

| Component | Repo | Where it runs | Who uses it |
|---|---|---|---|
| Ringmaster | `ringmaster` | ringbaker | Everyone — the AI hub |
| Performer agent | `performer` | pearlybaker, nomadbaker | Clarke |
| Web client | `client-web` | any browser | Family on Mac/Windows |
| TUI client | `client-tui` | any terminal | Clarke on MojOS machines |
| MojOS installer | `mojos` | bare metal | Any machine joining the circus |

---

*Private. Not open source. See [LICENSE](LICENSE).*
