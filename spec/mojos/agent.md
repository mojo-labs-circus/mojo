# mojo-agent

The MojOS agent. Runs on every MojOS machine as a systemd service. Behaviour depends on `FLEET_PROFILE`. See [topology.md](topology.md) for role definitions.

---

## Local Stack

Every MojOS machine runs a full local agent stack:

| Component | Purpose |
|---|---|
| LangGraph graph | Agent reasoning and routing |
| Ollama | Local inference |
| Local vault | Credentials, namespaced by circus |
| Local vector store | Embeddings for local memory (details TBD) |
| SQLite | Conversation history, tasks, auth cache |
| Local API endpoint | WebSocket server on localhost for TUI, native app, local browser |

---

## LangGraph Graph

> **Open — needs planning.** Node breakdown and local services per tier not yet specced.

Known nodes:
- `ROUTER` — classifies intent, decides local vs ringmaster
- `CONVERSATION` — general chat
- `SYSTEM` — OS management tasks
- `MEMORY` — memory read/write
- `RESPONDER` — formats and returns response

---

## Routing Logic (Performer)

Two-stage. Only relevant in circus mode — standalone handles everything locally.

**Stage 1 — Rules (fast path):**

| Route local when | Route ringmaster when |
|---|---|
| OS management task | Needs ringmaster memory from previous sessions |
| References local files or machine state | Multi-step task requiring planner |
| Privacy flag set | Cross-machine or cross-surface coordination |
| Ringmaster unreachable (automatic fallback) | Local hardware is lite tier and task needs real reasoning |
| Single intent, no conversation context needed | Web search |

**Stage 2 — Model classifies (ambiguous cases only):**

A lightweight local model classifies `local_capable: true/false` for cases the rules don't resolve. No LLM call for clear-cut cases. The routing decision never requires a ringmaster round-trip.

Ringmaster fallback is always graceful — if unreachable, mojo-agent handles everything. No explicit offline mode.

> **Open — needs planning.** What the performer sends to the ringmaster, how context is bundled, streaming response routing. See [memory.md](memory.md) for memory sync specifics.

---

## Hardware Tiers

`PROFILE` (chassis) + GPU VRAM → hardware tier → Ollama model selection. Derived at install time by `post-install.sh`. Not user-settable.

| Tier | Hardware | Models |
|---|---|---|
| `lite` | No discrete GPU | phi3-mini, qwen2.5:0.5b, smollm2 |
| `standard` | Integrated / low VRAM | llama3.2:3b, phi3:3.8b |
| `full` | Discrete GPU 8–24GB | llama3.1:8b, mistral:7b, qwen2.5:7b |
| `max` | GPU 24GB+ | llama3.1:70b, qwen2.5:32b |

Tier also influences routing in circus mode — lite machines route more to the ringmaster.

---

## WebSocket Endpoints

| Endpoint | Who connects |
|---|---|
| `localhost/ws` | Local clients (TUI, native app) — connects to mojo-agent daemon |
| `ringmaster/ws/client` | Human-facing thin clients (web, mobile) |
| `ringmaster/ws/agent` | mojo-agent performers; any future AI-to-AI connections |

> **Open — needs planning.** Message types, auth handshake, connection lifecycle, streaming, outbox flush on reconnect.

---

## Proactive Monitoring

Local agent monitors machine state and surfaces relevant events to the user:

- Low disk → notify with context
- Failed service → notify, offer to diagnose or fix
- `mojo-update` available → report what changed

These are always handled locally. No ringmaster involvement.
