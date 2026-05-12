# Idea: Fork Model

> Deferred — build base MojOS first, dogfood it, then add fork support. Clarke becomes the first fork user (baker). See planning.md for rationale.

---

## What a Fork Is

A fork is a git fork of `mojos` with a `config/` directory that owns the product identity. The framework never touches `config/`, so upstream merges are always clean.

Examples: BakerOS (personal), AtlasOS (company product), DadOS (minimal/beginner).

---

## Fork Structure

```
my-os/
  config/
    identity.yaml      ← OS name, agent name, branding, author
    packages.txt       ← additional packages on top of base
    persona.md         ← system prompt additions, agent tone
    hooks/
      pre-install.sh
      post-install.sh
      pre-update.sh
      post-update.sh
  [mojos framework]    ← upstream — pulled via git, never edited in fork
```

---

## config/identity.yaml

```yaml
os_name: BakerOS
os_slug: baker
agent_name: Jarvis
author: the_baker
distro_default: arch    # preferred distro — informational, not enforced
wm_default: hyprland
```

---

## config/persona.md

System prompt additions that extend (not replace) the base Jarvis persona.

```markdown
You are running on BakerOS, a personal agentic OS built for the_baker.
Prefer concise responses. The user is technical.
```

---

## Upstream Sync

```bash
git fetch upstream
git merge upstream/main
```

Clean merge because fork config lives entirely in `config/`. `mojo-update` automates this.

---

## mojo-init (Scaffolding)

Run once to create a new fork:

```
$ mojo-init
OS name?     BakerOS
Slug?        baker
Username?    the_baker
Mode?        solo

→ Fork mojos repo
→ Create config/ layer
→ Clone to ~/baker
→ Run ./install.sh to install
```

Circus setup is separate after install — `mojo-circus-join`.

---

## Cross-Fork Circus Compatibility

Circus membership is fork-agnostic. Protocol version is the compatibility gate; fork identity is informational only (logged in registry, never used for access control).

---

## Mk2 — Fork Extensions

In Mk1, forks configure `mojo-agent` but don't extend it. In Mk2:
- Add custom LangGraph nodes
- Register additional tools
- Extend protocol additively with versioning

Constraint: keep graph nodes modular and tool registry open so Mk2 extensions aren't painful.

---

## Distro Freedom

Fork can declare a default distro but cannot enforce it. Users can run any supported distro regardless of fork defaults.
