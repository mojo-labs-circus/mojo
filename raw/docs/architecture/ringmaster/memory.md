# Memory Architecture

## Short-Term Memory (Conversation History)
- Owned by FastAPI — not LangGraph
- Uses the same repository pattern as TASKS: `SQLiteHistoryRepository` in dev, `PostgresHistoryRepository` in production, selected via `RINGMASTER_DB_BACKEND` env var
- FastAPI loads the user's most recent conversation history up to `CONTEXT_WINDOW_BUDGET` tokens, dropping oldest exchanges first, and injects it into `RingmasterState.messages`. FastAPI then appends `current_input` as the final entry. 16,000 tokens fits safely within all models' context windows while leaving headroom for system prompt, retrieved context, current input, and response. This value is a config key and straightforward to adjust.
- Each dict in `messages` has the shape `{"role": str, "content": str}` — this matches the Ollama chat API format exactly. The repository's `load` method is responsible for returning this stripped format; `HistoryEntry` retains all storage fields internally.
- At the end of every invocation, FastAPI writes the new exchange back to the repository
- Each user's conversation history is fully isolated by `user_id`
- Retention period configured per `RETENTION_DAYS` — enforced by the daily ofelia maintenance job
- **Message editing** — to edit a past message, the client deletes all history entries at or after the selected message and resends the replacement through the normal WebSocket flow. Discarded history is not recoverable. The repository exposes a `delete_from(user_id, entry_id)` method for this purpose.

## History Repository Interface

```python
def load(self, user_id: str) -> list[dict]:
    # Returns the user's recent history as [{"role": ..., "content": ...}, ...]
    # Bounded by CONTEXT_WINDOW_BUDGET tokens — oldest exchanges dropped first
    # Reads CONTEXT_WINDOW_BUDGET from config internally

def save(self, entry: HistoryEntry) -> None:
    # Appends a single exchange turn to the history for this user

def delete_from(self, user_id: str, entry_id: int) -> None:
    # Deletes all entries for this user with id >= entry_id
    # Used for message editing — truncates history at the edit point
```

## `HistoryEntry` Dataclass

| Field | Type | Notes |
|---|---|---|
| `id` | integer | Primary key |
| `user_id` | string | Foreign key → users table |
| `role` | string | `user` \| `assistant` |
| `content` | string | Message text |
| `created_at` | datetime | Used for ordering and retention enforcement |

## Long-Term Memory (ChromaDB)

**Collection naming convention — used everywhere, no exceptions:**

| Collection | Contents |
|---|---|
| `memory_{user_id}` | Personal long-term memory for this user |
| `skills_{user_id}` | Personal skills for this user |
| `memory_shared` | Shared family memory |
| `skills_shared` | Shared family skills |

**Personal memory** — per user, fully private:
- ChromaDB collection: `memory_{user_id}`
- Source of truth: user's personal vault
- Embedding model: `nomic-embed-text` via Ollama

**Shared family memory** — readable by all users, written by the Ringmaster via classifier:
- ChromaDB collection: `memory_shared`
- Source of truth: shared family vault
- Contains: household info, family notes, shared knowledge

**Dual-scope retrieval** — `tools/memory.py` `retrieve_context()` always queries both `memory_{user_id}` and `memory_shared` in parallel. Results are merged and deduplicated by chunk ID, then ranked top-k by score. Every user automatically gets personal and shared context in every retrieval — no caller configuration required.

## Memory Persistence — `memory/persist.py`

After the graph completes at RESPONDER and FastAPI has sent the `done` frame, FastAPI fires `memory/persist.py` as an asyncio background task. This fires after every exchange that completes with a `done` frame — it is never scheduled when the global exception handler sends an `error` frame instead. The evaluator inside `memory/persist.py` decides whether anything in the exchange is worth writing to long-term memory; it may extract zero units and exit cleanly.

`memory/persist.py` evaluates the completed exchange (`current_input` + `assembled_response`) and extracts a list of discrete memory units — each unit is a single fact, moment, or piece of knowledge worth preserving. One exchange can produce zero, one, or many units. Each unit is processed independently end to end:

1. The classifier assigns each unit to personal or shared scope.
2. The unit is written as a raw dated markdown file to `capture/` under the appropriate vault path — minimal formatting, just the captured content and a timestamp.
3. The unit is immediately ingested into the corresponding ChromaDB collection (`memory_{user_id}` or `memory_shared`) via `memory/ingest.py` — queryable right away, before dream mode runs.

The vault is the source of truth — users can read, edit, or delete any vault file in Obsidian. ChromaDB is always derived from the vault.

It uses `tools/llm.py` for inference calls, `tools/vault.py` for the vault write, and `memory/chroma.py` for the ChromaDB write.

**Improvement log:** writes a `persist_decision` event for every exchange evaluated — both "persist" and "skip" verdicts. See `spec/improvement.md`.

**Failure handling** — retries apply per inference step, per unit, independently:
- If the evaluator fails after one retry, the task exits and no units are persisted for this exchange.
- If the classifier fails for a specific unit after one retry, that unit defaults to personal scope rather than being dropped — it is safer to persist to the wrong scope than to lose the memory. Other units in the same exchange are unaffected.
- All failures are logged at `ERROR` level with no per-failure admin notification. Repeated failures accumulate in the error log and trigger the daily maintenance job threshold notification.

On ChromaDB unavailable: logs the failure and calls `notify_admin("chromadb_unavailable", ...)`.

**Future work:**
- **Memory pruning** — a scheduled maintenance pass (added to `maintenance/cleanup.py`) that reviews long-term memory and removes stale or superseded entries.
- **Dream mode** — a consolidation pass that runs on a schedule (e.g. nightly). Reads raw captures from `capture/`, consolidates them into well-structured human-readable markdown files in the appropriate vault folders (`episodic/`, `semantic/`, `self/`, etc.) with clear headings and sections — organised by topic, not a flat list of facts. Clears the capture folder once consolidated. Then triggers a full re-ingest of the affected ChromaDB collections so the clean structured versions replace the raw captures. The result is a vault you can open in Obsidian and actually read — a living picture of what the Ringmaster knows about you.

## Personal → Shared Classification

The classifier runs per memory unit, not per exchange. A single exchange can produce units that go to different scopes.

```
Personal → private thoughts, preferences, work, personal matters
Shared   → household info, shared plans, family events, shared resources
Ambiguous → personal (default to private, always)
```

The user can always override explicitly: "add this to the family brain" or "keep this private."

## `db/schema.py`

`db/schema.py` exposes a single `create_tables()` function. It opens connections to all three SQLite files and creates the appropriate tables in each:

- `auth.db` — `users`, `refresh_tokens`, `invites`
- `tasks.db` — `tasks`
- `history.db` — `history`

`create_tables()` is called from FastAPI's lifespan context manager on startup, conditional on `DB_BACKEND == "sqlite"`. It is never called at module import time. Using `CREATE TABLE IF NOT EXISTS` means it is safe to call on every startup — it will not alter or overwrite existing tables. When the schema changes during development, delete the relevant `.db` file and let startup recreate it: delete `~/.ringmaster/auth.db` for auth tables, `~/.ringmaster/tasks.db` for tasks, `~/.ringmaster/history.db` for history. Startup will recreate the file and all tables in it. Proper migrations (Alembic) are introduced in Phase 6.

## Vault File Watcher

persist.py keeps ChromaDB in sync for writes the Ringmaster makes. But users may also edit or delete vault files directly in Obsidian — ChromaDB won't know about those changes until something triggers a re-ingest.

A background file watcher handles this. Using `watchfiles` (already a dependency via uvicorn), FastAPI starts a watcher on `{vault_base}` at startup that monitors for `.md` file changes. On change or deletion, it triggers a targeted re-ingest of the affected file into the appropriate ChromaDB collection. This runs as an asyncio background task — it does not block the server.

The watcher starts only when a vault path exists — graceful no-op on nomadbaker where no vault is present. It is a permanent feature of the system, running on pearlybaker and the home server alike.

## Vault Structure

The vault is organised around cognitive memory types — each folder answers a different question, which eliminates ambiguity about where any given memory unit belongs.

**Ringmaster behavior config (assistant name, tone, technicality level) is not in the vault.** It lives in the `users` DB table and is served via `GET /PATCH /profile` endpoints. The vault holds knowledge about the user as a person, not how the Ringmaster is configured.

Per-user vault (`{memory.vault_base}/{user_id}/` — on pearlybaker this resolves via the `~/ringmaster-brain` symlink to `/mnt/hdd/ringmaster/<username>/`):
```
<username>/
├── capture/       # Raw staging — captures from persist.py, cleared by dream mode
├── episodic/      # What happened — time-indexed experiences, notable conversations, moments
├── semantic/      # What I know — timeless facts about people, places, concepts
├── procedural/    # How to do things — skills
│   ├── approved/  # Live skills the assistant can use
│   ├── pending/   # Candidate skills awaiting review
│   └── retired/   # Old skills kept for reference
└── self/          # Who I am — values, preferences, personal history, identity
```

**Note:** Tasks and calendar are not in the vault — tasks live in the tasks DB, calendar via `tools/calendar.py` (see below). The vault holds unstructured knowledge and memory only.

Shared vault (`{memory.vault_base}/shared/` — on pearlybaker: `/mnt/hdd/ringmaster/shared/`):
```
shared/
├── capture/       # Raw shared captures — cleared by dream mode
├── episodic/      # Shared family moments and events
├── semantic/      # Household facts, shared contacts, family knowledge
└── procedural/    # Shared skills
    ├── approved/
    └── pending/
```

**Note:** Shared tasks are not in the vault. Shared calendar is iCloud CalDAV — see below.

---

## Calendar — `tools/calendar.py`

Calendar is not stored in the vault. It is structured, time-indexed, transactional data — the wrong fit for markdown and ChromaDB vector search. `tools/calendar.py` is the single interface all nodes use to read and write calendar data.

**Backend:** iCloud CalDAV via the `caldav` Python library. iCloud exposes a CalDAV endpoint at `https://caldav.icloud.com`. Apple does not allow third-party apps to use the main Apple ID password — each Ringmaster deployment uses an **app-specific password** generated in Apple ID settings.

**Why CalDAV:** it is an open standard. The same `tools/calendar.py` interface could swap its backend for Google CalDAV or any other CalDAV provider without changing any node code.

**Credentials** — stored as env vars, never in the vault or DB:

| Env var | Contents |
|---|---|
| `RINGMASTER_CALDAV_USER_<user_id>` | Apple ID email for this user |
| `RINGMASTER_CALDAV_PASSWORD_<user_id>` | App-specific password for this user |
| `RINGMASTER_CALDAV_SHARED_USER` | Apple ID with access to the shared family calendar |
| `RINGMASTER_CALDAV_SHARED_PASSWORD` | App-specific password for the shared calendar account |

**Config keys:**

| Key | Default | Notes |
|---|---|---|
| `calendar.backend` | `caldav` | `caldav` \| `mock` |
| `calendar.caldav_url` | `https://caldav.icloud.com` | Override for non-iCloud CalDAV |

**`tools/calendar.py` interface:**

```python
def get_events(user_id: str, start: datetime, end: datetime, shared: bool = False) -> list[CalendarEvent]
def create_event(user_id: str, event: CalendarEvent, shared: bool = False) -> CalendarEvent
def update_event(user_id: str, event_id: str, updates: dict, shared: bool = False) -> CalendarEvent
def delete_event(user_id: str, event_id: str, shared: bool = False) -> None
```

`shared=True` routes to the shared family calendar using the shared credentials. `shared=False` routes to the user's personal calendar.

**Mock mode** (`calendar.backend: mock`): returns empty results or fixture data, no network calls. Graceful no-op on nomadbaker where no iCloud credentials are present.

---

# Skills System

Skills are procedural memory — not facts but *how to do things*. Two scopes exist: personal (one user's preferences) and shared (consistent behaviour across all users).

| Type | Scope | Approval |
|---|---|---|
| Personal skill | One user only | That user approves |
| Shared skill | All users | Admin approves |

**Skill format:**
```markdown
# Skill: Debug Python Traceback
tags: [coding, python, debugging]
scope: personal                    # or: shared
version: 1.2
approved: true
approved_by: clarkehines
approved_date: 2026-04-09

## Trigger
When asked to debug a Python error or traceback.

## Process
1. Extract the exception type and message
2. Identify the deepest frame in user code (skip stdlib)
3. Check ChromaDB for similar past errors in this project
4. Hypothesise root cause before suggesting fix
5. Propose fix with explanation, not just code

## Notes
- Always ask to see surrounding context if traceback is partial
```

**Skills paths:**
- Personal skills: derived at runtime — `{memory.vault_base}/{user_id}/procedural/approved/`
- Shared skills: explicit config key — `skills.shared_approved_path` (resolves to `{memory.vault_base}/shared/procedural/approved/`)

**Approval flow:**
```
Assistant proposes skill
        │
        ▼
  procedural/pending/     ← user reviews (Obsidian or future web UI)
        │
   approved (shared: Admin only)
        │
        ▼
  procedural/approved/    ← assistant can now use it
        │
        ▼
  ChromaDB ingestion      ← immediately queryable
```

ROUTER checks `skills_{user_id}` then `skills_shared` before every action. Personal skills take precedence over shared ones when there is a conflict. Both checks are graceful no-ops if the collection does not exist yet.
