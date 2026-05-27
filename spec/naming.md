# Naming Conventions

Canonical names for every component, concept, role, and code-level term in this project. When in doubt, use this file.

---

## System Components

| Name | What it is |
|---|---|
| **Circus** | The full system — a Ringmaster and all its Performers, connected over Tailscale |
| **Ringmaster** | The hub. Runs on a server machine. Owns shared memory, heavy models, multi-user auth, and coordination across the Circus. One per Circus. |
| **mojo-agent** | The local AI agent. Runs on any machine. Handles local tasks, routes to the Ringmaster when needed, degrades gracefully when the Ringmaster is unreachable. |
| **MojOS** | Arch Linux with mojo-agent integrated at the session level. A purpose-built environment — not a requirement for running an agent. |
| **mojo-sdk** | Shared API contracts, WebSocket frame types, and client library for communicating with the Ringmaster. Used by performer and clients. |
| **client-web** | Browser-based thin client. Connects directly to the Ringmaster over Tailscale. For non-MojOS users. |
| **client-tui** | Terminal UI client. Connects to mojo-agent on MojOS machines, or directly to the Ringmaster elsewhere. |

---

## Machine Roles

Assigned by chassis type at install time. Not manually configured.

| Role | Chassis | Behaviour |
|---|---|---|
| **Ringmaster** | Server chassis | Auto-configures as Circus hub on install. |
| **Performer** | Non-server, joined a Circus | Runs mojo-agent as a proxy. Routes complex requests to the Ringmaster. |
| **Solo** | Non-server, not in a Circus | Runs mojo-agent fully locally. No Ringmaster connection. |

---

## Repos

| Repo | Purpose |
|---|---|
| `circus` | Spec and planning only. No runnable code. |
| `ringmaster` | Ringmaster server — FastAPI, LangGraph, Ollama. |
| `performer` | mojo-agent for performer and solo machines. |
| `mojos` | MojOS install system — bootstrap, install, configure scripts. |
| `mojo-sdk` | Shared contracts and client lib. |
| `client-web` | Web client. |
| `client-tui` | TUI client. |

All repos live as siblings at `~/projects/<name>/`.

---

## Baker Fleet

The reference Circus used for development and Mk1.

| Machine | Role | Hardware |
|---|---|---|
| `ringbaker` | Ringmaster | Home server |
| `pearlybaker` | Performer | Desktop, A6000 |
| `nomadbaker` | Performer | Laptop |

Tailnet: `circus-tent`

---

## Code-Level Names

### Graph state types

| Type | Repo | What it represents |
|---|---|---|
| `RingmasterState` | `ringmaster` | LangGraph state for a single Ringmaster request |
| `AgentState` | `performer` | LangGraph state for a single mojo-agent request |

### Graph node names

Nodes are uppercase constants in code and in documentation.

| Node | Lives in | Purpose |
|---|---|---|
| `ROUTER` | both | Classifies intent, applies tier gate |
| `PLANNER` | ringmaster | Produces a `StepPlan` from intent |
| `DECOMPOSER` | ringmaster | Fills `Step.prompt` for each step in the plan |
| `ORCHESTRATOR` | ringmaster | Resolves dependencies, dispatches steps, loops until plan exhausted |
| `CONVERSATION` | both | General chat |
| `TASKS` | both | Creates, updates, and completes tasks |
| `SYSTEM` | performer | OS management — machine state, services, disk |
| `MEMORY` | both | Memory read/write via vector store |
| `WEB` | ringmaster | Web search via DuckDuckGo + Playwright |
| `RESPONDER` | both | Assembles final response from step results, emits token frames |

### WebSocket frame types

All frames are lowercase strings.

| Frame | Direction | Meaning |
|---|---|---|
| `token` | server → client | Streaming response chunk |
| `done` | server → client | Request complete; carries `refresh` array |
| `error` | server → client | Request failed; carries error detail |
| `status` | server → client | In-progress status update |
| `profile` | server → client | Profile changed; client should re-fetch `GET /profile` |

### Key variable names

| Name | Type | Where used | What it holds |
|---|---|---|---|
| `user_id` | `int` | state, DB | Primary key for a user |
| `username` | `str` | state, auth | Login handle |
| `tier` | `str` | state, profile | `admin` / `power` / `standard` |
| `assistant_name` | `str` | state, profile | User-configured name for their agent persona |
| `step_plan` | `StepPlan` | state | Planner output — ordered list of steps |
| `step_results` | `dict` | state | Accumulated outputs from completed steps |
| `assembled_response` | `str` | state | Final response assembled by RESPONDER |
| `detected_skills` | `list` | state | Skills identified by ROUTER |
| `intent` | `str` | state | Classified intent from ROUTER |
| `error` | `str` | state | Set by any node on failure; clears after RESPONDER handles it |

---

## Agent Context Files

`.mojo` files are hidden config files that live in directories on a MojOS machine. They define how mojo-agent behaves when operating in that directory — tools available, permissions, narrative context. The agent walks root → CWD, loading each `.mojo` in order; most-specific scope wins on conflict.

MojOS pre-populates `.mojo` files in key system directories at install time. Users and projects layer on top.

| Path | Purpose |
|---|---|
| `/.mojo` | Machine-level baseline. Safety rules, base tools, machine identity. |
| `/etc/.mojo` | System config context. Flags this area as system-managed. |
| `/home/.mojo` | Minimal pass-through. Scopes agent to user home space. |
| `~/.mojo` | User-level defaults. Identity, preferences, workflow. |
| `<project>/.mojo` | Project-specific context. Layered on top of system files. |

---

## Config and CLI

| Name | What it is |
|---|---|
| `.mojo_config` | Per-machine config file. Single source of truth for machine role, hardware tier, circus membership, and credentials. |
| `mojo-circus-join` | CLI command to join a machine to a Circus. |
| `mojo-update` | CLI command to converge a machine to current desired state (OS + agent together). |
| `install-ringmaster.sh` | Sets up the Ringmaster on a server machine. |
| `install-agent.sh` | Drops mojo-agent onto any existing machine. |
| `install-mojos.sh` | Bare metal MojOS install. |

---

## User Tiers

Tiers are a safety boundary, not a feature gate.

| Tier | Who | What it controls |
|---|---|---|
| `admin` | Clarke | Full access — all nodes, system shell, user management |
| `power` | Power users | Full access — all nodes, system shell, skill management |
| `standard` | General family | Chat, tasks, memory, web — no shell, no coding team |

---

## The Assistant Persona

Each user configures their own assistant name (e.g. "JARVIS", "Gilgamesh"). This is stored per-user in the database and served via `GET /profile`. It is not a system name — it has no meaning in code, config, or architecture. It is purely the name a user gives their agent.
