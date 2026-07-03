---

kanban-plugin: board

---

## Done

- [x] mojo-labs workspace structure (repos moved to ~/projects/mojo-labs/)
- [x] docs/ directory structure (spec/ → docs/ with typed subdirectories)
- [x] ADRs established (0001–0006, 0008 + template)
- [x] mojo-framework-dev-team collective stub
- [x] undeveloped-thoughts.md tagged with session references
- [x] Kanban tracking set up (this board)
- [x] Claude Code setup (global CLAUDE.md, guardrails)
- [x] Configure Claude Code statusline
- [x] Shape global Claude setup to embody the Mojo framework principles — quality over speed, Clarke as lead, Claude as worker, accountability, honest pressure-testing, mutual probing of ideas
- [x] Dotfiles moved to ~/projects/mojo-labs/dotfiles/, transferred to mojo-labs-circus org, symlinks updated
- [x] Review and tighten global Claude config (CLAUDE.md, guardrails.md, settings.json) — fix the gh vs MCP conflict, anything stale
- [x] Install Obsidian and open mojo-labs vault
- [x] Open Obsidian vault at ~/projects/mojo-labs/ and install plugins: Kanban, Dataview, Templater, Hidden Folder

## In Progress

## Blocked

- [ ] Obsidian vault clutter — Hidden Folder plugin installed but can't right-click to hide folders (blocked by trackpad issue)
- [ ] Fix trackpad two-finger tap (right-click) — device block added to hyprland.conf but not working yet

## Backlog

- [ ] Plan actual next steps — shift from framework sessions to building; define performer v1 scope, build order, and what crude-but-working looks like for mojo agent + mojos installer
- [ ] Audit and clean up Claude memory files — too much is being saved there that should live in docs
- [ ] Research: project tracking, docs structure, and how software dev teams work — how do we want to run this project before we build it
- [ ] Install Whisper Flow (voice dictation)
- [ ] Session 00B — blockchain / decentralisation research
- [ ] Session 01 — first principles
- [ ] ADR 0009 — standard/mechanism/policy architecture philosophy (write after session 01)
- [ ] Session 02 — agent model
- [ ] Session 03 — frontier interface
- [ ] Session 04 — org structure
- [ ] Session supplement — constitution
- [ ] Session 05 — communication model + KO design
- [ ] ADRs — KO primitive, member-tagged KOs, sender/receiver decomposition, brief as renderer, capture skill (write after session 05)
- [ ] Session 06 — leadership interface
- [ ] Session 07 — layers and contracts
- [ ] Session supplement — domains
- [ ] Session 08 — dev team instance
- [ ] Session supplement — skill system (skill as framework primitive, contract design)
- [ ] Framework design: vault hierarchy and collective memory composition — how sub-collective vaults compose into parent collectives, shared memory boundaries, cross-collective linking
- [ ] Post-session synthesis — draw the kernel boundary, design the integration/adapter model (how tooling plugs into the collective), specify what Mojo Circus implements
- [ ] Spec cleanup (move old architecture docs to component repos)
- [ ] Constitute mojo-framework-dev-team formally (using the framework)
- [ ] Capture skill — design + implement (~/.claude/skills/capture/)
- [ ] Brief skills — design + implement (brief, brief-adapt, brief-renderer)
- [ ] Presentation skills — design + implement (presentation, presentation-adapt, presentation-renderer, presentation-script)
- [ ] Delete old brief/presentation skills (11 directories in ~/.claude/skills/)
- [ ] Stance.md — redo as captured KO rendered as brief (after capture + brief skills)
- [ ] Component speccing (ringmaster, performer, mojos, sdk, clients)
- [ ] PRD and implementation plan
- [ ] Build the pitch (use presentation skill)
- [ ] Full roadmap
- [ ] Build
- [ ] Mojo language — formal specification research (Dijkstra + Design by Contract applied to AI)

%% kanban:settings

```
{"kanban-plugin":"board","list-collapse":[false,false,false,false]}
```

%%
