# Circus

Formation mechanics, auth, and join flow. For roles and topology see [topology.md](topology.md).

---

## Formation

**Ringmaster** — any machine on a server chassis auto-configures as a ringmaster on install. No manual step. Docker containers spin up, the Ringmaster starts, the circus is live.

**Performer** — any non-server chassis starts as Solo. Joins a circus explicitly via `mojo-circus-join`.

---

## mojo-circus-join

```
mojo-circus-join <ringmaster-hostname>
```

1. Connects the machine to the circus's Tailscale tailnet
2. Authenticates with the user's Ringmaster credentials for that circus

On success: writes `AGENT_TOKEN` to `.mojo_config`, configures `mojo-agent` to route to ringmaster.

---

## Auth

- Admin generates invite tokens on the ringmaster
- Users register with an invite token via any client or directly on a MojOS machine
- Registration issues a `CIRCUS_TOKEN` (JWT) stored in `.mojo_config`
- All subsequent `mojo-agent` ↔ ringmaster connections use the JWT

Registration handshake:
```json
POST /circus/register
{
  "hostname":       "nomadbaker",
  "hardware_info":  { "tier": "full" },
  "tailscale_ip":   "100.x.x.x",
  "circus_ticket":  "...",
  "agent_protocol": "1.0"
}
```

Response issues `CIRCUS_TOKEN`. Protocol version is the compatibility gate.

---

## Each Circus is Self-Contained

- Own Tailscale tailnet
- Own ringmaster
- Own Ringmaster auth server

MojOS is a framework — anyone can run their own circus. No shared infrastructure.

---

> **Mk1: one circus per performer.** Multi-circus is deferred. See `ideas/multi-circus.md`.
