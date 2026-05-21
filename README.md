# MojOS Circus

> Your computers. Your data. Your AI.

A framework for turning a group of computers into a coordinated, intelligent system. One machine acts as the Ringmaster — the AI hub, shared memory, and coordination centre. The rest run MojOS as performers — each with a local agent, local AI, and a private connection to the Ringmaster. Built for multiple people: shared infrastructure, completely separate identity and memory per user.

The OS and AI layers are designed together from the start. They share a single config source, update together, and understand each other. Not an AI app installed on top of Linux — a platform where intelligence is part of the foundation.

**Mk1 goal:** ringbaker running as Ringmaster serving the family via web client. pearlybaker and nomadbaker running MojOS with a local agent connected to ringbaker.

---

## Repo Structure

This repo (`circus`) is spec and planning only. Component code lives in sibling repos under `~/projects/`.

```
~/projects/
├── circus/         ← this repo — spec, planning, memory
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
| Performer agent | `performer` | pearlybaker, nomadbaker | Clarke only |
| Web client | `client-web` | any browser | Family on Mac/Windows |
| TUI client | `client-tui` | any terminal | Clarke on MojOS machines |
| MojOS installer | `mojos` | bare metal | Any machine joining the circus |

## How We Work

`circus` sessions handle spec, cross-component decisions, and repo structure. Implementation happens in component sessions opened from each component's own repo. Spec lives in `circus/spec/` — never inside component repos.

---

*Private. Not open source. See [LICENSE](LICENSE).*
