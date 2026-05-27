# MojOS — Topology

## Roles

Assigned by chassis type, not manual config.

| Role | Chassis | Behaviour |
|---|---|---|
| **Solo** | Non-server, not in a circus | Default state. Full local mojo-agent. No server connection. |
| **Performer** | Non-server, joined a circus | mojo-agent runs as a proxy to the ringmaster. |
| **Ringmaster** | Server chassis | Auto-configures on install. Circus hub. Full Ringmaster stack. |

---

## The Proxy Model (Performer)

Every request from a MojOS terminal on a performer hits mojo-agent first. mojo-agent decides:
- Handle locally (OS tasks, private memory, offline, anything local hardware can handle)
- Forward to ringmaster (complex reasoning, shared memory, web search)

The terminal always talks to `localhost/ws` — it never knows whether the response came from local or ringmaster.

What the proxy adds:
- Local context injection (machine state, OS info) bundled into forwarded requests
- Privacy filtering — private memory never forwarded
- Offline resilience — mojo-agent handles what it can; requests queue until ringmaster reachable
- OS-level answers without a round trip

> Thin clients (web, mobile) connecting through the local proxy is deferred. See `ideas/thin-client-proxy.md`.

Routing logic: see [agent.md](agent.md). Memory tiers and privacy: see [memory.md](memory.md).

---

## Client Topology

```
[Ringmaster]
  /ws/client   ← thin clients (web, mobile)
  /ws/agent    ← mojo-agent performers
       │
       │  WebSocket over Tailscale
       │
  ┌────┴────────────────┐
  │                      │
[Performer]          [Performer]          [web / mobile]
  localhost/ws          localhost/ws        ringmaster/ws/client
  ↓ mojo-agent          ↓ mojo-agent        (direct)
  → ringmaster          → ringmaster
    /ws/agent             /ws/agent
```

---

Formation and auth: see [circus.md](circus.md). Multi-circus support is deferred to a later Mk — see `ideas/multi-circus.md`.

---

## Multi-User (Ringmaster)

- Each user gets isolated memory and identity on a shared ringmaster.
- Personal memory: `memory_{user_id}` — fully isolated per user.
- Shared memory: `memory_shared` — readable by all members, classifier auto-routes writes.
- A MojOS performer is scoped to one user via config. Multiple performers per user supported.
- User tiers: `admin` / `power` / `standard` — safety boundary, not a feature gate.

---

## Baker Fleet (Reference)

```
ringbaker     — ringmaster
pearlybaker   — performer (desktop, A6000)
nomadbaker    — performer (laptop)
```

Tailnet: `circus-tent`. Non-MojOS family members connect to ringbaker via `/ws/client`.
