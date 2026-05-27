# Next Move

## Where We Are

Repo restructure complete. Naming conventions cleaned up across all spec files. Canonical naming reference in place.

| Done | Notes |
|---|---|
| `mojo-labs-circus` org live | All repos in place |
| `circus` repo — spec only | This repo. Clean. |
| `ringmaster` repo | Real code. Full history from old monorepo. |
| Stub repos live | `performer`, `mojos`, `client-tui`, `mojo-sdk`, `client-web` |
| `circus` CLAUDE.md rewritten | Reflects polyrepo structure and spec-only role |
| Naming convention cleanup | All spec files use correct names throughout |
| `spec/naming.md` created | Canonical naming reference for all component sessions |

---

## Next Actions

### 1. Code ownership triage — `ringmaster` repo

The `ringmaster` repo has real code. Some of it may belong elsewhere now that the architecture is a clean polyrepo:

- **What stays in `ringmaster`**: server-side logic, LangGraph graph, FastAPI endpoints, DB, memory
- **What moves to `mojo-sdk`**: WebSocket frame types, API request/response schemas, any shared contracts that performers and clients need
- **What moves to `client-tui`**: any TUI/client code that ended up in the ringmaster codebase

Open a Claude session from `../ringmaster/` and audit for misplaced code. Don't move anything yet — produce a list of what needs to move and where.

### 2. Write `CLAUDE.md` for each component repo

Each repo needs its own dev session context before any implementation work starts. Write one per repo, in this order:

- `../ringmaster/CLAUDE.md` — most urgent, most code
- `../mojo-sdk/CLAUDE.md`
- `../performer/CLAUDE.md`
- `../client-tui/CLAUDE.md`
- `../mojos/CLAUDE.md`
- `../client-web/CLAUDE.md`

Each CLAUDE.md should cover: what the component does, how it fits in the circus, the Mk1 scope, and how to work on it day-to-day. Point to `spec/naming.md` for naming conventions.

### 3. Performer and mojo-agent spec planning session

`performer` is a stub with no spec. Before building anything, spec out what mojo-agent needs to do for Mk1: node structure, routing logic, WebSocket protocol with the Ringmaster, what's in-scope vs deferred. This is a top-level session task (circus repo) — output goes in `spec/mojos/agent.md` and `spec/mojos/topology.md`.

### 4. Plan Mk1 dev phases

Based on the ringmaster audit and the new polyrepo structure, plan the concrete build sequence toward Mk1. What gets built first, in what order. The current `spec/ringmaster/phases.md` has the feature list — what's needed is a sequenced build plan with clear unblocking dependencies.

Key question to answer: does `mojo-sdk` need to exist before performer dev can start, or can performer start with local stubs and sdk extraction happen later?

### 5. Local dev environment

Ensure everything is cloned and ready on both machines before the summer build push. All repos cloned, venvs initialised, dev tooling in place. Goal: sit down on either machine and be able to work on any component without setup friction.
