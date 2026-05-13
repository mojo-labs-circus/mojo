# Claude Code Setup Plan

Getting Claude Code configured properly before the real work starts. This is about the tooling and working relationship, not the product.

---

## A. `~/.claude/CLAUDE.md` — Global Working Identity

New file. Applies system-wide to all projects.

**Contents:**
- Clarke is not a vibe coder. Every project is Clarke's. Claude is a managed tool — it proposes, Clarke decides, approves, and owns every outcome. Clarke must be able to explain every decision in the code.
- Collaboration model: explain before writing, wait for explicit approval, never start an implementation sequence without a green light.
- Comprehension gate: never mark anything done unless Clarke has demonstrated understanding. Stop and explain anything unclear before moving on.
- Communication style: concise, no trailing summaries, no narrating internal deliberation, complete sentences.
- Token discipline: no speculative tool use. State intent before each tool call. Ask before any sequence longer than ~3 steps. Do not read files you don't need.

---

## B. `~/.claude/settings.json` — Global Permissions

Update existing file (currently just has theme). Auto-allow all non-destructive read/inspect operations globally so permission prompts don't interrupt safe ops:

**Auto-allow (global baseline):**
- `git status`, `git log`, `git diff`, `git show`, `git branch`
- `ls`, `find`, `grep`, `cat`, `wc`, `file`, `which`, `env`, `pwd`
- `python --version`, `pip list`, `pip show`
- `curl` read-only fetches (GET only)

**No deny list.** Destructive operations (rm, force push, hard reset, DROP, overwriting uncommitted work, etc.) hit the standard Claude Code permission prompt naturally since they won't be in the allow list. That's gate one.

**Double confirmation via CLAUDE.md instruction (gate two):** Before running any destructive command, Claude must:
1. Name the command, state exactly what it does, and what will be lost or become unrecoverable
2. Ask explicitly: "Are you sure you want to do this?" and wait for a clear "yes" before proceeding

This gives two gates for anything dangerous: the behavioral warning + the system permission prompt.

---

## C. `circus/.claude/settings.json` — Project Permissions

New file. Extends the global baseline with project-specific auto-allows:

**Auto-allow (project additions):**
- `pytest`, `python -m pytest` (test runs)
- `python -m` module invocations (non-destructive)

More will be added here as the dev workflow firms up.

---

## D. Sprint Session Workflow

This is how we work. Every time Clarke is ready to do some work on this project, this is the ritual — not optional, not skippable.

**1. Session start**
Claude reads relevant CLAUDE.md files and any spec docs flagged in the current task. No tool use beyond Read until this is done.

**2. Plan phase — hard-gated**
- Tools restricted to: Read, WebFetch, WebSearch
- No code written, no files modified
- We define together: what we're building, which spec it aligns to, the exit criteria, and which files will be touched
- Clarke explicitly approves the plan in writing before anything else happens
- If the plan isn't approved, we stay in plan phase

**3. Implementation phase — fully guided**
- All tools available, but every action is narrated before execution
- Clarke drives every decision. Claude proposes options, Clarke picks
- No autonomous sequences. No "I'll just also fix this while I'm here"
- Implementation is still completely guided — Clarke is in control of every step

**4. Sprint close**
- Commit meaningful changes
- Update the component's CLAUDE.md current task section
- Note any open questions for the next session

*(Flow details to be discussed and refined before we lock this in)*

---

## E. Context Management

**TODO — needs more research before implementing anything.**

Questions to answer:
- Does `autoCompact` exist in Claude Code settings? What are the valid options?
- What does `/compact` do exactly — does it summarise or truncate?
- Are there hooks that fire near context limit?
- What's the right threshold to compact vs. start a new session?

Do not add any context management configuration until this is understood properly.

### E1. Repo Subdivisions for Context

Future/speculative ideas should live in the repo (they belong here) but shouldn't pollute the context window during active work sessions. The mechanism is already in place — Claude Code loads `CLAUDE.md` files per directory, and component sessions only see their local one. The gap is the `spec/ideas/` subtrees, which bleed into top-level sessions.

**Convention to adopt:** Flag `spec/ringmaster/ideas/` and `spec/mojos/ideas/` explicitly in the top-level `CLAUDE.md` as "do not load unless explicitly requested." A single sentence in the instructions is enough — Claude Code respects it.

**Longer term:** `.mojo` spec files (see ringmaster/ideas) could become the natural subdivision unit — you load context by entrypoint, not by directory.

---

## Files to Create / Modify

In order of execution once this plan is approved:

| # | File | Action |
|---|------|--------|
| 1 | `~/.claude/CLAUDE.md` | Create (global working identity) |
| 2 | `~/.claude/settings.json` | Update (add global permissions) |
| 3 | `circus/.claude/settings.json` | Create (project permissions) |
| 4 | `circus/CLAUDE.md` | Update (add sprint workflow section) |
| 5 | `mojos/ringmaster/CLAUDE.md` | Rewrite (stale Jarvis context — actively misleading) |
| 6 | `mojos/agent/CLAUDE.md` | Create stub |
| 7 | `clients/web/CLAUDE.md` | Create stub |
| 8 | `clients/tui/CLAUDE.md` | Create stub |
