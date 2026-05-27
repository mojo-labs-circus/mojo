# Install System

Full install flow from live ISO to running system. See [mojo-config.md](mojo-config.md) for the `.mojo_config` schema. See [configure.md](configure.md) for configure.sh detail.

---

## Script Flow

```
bootstrap.sh          ← live ISO entry point, single URL
  └── install.sh      ← fully unattended after bootstrap
        └── configure.sh  ← chroot config; also runs on every mojo-update
              [reboot]
        post-install.sh   ← shared base (packages, dotfiles, user services)
              └── post-install/
                    standalone.sh
                    hybrid.sh
                    server.sh
              [reboot]    ← hybrid and server only
        post-reboot/
              hybrid.sh
              server.sh
              [system running]
        mojo-update       ← run any time to converge to desired state
```

---

## bootstrap.sh

Entry point from live ISO. All interactive prompts happen here. Nothing downstream ever prompts the user.

1. Detect `DISTRO` from `/etc/os-release` — set once, all scripts read this var
2. Silent hardware detection: GPU, CPU, chassis type → `PROFILE`, `FLEET_PROFILE`, timezone hint, dual boot
3. Interactive prompts: hostname, username, locale, keymap, disk, dual boot, HDD, LUKS, passwords, fleet profile
4. Summary + final confirm before any destructive action
5. Write `.mojo_config` to `/tmp/mojos/.mojo_config`
6. `export LUKS_PASS ROOT_PASSWORD USER_PASSWORD` — passwords only, never written to disk
7. Clone mojos repo to `/tmp/mojos`
8. `exec install.sh`

---

## install.sh

Fully unattended. Sources `.mojo_config`, inherits password exports. Never prompts.

1. Partition + format disk
2. LUKS setup if `LUKS=true`
3. Base OS install (distro-dispatched — see Multi-Distro below)
4. Generate fstab
5. Fill in `LUKS_UUID` in `.mojo_config`
6. Copy `.mojo_config` to `/mnt/home/$SYSTEM_USER/.mojo_config`
7. Chroot → `configure.sh` (distro-dispatched: `arch-chroot` vs `chroot`)
8. Copy `post-install.sh` + `post-install/` to new system
9. Copy `post-reboot/` to new system (hybrid and server only)
10. Reboot

---

## post-install.sh

Runs as regular user after first boot into the desktop. Shared base for all profiles.

1. Source `.mojo_config`
2. Network check — auto-wifi on laptop using saved credentials, strips them from `.mojo_config` after
3. Home directories + XDG config
4. Distro-dispatched package helper (Arch: install `yay`)
5. AUR packages from `packages/base.txt` (Arch only)
6. Flatpak remote + packages
7. User services: `pipewire`, `wireplumber`, `pipewire-pulse`
8. Optional dotfiles: prompt for URL, clone + run install script
9. SSH keypair generation
10. Dispatch to `post-install/$FLEET_PROFILE.sh`
11. Self-delete, reboot

**post-install/standalone.sh**
- Install Ollama, pull models based on hardware tier
- Configure and install `mojo-agent` as systemd service (local-only mode)
- No post-reboot needed

**post-install/hybrid.sh**
- Install Tailscale
- Configure `mojo-agent` pointing at `AI_SERVER`
- Stage `post-reboot/hybrid.sh`

**post-install/server.sh**
- Install + initialise Postgres
- Install ChromaDB, Ollama + full model stack
- Install Ringmaster services
- Configure fleet registry schema
- Stage `post-reboot/server.sh`

---

## post-reboot/hybrid.sh

1. `tailscale up` → browser auth
2. Wait for Tailscale connection
3. Verify `AI_SERVER` reachable over Tailscale
4. `mojo-agent` registers with ringmaster, receives `AGENT_TOKEN`
5. Write `AGENT_TOKEN` to `.mojo_config`
6. Verify `mojo-agent` ↔ ringmaster connection live
7. Self-delete

## post-reboot/server.sh

1. `tailscale up` → browser auth
2. Verify Postgres running, fleet registry schema exists
3. Start Ringmaster services, verify healthy
4. Generate initial `FLEET_KEY` (join code for new machines)
5. Self-delete

---

## mojo-update

Convergence script. Safe to run any time.

1. Pull latest mojos repo
2. System upgrade (distro-dispatched)
3. Install missing packages from manifests (`--needed`)
4. AUR packages (Arch only)
5. Flatpak packages
6. Pull dotfiles + re-symlink (if `DOTFILES_URL` set)
7. Update `mojo-agent` — pull latest, restart service
8. Re-detect hardware → rewrite hardware fields in `.mojo_config`
9. Run `configure.sh`

> **Mk1: Arch only.** Multi-distro support is deferred. See `ideas/multi-distro.md`.
