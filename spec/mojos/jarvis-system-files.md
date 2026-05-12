# System-Level .jarvis Files

MojOS pre-populates `.jarvis` files in key system directories at install time. These form the base of every compiled context — user and project files layer on top via section override.

Section names are stable and documented here so users know what they can target for override.

---

## Files and Their Role

| Path | Role |
|---|---|
| `/.jarvis` | Machine-level baseline. Safety rules, base tools, machine identity. |
| `/etc/.jarvis` | System config context. Flags this area as system-managed. |
| `/home/.jarvis` | Minimal pass-through. Scopes Jarvis to user home space. |
| `~/.jarvis` | User-level defaults. Identity, preferences, workflow. Most commonly overridden. |

---

## /.jarvis

```
---
inherit:
  body: extend
---

## Identity
This is a MojOS machine. Jarvis is the local AI agent.
Machine: {HOSTNAME}. Role: {FLEET_PROFILE}.

## Safety
Always ask before:
- Deleting or overwriting files
- Modifying system configuration
- Running commands with sudo or root
- Any action that affects other users

Never run destructive operations without explicit confirmation.

## Tools
Default tools available at this level: filesystem (read-only), terminal (read-only).
Write and execute permissions are granted at lower levels.
```

---

## /etc/.jarvis

```
---
inherit:
  body: extend
---

## Context
System configuration directory. Files here are managed by MojOS and configure.sh.
Manual edits may be overwritten by mojo-update.

## Safety
Do not edit files in /etc directly unless you know what manages them.
Prefer editing .mojo_config and running mojo-update to change system state.
```

---

## /home/.jarvis

```
---
inherit:
  body: extend
---

## Context
User home directories. Each subdirectory belongs to a single user.
```

---

## ~/.jarvis

```
---
inherit:
  body: extend
---

## Identity
User: {SYSTEM_USER}.
Jarvis style: {JARVIS_STYLE}.
Preferred language: {JARVIS_LANGUAGE}.

## Workflow
Default to concise responses.
Prefer explaining what you're about to do before doing it.
Ask once if intent is ambiguous — do not ask repeatedly.

## Tools
filesystem (read/write within home), terminal, editor.

## Privacy
Private memory is stored locally and never forwarded to the ringmaster.
```

---

## Section Names (Stable)

These section names are used across system files and are safe to target for override in user or project `.jarvis` files:

| Section | Defined in |
|---|---|
| `Identity` | `/.jarvis`, `~/.jarvis` |
| `Safety` | `/.jarvis`, `/etc/.jarvis` |
| `Tools` | `/.jarvis`, `~/.jarvis` |
| `Workflow` | `~/.jarvis` |
| `Context` | `/etc/.jarvis`, `/home/.jarvis` |
| `Privacy` | `~/.jarvis` |

Project-level `.jarvis` files can introduce any new sections they need — these are just the reserved system names.

---

## Variable Substitution

System files use `{VAR}` placeholders filled from `.mojo_config` at install time:

| Placeholder | Source |
|---|---|
| `{HOSTNAME}` | `HOSTNAME` |
| `{FLEET_PROFILE}` | `FLEET_PROFILE` |
| `{SYSTEM_USER}` | `SYSTEM_USER` |
| `{JARVIS_STYLE}` | `JARVIS_STYLE` (deferred — fork/identity feature) |
| `{JARVIS_LANGUAGE}` | `JARVIS_LANGUAGE` (deferred — fork/identity feature) |
