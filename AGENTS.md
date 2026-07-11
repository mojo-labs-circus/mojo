# mojo: the thinking repo

Clarke's thinking repo for the Mojo project: vision, philosophy, plans, ideas.
No code lives here. The code repos (`mojos`, `mojo-agent`) reference this repo
for context, never the reverse.

Skim the last couple of [devlog.md](devlog.md) entries first (especially any
HANDOFF one) to pick up where things left off.

**Before working on anything Mojo-related, in any repo:** read
[anatomy.md](anatomy.md) first. It's what a Mojo system concretely is, the map
everything else is written against. Then [vision.md](vision.md) and
[philosophy.md](philosophy.md), which are how you understand Clarke and this
project. [roadmap.md](roadmap.md) has the current focus and its finish line.
[research-plan.md](research-plan.md) is the research tracker, re-derived from
the anatomy 2026-07-11 and keyed to its seams. Check its Status column before
assuming any piece of the system is decided (most of it says Open on purpose).
[msi-steps.md](msi-steps.md) is the step-by-step procedure through that plan;
sessions work its first unchecked step. Public-facing docs use plain names
only, and no em dashes; see
[naming-conventions.md](naming-conventions.md).

## Current stage

A real systems-development lifecycle (locked 2026-07-08, see devlog):
requirements (vision.md/philosophy.md, done) → the Mojo System Interface, Mk1
(current phase) → Mk1 system implementation, built against that standard →
iterate, versioning both the standard and the system as real use teaches what
Mk1 got wrong.

Current phase: draft **MSI-1** by walking [msi-steps.md](msi-steps.md) in
order: finish the anatomy piece pass, walk the seams (drafting msi.md as each
lands), then the assembly pass. A first-pass, complete-coverage answer for
every seam, good enough to build against; depth comes from iterating across
versions once something real exists. After MSI-1: reference implementations,
stitched into the first runnable Mojo system.

Mojo's core architecture is a foundational, cross-cutting layer, so getting
the core abstractions right comes before any code depends on them. It's the
same bet real standards bodies and kernel projects make.

## The habit

Any session that produces real thinking captures it here before it ends: a
[devlog.md](devlog.md) entry, one-liners into [ideas.md](ideas.md), or an edit
to vision/philosophy/roadmap. Thoughts that only live in a chat log are lost.

## Conventions

- Written in Clarke's voice, first person, honest. Nothing described as
  existing unless it exists.
- Plain prose, no em dashes, no invented vocabulary in public-facing docs
  (see [naming-conventions.md](naming-conventions.md)).
- Flat structure. A new file only when a topic earns one (three pages of real
  content, not before). No empty scaffolding.
- New wants during the current milestone go to [ideas.md](ideas.md), not the
  roadmap.
- `roadmap.md` is revised in place; `devlog.md` is append-only, newest first,
  one entry per session, not per day. A single date can carry more than one
  entry if more than one session happened on it; don't merge a new session's
  thinking into an existing same-day entry.
- Standard `[text](path.md)` links only, no wikilinks. Prose for reasoning;
  bullets for genuine enumerations only.
- Pre-reset history (the old `raw/` corpus, framework session docs,
  ringmaster-era design) lives in git history, not in the tree. Dig there
  before re-deriving old thinking from scratch.
