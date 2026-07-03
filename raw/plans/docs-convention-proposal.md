# Docs Convention Proposal — Draft for Verification

## Purpose and scope

A project-agnostic, tool-agnostic convention for how any software project lays out its
documentation — destined for `~/.claude/` (a global rule plus a skill that scaffolds it
into a repo). Not designed around mojo specifically. Mojo's own polyrepo structure
(hub manifest, cross-repo breadcrumbs) adapts this convention afterward, as a separate
step — it is not part of the convention itself.

This is a first instance of a broader pattern mojo is going for: giving people and
their agents access to the tools and conventions that make software developers
effective, at every phase — not just once "collectives" (Phase 3) exists. MojOS making
it easy to scaffold a well-conventioned project is a Phase 1 capability, useful to a
single developer working alone, before it's ever useful to a group.

**Status:** Verified. An independent session checked every claim below against real,
named, sourced conventions (see `docs-convention-verification-brief.md`). All items
confirmed as real practice; several field-level corrections from that pass are applied
inline below.

---

## Repo root

- **`README.md`** — human landing page: pitch, quickstart. Always present. This is the
  file recruiters, contributors, and casual visitors actually read — not `AGENTS.md`.
- **`AGENTS.md`** — canonical, tool-agnostic operational instructions: build/test/lint
  commands, non-obvious conventions, hard constraints. Only what can't be inferred from
  the code itself — no hand-maintained directory trees, no restating what a linter
  already enforces, no generic advice. **Verified:** real open standard (agents.md,
  stewarded by the Agentic AI Foundation / Linux Foundation), natively recognised by
  Cursor, OpenAI Codex, GitHub Copilot's coding agent, and Google's Gemini CLI — 60k+
  repos already have one. Claude Code is the exception: it does not auto-read
  `AGENTS.md` (Anthropic's own docs: "Claude Code reads CLAUDE.md, not AGENTS.md") —
  the import below is required, not automatic.
- **`CLAUDE.md`** (and any other tool-specific entry file a given tool requires) — a
  single line, `@AGENTS.md`, importing the real content. Thin adapter, not a duplicate.
  This exact pattern is now Anthropic's own documented recommendation
  (code.claude.com/docs/en/memory), though it started as a community workaround.
- **Community health files** (`LICENSE`, `CONTRIBUTING.md`) — added once the project is
  past the earliest maturity stage (see below), not forced from day one.

## `docs/`

- **`docs/roadmap.md`** — vision/mission stated early in the doc, followed by the
  concrete phased plan. Revised in place as direction evolves; never "finished."
  Replaces a standalone `vision.md` — re-checked directly against real examples
  (sigstore/community, kubewarden/community, both confirmed to fold Mission/Vision
  sections into ROADMAP.md ahead of the execution plan) and confirmed: a standalone
  vision-only document isn't an actual convention anywhere; ambient direction without
  concrete milestones attached tends to go stale and unfalsifiable. Caveat: this isn't a
  GitHub-blessed standard — GitHub's own official "community health files" list doesn't
  include ROADMAP.md at all, so treat it as widespread informal practice, not a platform
  requirement. Mojo's own existing `docs/roadmap.md` already happens to follow this
  shape (vision framing, then phase-by-phase status) — independent corroboration from a
  file that predates this design discussion.
- **`docs/devlog.md`** — append-only, dated, chronological. Where messy thinking,
  agent conversations, and rants live permanently. Modelled on John Carmack's `.plan`
  files. Never needs triage to stay useful — nothing in it is pending. Occasionally
  worth skimming for a buried decision, but that's optional mining, not an obligation.
- **`docs/ideas.md`** — flat list, one line per unfleshed idea, zero ceremony to append.
  Modelled on GTD's Someday/Maybe list (verified against David Allen's own site) and
  the "icebox" pattern — a term popularized specifically by Pivotal Tracker, not generic
  Agile canon the way "backlog" is, though it's since become common informal shorthand
  elsewhere. Needs periodic triage: each line eventually promoted (usually into a draft
  decision) or deleted. Unlike the devlog, this one does rot into a useless pile if
  never reviewed.
- **`docs/decisions/`** — ADRs, MADR-style. One file per decision, numbered, never
  deleted (only superseded, with a link to what replaced it) — the immutability rule
  traces to Michael Nygard's original ADR pattern, not MADR specifically. The one place
  frontmatter is justified: `status` (`proposed` → `accepted` → `superseded by
  ADR-xxxx` — MADR has no `draft` state), `date`, `decision-makers` (MADR's actual
  field name; not `deciders`/`owner`) — not for tool-parsing (nothing parses it) but
  for later grep/script queryability ("show me every decision still `proposed`").
- **`docs/index.md`** — pure navigation, one line per file. Added only once `docs/`
  holds enough files that scanning the directory stops being enough on its own
  (rule of thumb: 5+). Must stay strictly thinner than the docs it points to — if it
  grows into a second copy of the content, it becomes a second source of truth that
  drifts from the first.
- **Diátaxis folders** (`reference/`, `how-to/`, `tutorials/`, `architecture/` — the
  last standing in for "explanation," a clearer label for a systems-heavy project) —
  never pre-scaffolded empty. Diátaxis's own authors are explicit on this
  (diataxis.fr): *"It certainly does not mean that you should create empty structures
  ... with nothing in them. Don't do that. It's horrible."* Added one file at a time,
  as content and project maturity actually earn them.

## Maturity gating

Borrowed in spirit (not literally — CNCF's process assumes a foundation governance
model with formal stage applications, which doesn't apply to a solo/small project) from
CNCF's sandbox/incubating/graduated criteria — verified directly against the CNCF TOC's
actual application templates. Documentation depth does scale with each level (sandbox
light, incubating adds governance/roadmap/architecture docs, graduated adds things like
a documented `RELEASES.md` and security-review docs), though it's woven through each
level's Governance/Engineering/Security/Ecosystem sections rather than existing as one
standalone "documentation criteria" checklist:

- **Pre-release** — README + whatever of `devlog`/`ideas`/`roadmap`/`decisions` already
  has content. Nothing else forced.
- **Active development** — `decisions/`, `reference/`, `architecture/` accumulate
  naturally as real decisions and structure exist to document.
- **Shipped, has or expects outside users** (not necessarily "shipped and used by
  thousands" — includes "working for me and I'm about to post it somewhere people might
  try it") — `how-to/` and `tutorials/` start being worth writing, since there's now
  someone other than the author to write them for.

## Format rules (all markdown, any repo)

- No invented frontmatter schema beyond the ADR exception above and whatever a tool
  itself actually parses (Claude Code only reads `paths:`, inside `.claude/rules/`).
  Status/owner/date elsewhere: first line of body text, not metadata nobody reads.
- Standard `[text](path.md)` links only — no wikilink syntax (`[[page]]`), since it
  doesn't render on GitHub and there's no reason to depend on a specific tool (Obsidian)
  understanding it.
- Prose for reasoning and decisions, bullets for genuine enumerations. A bullet needing
  a sub-bullet to explain itself is a sentence, not a list item.

## Explicitly out of scope here (mojo-specific, decided separately)

- A manifest in mojo's hub repo listing sibling repos' path, role, and phase.
- A one-line breadcrumb in each mojo-org repo's `CLAUDE.md`/`AGENTS.md` pointing back to
  that hub — not a copy of the manifest.

These patterns only make sense for repos that are actually part of a polyrepo org with
a hub. They do not belong in the global convention.
