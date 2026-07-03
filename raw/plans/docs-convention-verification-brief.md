# Verification Brief — Docs Convention Proposal

You're being handed a documentation-convention proposal to independently verify, not
to redesign. This came out of a long design conversation with Claude Code, and the
person who wrote it (Clarke) has already caught it inventing conventions twice —
once proposing an ad-hoc "raw/" folder as if it were professional practice (it isn't;
real teams keep pre-decision brainstorming out of the repo entirely), and once
proposing a standalone `vision.md` (also not real; real projects fold vision into
`ROADMAP.md`'s opening section instead). So: assume plausible-sounding claims below
could still be wrong, and check rather than trust.

**Your job:** go through every convention claimed in the proposal and check it against
real, named, sourced practice — actual standards, actual well-known projects, actual
primary documentation. For each one, report: confirmed / corrected (with what's
actually true instead) / unverifiable (say so plainly, don't paper over it). Do not
propose new structure, don't add scope, don't invent replacements for anything you
can't verify — flag it and move on.

The two claims already flagged as unverified going in:

1. **AGENTS.md as a genuinely cross-tool-recognised standard.** The proposal assumes
   Claude Code, Codex, Cursor, Copilot, and Gemini all recognise `AGENTS.md` as an
   agent-instructions file, with tool-specific files (like `CLAUDE.md`) importing it.
   This has only been corroborated by AI-session synthesis so far, not independently
   checked. Find out how broad the real adoption actually is — is this genuinely a
   cross-tool convention, or mostly one tool's pattern being generalised?
2. Everything else below has been checked at least once already in this conversation,
   but re-verify it independently rather than trusting that prior pass — that's the
   entire point of a second session doing this.

---

## The proposal

### Purpose and scope

A project-agnostic, tool-agnostic convention for how any software project lays out its
documentation. Not shaped around any one specific project — meant to be a general
default usable for any repo, of any maturity, regardless of which AI coding tool is in
use.

### Repo root

- **`README.md`** — human landing page: pitch, quickstart. Always present.
- **`AGENTS.md`** — canonical, tool-agnostic operational instructions: build/test/lint
  commands, non-obvious conventions, hard constraints. Only what can't be inferred from
  the code itself.
- **`CLAUDE.md`** (or whatever entry file a given tool requires) — a single line,
  `@AGENTS.md`, importing the real content rather than duplicating it.
- **Community health files** (`LICENSE`, `CONTRIBUTING.md`) — added once the project
  passes the earliest maturity stage, not forced from day one.

### `docs/`

- **`docs/roadmap.md`** — vision/mission stated in the opening section, followed by
  the concrete phased plan. Revised in place; never "finished." No separate
  `vision.md`.
- **`docs/devlog.md`** — append-only, dated, chronological log of raw thinking and
  agent conversations, modelled on John Carmack's `.plan` files. Never needs triage to
  stay useful. Occasionally worth skimming for a buried decision, but that's optional.
- **`docs/ideas.md`** — flat list, one line per unfleshed idea, modelled on GTD's
  Someday/Maybe list / the Agile icebox. Needs periodic triage — each line eventually
  promoted or deleted, or it rots into a useless pile.
- **`docs/decisions/`** — ADRs, MADR-style. One file per decision, numbered, never
  deleted (only superseded). Frontmatter: `status` (draft → accepted → superseded),
  `date`, `deciders`/`owner`.
- **`docs/index.md`** — pure navigation, one line per file. Added only once `docs/`
  holds enough files that scanning stops being enough alone (rule of thumb: 5+).
- **Diátaxis folders** (`reference/`, `how-to/`, `tutorials/`, and `architecture/`
  standing in for "explanation") — never pre-scaffolded empty. Added one file at a
  time, as content and maturity earn them.

### Maturity gating

Loosely modelled (not literally copied — the source assumes a foundation governance
process that doesn't apply here) on CNCF's sandbox/incubating/graduated documentation
criteria, which scale required doc depth to project maturity:

- **Pre-release** — README + whatever of `devlog`/`ideas`/`roadmap`/`decisions` already
  has content. Nothing else forced.
- **Active development** — `decisions/`, `reference/`, `architecture/` accumulate as
  real decisions and structure exist to document.
- **Shipped, has or expects outside users** — `how-to/` and `tutorials/` start being
  worth writing.

### Format rules (all markdown, any repo)

- No invented frontmatter schema beyond the ADR exception and whatever a tool actually
  parses. Status/owner/date elsewhere: first line of body text, not metadata.
- Standard `[text](path.md)` links only — no wikilink syntax, since it doesn't render
  on GitHub.
- Prose for reasoning and decisions, bullets for genuine enumerations.

---

## What's already been checked once (re-verify, don't just accept)

- MADR format for ADRs (status/date/deciders frontmatter, numbered, superseded-not-deleted).
- Diátaxis (tutorials/how-to/reference/explanation), including the claim that its own
  authors (diataxis.fr) explicitly warn against pre-building empty category structure.
- CNCF's sandbox/incubating/graduated criteria as a real, staged documentation-maturity model.
- John Carmack's `.plan` files as the basis for the devlog pattern.
- GTD's Someday/Maybe list and the Agile "icebox" as the basis for the ideas list.
- ROADMAP.md (opening with vision, then concrete milestones) as the real convention,
  versus a standalone vision document, which isn't.

## Report back as

For each item: **confirmed** (with source) / **corrected** (what's actually true,
with source) / **unverifiable** (say so, don't guess). Flag anything where the
underlying idea is fine but the source given for it is weak (a single company blog,
an uncited claim, etc.).
