# Paredros: the thinking repo

The thinking repo for Paredros: the standard, its philosophy, its plans, its
ideas. No code lives here. Reference implementations, once they exist, get
their own repos under this org.

Skim the last couple of [devlog.md](devlog.md) entries first (especially any
HANDOFF one) to pick up where things left off.

**Before working on anything Paredros-related, in any repo:** read
[anatomy.md](anatomy.md) first. It's what a Paredros system concretely is, the map
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
