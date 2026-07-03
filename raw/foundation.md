# Foundation

## Goal

Restructure the project knowledge base from scratch. The existing docs were moved into
`raw/` to clear the slate. The task is to migrate that content into the right homes
across the component repos, with the right structure and format — serving two distinct
needs:

**Working layer** — AI sessions and Clarke navigating the project while building. The
docs need to hold the complete picture of the project with minimal token overhead. A
session should be able to open a repo cold and immediately understand what it is, what
phase it's in, and what needs building — without reading everything. Cross-repo
navigation should be efficient. This is what the tooling and graph research is about.

**Public layer** — contributors and recruiters landing on the repos. Docs need to read
well on GitHub, demonstrate strong systems thinking, and make it easy for someone new
to understand the project and how to contribute.

---

## Current state

Everything from the old `docs/` structure now lives in `raw/docs/`. The real `docs/`
is nearly empty. The ringmaster repo is archived and its architecture thinking needs
distributing before it can be left alone.

---

## Active repos and their roles

| Repo | Role |
|---|---|
| `~/projects/mojo/` | Org hub — vision, phases, framework design, org-level decisions |
| `~/projects/mojos/` | MojOS — the OS layer (Phase 1) |
| `~/projects/mojo-agent/` | The AI agent (Phase 1 onwards) |
| `~/projects/dotfiles/` | System config and aesthetic |
| `~/projects/ringmaster/` | Archived — useful thinking to distribute, then leave it |

---

## The three phases

**Phase 1 — Personal AI + OS**
MojOS and mojo-agent together. One machine, one person, fully working.

**Phase 2 — Fleet**
The same agent across all your machines. Topology undecided — designed once Phase 1 is solid.

**Phase 3 — Collectives**
Formal collectives of humans and agents. Deprioritised.

---

## What needs to happen first

Before any content moves, decide how to structure it correctly. The core question is:
what tools, conventions, and doc format let an AI session hold the complete picture of
a polyrepo project with minimal token overhead — navigating across repos efficiently
without reading everything?

Research:

- What tools exist for this (lat.md, Loom, Obsidian, MCP-based options) — what each
  actually does, what constraints it imposes on doc format
- What frontmatter fields genuinely help AI sessions vs. overhead
- How cross-repo references should work in a polyrepo setup
- Whether the docs-as-working-layer and docs-as-public-face can share the same format,
  or need separate layers
- What conventions make GitHub rendering good for contributors and recruiters without
  compromising the working layer

The answers gate the migration. Don't move content until the format is decided.
