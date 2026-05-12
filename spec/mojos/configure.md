# configure.sh

Convergence engine for system state. Called by `install.sh` (inside chroot) and by `mojo-update` (live system). Same behaviour in both contexts. Safe to run at any time.

Reads `.mojo_config` as desired state. Never prompts. See [mojo-config.md](mojo-config.md) for schema.

---

## Pattern

Every configurator follows:
1. Re-detect current state from live system
2. Diff against `.mojo_config` desired state
3. Reconcile — remove stale config, apply new, only if changed

---

## Execution Context

Each configurator must handle both contexts:

| Context | Notes |
|---|---|
| Chroot (install time) | `systemctl` fails silently — file writes handle config. No network. `.mojo_config` passed as arg. |
| Live system (mojo-update) | `systemctl` works normally. Network available. `.mojo_config` at `~/.mojo_config`. |

Pattern:
```bash
[[ -z "$HOSTNAME" ]] && source "${MOJO_CONFIG:-$HOME/.mojo_config}"
systemctl enable foo 2>/dev/null || true
```

---

## Orchestrator

`configure.sh` routes to configurators in dependency order. Contains no configuration logic itself.

```
configure/timezone.sh
configure/locale.sh
configure/hostname.sh
configure/sudo.sh
configure/mirrors.sh
configure/cpu.sh
configure/gpu.sh
configure/initramfs.sh    ← depends on gpu.sh
configure/bootloader.sh   ← depends on initramfs.sh
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
configure/circus.sh
configure/shell.sh
```

All configurators receive `.mojo_config` path as `$MOJO_CONFIG`.

---

## Configurators

### timezone.sh
**Driven by:** `TIMEZONE`
- Set `/etc/localtime` symlink
- Sync hardware clock (`hwclock --systohc`)
- Enable NTP, select pool based on locale region

### locale.sh
**Driven by:** `LOCALE`
- Uncomment locale in `/etc/locale.gen`, run `locale-gen` only if changed
- Write `/etc/locale.conf`
- Set `LANG`, `LC_TIME`, `LC_MONETARY`, `LC_PAPER`
- **Distro dispatch:** `locale-gen` (Arch/Debian) vs `localedef` (Fedora)

### hostname.sh
**Driven by:** `HOSTNAME`
- Write `/etc/hostname` and `/etc/hosts`
- `hostnamectl set-hostname` on live system (no-op in chroot)

### sudo.sh
**Driven by:** `SYSTEM_USER`, `DISTRO`
- Wheel group (Arch/Fedora) or sudo group (Debian/Ubuntu)
- Write to `/etc/sudoers.d/mojo` drop-in — never touch main sudoers

### mirrors.sh
**Driven by:** `DISTRO`, `LOCALE`
- Arch: `reflector` with region from locale
- Debian/Ubuntu: write `/etc/apt/sources.list` with regional mirrors
- Fedora: `dnf-automatic` fastest mirrors
- Skip if last run < 24h ago

### cpu.sh
**Driven by:** `CPU`, `PROFILE`
- Detect CPU vendor, ensure correct microcode (`intel-ucode` / `amd-ucode`)
- Governor: `performance` for workstation/server, `powersave` for laptop

### gpu.sh
**Driven by:** `GPU`
- Detect GPU from `lspci` + loaded modules
- If changed: remove old drivers, install new
- Nvidia: `linux-headers`, `nvidia-open-dkms`, `nvidia-utils`, DRM modeset
- AMD: `mesa`, `vulkan-radeon`, `libva-mesa-driver`
- Intel: `mesa`, `vulkan-intel`, `intel-media-driver`
- Hybrid GPU (Nvidia + Intel): configure `prime-run` / `envycontrol`
- Write back detected GPU to `.mojo_config`

### initramfs.sh
**Driven by:** `GPU`, `LUKS`, `DISTRO`
- Build HOOKS line: exclude `kms` for Nvidia, include `encrypt` if `LUKS=true`
- Only regenerate if HOOKS actually changed
- **Distro dispatch:** `mkinitcpio -P` / `update-initramfs -u` / `dracut --force`

### bootloader.sh
**Driven by:** `LUKS`, `LUKS_UUID`, `GPU`, `DUAL_BOOT`, `GRUB_TIMEOUT`, `DISTRO`
- Build kernel cmdline: LUKS device mapping, `nvidia_drm.modeset=1` for Nvidia
- Set GRUB timeout, enable `os-prober` if `DUAL_BOOT=true`
- Only run `grub-mkconfig` if a value changed

### display.sh
**Driven by:** `PROFILE`, detected monitors
- Detect monitors via `wlr-randr` / `hyprctl monitors`
- Configure multi-monitor layout, HiDPI scaling, sddm settings
- Write Hyprland monitor config to a path dotfiles can symlink

### input.sh
**Driven by:** `KEYMAP`, `PROFILE`
- Set console keymap in `/etc/vconsole.conf`
- Set Wayland keymap
- Laptop: configure touchpad via libinput (natural scroll, tap-to-click, disable-while-typing)
- Write Hyprland input config

### audio.sh
**Driven by:** `PROFILE`, detected devices
- Detect devices via `pactl list`, set default sink/source
- PipeWire: low-latency config for workstation, standard for laptop
- Enable `pipewire-bluetooth` if adapter present

### storage.sh
**Driven by:** `HDD`, `HDD_MOUNT`, detected drives
- Reconcile detected drives against fstab
- New drive: create mount point, add fstab entry (user confirmation for new drives)
- Configure SMART monitoring (`smartd`)
- I/O scheduler: NVMe → `none`, HDD → `mq-deadline`, SSD → `bfq`

### network.sh
**Driven by:** `HOSTNAME`, `FLEET_PROFILE`, `DISTRO`
- Ensure NetworkManager + systemd-resolved configured
- Hybrid profile: ensure Tailscale present and configured

### bluetooth.sh
**Driven by:** `PROFILE`, detected adapters
- Laptop: enable `bluetooth.service`, power on by default
- Workstation: disabled unless adapter detected
- Server: always disabled

### power.sh
**Driven by:** `PROFILE`
- Laptop: TLP (battery thresholds, USB autosuspend), suspend/hibernate on lid close, `powertop` auto-tune on battery
- Workstation: disable TLP, `performance` governor, disable suspend
- Server: `performance` governor, disable all sleep states, enable wake-on-LAN

### sshd.sh
**Driven by:** `FLEET_PROFILE`
- Write `/etc/ssh/sshd_config.d/99-mojo.conf` drop-in (never touch main config)
- Always: key-only auth, no passwords, no root login
- Hybrid/server: listen on Tailscale subnet only
- Standalone: listen on all interfaces

### firewall.sh
**Driven by:** `FLEET_PROFILE`, `PROFILE`, `DISTRO`
- **Distro dispatch:** `ufw` (Arch/Debian/Ubuntu), `firewalld` (Fedora)
- Base: deny incoming, allow outgoing, allow SSH
- Server: open WebSocket port, Postgres port (Tailscale only), fleet agent port
- Hybrid: open mojo-agent port (Tailscale only)

### services.sh
**Driven by:** `PROFILE`, `FLEET_PROFILE`, `GPU`
- Always: `NetworkManager`, `sshd`, firewall, `systemd-timesyncd`
- Laptop adds: `tlp`, `bluetooth`
- Workstation adds: `cups`
- Server adds: `postgresql`, `nginx`/`caddy`
- Server + Jarvis: Jarvis systemd services, ChromaDB, Ollama
- Hybrid: `mojo-agent`
- Nvidia: `nvidia-persistenced`

### circus.sh
**Driven by:** `FLEET_PROFILE`
- **Standalone:** no-op
- **Hybrid:** ensure Tailscale installed + authenticated, write `mojo-agent` config with `AGENT_TOKEN`, verify ringmaster connectivity
- **Server:** ensure Postgres running + circus schema exists, Jarvis services installed, manage `FLEET_KEY`

See [circus.md](circus.md) for circus formation detail.

### shell.sh
**Driven by:** `SYSTEM_USER`, `FORK_DIR`, `OS_NAME`
- Ensure correct default shell (`chsh`)
- Write system-wide aliases to `/etc/profile.d/mojo.sh`: `mojo-update`, `{os_name}-update`
- Add `FORK_DIR` and `MOJO_CONFIG` to environment
- Does not touch user dotfiles (`.bashrc`/`.zshrc`)
