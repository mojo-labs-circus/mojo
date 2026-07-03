# Claude Code Setup — Research Brief

**Session type:** Research and synthesis only. No implementation.
**Output format:** Structured synthesis document covering all topics below. Where sources conflict, note it. Where best practice is unclear, say so. End goal is a generic best-practices document we can apply to any project, starting with Mojo.

---

## Context

Clarke is a first-year CS student, strong in Python, C, Bash, Java. Linux/Arch. Not a beginner — skip hand-holding. Thinks systems-first. The project is Mojo: a collective intelligence framework where human and AI members collaborate on a shared codebase. Currently solo dev, framework design phase, dev paused across all component repos. Eventually a team — human and AI members working together. Everything we build needs to scale to that.

The goal of this research session is to understand how to configure Claude Code as well as possible for a long-running, complex project that will eventually involve a collective. After this session we'll produce a best-practices document and then apply it.

Clarke already has:

- `~/.claude/CLAUDE.md` (global rules, 84 lines)
- `~/.claude/rules/guardrails.md` (auto-loaded safety rules)
- `~/.claude/RTK.md` (token compression tool, @referenced)
- RTK installed — PreToolUse hook that compresses command output 60–90% before it hits context
- Project CLAUDE.md files across mojo-labs components
- Memory files under mojo-labs/mojo/

---

## Research Agenda

### 1. File Structure — Global and Project

**Global (`~/.claude/`):**

- Full map of every directory and file Claude Code recognises. Not just CLAUDE.md — what does `agents/`, `commands/`, `skills/`, `rules/`, `plans/`, `tasks/`, `plugins/` do? What auto-loads vs needs explicit @reference?
- SOUL.md — is this a real Claude Code concept, a community convention, or just a naming pattern? How does it work?
- RULES.md — same question
- AGENTS.md — Karpathy uses this. Is it a native concept or convention?
- MEMORY.md — how does the file-based memory system actually work at the global level?
- Any other undocumented or community-discovered files

**Project (`.claude/` inside a repo):**

- Full map of project-level `.claude/` — what goes where, how it layers with global
- `settings.json` vs `settings.local.json` — what each contains, how they merge
- `.mcp.json` — how this works, project vs global scope
- `.claudeignore` — full behaviour, what it affects

**CLAUDE.md hierarchy:**

- Exactly how parent CLAUDE.md loading works — does CWD=`packages/api/` load both root CLAUDE.md and `packages/api/CLAUDE.md`? How deep?
- Multi-repo vs monorepo CLAUDE.md patterns
- Best structure for a project like Mojo (monorepo wrapper, multiple component repos, planning repo)
- Size limits — is 200 lines a hard limit or a guideline? What happens beyond it?

**@reference system:**

- Exactly how `@filename` works — eager loading vs lazy, size limits, nesting (@file that @references another)
- When to @reference vs inline content

---

### 2. Ways of Working

**Boris Cherny (creator of Claude Code):**

- Full documented workflow — everything he's said publicly about how he uses it
- His CLAUDE.md structure specifically
- Parallelisation approach — 5 local sessions + 5–10 browser sessions, how he coordinates
- Plan mode usage
- Slash commands he uses daily
- How he handles errors → CLAUDE.md updates

**Andrej Karpathy:**

- His 4 principles (think before coding, simplicity first, surgical changes, goal-driven execution) — full detail
- AGENTS.md — what it contains, how he structures it
- LLM Wiki pattern — full implementation detail: raw/, wiki/ directories, CLAUDE.md schema, ingest/query/lint operations. How applicable is this to a project knowledge base?

**Other practitioners:**

- Any other well-documented Claude Code setups from engineers — real examples, not generic takes

**Context window management:**

- When to start a fresh session vs continue
- How to hand off between sessions cleanly — what to write down, where
- Context compaction — what gets preserved automatically, what doesn't
- Saving plan state so it survives compaction

**Parallelisation:**

- Git worktrees with Claude Code — how to set up, coordinate, merge
- Multiple Claude sessions on same repo — conflict risks, coordination patterns
- How to decompose work across parallel agents effectively

**Long-running projects:**

- Managing continuity across dozens of sessions
- Preventing context rot in CLAUDE.md
- Keeping project memory accurate over time

---

### 3. Hooks — Deep Dive

**Full system documentation:**

- All hook events: PreToolUse, PostToolUse, Stop, UserPromptSubmit — full behaviour of each
- What each can and can't do (block, modify, inject, notify)
- Timeout behaviour, async vs sync, failure modes
- Security implications — hooks run with full user permissions, what this means

**Patterns for a team on a shared codebase:**

- Audit logging — logging every tool use with timestamp, session, what was done. How to implement. Where logs go.
- Quality gates — blocking commits/writes unless tests pass, linting passes. Implementation patterns.
- Permission enforcement — hooks that restrict what anyone can touch. File-level, directory-level.
- Cross-session state injection — hooks that load current project state at session start
- Notification hooks — alerting on significant events (commit, PR, test failure)
- CI/CD integration — hooks that trigger external pipelines

**Shared hook configuration:**

- How to commit hooks to git so all team members get them automatically
- settings.json vs settings.local.json for shared vs personal hooks
- Hook versioning — what happens when hook config changes

**Real examples:**

- The most useful hooks people are actually running day-to-day
- Solo dev hook setups worth adopting immediately

---

### 4. MCP Servers

- Which MCP servers are genuinely useful for a developer workflow (not marketing — real utility)
- How to configure: global `~/.claude/` vs project `.mcp.json`
- Scope — which servers should be global, which project-specific
- Building custom MCP servers — when it's worth it, how hard it is
- Security model for MCP

---

### 5. Custom Commands and Subagents

**Slash commands (`.claude/commands/`):**

- Full mechanics — how commands are defined, parameterised, shared
- Commands worth building for a solo dev
- Commands worth building for a team (code review, verification, PR, onboarding)
- How Boris Cherny's `/commit-push-pr` and similar work

**Subagents (`.claude/agents/`):**

- How agent definitions work — what goes in them, how they're invoked
- Difference between a subagent and a slash command
- When subagents are worth the overhead
- Boris Cherny's verify-app, build-validator, code-simplifier patterns — full detail

---

### 6. Settings and Permissions

- Full `settings.json` reference — every field, what it does
- Global vs project vs local layering — what overrides what, what merges vs replaces
- Allowlist/denylist best practices — what to allow globally, what to restrict
- Model selection in settings — can you set default model per project?
- Worktree settings — sparse paths, symlink directories

---

### 7. Cost and Token Management

- RTK is already installed (PreToolUse hook, 60–90% compression on command output). What else exists?
- Context size strategies — keeping sessions lean
- Model selection for cost vs quality tradeoff (which tasks need Opus, which are fine with Sonnet/Haiku)
- Monitoring cost per session — are there tools for this?
- Cost attribution per task/session — how to track what different workstreams consume
- Caching strategies — prompt caching, what Claude Code does automatically

---

### 8. GitHub and Git Integration

- GitHub MCP server (`mcp__github__*`) — full capability map, what it can do vs what `gh` CLI is needed for
- PR review workflows — how to automate Claude reviewing PRs
- Issue tracking integration
- Commit patterns that work well with Claude Code
- Branch strategies — one branch per task, worktrees, how to manage

---

### 9. Testing and Verification

- Verification loops — Boris Cherny's verify-app pattern, giving Claude a way to check its own work
- Test-driven Claude Code — how to structure so Claude writes tests alongside implementation
- Quality assurance patterns — what checks happen at which stage
- How hooks and verification interact

---

## Output Format

For each topic area, produce:

1. **What's real** — what Claude Code actually does natively, with citations
2. **What's convention** — community patterns that work but aren't native features
3. **Best practice** — what the best setups actually do, with concrete examples
4. **Relevant to Mojo** — specific implications for a complex long-running project that will eventually involve a team

Where sources conflict, note it. Where something is unclear or unverified, flag it rather than guessing.

At the end: a one-page summary of the highest-impact changes a developer should make to their Claude Code setup, ordered by impact.
