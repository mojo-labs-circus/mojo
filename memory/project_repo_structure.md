---
name: project-repo-structure
description: Decided repo structure under mojo-labs-circus org, and the reasoning behind polyrepo choice
metadata:
  type: project
---

## Decided Structure

Polyrepo under `mojo-labs-circus` GitHub org:

| Repo | Purpose | Status |
|---|---|---|
| `circus` | Spec and planning only (this repo) | live — spec only |
| `ringmaster` | Server/hub code | create |
| `performer` | Local performer agent (was `mojos/agent/`) | create |
| `mojos` | MojOS install system (was `mojos/os/`) | create |
| `client-tui` | TUI thin client | create |
| `dotfiles` | Already exists separately | unchanged |
| `client-web` | Web thin client | hold |
| `mojo-sdk` | Shared API contracts / client lib for Ringmaster | create soon |

Specs live in `circus`. Each component repo gets a `CLAUDE.md` for dev session context, no spec docs.

## Why Polyrepo

Clarke chose polyrepo deliberately. Cross-repo overhead is acceptable because AI assistance reduces solo-dev friction. Polyrepo gives clean deployment boundaries and secrets isolation per component. Not expected to restructure again.

## What Comes After

After repos are in place: plan development phases and decide concrete build steps. The restructure is the prerequisite, not the goal.
