# Dev Setup — Ground Up

> Everything here was decided deliberately. Discuss before adding anything.

---

## The Model

Clarke is the project lead. Agents are the team — professional-grade coders working under his direction. The goal is to replicate how a real dev team operates: work is tracked, communicated, reviewed, and merged through proper process. Clarke makes every decision. The team executes, communicates, and asks when stuck.

This isn't just a workflow for building MojOS. It's also the first proof that the model works — MojOS itself will eventually productise this into an autonomous agent dev team. A is the manual prototype. B is the system.

**The principle:** Clarke's decisions, Clarke's consequences. The system makes those decisions faster, easier, and doable from anywhere — not fewer.

---

## The Workflow

Every piece of work moves through six stages. Clarke must explicitly advance it between each one. Nothing auto-progresses.

```
Inbox → Backlog → Ready → In Progress → In Review → Done
```

**Inbox** — newly created ticket, by Clarke or an agent. Unreviewed. If agent-created, Clarke gets a GitHub notification.

**Backlog** — Clarke has reviewed and accepted it. Not yet scheduled. Clarke enriches the ticket description here.

**Ready** — brief is complete. Clarke has approved it for execution. This is the most important gate: Clarke is saying "this is fully defined, an agent can pick this up."

**In Progress** — agent has started. Worktree created, tmux session running, agent commenting on the issue throughout.

**In Review** — agent has opened a PR referencing the ticket. Clarke gets a GitHub notification. He reviews the diff and the agent's report at his own pace.

**Done** — Clarke merges. Ticket auto-closes via `Closes #N` in the PR. Agent cleans up the worktree.

---

## Ticket System

**Tool:** GitHub Issues + one org-level GitHub Projects board (mojo-labs-circus).

One board covers all repos. Clarke sees the full picture in one place and can filter by repo when focused.

**Ticket = Brief.** One document, progressively enriched. Agents create tickets with as much context as they can. Clarke reviews, corrects, and completes the brief before moving to Ready. By the time a ticket reaches Ready, an agent should be able to pick it up and execute without asking basic questions.

**Labels:**

- Type: `feature`, `fix`, `chore`
- Repo: `ringmaster`, `performer`, `mojos`, `client-web`, `client-tui`, `mojo-sdk`

**Branch naming:** matches the type label.

- `feature/<name>`
- `fix/<name>`
- `chore/<name>`

**Ticket template:**

```markdown
## Spec Reference
- Doc: `spec/<path>/<file>.md`
- Section: [section heading]

## Scope
**In scope:**
- 

**Out of scope:**
- 

## Files in Scope
<!-- Exact paths the agent may touch. Anything not listed requires a check-in. -->

## Definition of Done
- [ ] 
- [ ] 

## Constraints
<!-- Known decisions already made that the agent must respect -->

## Branch
`<type>/<name>`
```

---

## Communication Model

Agents communicate via GitHub Issue comments throughout execution. Clarke gets phone notifications. The standard is: keep Clarke informed the way a good developer keeps their lead informed on Slack — professional, direct, not robotic.

**Touchpoints:**

1. **On start** — "Starting work. Branch `feature/X`, worktree created. My plan: [brief breakdown in plain English]."

2. **Key design decision** — any time a non-obvious choice is made: "Chose X over Y — [one sentence reason]. Continuing."

3. **Milestone reached** — natural completion points, not every commit: "Core implementation done. Moving to tests."

4. **Blocker** — agent stops and asks rather than guesses: "Stuck on X. Need your call: [option A] or [option B]? Pausing until you respond."

5. **Tests passing** — "All tests passing. Opening PR now."

6. **PR description** — the full report: what was built, decisions made, anything that deviated from the brief, how to verify, any follow-up tickets suggested.

**Tone:** "Done X, moving to Y." Not "The implementation phase has been completed successfully."

---

## Execution Environment

- **Session manager:** tmux — persistent sessions, SSH-able from any device including phone. Session naming: `mojo-<repo>-<name>` e.g. `mojo-ringmaster-graph`
- **Isolation:** git worktrees at `~/projects/.worktrees/<repo>-<type>-<name>` — one per ticket, independent of the main working tree
- **Notification:** agent comments on issue throughout + opens PR on completion → GitHub notifications to Clarke's phone
- **Phone access:** SSH via Tailscale → `tmux attach -t <session>` to check in on a running agent

**Worktree commands (configured in `~/.bashrc`):**

```bash
# wt-new <type> <name> — from inside the repo
# creates branch <type>/<name> and worktree at ~/projects/.worktrees/<repo>-<type>-<name>
wt-new fix profile-bug       # → fix/profile-bug on .worktrees/ringmaster-fix-profile-bug
wt-new feature graph         # → feature/graph on .worktrees/ringmaster-feature-graph
wt-new chore cleanup         # → chore/cleanup on .worktrees/ringmaster-chore-cleanup

wtl                          # list all active worktrees
wt-clean <type> <name>       # remove worktree and delete branch
```

---

## Comprehension Tiers

- **Architecture and interfaces** — Clarke must own these before any code exists
- **Behavioural contracts / tests** — Clarke owns the contract; implementation follows
- **Implementation details** — review on demand, not a gate; go deep when something feels off or on first pass through a new component

**Spec leads code.** Every piece of code traces back to a decision in `spec/`. Clarke owns the spec. If code exists without a traceable spec decision, that's the accumulation problem in embryo.

---

## Setup

### 1. Base Tools

- VS Code — install, link Claude Code extension, install Pylance extension
- uv — install for your distro/OS (Arch: `pacman -S uv`; other: `curl -LsSf https://astral.sh/uv/install.sh | sh`)
- npm global prefix — set to `~/.local` so globals don't need sudo:

  ```bash
  npm config set prefix ~/.local
  # ensure ~/.local/bin is in $PATH
  ```

- markdownlint-cli2:

  ```bash
  npm install -g markdownlint-cli2
  ```

### 2. SSH + Git + GitHub

- SSH key — generate ed25519, add public key to GitHub:

  ```bash
  ssh-keygen -t ed25519 -C "<machine-name>"
  ```

- git config:

  ```bash
  git config --global user.name "Clarke Hines"
  git config --global user.email "clarkehines@icloud.com"
  ```

- gh CLI — install, authenticate, set SSH protocol, confirm mojo-labs-circus org access:

  ```bash
  gh auth login   # choose GitHub.com → SSH → authenticate
  gh config set git_protocol ssh
  ```

### 3. Claude Config

Copy from an existing machine or set up fresh:

- `~/.claude/CLAUDE.md` — global behavioral rules
- `~/.claude/settings.json` — permissions, deny rules, markdown lint hook
- `~/.claude/rules/guardrails.md` — security and safety rules
- Superpowers plugin — install structured skills (plan, TDD, debug, etc.)

### 4. tmux Config

Create `~/.tmux.conf`:

```
set -g mouse on
set -g status-left "[#S] "
```

### 5. Git Worktree Helpers

Add to `~/.bashrc`:

```bash
wt-new() {
    local type="$1" name="$2"
    if [[ -z "$type" || -z "$name" ]]; then
        echo "Usage: wt-new <type> <name>"
        return 1
    fi
    local repo
    repo=$(basename "$(git rev-parse --show-toplevel 2>/dev/null)")
    if [[ -z "$repo" ]]; then
        echo "Not inside a git repo"
        return 1
    fi
    local branch="${type}/${name}"
    local path="$HOME/projects/.worktrees/${repo}-${type}-${name}"
    git worktree add -b "$branch" "$path"
    echo "Worktree: $path"
    echo "Branch:   $branch"
}

wt-clean() {
    local type="$1" name="$2"
    if [[ -z "$type" || -z "$name" ]]; then
        echo "Usage: wt-clean <type> <name>"
        return 1
    fi
    local repo
    repo=$(basename "$(git rev-parse --show-toplevel 2>/dev/null)")
    if [[ -z "$repo" ]]; then
        echo "Not inside a git repo"
        return 1
    fi
    local branch="${type}/${name}"
    local path="$HOME/projects/.worktrees/${repo}-${type}-${name}"
    git worktree remove "$path" && git branch -d "$branch"
}

wtl() { git worktree list; }
```

### 6. GitHub MCP

Gives agents native tools for Issues and PRs — required for agents to comment, open PRs, and move tickets without shelling out to `gh`.

Two credentials — keep separate:

- `gh auth login` (step 2) — OAuth, not a PAT
- GitHub MCP — needs its own fine-grained PAT scoped to mojo-labs-circus

PAT naming: `mojo-<machine>-mcp` (e.g. `mojo-ringbaker-mcp`). Create at: `https://github.com/settings/personal-access-tokens/new`

PAT permissions: Contents (read), Issues (read/write), Pull requests (read/write), Metadata (read), Commit statuses (read).

```bash
claude mcp add github -s user -e GITHUB_PERSONAL_ACCESS_TOKEN=<token> -- npx -y @modelcontextprotocol/server-github
```

### 7. GitHub Projects Board

Create one org-level board at mojo-labs-circus with columns: Inbox, Backlog, Ready, In Progress, In Review, Done. Add the Issue template from the Ticket System section above.

### 8. Per-Repo CLAUDE.md

Every component repo needs its own CLAUDE.md so each session starts with full context. Do when work begins there, not all at once.

- `ringmaster/` — has one (needs updating for new workflow model)
- `performer/` — doesn't exist yet
- `mojos/` — not done
- `client-web/` — not done
- `client-tui/` — not done
- `mojo-sdk/` — not done

### 9. Pre-Commit Hooks

Per repo, when dev work starts there.

- Python (ringmaster, performer): `ruff` + `pyright`
- TypeScript (client-web, client-tui, mojo-sdk): `eslint` + `tsc --noEmit`
- mojos: `shellcheck`

### 10. CI/CD

Defer. Mk1 goal is getting the system running, not automating deployment.

### 11. Node.js Tooling

Install when a repo actually needs it. `typescript` and `eslint` when starting client-web or client-tui.

---

## What's Done — nomadbaker

- VS Code — installed, Claude Code extension linked
- Pylance — VS Code extension installed
- uv — installed via pacman; ringmaster `.venv` created and synced
- `~/.claude/CLAUDE.md` — global behavioral rules
- `~/.claude/settings.json` — permissions, deny rules, markdown lint hook
- `~/.claude/rules/guardrails.md` — security and safety rules
- Superpowers plugin — structured skills (plan, TDD, debug, etc.)
- markdownlint-cli2 — auto-runs on `.md` file writes
- npm global prefix — `~/.local`, no sudo needed
- SSH key — ed25519, nomadbaker
- git config — user.name and user.email set
- gh CLI — authenticated, SSH protocol, mojo-labs-circus org confirmed
- tmux config
- Git worktree helpers
- GitHub MCP — token: `mojo-nomadbaker-mcp`
