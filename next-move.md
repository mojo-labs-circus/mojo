# Next Move: Audit and Dev Planning

## Where We Are

The repo restructure is done. `mojo-labs-circus` org is live with all repos in place.

| Repo | Status | Notes |
|---|---|---|
| `mojo` | Live — spec only | This repo. Clean. |
| `ringmaster` | Live — code + full history | Extracted from `mojos/ringmaster/` |
| `performer` | Live — empty stub | No code existed yet |
| `mojos` | Live — empty stub | No code existed yet |
| `client-tui` | Live — stub files | 2 files pushed as initial commit |
| `mojo-sdk` | Live — empty | Holding until performer + client-tui both need it |
| `client-web` | Live — empty | Holding |
| `dotfiles` | Separate repo — unchanged | |

---

## Next Actions

1. **Full audit of `ringmaster`** — the only repo with real code. Assess current state: what's working, what's stale, what's missing, what needs rewriting. Open a Claude session from the `ringmaster` repo directory.

2. **Full audit of `performer`** — what needs to be built from scratch. What does the performer need to do for Mk1?

3. **Full audit of `mojos`** — same treatment. What does the MojOS installer need for Mk1?

4. **Full audit of `client-tui`** — the 2 stub files are basically nothing. What needs building?

5. **Rewrite `CLAUDE.md` for `mojo`** — current one reflects the old monorepo layout. Needs to describe the new org structure and spec-only role of this repo.

6. **Write a `CLAUDE.md` for each component repo** — each one needs its own dev session context: what the component does, how it fits in, how to work on it.

7. **Plan development phases** — based on audit findings, decide the concrete build steps toward Mk1. What gets built first, in what order, and what the milestones look like.

8. **Create `mojo-sdk`** — once `performer` and `client-tui` both need to call the Ringmaster API, create the shared client lib. Don't wait for duplication to become painful — create it at the start of dev phase planning.
