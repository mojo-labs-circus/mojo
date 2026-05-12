# MojOS Circus

> Your computers. Your data. Your AI.

A framework for turning a group of computers into a coordinated, intelligent system. One machine acts as the Ringmaster — the AI hub, shared memory, and coordination centre. The rest run MojOS as performers — each with a local agent, local AI, and a private connection to the Ringmaster. Built for multiple people: shared infrastructure, completely separate identity and memory per user.

The OS and AI layers are designed together from the start. They share a single config source, update together, and understand each other. Not an AI app installed on top of Linux — a platform where intelligence is part of the foundation.

**Mk1 goal:** ringbaker running as Ringmaster serving the family via web client. pearlybaker and nomadbaker running MojOS with a local agent connected to ringbaker.

---

## Directory Map

```
circus/
├── spec/                   ← all planning and spec docs
├── clients/
│   ├── tui/                ← terminal UI client
│   └── web/                ← web client for non-MojOS users
└── mojos/
    ├── ringmaster/         ← AI server backend (FastAPI + LangGraph + Ollama)
    ├── agent/              ← local mojo-agent for performer machines
    └── os/                 ← MojOS install system
```

## Components

| Component | Where it runs | Who uses it |
|---|---|---|
| `mojos/ringmaster/` | ringbaker | Everyone — the AI hub |
| `mojos/agent/` | pearlybaker, nomadbaker | Clarke only — local AI + proxy to Ringmaster |
| `clients/web/` | any browser | Family on Mac/Windows |
| `clients/tui/` | any terminal | Clarke on MojOS machines |
| `mojos/os/` | bare metal | Any machine joining the circus |

## How We Work

Top-level circus sessions handle spec, cross-component decisions, and repo structure. Implementation happens in component sessions opened from the component's own directory. Spec lives in `spec/` — never inside component directories.

---

*Private. Not open source. See [LICENSE](LICENSE).*
