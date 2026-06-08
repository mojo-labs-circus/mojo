# Filesystem-Scoped Agent Config

CWD is agent context. Navigate to a directory and the agent adapts. No manual mode switching.

---

## .mojo Files

Every directory can have a `.mojo` file. MojOS pre-populates key system directories at install time — so traversal always finds something from root down.

A `.mojo` file is YAML frontmatter (structured config) + markdown body (narrative instructions):

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

The body is the narrative context the agent loads into its prompt. The frontmatter is structured config — tools, permissions, hooks. Both are optional.

---

## Traversal

The agent walks root → CWD, loading each `.mojo` in order. Most-specific scope wins on conflict.

```
/.mojo
/home/.mojo
/home/clarke/.mojo
/home/clarke/projects/.mojo
/home/clarke/projects/my-app/.mojo   ← most specific, wins on conflict
```

---

## Composition

Child configs override, extend, or replace anything a parent set. No locks.

```
~/uni-work/.mojo        →  inherit: { allow_coding: false }
~/uni-work/ai-project/.mojo  →  inherit: { allow_coding: true }   ← wins
```

---

## Scoping: local vs inherit

| Block | Behaviour |
|---|---|
| `inherit:` | Propagates to all child directories. Picked up during traversal by children. |
| `local:` | Only loaded when operating in this exact directory. Not propagated. |

When the agent is at `~/projects/my-app/`:
- Loads `inherit:` blocks from every ancestor
- Loads both blocks from `~/projects/my-app/.mojo`
- `local:` blocks from ancestors are ignored

---

## Pre-populated Directories

MojOS writes default `.mojo` files during install to `/.mojo`, `/etc/.mojo`, `/home/.mojo`, and `~/.mojo`. These provide a baseline for every traversal. User and project `.mojo` files layer on top.

For content and section names of system files: see [mojo-system-files.md](mojo-system-files.md).

---

## Deferred

Settings UI per `.mojo` level — later Mk.
