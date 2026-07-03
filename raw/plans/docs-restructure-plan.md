# Docs Restructure — The Plan

Step 2 deliverable of `docs-restructure-handoff.md`. Grounded in four research
reports (see `docs-restructure-progress.md` for the condensed findings and what was
verified against primary sources). Nothing below is presented as industry convention
unless it verified as one; deliberate choices of ours are marked *(our choice)*.

Status: awaiting Clarke's approval. Execute nothing until approved.

---

## Part 1 — The design

### Principles (evidence-backed)

1. **Vision separate from roadmap, timeless, operational.** Verified: projects whose
   vision survived (helix `docs/vision.md`, TigerBeetle TIGER_STYLE, cilium hub
   `VISION.md`) keep it out of the roadmap and free of dates; sigstore folded vision
   into a time-boxed roadmap and it went stale with it; Redwood/Deno welded manifestos
   to marketing surfaces and lost them in pivots. Mojo is unusually vision-heavy, so
   vision gets three layers: README pitch → `vision.md` (timeless picture + concrete
   goals, helix-style) → `philosophy.md` (the principles that govern decisions,
   TigerBeetle-style — philosophy survives when it does daily work).
2. **Root README = short pitch, status, pointers.** Every strong example does this.
3. **AGENTS.md = map, not manual.** Only what an agent can't infer: identity, current
   phase, where things live, non-obvious conventions and hard constraints. Pointers
   over copies; no sibling-repo lists (stale-list failure mode) — one backlink to the
   hub. `CLAUDE.md` = one line, `@AGENTS.md` (Anthropic's documented pattern). Grows
   by the Cherny method: session does something wrong → one line added.
4. **`docs/README.md` = annotated map of the docs tree.** GitHub renders any
   directory's README as its landing page (verified); kubernetes/dotnet use this as
   the wayfinding mechanism. Replaces the invented "index.md at 5+ files" rule.
5. **One fact, one place.** The strongest convergent finding across the AI-tooling
   research: duplication drifts, and agents confidently consume stale copies. Every
   fact gets a canonical home; everything else points at it.
6. **Few files, each substantial; no empty scaffolding.** Diátaxis's own rule, and
   what the pre-code repos that aged well (Oxide, TigerBeetle) actually did.
   Directories are added when content earns them, never ahead of it.
7. **Phase-scoped docs.** Active docs are Phase 1 by default. Later-phase material
   lives in `phase-2/` / `phase-3/` dirs in the hub — the directory-as-status pattern
   (dotnet designs), keyed by phase *(our choice)*. Everything parked gets one line
   on *why* it was parked (TC39 habit).
8. **ADRs: MADR 4.x.** Numbered, never deleted, superseded only (Nygard). Frontmatter
   `status` / `date` / `decision-makers` (current MADR field name). No draft status.
   Org-level decisions → hub; component decisions → that component.
9. **Devlog: one append-only file, dated headings, newest-first, zero ceremony**
   *(our choice — validated small-project practice, not a big-project convention;
   newest-first because the reader is checking "what's happening lately")*.
10. **ideas.md: flat list, periodic triage** (GTD Someday/Maybe — verified).
11. **Format rules:** standard markdown links only (wikilinks don't render in repo
    files — verified); no frontmatter outside ADRs (matches rust-lang/rfcs,
    golang/proposal practice); prose for reasoning, bullets for enumerations.
12. **Hub owns the org picture** (vision, philosophy, org roadmap, org ADRs, repo
    manifest, conventions); **components own their own content** and backlink to the
    hub — the kubernetes/cilium split. `.github` repo stays plumbing: profile README
    (condensed pitch + link to hub vision, so a pivot can't kill the canonical copy)
    and community health defaults. No `~/projects/mojo-labs/` dir (decided): any
    parent-level CLAUDE.md stays org-agnostic.

### Target structure — mojo (hub)

```
README.md          pitch, repo manifest (name/role/phase — THE canonical repo map),
                   current stage, license
AGENTS.md          orientation + conventions pointer   CLAUDE.md → @AGENTS.md
docs/
  README.md        annotated map of everything below
  vision.md        the future picture + concrete goals; timeless, no dates
  philosophy.md    craft problem, sovereignty axioms, symbiotic AI, proof-not-promise
  roadmap.md       the three phases, status per phase, revised in place
  conventions.md   the rules in Part 1, condensed — governs all org repos
  decisions/       org-level ADRs (MADR 4.x)
  devlog.md        append-only, newest-first
  ideas.md         flat, triaged
  research/        primary sources & maintained research that survives triage
                   (philosophy-raw, vision-raw extracts; academic-field; competitors)
  phase-2/         parked fleet-era material, README explains what/why
  phase-3/         parked collective/framework material incl. historical deliverables
```

### Target structure — components

Every component repo: `README.md` (what/status/backlink) + `AGENTS.md`/`CLAUDE.md` +
`docs/` with only what's earned. Specifics:

- **mojo-agent:** `docs/roadmap.md`, `docs/decisions/`, `docs/prior-art/` (ringmaster
  *agent internals only*: node spec, WS frame protocol, local memory/vault model,
  testing discipline, capability ideas — clearly labeled as inherited reference, not
  decisions; ringmaster's values, fleet-era, and mojo-language material go to the
  hub per the disposition map), `devlog.md`, `ideas.md`.
- **mojos:** `docs/nixos-plan.md` (the missing source-of-truth doc — new design work,
  see chunk 5), `docs/roadmap.md`, `docs/decisions/` (first ADR: NixOS over
  Arch+Ansible+Btrfs), `devlog.md`, `ideas.md`. Fleet/circus-era content parks in
  hub `phase-2/`.
- **dotfiles:** light touch — already the healthiest repo. Commit pending changes,
  README/AGENTS tune-up, hub backlink.
- **herald:** becomes a real repo under the org (git init, push, license). Docs
  already conform; fix dead links pointing at it.
- **ringmaster:** untouched. Already correctly archived; its thinking is distributed
  from the copies in `raw/`.

## Part 2 — raw/ disposition map

Category-level; file-level calls happen inside each chunk, and anything ambiguous
gets asked, not guessed.

| raw/ content | Destination |
|---|---|
| vision/, craft-problem, philosophy-raw, vision-raw, stance | Distilled into hub `vision.md` + `philosophy.md`; raw extracts preserved in hub `docs/research/`; stance/brief/presentation → `phase-3/` (historical) |
| framework/ (14 files), constitution, domains, KO design, blockchain research | Hub `phase-3/` with parking notes |
| architecture/ringmaster/ north-star values & Code of Ethics | Source material for hub `philosophy.md` (org values, not agent internals) |
| architecture/ringmaster/ agent internals: ai.md node spec, api.md WS frame protocol, local memory/vault model, testing.md discipline, improvement.md, capability ideas (skills, onboarding, local clients, dream mode) | mojo-agent `docs/prior-art/` |
| architecture/ringmaster/ multi-user & fleet era: auth.md (JWT/invites/tiers), tier model, server-memory/sync, server.md hardware builds, Tailscale ACLs, thin-client routing | Hub `phase-2/` — becomes relevant again at fleet stage |
| architecture/ringmaster/ideas/mojo-language.md + coding-team subgraph thinking | Hub `phase-3/` — formal-spec/AI-org thread; mojo-language.md is the historic pivot document |
| architecture/ringmaster/ structure.md, phases.md, stale machine rosters | Dropped (git history keeps them) |
| architecture/mojos/ + ideas/ | Concepts that survive NixOS (.mojo files, single-config-source, hardware tiers) → feed `nixos-plan.md`; Arch-specific mechanics → dropped; multi-machine → hub `phase-2/` |
| decisions/ (old ADR set) | Historical record — superseded set stays only in git history; new ADR sets written fresh where decisions are still true |
| research/competitors | Hub `docs/research/` (living doc — carries a last-updated line) |
| research/academic-field | Hub `phase-3/` — it argues the collective-intelligence positioning, which is Phase 3's case, not current work |
| claude-code-* research/plans, global-docs-skill handoff, docs-maintenance-automation | Not mojo content — relocate to `~/.claude/` (Clarke's global tooling project), out of the repo |
| org-setup, kanban, dev-setup, current.md snapshots, foundation.md, meta-project-setup | Dropped — superseded session-tracking artifacts (git history keeps them) |
| docs-convention proposal + verification brief | Superseded by hub `conventions.md` — dropped after it exists |
| deliverables/ html files | Dropped (generated artifacts; .md versions parked in phase-3) |
| restructure handoff/progress/plan files | Dropped in the final chunk, once done |

## Part 3 — Execution chunks

Each chunk is one session, hand back one at a time. Order matters: hub first
(everything backlinks to it), components second, public face after the story is
consistent, verification last.

1. **Hub: vision + philosophy.** Write `docs/vision.md` and `docs/philosophy.md`,
   distilling org profile README, stance.md, vision/ raws, craft-problem,
   philosophy-raw/vision-raw. Move preserved extracts to `docs/research/`. The
   highest-judgment chunk — Clarke reviews the two docs before anything builds on
   them.
2. **Hub: roadmap, conventions, README, AGENTS.md.** Write `roadmap.md`,
   `conventions.md`, new root README (with the canonical repo manifest), rewritten
   AGENTS.md, `docs/README.md` map. Seed `devlog.md` + `ideas.md` (ideas harvested
   from kanban backlog + ideas raws).
3. **Hub: decisions + phase parking.** Starter org ADRs (polyrepo; one-agent model
   superseding the Ringmaster split; license split; this docs structure; herald
   joins org). Create `phase-2/` and `phase-3/` with READMEs and move all parked
   material in, one parking line each.
4. **Hub: final raw/ triage.** Relocate the global-tooling material to `~/.claude/`,
   move research keeps, execute the drops, delete `raw/`. Hub is done.
5. **mojos.** README rewrite (NixOS story), resolve uncommitted state, `roadmap.md`
   - NixOS ADR, devlog/ideas. **Honest flag:** `nixos-plan.md` is new design work,
   not migration — no NixOS technical plan exists anywhere yet, only the direction
   (generations staging, flakes). This chunk needs Clarke's input mid-session on the
   actual NixOS mechanics; the Arch-era material contributes the concepts that
   survive (.mojo files, single config source, hardware tiers) and the requirements,
   not the plan itself.
6. **mojo-agent.** README, commit AGENTS/CLAUDE, `roadmap.md`, first ADRs,
   `prior-art/` from ringmaster agent-internals (per the disposition map — values,
   fleet-era, and mojo-language material go to the hub instead), devlog/ideas.
7. **herald + dotfiles.** herald: git init, license, `gh repo create` under org,
   push, link fixes. dotfiles: review + commit pending changes, README/AGENTS
   tune-up, backlink.
8. **.github org profile.** Update profile README: NixOS, herald row, condensed
   pitch aligned with (and linking to) hub `vision.md`; sanity-check health files.
9. **Verification pass.** Cross-repo link check; manifest accuracy; fresh-session
   stranger test against each repo ("what is this, what phase, what next?"); drop
   the restructure planning files. Done per the handoff's done-when.

## Part 4 — Defaults I'll apply unless vetoed

- **herald license: AGPL-3.0** (matches the org's code default).
- **Global Claude-tooling material moves to `~/.claude/`** rather than staying
  anywhere in the mojo repo.
- **Old ADR sets are not migrated** — new, small, true-today ADR sets are written
  fresh; superseded history lives in git, not in the decisions dirs.
- **Hub keeps no `architecture/` dir** — "how the pieces fit" lives as a section of
  `vision.md` until it outgrows it (few files, each substantial).
