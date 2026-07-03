# Docs Restructure — Session Progress (2026-07-02)

Working state for the restructure defined in `docs-restructure-handoff.md`. Read that
first; this file is where the work actually got to. Continue from "Where we are".

---

## Where we are

**Step 1 (understand everything) — DONE and confirmed by Clarke.**
**Step 2 (produce the plan) — research phase, 2 of 4 research reports in.** No plan
written yet. Nothing restructured yet. Baseline of the mid-migration working tree
committed as `bccf2d8`.

## Step 1 understanding (confirmed correct by Clarke)

- **Vision:** sovereign personal AI — your own intelligence on your own hardware,
  lifetime context, no company owns it. Three gaps: capability (Linux power via
  conversation), sovereignty ("a proof, not a promise" — architecture, not policy),
  context (persistent picture of you). Philosophical layer: the craft problem — AI as
  sensei that develops you, anti-vibe-coding. Root: JARVIS; first audience: family.
- **History (two pivots, docs are sediment of both eras):** Era 1 = jarvis/ringmaster,
  a multi-user family home-server (FastAPI/LangGraph/Ollama) with real tested code
  (auth, WebSocket, data layers) — archived when the agent graph was never wired and
  scope outgrew the design. Era 2 = collective-intelligence framework (8–10 design
  sessions, KOs, constitutions, blockchain research) — mostly unexecuted briefs, now
  Phase 3. Current era: **one agent type (mojo-agent), deployed anywhere, scales to
  hardware** — idea originates in ringmaster's `mojo-language.md`.
- **Phases:** 1 = one machine/one person (mojo-agent + MojOS + dotfiles as one
  system). 2 = fleet, topology deliberately TBD, Ringmaster-as-hub dead as baseline.
  3 = collectives, deprioritized.
- **Repo real states:** mojo = hub, docs/ empty, everything in raw/ (97 files).
  mojo-agent = one init commit, no code/docs (core Phase 1 deliverable, emptiest repo).
  mojos = mid-flight, AGENTS.md references nonexistent nixos-plan.md. dotfiles = only
  working content in org. herald = best docs, but NOT a git repo / not on GitHub.
  ringmaster = properly archived; its raw/ docs still hold reusable material
  (north-star ethics, WS frame protocol, auth design, memory/vault model). baker
  (outside org) = battle-tested Arch installer, ancestor of MojOS install design,
  Clarke's live OS.

## Clarke's decisions this session (all confirmed)

1. **NixOS, not Arch.** Flakes/declarative won; Arch references everywhere are stale.
   The org profile, mojos README, and all old architecture docs need updating.
2. **Herald joins the org** — make it a real git repo under mojo-labs-circus. It's
   part of the wider vision (capture knowledge once, render per receiver).
3. **Baker stays out of scope.** Not part of Mojo; MojOS will replace it. Don't
   archive while it's his live OS (dotfiles depends on it). Story mentions only.
4. **Deliverables (stance/brief/presentation) are historical.** Good thinking inside;
   collective/framework material parks in a phase-3 docs section so it isn't lost.
5. **Docs must be scoped per phase** so phases can be worked through correctly.
6. **Nothing from raw/ is trusted as verified** — including the docs-convention
   proposal; everything re-verified from primary sources (done, see below).

## Research report 1 — convention re-verification (COMPLETE)

Independent re-check of `docs-convention-proposal.md` against primary sources:

- **Confirmed:** AGENTS.md standard (tool list, 60k+ repos, Linux Foundation
  stewardship; Anthropic docs still say "Claude Code reads CLAUDE.md, not AGENTS.md" —
  `@AGENTS.md` import is their documented recommendation). MADR (current field is
  `decision-makers` not `deciders` — that's MADR 4.x; no `draft` status; never-delete
  traces to Nygard 2011). Diátaxis four categories + verbatim anti-empty-scaffolding
  quote; renaming "explanation" is a sanctioned kind of adaptation. CNCF maturity
  depth-scaling (woven through sections, no standalone checklist). Wikilinks don't
  render in GitHub repo files. Minimal frontmatter matches real practice
  (rust-lang/rfcs, golang/proposal use body-text metadata).
- **Corrections:** (1) kubewarden/community citation was FABRICATED — its ROADMAP.md
  never held Mission/Vision (single-commit history); only sigstore supports
  vision-in-roadmap. (2) "Standalone vision docs aren't a convention anywhere" is
  FALSE — helix-editor `docs/vision.md`, ~2.8k VISION.md files on GitHub, and
  cilium/community `VISION.md` at the hub. (3) `docs/index.md` "pure navigation at 5+
  files" rule is INVENTED — only the filename is real (MkDocs homepage). (4) Marginal:
  `devlog.md` as file convention is an extrapolation from Carmack's .plan (~816 GitHub
  hits, no notable repo); icebox-coinage claim secondary-sourced only.
- Implication: nav-index and devlog can still be deliberate choices, but must be
  labeled as our choices, not industry convention. Vision-into-roadmap claim is weak;
  standalone vision doc is defensible (see report 3 when it lands).

## Research report 2 — org/multi-repo patterns (COMPLETE)

- **Hub repos are foundation-scale patterns** (kubernetes/community, cilium/community,
  jupyter/governance, dotnet/core); no verifiable solo-dev hub precedent — we're
  deliberately borrowing up a scale. Transferable: hub holds vision/roadmap/org
  decisions + a `REPOSITORIES.md`-style index of every repo with role (cilium);
  component repos carry a one-line backlink (k8s pattern), never duplicated content;
  `.github` repo is plumbing only (profile + defaults), nobody plans in it.
- **Deferred work primitives** (from rust-lang postponed / KEP+PEP status fields /
  TC39 inactive index / dotnet designs dirs): (a) status marker on the doc, (b) a
  physical "not now" home so the active set stays clean, (c) record WHY parked, one
  line (TC39 habit). Phase-keyed dirs (`phase-2/`, `phase-3/`) are a legitimate
  variant of directory-as-status.
- **Pre-code repos that aged well** (Oxide RFDs, TigerBeetle single DESIGN.md, Oil
  shell): few files, each substantial; explicit state/date markers so stale docs
  announce themselves. Credible minimum: README + substantial design doc(s) with
  states + roadmap/phase statement + dated notes. What good ones never had pre-v0.1:
  empty dirs, per-component doc trees before components exist, elaborate taxonomies.

## Research report 4 — AI-navigable docs (COMPLETE, 2026-07-03)

- **Agent files (converged across Anthropic docs, Codex guide, Cursor docs, real
  files from temporal/ghostty/codex/airflow):** only non-inferable rules — commands
  the agent can't guess, conventions that differ from defaults, gotchas. Nothing
  inferable from code, no file-by-file descriptions, no code snippets (rot fastest).
  Anthropic's pruning test: "Would removing this cause Claude to make mistakes? If
  not, cut it. Bloated CLAUDE.md files cause Claude to ignore your actual
  instructions." Good length varies (ghostty 1.4KB → codex 22KB) but density is
  constant: rules only, never explanation. temporalio/sdk-java (~60 lines: layout
  one-line-per-module, commands, conventions, pointer to CONTRIBUTING) is the
  platonic ideal. Boris Cherny practice: CLAUDE.md is an accumulating
  error-correction log — every time a session errs, add one line. Airflow generates
  part of its AGENTS.md from source to prevent drift.
- **Progressive disclosure:** root file = map not manual (identity paragraph, current
  phase, pointers); deep content in docs/ read on demand. Claude Code primitives:
  parent-dir CLAUDE.md walk-up (loads in every child repo session), child-dir
  CLAUDE.md loads on demand, @imports, `.claude/rules` with paths globs.
  "Prefer pointers to copies" is the single most-converged finding.
- **llms.txt:** irrelevant for plain GitHub repos (web-root convention; no major
  platform consumes it autonomously; adoption numbers are a Mintlify hosting
  default). But its shape — small index of one-line-described links — is exactly what
  a good AGENTS.md is.
- **Anti-drift:** one-fact-one-place; never restate what a file says, point at it;
  greppability (stable names, descriptive headings, consistent terms) beats any
  bundle format; skip repomix-style context packs for CLI agents in a checkout.
  Failure modes: bloat → rules ignored; duplicated lists go stale; agents confidently
  consume stale docs.
- **Cross-repo (polyrepo):** best practice at our scale = (1) a CLAUDE.md in the
  directory ABOVE the checkouts — loads automatically in every repo session via
  walk-up; one line per repo (name, role, path) + "org decisions live in mojo";
  (2) hub repo is the canonical cross-repo source, parent file points there;
  (3) component AGENTS.md never lists siblings (stale-list failure) — one pointer to
  hub; (4) `--add-dir ../mojo` when a component session needs org context. NOTE:
  mojo repos live flat in ~/projects mixed with non-mojo repos (baker, cloudmundi,
  mojo-fund) — a parent CLAUDE.md at ~/projects would load for everything. Old ADR
  0007 proposed ~/projects/mojo-labs/ workspace dir; decide in the plan whether to
  revive that or keep the parent file org-agnostic.

## Research report 3 — vision placement, navigation, devlog — RE-RUNNING

First run died at session limit with no output. Re-launched 2026-07-03 with same
scope: where vision-heavy projects keep vision/philosophy (helix, Zig, Oxide,
TigerBeetle TIGER_STYLE, Deno manifesto, RedwoodJS, curl, GitLab handbook) — layered
pitch→vision→philosophy vs folded-into-roadmap; docs/README.md as directory landing
page on plain GitHub (verify + major examples); real dated-devlog practice (Zig
devlog format, TigerBeetle updates, in-repo examples) for a zero-ceremony append-only
log.

Clarke's explicit asks driving 3 & 4: "where should we keep our vision stuff —
it's important, especially at this early stage" and "how do you effectively get AIs
working on and understanding the projects."

## Next actions (in order)

1. Get reports 3 & 4 (re-run if outputs lost).
2. Synthesize all four into the Step 2 deliverable: what good docs look like for THIS
   project (README + agent files + docs/ per repo, phase-scoped, hub vs component
   split, vision placement, AI navigability) — then a concrete, ordered, chunked plan
   (each chunk executable in one session). Write plan to `raw/plans/`, present to
   Clarke BEFORE executing anything.
3. Step 3 (only after Clarke approves plan): execute chunk by chunk. Every repo ends
   with real README, real agent files, real populated docs/; raw/ fully triaged —
   everything finds a home or is deliberately dropped; herald becomes a git repo under
   the org; NixOS story corrected everywhere (org profile, mojos, READMEs).

## Session context worth keeping

- Ringmaster survey and framework/research survey reports (Step 1) were detailed;
  their key conclusions are baked into the Step 1 understanding above. Full ringmaster
  detail worth re-deriving only when triaging `raw/docs/architecture/ringmaster/`
  during execution — the reusable pieces are: north-star Code of Ethics, WebSocket
  frame protocol (api.md), auth design (auth.md, was built+tested), memory/vault model
  (memory.md), mojo-language.md as the pivot document.
- `stance.md` is the single best consolidated vision artifact in the project — anchor
  candidate for the new vision doc.
- Naming hazard: framework docs use "Mojo" for Modular's Mojo language in two places
  (`research-ai-paradigms.md`, `02-agent-model.md`) — disambiguate during triage.
- Unresolved Phase 3 tension preserved as-is: human+AI pair = one member vs two
  (framework docs pick different sides; flagged in the docs themselves).
