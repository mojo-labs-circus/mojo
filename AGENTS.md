# mojo

Org hub for the mojo-labs project — vision, roadmap, org-level decisions. No runnable
code. Component repos (`mojo-agent`, `mojos`, `dotfiles`, `herald`) hold their own docs
as they develop.

## Current stage

Foundation setup. We just finished settling on a docs convention
(`raw/plans/docs-convention-proposal.md`) and are applying it across every repo in the
org, migrating scattered thinking out of `raw/` into proper homes as it becomes decided
and relevant. Framework design (formal collective-intelligence spec) is deprioritized —
see `docs/roadmap.md` Phase 3. Everything active right now is Phase 1: one machine
working as a proper agentic environment (mojo-agent + MojOS + dotfiles).

## Where things live

- `docs/roadmap.md` — vision + the three-phase plan. Start here.
- `docs/decisions/` — ADRs. Small and current on purpose — see note below.
- `docs/devlog.md` — dated, chronological, append-only. Messy thinking lives here.
- `docs/ideas.md` — flat someday/maybe list, needs periodic triage.
- `docs/architecture/` — conceptual explanation of the system (Circus model, why this
  approach). Background context, not a task list.
- `docs/phase-2/`, `docs/phase-3/` — holding pens for content relevant to later phases,
  so it has a home without cluttering Phase 1 work. Not active until that phase starts.
- `raw/` — the standing inbox for unprocessed thinking, old ADRs, and superseded specs.
  Being triaged by hand; content only leaves once it has a confirmed correct home
  elsewhere. Don't treat anything in `raw/` as current — check `docs/` first.

## Conventions

- ADRs in `docs/decisions/` — MADR format, numbered, never deleted, only superseded.
  This set was deliberately reset this session rather than migrated 1:1 from the old
  numbering in `raw/docs/decisions/` — several of those no longer reflect reality
  (framework-first was reversed, some assumed a Ringmaster/performer split that no
  longer exists as the Phase 1 baseline). The old set stays in `raw/` as historical
  record.
- Standard `[text](path.md)` links only, no wikilinks.
- Prose for reasoning, bullets for genuine enumerations only.
