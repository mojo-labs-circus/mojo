# mojo

Planning and documentation repo for the Mojo project. No runnable code — this is where
the framework gets designed and architecture gets specced before dev begins.

Full project context: `docs/vision/`, `docs/framework/framework.md`

---

## Current Stage

Framework design. Dev is paused. Ten planning sessions need to run before dev resumes.
Session briefs are in `docs/framework/`, completed output accumulates in `docs/framework/framework.md`.

Read `docs/framework/00-session-guide.md` before opening any session brief. Sessions run
sequentially — do not start N+1 until N is signed off in the session status table.

---

## Directory Map

```
docs/
├── vision/         ← product vision, pain points, aesthetic
├── framework/      ← session briefs (01-08) + framework.md output
├── architecture/   ← component architecture (temporary — moves to component repos post-framework)
├── decisions/      ← ADRs (MADR format)
├── collective/     ← governance docs
├── research/       ← exploratory ideas, competitor analysis
└── reference/      ← naming conventions, coding standards
```

---

## Conventions

- Architecture decisions → ADRs in `docs/decisions/` (MADR format)
- Open architecture questions → relevant `docs/` file, not code comments
- Docs in `docs/architecture/` are temporary — they move to each component repo post-framework
- Writing conventions → `docs/reference/conventions.md`
