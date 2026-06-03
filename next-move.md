# Next Move

## Where We Are

Spec-only repo. All component repos live as siblings under `~/projects/`. Framework planning sessions designed but not yet run. Conventions doc started, constitution and domain pre-session briefs created. No implementation work has begun.

---

## The Sequence

### 1. Obsidian vault setup

Set up a vault spanning all project repos so specs are navigable and cross-references work cleanly across the polyrepo.

### 2. Finish Claude setup

Add coding principles to `~/.claude/CLAUDE.md` and anything else that would improve session quality before the deep framework work begins.

### 3. Framework sessions

Complete sessions 01–08 plus two new sessions: one for the constitutional contract (how any collective declares itself in the framework) and one for domains (how collectives specialise into predefined categories — software dev, research, company, etc.). Produces `spec/framework/framework.md`.

Pre-session briefs: `spec/framework/constitution.md`, `spec/framework/domains.md`.

### 4. Spec cleanup

Move current `spec/` to `old-spec/`. Use it as reference while building the new framework-grounded structure. Delete when done.

### 5. Framework setup

Constitute the mojo-labs-circus collective using the framework — charter, intelligence definitions, decision records, protocols, goals. Done naively in Claude Code since the mojo-agent doesn't exist yet. This hand-built instance becomes the reference implementation for what the mojo-agent will eventually automate.

### 6. Component speccing

Spec every component from first principles under the new framework: mojo-agent, mojo-sdk, ringmaster, performer, clients, mojos. Each component repo gets its own spec directory.

### 7. PRD and implementation plan

Define what the system does from a user perspective and how it gets built. May emerge partially from component speccing depending on what the dev domain framework produces.

### 8. Build the pitch

Reveal.js presentation with prototype mockups showing what a mojo agent looks and feels like in use. Talking points for presenting in person. Audience: family, potential collaborators, anyone who has never heard of this and needs to understand why it is a good idea.

### 9. Full roadmap

Decide which components get built first and why. The sequencing and prioritisation decision, using the implementation plan as input.

### 10. Build

Reassess when we get here.
