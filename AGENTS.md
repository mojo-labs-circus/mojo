# mojo — the thinking repo

Clarke's thinking repo for the Mojo project: vision, philosophy, plans, ideas. No
code lives here. The code repos (`mojos`, `mojo-agent`) reference this repo for
context — never the reverse.

Skim the last couple of [devlog.md](devlog.md) entries first — especially any
HANDOFF one — to pick up where things left off.

**Before working on anything Mojo-related, in any repo:** read
[vision.md](vision.md) and [philosophy.md](philosophy.md) — they are how you
understand Clarke and this project. [roadmap.md](roadmap.md) has the current focus
and its finish line. [research-plan.md](research-plan.md) is the live tracker for
the current phase — check its Status column before assuming any piece of the
system is decided. Most of it says Open on purpose.

## Current stage

A real systems-development lifecycle (locked 2026-07-08, see devlog):
requirements (vision.md/philosophy.md, done) → the Mojo System Interface, Mk1
(current phase) → Mk1 system implementation, built against that standard →
iterate, versioning both the standard and the system as real use teaches what
Mk1 got wrong.

Current phase is Mk1 of the Mojo System Interface: a first-pass, complete-coverage
answer for every piece in research-plan.md's tracker — good enough to build against.
Depth comes from iterating across versions once something real exists.

Mojo's core architecture is a foundational, cross-cutting layer, so getting the
core abstractions right comes before any code depends on them — the same bet
real standards bodies and kernel projects make.

## The habit

Any session that produces real thinking captures it here before it ends: a
[devlog.md](devlog.md) entry, one-liners into [ideas.md](ideas.md), or an edit to
vision/philosophy/roadmap. Thoughts that only live in a chat log are lost.

## Conventions

- Written in Clarke's voice, first person, honest — nothing described as existing
  unless it exists.
- Flat structure. A new file only when a topic earns one (three pages of real
  content, not before). No empty scaffolding.
- New wants during the current milestone go to [ideas.md](ideas.md), not the roadmap.
- `roadmap.md` is revised in place; `devlog.md` is append-only, newest first.
- Standard `[text](path.md)` links only, no wikilinks. Prose for reasoning; bullets
  for genuine enumerations only.
- Pre-reset history (the old `raw/` corpus, framework session docs, ringmaster-era
  design) lives in git history, not in the tree. Dig there before re-deriving old
  thinking from scratch.
