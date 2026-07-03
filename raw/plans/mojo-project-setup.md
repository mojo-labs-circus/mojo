# Mojo Project Setup Plan

Goal: get the Mojo project structured correctly — CLAUDE.md hierarchy, .claude/ config, GitHub integration, verification, and team/collective readiness.
Reference material: `research/claude-code-findings.md`

**Dependency on framework sessions:** This plan cannot be completed without the framework design outputs. The collective model, roles, governance rules, and collaboration patterns defined in the framework sessions directly shape the project config. Do not start Phase 1 until the framework sessions are complete (or at least far enough to answer: what roles exist, how do humans and AIs collaborate, what are the governance boundaries).

**Dependency on Claude Code setup:** `plans/claude-code-setup.md` — global Claude Code setup should precede or run in parallel. Hooks, MCP, and agents designed there are prerequisites for sessions 2–4 here.

Status key: `[ ]` not started · `[~]` in progress · `[x]` done

---

## Phase 0 — Collective Model (prerequisite)

### Session 0: Collective Model and Team Conventions

**Goal:** Translate framework design outputs into concrete decisions about how the collective works in practice. These decisions feed into every session that follows.

**Prerequisite:** Framework sessions complete enough to answer the questions below.

- [ ] What roles exist in the collective? (architect, implementer, reviewer, etc. — how do they map to humans vs AI agents?)
- [ ] What does AI agency look like in this project? What can an AI agent do autonomously vs what requires human approval?
- [ ] How are commits and PRs attributed? Does an AI agent have an identity in git history?
- [ ] What are the governance rules for the codebase? Who (or what) can merge? What gates a merge?
- [ ] How is work decomposed and assigned across a mixed human-AI team?
- [ ] What does membership in the collective mean for an AI agent — persistent identity across sessions? Scoped permissions?
- [ ] Encode answers in `.claude/rules/collective-conventions.md` — this file governs every collaborator

**Output:** A set of decisions that directly inform the config in sessions 1–7. Not a design document — a decision record.

---

## Phase 1 — Structure

### Session 1: CLAUDE.md Hierarchy

**Goal:** Get the CLAUDE.md structure right across the full mojo-labs tree.

- [ ] Audit current `mojo-labs/CLAUDE.md` — what's in it, what should move out, what's missing
- [ ] Establish the content rule: CLAUDE.md contains only things that apply every session. Everything else has a better home.
- [ ] Agree on the progressive disclosure structure for Mojo:
  - `mojo-labs/CLAUDE.md` — project identity, component map, critical cross-component facts, @references to extended docs
  - Per-component `<component>/.claude/CLAUDE.md` — domain conventions, test commands, component-specific context
  - `~/.claude/rules/` — global principles (coding-principles.md, guardrails.md) — already handled in Claude Code setup
  - `.claude/rules/` — component-specific rules added as conventions emerge (not speculatively)
- [ ] Write or update root CLAUDE.md with: project description, component map with roles, current phase, where to find what
- [ ] Decide what gets @referenced vs inlined (large specs, framework docs → @reference)
- [ ] Stub per-component CLAUDE.md files — even if minimal for now, establish the files so they load correctly

**CLAUDE.md loading reminder:** Claude walks up from CWD, loads all CLAUDE.md files root-to-CWD. Per-component files load automatically when working in that directory. No manual coordination needed.

---

### Session 2: Project .claude/ Structure

**Goal:** Set up the committed `.claude/` directory at the mojo-labs root correctly.

- [ ] Create `mojo-labs/.claude/settings.json` — project-wide: model defaults (Opus for now, see Claude Code setup §8), hooks specific to this project, env vars
- [ ] Set `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=60` in project env
- [ ] Create `.claude/rules/` directory — empty for now, add files as component conventions emerge
- [ ] Create `.claude/agents/` directory — stub, populated in session 4
- [ ] Create `.claude/skills/` directory — stub, populated when project-specific commands are designed
- [ ] Ensure `settings.local.json` is gitignored

**What goes in project settings vs global settings:**

- Project `settings.json`: model defaults, project-specific env vars, project-specific hooks, MCP servers for Mojo infrastructure
- Global `~/.claude/settings.json`: personal preferences, universal allow lists, global MCP servers (GitHub, etc.)

---

### Session 3: GitHub and Git Integration

**Goal:** Wire up GitHub MCP and Actions. Establish branch and PR conventions.

- [ ] Confirm GitHub MCP server is configured globally (from Claude Code setup §4) and accessible in Mojo sessions
- [ ] Install GitHub Actions (`/install-github-app` or manual): `anthropics/claude-code-action@v1`
  - Trigger: `@claude` mentions in PR/issue comments
  - Permissions: Contents (R/W), Issues (R/W), Pull Requests (R/W)
  - Add `ANTHROPIC_API_KEY` to repo secrets
- [ ] Set up automated PR review workflow:
  - Trigger: `pull_request` types `[opened, ready_for_review]` (not `synchronize` — avoids burning API on every push)
  - Action posts review as PR comments
- [ ] Establish branch strategy:
  - One branch per task / Claude session
  - `claude --worktree <name>` for parallel tasks
  - Add `.claude/worktrees/` to `.gitignore`
  - Add `.worktreeinclude` for files that need to exist in each worktree (e.g., env files)
- [ ] Establish `@.claude` PR comment convention: when a mistake is caught in review, tag Claude to add a rule to CLAUDE.md as part of the PR diff
- [ ] Decide on `/commit-push-pr` slash command usage (from Claude Code setup §6) — this becomes the standard exit path for every Claude session

---

## Phase 2 — Quality

### Session 4: Verification Agents

**Goal:** Build the verify-<component> agents that prove changes actually work. One per runnable component.

**Prerequisite:** each component needs at minimum a basic test suite before its verify agent is meaningful.

- [ ] Define the "verify" contract for each component:
  - `ringmaster/`: what does a working ringmaster look like? API endpoints responding? Integration tests passing? Load test?
  - `performer/`: local agent working end-to-end? Proxy to ringmaster functional?
  - `client-tui/`: renders correctly? Key bindings work?
  - `client-web/`: browser tests? Core user flows?
- [ ] Build verify agent per runnable component once test suite exists:
  - `.claude/agents/verify-<component>.md`
  - Detailed instructions for how to test that component specifically
  - Model: Sonnet (speed matters here)
  - Include: what to run, what output means success, what to check if failure
- [ ] Build `build-validator` agent at project level: runs build for all components, reports what fails
- [ ] Boris's principle encoded in each agent: never mark a task complete without proving it works

**Note:** This session expands over time as components are built. Start minimal and add detail as the components develop.

---

### Session 5: Project Memory Structure

**Goal:** Establish what the project auto-memory captures and how to keep it accurate.

- [ ] Understand current state: what has auto-memory written to `~/.claude/projects/mojo-labs/memory/`?
- [ ] Decide what should be in `MEMORY.md` at project level: framework decisions, key constraints, component relationships, what's been decided vs what's still open
- [ ] Establish memory hygiene: auto-memory is written by Claude and can become stale. Periodic audits needed. Trust but verify.
- [ ] Decide whether component-specific memory should live at component level or all in the root project memory
- [ ] Consider LLM Wiki pattern (Karpathy) for framework documentation: `raw/` for source docs, `wiki/` for LLM-generated synthesis, ingest/query/lint operations. May be more relevant here than standard memory. Evaluate as framework docs grow.

---

## Phase 3 — Scale

### Session 6: Component Conventions

**Goal:** Add component-specific rules and conventions as implementation begins on each component.

- [ ] Triggered when: a component starts active development
- [ ] Per component: create `.claude/rules/<component>-conventions.md` with `paths:` frontmatter scoped to that component's directory
- [ ] Cover: language/framework specifics, file structure conventions, what to never touch, test patterns
- [ ] Review and update as the component evolves — these files are living documents

---

### Session 7: Collective Readiness Audit

**Goal:** Before adding any human or AI collaborators, verify the project config actually governs all of them correctly. This is a check against the decisions made in Session 0 — are they encoded, not just described?

- [ ] All hooks in `.claude/settings.json` (committed) — every collaborator gets them automatically
- [ ] Audit logging hook in place — who did what, when (essential for a multi-agent collective)
- [ ] Verify agents exist for all active components
- [ ] CLAUDE.md reviewed and accurate — every new collaborator reads this first
- [ ] GitHub Actions automated review in place
- [ ] `.claude/rules/collective-conventions.md` exists and covers the decisions from Session 0
- [ ] Test: onboard a new Claude session as if it were a new team member. Does it have everything it needs?

---

## Future Research Session

**Dev conventions and best practices:**
Research conventions from top software engineering teams and companies (engineering blogs, public post-mortems, notable open source project structure, etc.). Goal: identify practices worth adopting for Mojo. Apply to project structure, code review process, testing standards, documentation, release process. Separate session once both setup plans are complete.

---

## Open Questions (carry into relevant sessions)

- LLM Wiki pattern: is it worth implementing for framework docs, or does standard memory + CLAUDE.md @references cover it?
- Per-component vs unified memory: one `~/.claude/projects/mojo-labs/memory/` for everything, or separate per component?
- Worktree strategy: when does parallel work start? What's the right decomposition for framework design sessions vs implementation sessions?
- `@.claude` GitHub Actions convention: needs GitHub Actions set up before it's usable
