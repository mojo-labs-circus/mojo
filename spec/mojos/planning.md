# MojOS — Planning Notes

> Design decisions made, not yet implemented. This is the thinking, not the spec.

---

## The Mission

One agent. Knows you. Works everywhere. Adapts to whatever hardware you have.

Three things the agent must carry across all surfaces:
1. **Identity** — personality, name, preferences. Identical everywhere, always.
2. **Memory** — what the agent knows.
3. **Tasks** — active work, open loops. Small data, high value.

**Private memory never leaves the device it lives on. Ever.**

---

## Two Modes

**MojOS Agentic OS** — mojo-agent running as part of your OS on a machine. Full local stack. Works standalone (Solo) or as a client to a MojOS Server. This is the primary MojOS experience.

**MojOS Server** — Ringmaster running headless as a hub. Always on, always reachable. The source of truth for identity, memory, and tasks. Not every node in a circus will be running MojOS.

---

## Client Topology

Not every circus member runs MojOS.

**Non-MojOS clients** (web, mobile, app, any thin client) → direct to the Ringmaster on server via `/ws/chat`.

**MojOS Agentic OS** → mojo-agent proxy → server (when needed).

mojo-agent is a **proxy**, not a parallel surface. Every request from a client on a MojOS machine hits mojo-agent first. mojo-agent decides: handle it here, or forward to server. The client always talks to `localhost` — it never knows or cares whether the response came from local or server.

What the proxy enables:
- **Local context injection** — machine state, open apps, OS info bundled into forwarded requests
- **Privacy filtering** — private memory stays local, never forwarded
- **Offline resilience** — mojo-agent handles what it can; server requests queue until reachable
- **OS-level answers without a round trip** — anything about this machine is answered locally

---

## Solo

No server in the picture. MojOS Agentic OS on a single machine. Full local agent experience. Nothing to sync, nothing to register with. Complete as-is.

Solo is the **default state** for any MojOS install on a non-server chassis. There is one version of MojOS — not separate solo/circus editions.

---

## Circus Formation

**Install model:**
- Non-server chassis (desktop, laptop): installs as Solo. Becomes a performer only after explicitly joining a circus.
- Server chassis: automatically configures as a ringmaster on install. Docker containers spin up, the Ringmaster starts, the circus is live. No manual setup step required.

**Joining a circus (`mojo-circus-join`):**
1. Connects the machine to that circus's Tailscale tailnet
2. Authenticates with the user's Ringmaster credentials for that circus

Users get credentials by registering with an invite token. The admin on a ringmaster generates invite tokens and distributes them. Users register via any client or directly on a MojOS machine.

**Each circus is fully self-contained:** its own Tailscale tailnet, its own ringmaster, its own Ringmaster auth server. MojOS is a framework — anyone can run their own circus on their own tailnet.

**Multi-circus:**
A performer can join multiple circuses. Each circus is fully independent. The local vault is namespaced by circus: `vault/{circus_id}/...`. Memory, tasks, and routing context are all circus-scoped.

**Active circus:**
One active circus at a time. Stored as a simple local config value (`active_circus_id`). Every local agent operation — vault reads/writes, routing decisions, sync — is scoped to the active circus. The agent never silently routes across circus boundaries. When offline from the active ringmaster, local cache for that circus serves what it has; no fallback to another circus's data.

This is the "your data, your control" principle: you always know which circus is active and data never crosses a boundary without explicit intent.

---

## MojOS Agentic OS Stack

Every MojOS machine runs a complete local agent stack:
- Local LangGraph graph
- Local Ollama
- Local vault
- Local vector store (details TBD)
- Local SQLite (history, tasks, auth cache)
- Local API endpoint (for native OS app, TUI, local browser clients)

---

## Filesystem-Scoped Agent Config

> The filesystem is the primary interface of the OS. The agent's behavior is defined by where you are in it.

**Where you are is how the agent works.**

In Unix, your CWD is your working context — everything you do is relative to it. MojOS extends this: your CWD is also your agent context. Navigate to a directory and the agent adapts automatically. No manual mode switching, no explicit "switch context" command. The OS and the agent are unified at the filesystem level.

This is what "agentic OS" means in practice — mojo-agent isn't a layer on top of the OS, it's woven into it.

---

### `.mojo/` directories

Any directory can contain a `.mojo/` directory that configures how the agent behaves when operating there. MojOS ships with sensible defaults at the system and user levels. Projects and directories layer on top.

Contents of a `.mojo/` directory:
- `instructions.md` — narrative context: what is this directory, how should the agent behave here
- `config` — tools, permissions, hooks

---

### Traversal

When the agent is operating in a directory, it walks root → CWD, loading each `.mojo/` config in order. Most-specific scope wins on conflict.

---

### Composition (Python-style inheritance)

Child configs can override, extend, or replace anything a parent set. No locks. A parent that disables a capability can be explicitly re-enabled by a child.

```
~/uni-work/.mojo/config             →  allow_coding: false
~/uni-work/ai-project/.mojo/config  →  allow_coding: true   ← wins
```

---

### `local` vs `inherit` scoping

Context management is baked in. Each config block is explicitly scoped:

- **`local:`** — only loaded when operating in this exact directory. Does not propagate to children.
- **`inherit:`** — propagates to all child directories. Picked up during traversal.

When the agent is at `~/uni-work/ai-project/`:
- Loads `inherit:` blocks from every ancestor (identity, global rules, workspace prefs)
- Loads the full config (both blocks) for the current directory
- `local:` blocks from ancestors are not loaded — they stay scoped to their own level

This keeps context tight. Rules and context that are irrelevant to the current directory never enter the context window.

---

### Deferred

- **Settings UI** — each `.mojo/` level will have a visual settings page. Later Mk.

---

## Circus-Aware Routing (Server mode)

The ROUTER node knows the circus state and routes accordingly.

```
Always local (Agentic OS):
  OS management (it's THIS machine)
  Private memory (never leaves the node)
  Quick conversational responses

Route to server (when reachable):
  Complex reasoning / long context
  Shared memory
  Web search
  Multi-device tasks

Always local regardless:
  Everything when offline
  Private scope, always
```

---

## Multi-User (Server mode)

Each user gets their own isolated agent — separate identity, separate memory, separate private data. Shared infrastructure.

Memory scopes:
- **Personal memory** (`memory_{user_id}`) — fully isolated per user
- **Shared family memory** (`memory_shared`) — readable by all, classifier auto-routes

User tiers: admin / power / standard — safety boundary, not a feature paywall.

---

## Deferred

- Fleet sync between MojOS nodes (multi-performer topology, outbox, sync protocol) — revisit in a later Mk
- Multiple servers / distributed stack
- Local vector store specifics
- Privacy control layer UX
- Local memory scope and curation details

---

## Install System Design

> Decisions locked in during planning session — 2026-05-08/09

### Build Order

Build base MojOS first — no fork/personalisation layer yet. The baker fork comes after MojOS is solid and dogfooded on real machines. Designing the fork feature from experience rather than speculation.

Fork feature gets added later as:
1. MojOS ships as a complete base OS
2. Clarke installs it, lives with it, finds what's missing
3. Baker fork built — Clarke becomes the first user of the fork feature
4. Fork support added to MojOS based on what was actually needed


---

### The Full Script Flow

```
bootstrap.sh                  ← live ISO entry point, single URL
  └── install.sh              ← fully unattended after bootstrap hands off
        └── configure.sh      ← chroot config, then again on every mojo-update
              │
           [reboot]
              │
        post-install.sh       ← shared base (packages, dotfiles, user services)
              └── post-install/
                    standalone.sh   ← local Ollama + full local agent stack
                    hybrid.sh       ← Tailscale install, mojo-agent config
                    server.sh       ← Postgres, full Ringmaster stack, fleet registry
              │
           [reboot]  ← hybrid and server only
              │
        post-reboot/
              hybrid.sh       ← Tailscale auth, mojo-agent registration
              server.sh       ← Tailscale auth, Ringmaster verify, fleet registry init
              │
           [system running]
              │
        mojo-update           ← run any time to converge to desired state
```

Post-reboot scripts only exist for `hybrid` and `server` profiles. Standalone users are done after the first reboot.


---

### `bootstrap.sh`

Single entry point. Runs from the live distro ISO. Owns all environment discovery and interactive prompts — install.sh never asks the user anything.

**Responsibilities:**
1. Detect `DISTRO` from `/etc/os-release` — set once, everything downstream reads this variable
2. Silent hardware detection: GPU, CPU, chassis type → profile, timezone, dual boot
3. All interactive prompts: hostname, username, locale, keymap, disk, dual boot, HDD, LUKS, passwords, fleet profile
4. Summary + final confirm before anything destructive
5. Write `.mojo_config` to `/tmp/mojos/.mojo_config` with everything known at this point
6. `export LUKS_PASS ROOT_PASSWORD USER_PASSWORD` — passwords only, never written to disk
7. Clone MojOS repo to `/tmp/mojos`
8. `exec install.sh`

**Key decision — `.mojo_config` written in bootstrap:**
Bootstrap writes the config file rather than exporting a wall of variables. install.sh sources the file. configure.sh always reads from the file. One consistent mechanism throughout. Passwords are the only exception — too sensitive to write anywhere.


---

### `.mojo_config`

Single source of truth for everything about the machine. Written by bootstrap, extended by later scripts, read by configure.sh and mojo-agent.

Fields split into two roles:

**User-editable desired state** — configure.sh reads these and applies them. The file IS the source of truth:
- `TIMEZONE`, `LOCALE`, `KEYMAP`, `HOSTNAME`, `DOTFILES_URL`, `GRUB_TIMEOUT`, `FLEET_PROFILE`

**Detected state snapshot** — configurators re-detect from live system, write back as a cache. The file is informational here, not authoritative:
- `GPU`, `CPU`, `LUKS`, `LUKS_UUID`, `DISK`, `DUAL_BOOT`, `HDD`, `HDD_MOUNT`, `PROFILE`

Eventually `.mojo_config` becomes the data source for a settings GUI — mojo-agent reads it, presents it visually, user changes values, mojo-agent writes back and triggers `mojo-update` automatically.

**Full schema:**
```bash
# --- Install ---
DISTRO=                 # arch | ubuntu | debian | fedora
DISK=                   # /dev/nvme0n1

# --- System (editable) ---
HOSTNAME=
SYSTEM_USER=
TIMEZONE=
LOCALE=
KEYMAP=
DOTFILES_URL=
GRUB_TIMEOUT=

# --- Fleet (editable) ---
FLEET_PROFILE=          # standalone | hybrid | server

# --- Hardware (auto-detected, do not edit) ---
PROFILE=                # workstation | laptop | server
CPU=                    # intel | amd
UCODE=                  # intel-ucode | amd-ucode
GPU=                    # nvidia | amd | intel | none
LUKS=                   # true | false
LUKS_UUID=
DUAL_BOOT=              # true | false
HDD=                    # true | false
HDD_MOUNT=

# --- AI (set by post-install, updated by mojo-update) ---
AI_SERVER=              # ringbaker tailscale hostname (hybrid/server only)
AI_PORT=
AGENT_TOKEN=            # machine JWT, set at fleet registration

# --- Identity (set by mojo-init — deferred to fork feature) ---
MOJO_USER=
OS_NAME=
REPO_SLUG=
FORK_DIR=

# --- Temporary (stripped after first boot) ---
WIFI_SSID=
WIFI_PASSWORD=
```


---

### `install.sh`

Fully unattended. Sources `.mojo_config`, inherits password exports from bootstrap. Never prompts.

1. Partition + format disk
2. LUKS setup if enabled
3. Base OS install — distro-dispatched:
   - `arch` → `pacstrap`
   - `ubuntu`/`debian` → `debootstrap`
   - `fedora` → `dnf --installroot`
4. fstab generation
5. Copy `.mojo_config` to `/mnt/home/$SYSTEM_USER/.mojo_config`
6. Chroot → `configure.sh` (distro-dispatched: `arch-chroot` vs `chroot`)
7. Inside chroot: user creation, bootloader, service enables (distro-dispatched)
8. Fill in `LUKS_UUID` in `.mojo_config` after formatting
9. Copy `post-install.sh` + `post-install/` to new system
10. Copy `post-reboot/` to new system (only if `FLEET_PROFILE != standalone`)
11. Reboot


---

### `configure.sh` (orchestrator)

Idempotent. Called by install.sh inside the chroot and by mojo-update on the live system. Always reads `.mojo_config` — no dual env-var mode.

Each individual configurator:
1. Re-detects current state from live system
2. Diffs against `.mojo_config` desired state
3. Reconciles — removes stale config, applies new config, only if something changed

Configurators (in dependency order):
```
configure/timezone.sh
configure/locale.sh
configure/hostname.sh
configure/sudo.sh
configure/mirrors.sh
configure/cpu.sh
configure/gpu.sh
configure/initramfs.sh      ← depends on gpu.sh
configure/bootloader.sh     ← depends on initramfs.sh
configure/display.sh
configure/input.sh
configure/audio.sh
configure/storage.sh
configure/network.sh
configure/bluetooth.sh
configure/power.sh
configure/sshd.sh
configure/firewall.sh
configure/services.sh
configure/fleet.sh
configure/shell.sh
```

See `temp_spec/configure-plan.md` for full per-configurator detail.

**Chroot vs live system:** each configurator handles both contexts. `systemctl` calls are silent in chroot (systemd not running). File writes handle the chroot case; `systemctl` handles the live case.


---

### `post-install.sh` (shared base)

Run as regular user after first boot into the desktop. Handles everything every machine needs regardless of profile.

1. Source `.mojo_config`
2. Network check — auto-wifi on laptop using saved credentials, strips them from `.mojo_config` after
3. Home directories + XDG config
4. Distro-dispatched package helper (Arch: install `yay`, others: N/A)
5. AUR packages from `packages/base.txt` (Arch only)
6. Flatpak remote + packages from `packages/base.txt`
7. User services (`pipewire`, `wireplumber`, `pipewire-pulse`)
8. Optional dotfiles — prompt for URL, clone + run dotfiles install script
9. SSH keypair generation
10. Dispatch to profile script: `bash post-install/$FLEET_PROFILE.sh`
11. Self-delete, reboot

**`post-install/standalone.sh`:**
- Install Ollama
- Pull models based on hardware tier
- Configure mojo-agent
- Install mojo-agent as systemd service (local-only mode)
- No post-reboot needed — done after reboot

**`post-install/hybrid.sh`:**
- Install Tailscale
- Configure mojo-agent pointing at `AI_SERVER`
- Stage `post-reboot/hybrid.sh` in home dir
- Note: Tailscale auth deferred to post-reboot (needs running tailscaled + browser)

**`post-install/server.sh`:**
- Install + initialise Postgres
- Install ChromaDB
- Install Ollama + full model stack
- Install Ringmaster services
- Configure fleet registry schema
- Stage `post-reboot/server.sh` in home dir


---

### `post-reboot/hybrid.sh`

Only exists for `FLEET_PROFILE=hybrid`. Staged by post-install/hybrid.sh.

1. Source `.mojo_config`
2. `tailscale up` → browser auth flow
3. Wait for Tailscale connection to confirm
4. Verify `AI_SERVER` is reachable over Tailscale
5. mojo-agent registers with fleet server, receives `AGENT_TOKEN`
6. Write `AGENT_TOKEN` to `.mojo_config`
7. Verify mojo-agent ↔ ringbaker connection is live
8. Self-delete

### `post-reboot/server.sh`

Only exists for `FLEET_PROFILE=server`.

1. Source `.mojo_config`
2. `tailscale up` → browser auth flow
3. Verify Postgres is running, fleet registry schema exists
4. Start Ringmaster services, verify healthy
5. Generate initial `FLEET_KEY` (join code for new machines)
6. Self-delete


---

### `upgrade.sh` (`mojo-update`)

Convergence script. Safe to run any time. Brings the machine to current desired state.

1. Pull latest mojos repo
2. System upgrade — distro-dispatched (`pacman -Syu` / `apt upgrade` / `dnf upgrade`)
3. Install missing packages from manifests `--needed`
4. AUR packages (Arch only)
5. Flatpak packages
6. Pull dotfiles + re-symlink (if `DOTFILES_URL` set)
7. Update mojo-agent — pull latest, restart service
8. Re-detect hardware → rewrite hardware section of `.mojo_config`
9. Run `configure.sh`


---

### Multi-Distro Strategy

`DISTRO` is detected once in bootstrap from `/etc/os-release $ID`. Every script that needs to branch on distro uses a `case "$DISTRO" in` block at the relevant section rather than separate per-distro scripts.

Sections that need distro dispatch:
- Package manager commands (`pacman` / `apt` / `dnf`)
- Package names (differ across distros)
- Base OS install method (`pacstrap` / `debootstrap` / `dnf --installroot`)
- Chroot tool (`arch-chroot` / `chroot`)
- Postgres init (Arch: manual `initdb`, Debian: auto, Fedora: `postgresql-setup --initdb`)
- Initramfs (`mkinitcpio` / `update-initramfs` / `dracut`)
- Firewall (`ufw` / `firewalld`)

Everything that doesn't differ stays distro-agnostic — most of the install is universal.
