# Filesystem-Scoped Agent Config

CWD is agent context. Navigate to a directory and Jarvis adapts. No manual mode switching.

---

## .jarvis Files

Every directory can have a `.jarvis` file. MojOS pre-populates key system directories at install time — so traversal always finds something from root down.

A `.jarvis` file is YAML frontmatter (structured config) + markdown body (narrative instructions):

```
---
inherit:
  tools: [filesystem, terminal]
  permissions: [read]
local:
  hooks:
    on_enter: ./setup.sh
---

This is the my-app project. Treat all files here as part of a single codebase.
Prefer local reasoning. Ask before running anything destructive.
```

The body is the narrative context Jarvis loads into its prompt. The frontmatter is structured config — tools, permissions, hooks. Both are optional.

---

## Traversal

Jarvis walks root → CWD, loading each `.jarvis` in order. Most-specific scope wins on conflict.

```
/.jarvis
/home/.jarvis
/home/clarke/.jarvis
/home/clarke/projects/.jarvis
/home/clarke/projects/my-app/.jarvis   ← most specific, wins on conflict
```

---

## Composition

Child configs override, extend, or replace anything a parent set. No locks.

```
~/uni-work/.jarvis        →  inherit: { allow_coding: false }
~/uni-work/ai-project/.jarvis  →  inherit: { allow_coding: true }   ← wins
```

---

## Scoping: local vs inherit

| Block | Behaviour |
|---|---|
| `inherit:` | Propagates to all child directories. Picked up during traversal by children. |
| `local:` | Only loaded when operating in this exact directory. Not propagated. |

When Jarvis is at `~/projects/my-app/`:
- Loads `inherit:` blocks from every ancestor
- Loads both blocks from `~/projects/my-app/.jarvis`
- `local:` blocks from ancestors are ignored

---

## Pre-populated Directories

MojOS writes default `.jarvis` files during install to `/.jarvis`, `/etc/.jarvis`, `/home/.jarvis`, and `~/.jarvis`. These provide a baseline for every traversal. User and project `.jarvis` files layer on top.

For content and section names of system files: see [jarvis-system-files.md](jarvis-system-files.md).

---

## Deferred

Settings UI per `.jarvis` level — later Mk.
