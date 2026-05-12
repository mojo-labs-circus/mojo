# JARVIS — Infrastructure Ideas

Future work on server infrastructure, networking, and platform-level capabilities.

---

## Life operating layer — service integrations

The long-term goal is for Jarvis to be the operating layer for your entire life. You shouldn't need to open an app to do anything — you just tell Jarvis and it handles it. Example: walking downstairs, say "turn the Masters on in the basement" and it's playing by the time you get there.

This requires two things working together:
1. **API-based tool wrappers** — one tool per service (Jellyfin, Spotify, Home Assistant, calendar, etc.), all living in `tools/`. Jarvis composes these to fulfil requests.
2. **Screen/UI control** — for services that don't have APIs, a cursor/screen-control layer (see `skills.md`). This fills the gaps.

Applies to everyone on the platform, not just admin. A family member should be able to ask Jarvis to do something without knowing what's running under the hood. Build the tool wrapper pattern early so adding new services is just adding a new file — not a re-architecture.

---

## Tailscale ACL groups

Three ACL groups (admin / power / standard) map to the three Jarvis user tiers and give network-level enforcement before requests reach FastAPI. Planned uses:
- SSH access — admin group only
- Database port — admin group only, even on Tailscale
- Maintenance endpoints — Tailscale admin group + app-level admin role as a double gate
- Future services (Gitea, dashboards) — inherit the same group topology automatically
- TUI client — admin/power only at the network layer before auth runs

Defense-in-depth: Tailscale ACL is the outer ring, app-level role checks are the inner ring.

---

## Admin observability dashboard

Full dashboard — usage metrics, per-user activity, model performance, memory growth over time. Post-base-development addition once the platform has enough runtime data to make it useful. The Phase 9 admin panel is intentionally minimal; this is the grown-up version.

---

## GPU/CPU workload splitting

On the home server, route AI workloads by urgency:
- **GPU** — anything the user is actively waiting on: chat, code generation, real-time reasoning
- **CPU** — background tasks where latency doesn't matter: memory indexing, batch processing, maintenance jobs, storage condensation passes

Keeps interactive response times fast by not letting background work starve the GPU. Implement once the server is up and there's enough load to make scheduling meaningful.

---

## Centralised token storage via Vaultwarden

Per-client token storage works but silos credentials per device. Future: clients authenticate against Vaultwarden on the server for centralised, consistent tokens across all devices.

---

## User notifications (`notify_user`)

User-facing async notifications — background task completion, deadline reminders, etc. Delivery via WebSocket push to the active client, ntfy as fallback for offline. Distinct from `notify_admin`.
