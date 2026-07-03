# Current Session Work

Claude Code setup — research phase. Next session: execute the research brief.

---

## Queue

- [ ] **Run research session** — open `research/claude-code-setup.md` in a fresh session and execute the full agenda. Output: structured synthesis document. After that: produce a generic best-practices doc, then apply it.

- [ ] **Claude Code cleanup** — blocked on research. Once findings are in, do in order: global CLAUDE.md, global memory, mojo-labs/CLAUDE.md, mojo/CLAUDE.md + memory, wipe dotfiles/ and ringmaster/ CLAUDE.mds.

- [ ] **Repeated workflows → automation** — identify what we do every session and set up hooks or scripts. Revisit after research findings on hooks.

- [ ] **last30days repo (Reddit)** — someone on Reddit built a repo around 30-day challenges or last-30-days logging. Investigate and see if anything is worth adopting or adapting.

- [ ] **anything repo** — unclear reference, needs clarification from Clarke.

- [ ] **opensrc by Vercel** — investigate Vercel's open source tooling/repo. See if anything useful for the setup or Mojo dev workflow.

---

## Done This Session

- [x] Researched rtk-ai/rtk — CLI proxy that compresses command output before it hits LLM context (60-90% token savings via PreToolUse hook)
- [x] Installed rtk v0.42.4 to `~/.local/bin/`
- [x] Ran `rtk init --global` — wired PreToolUse hook into `~/.claude/settings.json`, wrote `~/.claude/RTK.md`
- [x] Added `Bash(rtk *)` to global allow list
- [x] Read global `~/.claude/` structure and all component CLAUDE.mds + mojo-labs-mojo memory — inventory only, no changes made
- [x] First-pass research on Claude Code best practices (Boris Cherny, Karpathy, Anthropic docs, hooks, MCP, memory vs docs tradeoff)
- [x] Settled architecture: CLAUDE.md hierarchy is the primary context vehicle (not memory), docs not memory for intentional project knowledge, memory for runtime corrections only
- [x] Wrote `research/claude-code-setup.md` — 9-topic research brief for a dedicated research session covering file structure, ways of working, hooks (team-focused), MCP, commands/subagents, settings, cost/token management, GitHub integration, testing/verification
- [x] Deferred Obsidian/knowledge base to a separate tooling session
