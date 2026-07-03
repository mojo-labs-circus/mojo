# Docs Restructure Tracker

Working file for executing `raw/plans/docs-restructure-plan.md` (the design and
rationale — read it first) chunk by chunk. One chunk per session. Clarke hands a
chunk to a session by saying "do chunk N"; the session executes from the checklist
below, updates this file, and commits. Chunks with a **review gate** stop there —
Clarke reads the output before the next chunk starts.

Background: `raw/plans/docs-restructure-handoff.md` (the brief),
`docs-restructure-progress.md` (research findings, confirmed decisions).

This file is temporary — deleted in chunk 9 when the restructure is done.

## Working rules (apply in every chunk)

1. **Strict keep bar.** A file survives only if it's doctrine in active use,
   reference that will genuinely be consulted, or parked material future-us will
   actually re-read when its phase starts. "It contains thinking" is not enough —
   that's what the idea harvest is for. Borderline → ask Clarke in-session; never
   silently keep or silently drop.
2. **Idea harvest before any drop.** Before a file dies, any still-live idea in it
   becomes one line in hub `docs/ideas.md` (tagged with its origin, e.g.
   `[ex-framework]`). Hub `ideas.md` is deliberately the fullest file in the org:
   losing documents is fine, losing ideas is not.
3. **Hardened, actionable docs.** Every doc states what's decided, what's open, and
   what's next. No aspirational fluff, no describing things that don't exist as if
   they do — the standard is docs on a proper project.

## Status

| # | Chunk | Status |
|---|---|---|
| 1 | Hub: vision + philosophy | not started |
| 2 | Hub: roadmap, conventions, README, AGENTS | not started |
| 3 | Hub: ADRs + phase parking | not started |
| 4 | Hub: final raw/ triage | not started |
| 5 | mojos | not started |
| 6 | mojo-agent | not started |
| 7 | herald + dotfiles | not started |
| 8 | .github org profile | not started |
| 9 | Verification pass | not started |

---

## Chunk 1 — Hub: vision + philosophy  ⛔ review gate

**Goal:** the two anchor documents everything else builds on. Timeless, no dates, no
milestones (roadmap owns those). Vision = helix model (picture + concrete goals).
Philosophy = TigerBeetle model (principles that govern real decisions).

**Sources (read all before writing):**

- `raw/deliverables/stance.md` — best consolidated vision artifact, primary anchor
- org profile README (`gh api repos/mojo-labs-circus/.github/contents/profile/README.md`)
- `raw/docs/vision/vision.md`, `personal-circus.md`, `why-this-works.md`
- `raw/craft-problem.md` — the craft problem + sensei model (philosophy core)
- `raw/docs/research/philosophy-raw.md`, `vision-raw.md` — Clarke's raw extracts;
  "a proof, not a promise" lives here
- `raw/docs/architecture/ringmaster/north-star.md` — "Our AI. Our data. Our server."
  - the five-principle Code of Ethics (org values, feed philosophy.md)
- `raw/docs/research/undeveloped-thoughts.md` — sovereignty-as-axiom test ("who owns
  this data? unclear = design is wrong"), Unix-recursion test

**Steps:**

- [ ] Write `docs/vision.md`: the future picture (personal sovereign AI, the three
      gaps: capability/sovereignty/context), what we're building (agent + OS +
      dotfiles as one system; Circus as the eventual connective layer), who it's
      for, and a concrete Goals section. Update all terminology to the current
      one-agent model and NixOS.
- [ ] Write `docs/philosophy.md`: the craft problem (AI that develops you, sensei
      model, anti-vibe-coding), sovereignty as axiom + the who-owns-this-data test,
      "a proof not a promise" (architecture over policy), symbiotic AI (pushes back,
      it's-YOUR-life constraint), the Code of Ethics principles, open source as
      logical necessity.
- [ ] `philosophy-raw.md` / `vision-raw.md`: use as distillation sources only, then
      DROP — they're AI-session extracts, not Clarke's verbatim words (Clarke,
      2026-07-03). Anything in them that vision/philosophy doesn't absorb and still
      seems valuable: raise with Clarke in-session, don't silently keep or drop.
- [ ] Create `docs/research/` for the analysis keeps — confirm each with Clarke in
      session: `why-this-works.md` (honest objections doc, update solved/unsolved
      status); `monetisation.md` (exploratory); `pain-points.md` (enterprise
      angle); `aesthetic.md` → hold for chunk 5/7 decision (product look-and-feel —
      candidates: mojos docs or dotfiles).
- [ ] Commit. **Stop — Clarke reviews vision.md + philosophy.md before chunk 2.**

## Chunk 2 — Hub: roadmap, conventions, README, AGENTS

**Goal:** the hub becomes navigable and stranger-ready.

**Sources:** current `AGENTS.md`, `raw/foundation.md` (phase definitions),
org profile roadmap section, `raw/plans/docs-restructure-plan.md` Part 1 (the
conventions to condense), `raw/kanban.md` backlog + `raw/docs/ideas` content for the
ideas harvest.

**Steps:**

- [ ] Write `docs/roadmap.md`: one-paragraph link back to vision, then the three
      phases with current status. Phase 1 detailed (mojo-agent + MojOS + dotfiles,
      what "one machine working" means), Phases 2–3 outline-level with pointers to
      `phase-2/`/`phase-3/`.
- [ ] Write `docs/conventions.md`: condensed rules from plan Part 1 — repo file
      set (README/AGENTS/CLAUDE), docs/README.md maps, MADR 4.x ADR format
      (`status`/`date`/`decision-makers`, superseded-never-deleted), devlog
      (append-only, newest-first), ideas (flat, triaged), link style, prose rules,
      phase scoping, one-fact-one-place, no empty scaffolding. Mark which rules are
      verified convention vs our choice.
- [ ] Rewrite root `README.md`: pitch (3–4 sentences linking vision), the canonical
      repo manifest table (repo / role / phase / status — mojo, mojo-agent, mojos,
      dotfiles, herald, ringmaster-archived), current stage, license. This table is
      THE org repo map; everything else points here.
- [ ] Rewrite `AGENTS.md`: what this repo is, current stage, where things live
      (one line each), conventions pointer, raw/-is-not-current warning until
      chunk 4 empties it. Keep `CLAUDE.md` = `@AGENTS.md`.
- [ ] Write `docs/README.md`: annotated one-line-per-entry map of docs/.
- [ ] Create `docs/devlog.md` (newest-first; first entry: this restructure) and
      `docs/ideas.md` — the org's idea net. Seed from `raw/kanban.md` backlog and
      `raw/docs/research/undeveloped-thoughts.md` (478 lines of tagged idea
      threads — the single richest harvest source; drops after harvest + chunk 1
      has used it as a philosophy source). It keeps growing through chunks 3–6 as
      the harvest-before-drop rule fires; by chunk 9 every live idea in the old
      corpus has a line here.
- [ ] Commit.

## Chunk 3 — Hub: ADRs + phase parking

**Goal:** decision record started fresh; later-phase material curated — not
relocated wholesale. Parking bar (working rule 1): park only what future-us will
actually re-read when the phase starts. Everything else: harvest ideas to
`ideas.md`, then drop.

**Steps:**

- [ ] Write `docs/decisions/` starter set, MADR 4.x, numbered from 0001:
      polyrepo architecture · one-agent model (explicitly supersedes the
      Ringmaster/performer split — context from ringmaster history) · license split
      (AGPL-3.0 code / CC BY-SA 4.0 docs) · docs structure + conventions (this
      restructure, links plan for rationale) · herald joins the org.
      Plus `_template.md`.
- [ ] Create `docs/phase-2/README.md` (what fleet phase is, why parked, contents
      list). **Park:** ringmaster `auth.md` (tested design), `server.md` (hardware
      thinking returns at fleet), server-memory/sync parts of `memory.md`, tier
      model material, mojos-era `topology.md` + `circus.md`. **Harvest → drop:**
      `infrastructure.md`, `thin-client-proxy.md`, `multi-circus.md`,
      `fleet-sync.md`, `distributed.md`, `multi-distro.md`, `naming.md` — each is
      an idea sketch, not a design; one `ideas.md` line each (tagged `[phase-2]`),
      then gone. One parking line per kept file: what, why, what unparks it.
- [ ] Create `docs/phase-3/README.md`. **Park (substantial, re-readable):**
      `framework.md` (the real pre-session notes), `02-agent-model.md`
      (eight-dimension context-cost argument), `07-layers-and-contracts.md`
      (three-layer model + context math — the intellectual core),
      `constitution.md`, `domains.md`, `ko-design.md`,
      `00b-blockchain-research.md` (the data-sovereignty thread),
      `research-ai-paradigms.md` (verifiability thesis), `academic-field.md`,
      `mojo-language.md`, `stance.md` (best consolidated vision artifact,
      historical). **Harvest → drop:** `00-session-guide.md`, briefs
      `01/03/04/05/06/08` (unexecuted session prompts — pull embedded ideas like
      the topology taxonomy into `ideas.md` lines), `collective/…-dev-team.md`
      (stub), `brief.md`/`presentation.md` (renders of stance — stance is the
      superset). Parking README notes the two unresolved tensions (pair ontology;
      blockchain walked back) and the Modular-Mojo naming-collision hazard.
- [ ] Borderline calls surface to Clarke in-session — never silent.
- [ ] Update `docs/README.md` map. Commit.

## Chunk 4 — Hub: final raw/ triage

**Goal:** `raw/` deleted; every remaining file relocated or deliberately dropped.

**Steps:**

- [ ] Move `raw/research/claude-code-findings.md` → `docs/research/` (reference).
      Drop the tooling plan briefs (Clarke, 2026-07-03): `global-docs-skill-handoff.md`,
      `docs-maintenance-automation.md`, `raw/plans/claude-code-setup.md`,
      `raw/research/claude-code-setup.md` (brief superseded by findings). Add one
      `ideas.md` line: "docs maintenance needs a fresh plan once the restructure
      has been lived with — old draft thinking in git history." NO `docs/plans/`
      dir — transient work briefs go at repo root and die when done.
- [ ] Move `raw/docs/research/competitors.md` → `docs/research/` with a
      last-updated line.
- [ ] Drop (git history preserves): `org-setup.md`, `kanban.md`, `dev-setup.md`,
      `foundation.md`, `current.md` (root + raw), `meta-project-setup.md`,
      `docs-convention-proposal.md` + `docs-convention-verification-brief.md`
      (superseded by `docs/conventions.md`), `raw/docs/decisions/` (old ADR set),
      `raw/docs/reference/conventions.md` (spec-era), deliverables `.html` files,
      ringmaster `structure.md`/`phases.md`, anything left in
      `raw/docs/architecture/` not already moved (agent-internals wait for chunk 6
      — if executing chunk 4 before 6, move mojo-agent-bound and mojos-bound files
      to a clearly-named holding spot `docs/phase-2/incoming/`? NO — instead: do
      NOT delete raw/ until chunks 5+6 have pulled their files. Execute the drops
      and relocations now; delete `raw/` itself at the end of chunk 6.)
- [ ] Update AGENTS.md raw/ warning to reflect what's left. Commit.

**Note:** raw/ deletion is deferred to end of chunk 6 — chunks 5 and 6 pull their
source files directly from raw/. Chunk 4 only empties everything hub-bound.

## Chunk 5 — mojos

**Goal:** mojos is stranger-ready and NixOS-true. Docs setup only — NO design work.
The NixOS technical plan (`nixos-plan.md`) does not exist and is NOT written here;
it's the first post-restructure work item, and the docs say so honestly.

**Steps:**

- [ ] Resolve uncommitted state (deleted old docs, untracked AGENTS/CLAUDE) with a
      baseline commit, as was done for mojo.
- [ ] Create `docs/prior-art/` (+ README: "Arch-era design — superseded mechanics,
      surviving concepts feed the NixOS plan when it's written") and move from mojo
      raw/: `north-star.md` (MojOS design principles), `filesystem.md` +
      `mojo-system-files.md` (.mojo files / CWD-as-context concept),
      `mojo-config.md` (single-config-source concept), `agent.md` (hardware tiers →
      model selection). Drop `install.md`, `configure.md`, `planning.md` — Arch
      mechanics NixOS replaces wholesale (git history keeps them).
- [ ] Write `docs/roadmap.md`: component vision paragraph, the decided direction
      (NixOS + flakes, changes staged as generations before promotion), open
      questions listed plainly (flake layout, install story, agent-as-module), and
      "write the NixOS technical plan" as the explicit next work item.
- [ ] Write `docs/decisions/0001` — NixOS over Arch+Ansible+Btrfs (the decision is
      already made; recording it IS docs work — capture Clarke's real reasons:
      declarative config, flakes, generations = staging for free).
- [ ] `docs/devlog.md`, `docs/ideas.md` (harvest `architecture/mojos/ideas/` —
      fork model, multi-distro, etc. — one line each).
- [ ] Rewrite `README.md`: NixOS story (drop "custom Arch install"), status, hub
      backlink. Reconcile `AGENTS.md` to what now actually exists — remove the
      dead `nixos-plan.md` reference until that file is real.
- [ ] Commit + push.

## Chunk 6 — mojo-agent

**Goal:** the core Phase 1 repo tells its story.

**Steps:**

- [ ] Commit untracked AGENTS/CLAUDE baseline.
- [ ] Create `docs/prior-art/` with README ("inherited from archived ringmaster —
      reference, not decisions") and move from mojo raw/: ringmaster `ai.md` (node
      spec), `api.md` (WS frame protocol), local-memory parts of `memory.md`
      (vault model, dream mode, persist), `testing.md`, `improvement.md`,
      `ideas/ai.md`, `ideas/clients.md` (local surfaces), `ideas/memory.md`,
      `ideas/onboarding.md`, `ideas/skills.md`; also mojos-era `agent.md`
      (routing/hardware tiers) if not consumed by chunk 5.
- [ ] Write `docs/roadmap.md`: what mojo-agent must become in Phase 1 (local
      models, system service, memory/vault, the sensei behaviors from philosophy),
      explicitly single-user, pointer to hub phase-2 for fleet.
- [ ] **Deliberate ringmaster idea harvest** (Clarke: "a lot of ideas in the
      ringmaster repo should be kept and rethought in terms of the new model").
      Sources: the prior-art files above, the remaining ringmaster docs in raw/,
      AND the archived repo itself read-only (`~/projects/ringmaster` — README,
      `audit.md`, `jarvis-dev-context.md`, code-level concepts). This is a
      rethinking pass, not transcription: each surviving idea gets restated in
      one-agent terms before it's written down. Known candidates from the survey —
      constitutional check node; dream-mode consolidation + condensation
      principle; improve.jsonl self-improvement log; tier-aware responses
      (rethought as context-aware verbosity, no user tiers); confirmation-gate
      pattern; humaniser/style-DB (writes in the user's voice); self-maintained
      per-user behavior file (JARVIS.md equivalent); `/btw` interrupt channel;
      life-operating-layer (home devices via tool wrappers); wrist-client
      proves-API-is-client-agnostic; external-AI escape hatch (rethink against
      the first-mate/frontier-crew model — the vision shifted). Agent-capability
      ideas → mojo-agent `docs/ideas.md`; org-level ones → hub `docs/ideas.md`;
      all tagged `[ex-ringmaster]`. Borderline or surprising finds → ask Clarke.
- [ ] `docs/decisions/`: only write ADRs for genuinely decided things (candidates:
      local-models-first, secrets-via-env hard-fail, repository pattern — confirm
      with Clarke these are decisions vs aspirations). `docs/devlog.md`,
      `docs/ideas.md`.
- [ ] Write `README.md` (currently 5 lines referencing a "collective vault" that
      no longer exists): what it is, status, hub backlink.
- [ ] **Delete `raw/` in the mojo repo** — everything has now been pulled. Update
      mojo AGENTS.md (drop raw/ warning) and docs/README.md. Commit both repos,
      push.

## Chunk 7 — herald + dotfiles

**Steps:**

- [ ] herald: `git init`, add `LICENSE` (AGPL-3.0 default — veto in session if
      wrong), initial commit, `gh repo create mojo-labs-circus/herald` (public,
      description from README pitch), push. Fix hub README/manifest link.
- [ ] dotfiles: review uncommitted modifications (CLAUDE.md, README, bashrc,
      hyprland.conf, kitty.conf, +2), commit deliberately (separate config changes
      from docs changes), add hub backlink line to README/AGENTS, push.
- [ ] Commit.

## Chunk 8 — .github org profile

**Steps:**

- [ ] Update `profile/README.md`: MojOS described as NixOS-based; add herald to
      the repo table; ringmaster row says archived/superseded; condensed pitch
      stays but links hub `docs/vision.md` as the canonical version; roadmap
      section matches hub `docs/roadmap.md` phase status.
- [ ] Sanity-check CONTRIBUTING.md / CODE_OF_CONDUCT.md / SECURITY.md still fit.
- [ ] Push via PR or direct commit to `.github` repo.

## Chunk 9 — Verification pass

**Steps:**

- [ ] Link check: every relative link in every repo's README/AGENTS/docs resolves;
      no references to deleted raw/ paths anywhere.
- [ ] Manifest check: hub README table matches reality (repos, phases, statuses,
      herald URL live).
- [ ] Stranger test per repo, fresh session each: "what is this, what phase, what
      do I do next?" — must be answerable from README + AGENTS + docs/README alone.
- [ ] Delete `TRACKER.md`, `raw/plans/docs-restructure-*.md` leftovers if any
      remain (chunk 4/6 should have removed raw/ entirely).
- [ ] Final devlog entry. Commit. Done per handoff's done-when.
