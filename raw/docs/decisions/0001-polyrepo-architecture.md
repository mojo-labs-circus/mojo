# Polyrepo Architecture

* Status: accepted
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

The project spans multiple distinct components: spec/planning, AI server backend,
OS install system, local agent, clients, and shared SDK. Should these live in one
monorepo or separate repos?

## Considered Options

* Monorepo — all components in one repo with a shared build system
* Polyrepo — each component in its own repo

## Decision Outcome

Chosen option: **polyrepo**, because each component has a distinct purpose,
independent release cadence, and its own development session context. Coupling them
in a monorepo adds coordination overhead with no benefit at this stage.

### Consequences

* Good: each component repo has its own `CLAUDE.md` and independent Claude Code
  session — component work stays isolated from spec work.
* Good: git history per component is clean and independent.
* Bad: cross-repo references need explicit path conventions (`~/projects/mojo-labs/<name>/`).
* Bad: no shared build tooling — each repo manages its own until it becomes necessary.
