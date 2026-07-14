# mojo: the thinking repo

Clarke's thinking repo for the Mojo project: vision, philosophy, plans, ideas.
No code lives here. The code repos (`mojos`, `mojo-agent`) reference this repo
for context, never the reverse.

Skim the last couple of [devlog.md](devlog.md) entries first (especially any
HANDOFF one) to pick up where things left off.

**Before working on anything Mojo-related, in any repo:** read
[anatomy.md](anatomy.md) first. It's what a Mojo system concretely is, the map
everything else is written against; it's the union of [pieces.md](pieces.md)
(the piece list, split out so it can move on its own) and [seams.md](seams.md)
(the seam list, found fresh below). Then [vision.md](vision.md) and
[philosophy.md](philosophy.md), which are how you understand Clarke and this
project. [roadmap.md](roadmap.md) has the current focus and its finish line.
[research-plan.md](research-plan.md) and
[standards-research.md](standards-research.md) are research archives (prior
art per seam, and how real standards actually got made, both still true,
neither a tracker); [making-the-standard.md](making-the-standard.md) is the
plan for how the piece/seam map becomes MSI-1;
[seam-method.md](seam-method.md) is the procedure for finding and shaping
seams, [seams.md](seams.md) is where findings actually land. Check seams.md's
status per entry before assuming any seam is decided (most say `existence` or
nothing at all, on purpose). [msi-steps.md](msi-steps.md) is the step-by-step
procedure through the old plan, superseded by making-the-standard.md and
**not what a session works right now**; see Current stage below before
touching its checkboxes. Public-facing docs use plain names only, and no em
dashes; see [naming-conventions.md](naming-conventions.md).

## Grounding in prior art

Mojo standardizes real systems, not its own thinking about them. The same
move POSIX made: it wrote down what real Unix implementations already did,
converged from disagreement among them, not what sounded architecturally
clean on paper. Every piece and every seam gets checked the same way here.
anatomy.md, pieces.md, seams.md, and seam-method.md are Mojo's own reasoning,
a place to start looking, never a source of truth by themselves. A claim only
counts as settled once it's checked against how real systems actually do it:
the five digital-counterpart systems already used for the piece-list check
(OpenClaw, Hermes, Khoj, Letta, OpenFang) for day-to-day shape, and the
deeper protocol precedent archived in research-plan.md (POSIX, seL4, LSP,
Kubernetes' CRI/CNI/CSI and admission/RBAC, OCI, Matrix/XMPP) and
standards-research.md (how those standards were found, written, validated,
published, and grown) once a live protocol shape is at stake. Reasoning
straight from anatomy.md's prose and calling it checked is exactly the
mistake this rule exists to catch, even when the conclusion feels obviously
right. If real prior art doesn't have an answer, say so; that's an open
question, not license to assert one from our own doc instead.

## Current stage

A real systems-development lifecycle (locked 2026-07-08, ordering revised
2026-07-14, see devlog): requirements (vision.md/philosophy.md, done) → the
seam survey and the first system, grown together (current phase) → the MSI-1
draft, extracted from what actually runs → iterate, versioning both the
standard and the system as real use teaches what Mk1 got wrong.

The two questions that suspended the old seam walk (2026-07-13) both have
answers now:

1. **The piece list, checked for completeness.** Settled enough to move on,
   2026-07-13: the 13-node matrix held against a real digital-counterpart
   whole-system check (OpenClaw, Hermes, Khoj, Letta, OpenFang) and a second
   independent read, no missing 14th piece. Provenance stays standalone,
   Identity stays one node, only Identity owns durable state (see that day's
   devlog entries for the full reasoning). Settled means good enough to move
   on, not frozen: the piece list can still move if seam work exposes a real
   hole, the way it did once already for seam l.
2. **How a piece/seam map turns into a real, buildable standard.** Answered
   2026-07-14 against real history rather than assumption:
   [standards-research.md](standards-research.md) is the evidence (POSIX,
   Kubernetes' CRI/CNI/CSI, LSP, OCI, Matrix, XMPP, Linux, IETF/W3C process,
   and the OSI/CORBA failures);
   [making-the-standard.md](making-the-standard.md) is the plan derived from
   it. The headline: no successful standard published its document before
   running code existed, so the old ordering (finish the document, then
   build) is inverted. The existence pass continues with a build-ready
   finish line, the first system gets built against the draft map, MSI-1
   gets extracted from what runs, and "done" for MSI-1 means a second,
   independent implementation built from the text alone, not a finished
   document.

msi-steps.md is superseded by making-the-standard.md: still frozen, pending a
rewrite or retirement to match (deliberately not done in the same session the
plan landed; next housekeeping pass).

Mojo's core architecture is a foundational, cross-cutting layer, so getting
the core abstractions right comes before any code depends on them. It's the
same bet real standards bodies and kernel projects make, with the 2026-07-14
correction that "right" gets proven by a running system and a second
implementation, not by the document alone.

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
