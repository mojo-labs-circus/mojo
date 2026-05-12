# Idea: Multi-Circus

> Deferred — Mk1 supports one circus per performer. The design below is the intended long-term model.

---

## Model

A performer can join multiple circuses. Each circus is fully independent.

- One active circus at a time, stored as `active_circus_id`
- All operations (vault, routing, sync) scoped to active circus
- Jarvis never routes across circus boundaries silently
- Offline from active ringmaster → local cache for that circus only, no fallback to another circus
- Local vault namespaced by circus: `vault/{circus_id}/...`

## What Needs Building

- `active_circus_id` config management
- Vault namespacing by circus
- UX for switching active circus (likely a separate step after the backend is solid)
- Ensuring all operations (routing, sync, memory) correctly scope to active circus

## Mk1 Stub

Single circus. `active_circus_id` is always set to the one joined circus. No switching, no namespacing needed yet.
