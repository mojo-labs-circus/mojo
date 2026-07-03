# Claude Code Setup Plan

Goal: get Clarke's Claude Code installation configured to the best possible state.
Reference material: `research/claude-code-findings.md`

Status key: `[ ]` not started · `[~]` in progress · `[x]` done

---

## Phase 1 — Foundation

### Session 1: Memory Audit and Migration

**Goal:** Clean up all memory files. Get everything into the correct native locations. Wipe anything not worth keeping.

- [ ] Audit all existing memory files under `mojo-labs/mojo/` and any other non-standard locations
- [ ] Audit `~/.claude/projects/` to see what auto-memory already exists
- [ ] Decide what's worth keeping — framework decisions, key conventions, anti-patterns with rationale
- [ ] Migrate kept content to `~/.claude/projects/<project>/memory/` (correct native location)
- [ ] Wipe everything else
- [ ] Verify auto-memory is loading at session start (first 200 lines / 25KB of MEMORY.md)

**Notes:** Auto-memory is project-scoped by CWD. Mojo-labs sessions and per-component sessions will have separate memory directories. Understand this boundary before migrating.

---

### Session 2: Core Global Config

**Goal:** Get `~/.claude/CLAUDE.md`, `guardrails.md`, and the coding principles rule into clean, correct shape.

- [ ] Audit `~/.claude/CLAUDE.md` — prune anything redundant, fix anything wrong, verify structure
- [ ] Update `~/.claude/rules/guardrails.md` — review against findings, add anything missing (contents TBD in session)
- [ ] Create `~/.claude/rules/coding-principles.md` — Karpathy's four principles (Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution). No `paths:` frontmatter so it loads every session.
- [ ] Establish pruning discipline: CLAUDE.md is not append-only. Periodic audits required. Boris's rule: if it gets too long, delete it and start fresh.
- [ ] Establish errors-to-rules habit: every time Claude does something wrong that you don't want repeated, it becomes a rule in CLAUDE.md. Not a conversational fix — a persistent rule.

---

## Phase 2 — Automation

### Session 3: Hooks Design and Implementation

**Goal:** Design and implement the full hook system. This is the highest-leverage automation layer.

Research is complete (`findings.md §3`). Key decisions to make and implement:

- [ ] **Handoff brief + session transition flow**
  - Design: context approaches limit → write brief → save to standard location (e.g., `~/.claude/handoffs/latest.md`) → desktop notification → user opens new session → `SessionStart` hook detects recent brief and injects it
  - Investigate: does `PreCompact` hook output influence what survives compaction? (needs testing)
  - Implement: `/wrap` skill that generates brief + saves + notifies, usable manually or triggered by `PreCompact`
  - Implement: `SessionStart` hook that checks for a recent brief and injects it
  - Set `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=60` in project env so compaction triggers earlier when auto-compact does run

- [ ] **Auto-format on write** — `PostToolUse` matcher `Write|Edit`, runs formatter for each language

- [ ] **Desktop notifications** — `Notification` event, macOS `osascript` or Linux equivalent. Required for parallel session management.

- [ ] **Stop hook quality gate** — run test suite before session ends; exit 2 if failing. Include `stop_hook_active` guard to prevent infinite loops. Add once test suites exist per component.

- [ ] **Audit logging** — `PostToolUse` logs every tool call with timestamp, session ID, tool name, input. Append to `~/.claude/audit.jsonl`. Needed for team/collective visibility later.

- [ ] **Dangerous command block** — `PreToolUse` matcher `Bash`, exit 2 on patterns like `rm -rf /`, `DROP TABLE`, etc.

- [ ] **CwdChanged env injection** — `direnv` or equivalent to sync environment when changing into a component directory

- [ ] Decide what goes in `~/.claude/settings.json` (global, personal) vs `.claude/settings.json` (project, committed)

**Key principle:** CLAUDE.md rules have ~80% adherence. Hooks are 100% deterministic. If a CLAUDE.md rule keeps getting violated, convert it to a hook.

---

### Session 4: MCP Servers

**Goal:** Decide which MCP servers to add globally and configure them correctly.

- [ ] Add GitHub MCP server globally — replaces most interactive `gh` CLI use
- [ ] Decide what else goes global vs project-specific
- [ ] Review security model: verify trust before connecting, prompt injection risk from servers that fetch external content
- [ ] Update `~/.claude/settings.json` allow list for MCP tool patterns
- [ ] Document: global scope for general tools (GitHub), project scope in `.mcp.json` for project-specific infrastructure

**Candidates to evaluate:**

- GitHub MCP (confirmed useful — Boris's team uses it)
- Playwright/browser (needed for verify-app pattern)
- Database tools (when relevant to a component)
- Sentry, logging tools (when components are running)

---

## Phase 3 — Workflow

### Session 5: Global Agents

**Goal:** Design and create the agents that should be available across all projects.

- [ ] **code-simplifier** — reviews completed code for quality, reuse, efficiency. Never changes what code does, only how. Run post-implementation. Restrict to read tools only.
- [ ] **code-reviewer** — pre-PR review for correctness and security. Read-only tools. Can use Sonnet or Haiku.
- [ ] **build-validator** — ensures build passes all checks. Runs before PR creation.
- [ ] Decide model per agent in frontmatter: Haiku for fast passes, Sonnet for standard, Opus for deep review
- [ ] Decide tool restrictions per agent (read-only for reviewer, full access for validator)

---

### Session 6: Global Skills and Commands

**Goal:** Design and implement the global slash command set.

This session needs design time before implementation. Key commands to build:

- [ ] `/commit-push-pr` — commit → push → create PR. Pre-compute git state with `!` bash to reduce model calls. Used many times daily.
- [ ] `/wrap` — generate handoff brief, save to `~/.claude/handoffs/latest.md`, send desktop notification. Used at end of sessions or triggered by PreCompact hook.
- [ ] `/grill` — adversarial code review. Find problems before they become PRs.
- [ ] `/quick-commit` — fast commit without PR.
- [ ] `/test-and-fix` — run tests, fix failures, loop.

**Open:** What other commands does the Mojo workflow need? May require a design session before implementation.

---

### Session 7: Settings and Permissions Audit

**Goal:** Get `~/.claude/settings.json` into the correct shape. Understand the full precedence model.

- [ ] Audit current `~/.claude/settings.json` against full schema from findings
- [ ] Review current allow/deny lists — what's missing, what's wrong
- [ ] Verify merge behaviour: arrays merge across scopes, scalars use most-specific value
- [ ] Confirm gitignore of `settings.local.json` across projects
- [ ] Set up env vars that should apply globally

---

### Session 8: Model Selection

**Goal:** Make deliberate decisions about which model to use in which context and encode them in settings.

- [ ] Decide model defaults: global, per-project, per-subagent
- [ ] Current recommendation (from findings): Opus for framework design / complex planning; Sonnet for standard implementation; Haiku for fast subagent tasks
- [ ] Configure per-project model defaults in `.claude/settings.json`
- [ ] Configure per-subagent model overrides in agent frontmatter
- [ ] Review `effortLevel` settings — how much this affects cost vs quality

---

## Phase 4 — Efficiency

### Session 9: Token and Cost Management

**Goal:** Understand and optimise token usage beyond RTK (already installed).

- [ ] RTK is already running PreToolUse compression (60–90% on command output). What else matters?
- [ ] Review what prompt caching handles automatically and what invalidates it
- [ ] Set up `/usage` monitoring habit — check cost per session
- [ ] Investigate `stats-cache.json` for historical cost data
- [ ] Subagent delegation pattern: route verbose operations (log analysis, long test runs) to subagents that return summaries
- [ ] Revisit CLAUDE.md size — every line loaded every session has a cost

---

## Pending Research

**Dev conventions research session** (separate, later):
Research conventions and best practices from top software engineering teams and companies. Apply relevant practices to the Mojo project structure and workflow. Scope TBD once Claude Code setup is complete.

---

## Open Questions (carry into relevant sessions)

- Exact behaviour of `PreCompact` hook output — does it influence what survives compaction?
- Whether `SessionStart` hook can reliably inject a handoff brief from a file
- Per-hook `timeout` field — verify this is real before using it
- Exact `/wrap` skill implementation — needs design before coding
