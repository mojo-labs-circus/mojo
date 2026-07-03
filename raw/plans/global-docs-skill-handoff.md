# Handoff — Building the Global Docs Skill

## Why this exists

Software developers are probably the most efficient knowledge workers in history —
decades spent studying and building tooling around how work itself gets organised:
development lifecycle models (Agile, Waterfall, Extreme Programming), and the practices
and tools that sit inside them (Kanban boards, ADRs, trunk-based development, and the
rest). None of that is software-specific in principle. It generalises to any kind of
work. The reason it never spread was never that the ideas don't transfer — it's that
using them well has always required real knowledge and learning most people never had
reason to acquire.

This project is about closing that gap for one slice of it: documentation. Getting the
docs part of "how effective developers work" down correctly, and making it work by
default for any project — not through willpower or memorised convention, but because
the tooling makes the right thing the easy thing.

This is global-level work. It lives in `~/.claude/`, not inside any specific project's
repo, and it should not encode assumptions about any particular project. It needs to
work correctly for any codebase this account touches, present or future.

## What's already settled (research done, verified)

A full documentation-convention spec was designed and independently fact-checked
against real, named, primary-sourced professional and open-source practice — not
invented. Full detail lives at
`~/projects/mojo/raw/plans/docs-convention-proposal.md` (read it before building —
this summary is compressed and the real file has the sourcing and caveats). In brief:

- **Repo root:** `README.md` (human landing page) always. `AGENTS.md` as the real,
  cross-tool-recognised (Cursor, Codex, Copilot, Gemini CLI, 60k+ repos) canonical
  agent-instructions file — commands and hard constraints only, nothing inferable from
  the code itself. `CLAUDE.md` (and any other tool-specific entry file) is a single
  line, `@AGENTS.md`, importing it — Claude Code is the one exception that needs this,
  since it doesn't natively read `AGENTS.md`.
- **`docs/roadmap.md`** — vision stated up front, concrete phased plan follows, revised
  in place. No separate vision file — checked against real examples, standalone vision
  docs aren't an actual convention anywhere.
- **`docs/devlog.md`** — append-only, dated, chronological (modelled on Carmack's
  `.plan` files). Never needs triage to stay useful.
- **`docs/ideas.md`** — flat list of unfleshed ideas (modelled on GTD's Someday/Maybe
  list). Needs periodic triage — each line promoted or deleted.
- **`docs/decisions/`** — ADRs, numbered, MADR-format frontmatter (`status`: `proposed`
  → `accepted` → `superseded by ADR-xxxx`, `date`, `decision-makers`). Never deleted,
  only superseded.
- **`docs/index.md`** — pure navigation, one line per file. Added only once `docs/`
  holds enough files that scanning stops being enough alone (~5+).
- **Diátaxis folders** (`reference/`, `how-to/`, `tutorials/`, `architecture/` in place
  of "explanation") — never pre-scaffolded empty. Diátaxis's own authors are explicit
  that doing so is wrong. Added one file at a time as content and maturity earn them.
- **Maturity gating**, loosely modelled on CNCF's staged documentation criteria:
  pre-release needs only what already has content; active development grows
  `decisions/`, `reference/`, `architecture/` naturally; once something is shipped and
  has or expects outside users (including "working for me and about to post it
  somewhere"), `how-to/`/`tutorials/` start earning their place.
- **Format rules:** no invented frontmatter beyond the ADR exception; standard
  `[text](path.md)` links only, no wikilinks; prose for reasoning, bullets for genuine
  enumerations only.

## What this session needs to build

A global skill (and any supporting global rule) that scaffolds and/or maintains this
convention for any project, dynamically — not a fixed template that always creates the
same folders regardless of what the project actually is or needs.

Open questions this session needs to resolve, not just implement blindly:

- How does the skill determine a project's maturity stage — does it ask, or infer from
  signals in the repo (presence of a build/package file, git tags, a `LICENSE`, commit
  history depth)? Either is defensible; pick one deliberately and be able to say why.
- Is it safe to run on an existing project with existing docs, or only on a fresh one?
  If existing, it needs to not clobber or duplicate what's already there.
- Does it just create empty scaffolding for the "always" files (`README.md`,
  `AGENTS.md`, `CLAUDE.md`, `docs/roadmap.md`, `devlog.md`, `ideas.md`, `decisions/`),
  or does it also populate initial content (e.g., asking a few questions to seed
  `roadmap.md`)? The convention itself says never pre-scaffold Diátaxis folders empty —
  make sure the skill actually respects that rather than defaulting to "create
  everything."

## What is explicitly out of scope for this session

- Anything project-specific. This is global tooling — it should have no knowledge of
  any particular repo, org, or polyrepo structure. (Some later-stage project may adapt
  this convention with its own cross-repo patterns — that happens in that project's own
  repo, not here.)
- Keeping docs *up to date* once they exist (hooks, staleness reminders, drift
  detection). That's a separate, later effort, deliberately sequenced after this skill
  exists and has been tested against at least one real project — see
  `~/projects/mojo/raw/plans/docs-maintenance-automation.md` for that thinking, not to
  be built yet.

## Done when

The skill can be invoked against a fresh, empty project and produces a correct,
minimal scaffold matching the convention above — and against an existing project
without destroying anything already there.
