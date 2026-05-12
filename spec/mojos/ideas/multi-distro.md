# Idea: Multi-Distro Support

> Deferred — Mk1 is Arch-only. The dispatch framework below is the intended long-term design.

---

## Strategy

`DISTRO` detected once in `bootstrap.sh` from `/etc/os-release $ID`. All scripts branch with `case "$DISTRO" in` at the relevant section — no separate per-distro scripts.

Supported: `arch` | `ubuntu` | `debian` | `fedora`

## Sections Requiring Distro Dispatch

| Section | Arch | Debian/Ubuntu | Fedora |
|---|---|---|---|
| Package manager | `pacman` | `apt` | `dnf` |
| Base OS install | `pacstrap` | `debootstrap` | `dnf --installroot` |
| Chroot tool | `arch-chroot` | `chroot` | `chroot` |
| Initramfs | `mkinitcpio` | `update-initramfs` | `dracut` |
| Postgres init | manual `initdb` | auto | `postgresql-setup --initdb` |
| Firewall | `ufw` | `ufw` | `firewalld` |
| AUR packages | `yay` | N/A | N/A |

Everything that doesn't differ stays distro-agnostic.

## Configure.sh Dispatch

Each configurator that needs distro branching uses a `case "$DISTRO" in` block inline. Affected configurators: `locale.sh`, `mirrors.sh`, `initramfs.sh`, `bootloader.sh`, `firewall.sh`, `services.sh`.
