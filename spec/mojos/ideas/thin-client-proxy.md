# Idea: Thin Client Routing Through Local Proxy

> Deferred — current Mk has thin clients (web, mobile) connecting directly to the ringmaster. The local proxy is only in the path for MojOS terminal clients.

---

## The Gap

A user on a MojOS performer who opens the web client in their browser bypasses the local mojo-agent entirely — the web client connects direct to `ringmaster/ws/client`. They lose:
- Local context injection
- Private memory filtering
- Offline resilience

---

## What a Future Mk Could Do

The web client (served from ringmaster) could probe for a local mojo-agent at `localhost/ws` on load. If found, connect there instead. The local agent handles it like any other local client request — routing decision, context injection, privacy filtering all apply.

Same logic could apply to a mobile app when on the local Tailscale network.

---

## Dependency

Needs the core proxy model working and stable first. Don't design the detection/handoff until the MojOS terminal proxy path is solid.
