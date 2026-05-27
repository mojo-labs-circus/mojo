# Memory

Three tiers. Every mojo surface understands this model.

---

## Tiers

### Server Memory
*Circus only. Lives on ringmaster: Postgres + vector store.*

- Shared across all surfaces in the circus
- Conversations, tasks, projects, personality config (source of truth)
- User-scoped — each user's memory fully isolated from other users on the same ringmaster
- Not present in Solo mode

### Local Memory
*Every MojOS node. Machine-scoped.*

Auto-accumulated:
- OS interactions: commands run, updates applied, services managed
- Files and directories referenced in conversations on this machine
- Periodic machine state snapshots (disk, services, active processes)
- Project and directory activity

User-curated:
- Explicit local notes
- Pinned directories as persistent context

In circus mode, relevant local memory is bundled as context with requests forwarded to the ringmaster. The ringmaster uses it for that request but does not persistently store it.

Pruned by hardware tier:
| Tier | Retention |
|---|---|
| `lite` | ~30 days, small vector store |
| `standard` | ~90 days |
| `full` | ~1 year |
| `max` | Full history, large vector store |

### Private Memory
*Device-only. Never leaves the machine.*

- User-designated — marked in conversation, config, or via local flag
- Stored in encrypted local store
- Never sent as context to the ringmaster
- mojo-agent knows it; the ringmaster never sees it

---

## Sync

Event-driven, outbox pattern. Local store is a managed cache — not a full mirror of server memory.

What syncs: identity config, active tasks, relevant memory slice.

> **Open — needs planning.** Patch wire format, version tracking, embedding cache invalidation not yet specced.

---

## Identity Portability

Agent identity (name, display name, style, language) lives in `.mojo_config`. See [mojo-config.md](mojo-config.md).

- Solo, multi-machine: copy the identity block from `.mojo_config` to each new machine at install. Same agent persona, no sync protocol needed. Memory stays separate per machine.
- Circus mode: ringmaster holds personality config as source of truth for all surfaces.

---

## Data Transparency

Every piece of information the agent holds is associated with a location: server / local / private. The user can inspect, move, and lock data.

Full privacy control layer UX is deferred. See `ideas/`.
