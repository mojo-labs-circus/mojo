# .mojo_config

Single source of truth for both the OS layer and the AI layer. A bash-sourceable key=value file at `~/.mojo_config`.

Read by: `configure.sh`, `mojo-update`, `mojo-agent`, all post-install scripts.
Never prompted for after bootstrap — all values come from bootstrap interaction or auto-detection.

---

## Field Roles

Two categories:

**Desired state** — user-editable. `configure.sh` reads these and applies them. Changing a value + running `mojo-update` converges the machine to the new state.

**Detected state** — auto-written. Configurators re-detect from the live system and write back as a cache. Do not edit these manually — they will be overwritten.

---

## Schema

```bash
# --- Install (written by bootstrap, do not edit) ---
DISTRO=         # arch | ubuntu | debian | fedora
DISK=           # e.g. /dev/nvme0n1

# --- System (desired state — editable) ---
HOSTNAME=
SYSTEM_USER=
TIMEZONE=
LOCALE=
KEYMAP=
DOTFILES_URL=
GRUB_TIMEOUT=

# --- Fleet (desired state — editable) ---
FLEET_PROFILE=  # standalone | hybrid | server
                # standalone = Solo, hybrid = Performer, server = Ringmaster
                # see topology.md

# --- Hardware (detected state — do not edit) ---
PROFILE=        # workstation | laptop | server  (chassis type)
CPU=            # intel | amd
UCODE=          # intel-ucode | amd-ucode
GPU=            # nvidia | amd | intel | none
LUKS=           # true | false
LUKS_UUID=
DUAL_BOOT=      # true | false
HDD=            # true | false
HDD_MOUNT=

# --- AI (set by post-install, updated by mojo-update) ---
AI_SERVER=      # ringmaster tailscale hostname (hybrid/server only)
AI_PORT=
AGENT_TOKEN=    # machine JWT, written at circus registration

# --- Identity (deferred — set by mojo-init, fork feature) ---
MOJO_USER=
OS_NAME=
REPO_SLUG=
FORK_DIR=

# --- Temporary (stripped after first boot) ---
WIFI_SSID=
WIFI_PASSWORD=
```

---

## Who Writes What

| Script | Writes |
|---|---|
| `bootstrap.sh` | All fields except hardware cache and AI fields. Passwords exported only — never written. |
| `install.sh` | `LUKS_UUID` (after disk formatting) |
| `post-install/*.sh` | `AI_SERVER`, `AI_PORT` |
| `post-reboot/hybrid.sh` | `AGENT_TOKEN` |
| `mojo-update` | Re-detects and rewrites all hardware fields |
| `configure.sh` | Reads only — never writes |

---

## Future

Will become the data source for a settings UI. Jarvis reads it, presents values visually, user edits, Jarvis writes back and triggers `mojo-update`. See `ideas/` for fork/identity fields (`MOJO_USER`, `OS_NAME`, etc.).
