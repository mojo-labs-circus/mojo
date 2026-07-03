# mojo-labs Workspace Structure

* Status: accepted
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

All mojo-related repos lived as siblings in `~/projects/` alongside unrelated
projects (dotfiles, internship, etc.). This made it unclear which repos belong to
the mojo-labs collective, and any Obsidian vault pointing at `~/projects/` would
include unrelated content.

## Considered Options

* Keep all repos as siblings in `~/projects/` — simple, no migration
* Move mojo repos into a dedicated `~/projects/mojo-labs/` workspace directory

## Decision Outcome

Chosen option: **dedicated `~/projects/mojo-labs/` workspace**, because it gives
the collective a clear boundary on disk, scopes the Obsidian vault cleanly, and
makes `ls ~/projects/mojo-labs/` a complete picture of the project.

### Consequences

* Good: Obsidian vault at `~/projects/mojo-labs/` covers exactly the mojo repos and
  nothing else.
* Good: new contributors (human or AI) understand the project boundary immediately.
* Bad: path references in docs and configs needed updating from `~/projects/<name>/`
  to `~/projects/mojo-labs/<name>/`.
* Bad: non-mojo repos (`mojo-fund`, etc.) stay outside and must be accessed separately.
