# Current

## What we did

- Cleared the entire repo into `raw/` — clean slate
- Agreed on docs structure: `docs/vision.md`, `docs/roadmap.md`, `docs/architecture.md`,
  `docs/principles.md`, `docs/decisions/`
- Wrote `docs/roadmap.md` — three phases, outcome-focused, non-technical

## Where we are

`mojo/` is partially set up. Structure exists, one doc written. Everything else is in `raw/`.

## What's next (in order)

1. Write `docs/vision.md` — why Mojo exists, long-term picture
2. Write `docs/principles.md` — design philosophy, how decisions get made
3. Write `docs/architecture.md` — how the components fit together at a high level
4. Migrate valid ADRs from `raw/docs/decisions/` into `docs/decisions/` — rewrite the stale ones
5. Update `mojo/CLAUDE.md` to reflect the new structure
6. Create `~/projects/CLAUDE.md` — org-level context every session gets automatically
7. Clean up `mojos/` — Phase 1 focused docs only
8. Clean up `mojo-agent/` — Phase 1 focused docs only

## Key decisions made this session

- Phase 1 only for now. Phases are strictly sequential.
- One agent type (`mojo-agent`) deployed wherever needed — separate Ringmaster hub is
  probably dead. Phase 2 topology (P2P vs coordinator) decided once Phase 1 is solid.
- `raw/` at repo root is the inbox for unprocessed thinking. Move content to proper docs
  as it becomes relevant.
- Docs structure is flat — no deep nesting. Hub repo should be readable in one sitting.
- Framework sessions (Phase 3) are deprioritised. May not happen as currently described.
- GitHub org is `mojo-labs-circus`. This repo stays named `mojo`.
