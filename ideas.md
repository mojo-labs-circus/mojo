# Ideas

*The net. One line per idea so nothing gets lost. Anything here is unscheduled and
uncommitted — it graduates to [roadmap.md](roadmap.md) when its time comes, or dies
here honestly. Fuller write-ups of many of these exist in git history (pre-reset
`raw/`).*

## MojOS

- Modes as full vibe-presets: Grateful Dead mode, hacker mode, clean "normal" mode
  for company — palette, wallpaper, layout, terminal density all switch together.
- Workshop aesthetic: warm retro-futurism — aged paper, amber phosphor, vintage
  instruments; woody, not metal. Full palette spec in git history
  (`raw/docs/vision/aesthetic.md`).
- Fork model: MojOS as a forkable base where `config/` owns the product identity
  (DadOS, a company OS) and upstream merges stay clean.
- First-run file system scan: agent maps your existing chaos and proposes a
  reorganisation, fully on-device, diff + rollback before anything moves.
- Multi-distro support (agent on Ubuntu/Fedora, not just MojOS) — the one-agent
  model already implies this; mechanics in git history.
- Dashboard showing currently active vessels, owned and chartered alike —
  sourced straight from the watch bill (standing-orders.md #38), which already
  tracks every vessel running a task; just a presentation layer, not new data.
- GUI for per-machine flake configuration, for regular people: name each device
  you own (work laptop, home laptop, gaming PC, server...) and dial in exactly
  what's on it, no hand-editing Nix — swapping in a new laptop becomes trivial,
  and keeping theming/config consistent across every machine (or deliberately
  different per device) becomes trivial too. The agent itself can help drive the
  configuring, not just the GUI alone. Closes the capability gap for the config
  layer itself, not just for using the agent — customised machines for everyone,
  not just Windows default-slop.

## mojo-agent

- The brain, staged: plain markdown memory first; consolidation ("dream mode"),
  semantic recall, and the condensation principle (keep everything as compact as
  integrity allows) when files stop being enough.
- Git as the actual mechanism for the brain, not just an analogy: if the log is
  plain markdown, it's exactly git's substrate — full history for free (feeds
  the task ledger), real diffing (feeds the memory review/veto UX above), and
  the consort's disconnected-operation/reconciliation need is just clone,
  work offline, push/pull/merge on reconnect. Doesn't cover everything — a
  consort or chartered instance needing something live, mid-task, still needs
  an actual low-latency call, not a git pull. Also need condensed/partial
  copies (a consort that can't hold the whole brain, or a task that only needs
  a scoped slice) — git's sparse-checkout and partial-clone features (clone
  only specific paths, or filter out blobs) look like the natural mechanism for
  that, but the actual split (by directory? by topic? something else?) isn't
  decided — figure out once there's a real brain to test it against.
- Configurable flags/arguments on mojo-agent deployments: not every instance of
  the harness should behave identically — deployment-time config controlling
  what a given instance is allowed to do. Concrete motivating case: whether a
  chartered instance (standing-orders.md #16/#17) is permitted to hail the flagship for more context
  mid-task, or is locked to only what it was given upfront. Hailing means an
  ongoing live connection back home for the whole task, not a one-time payload
  — strictly more attack surface than a sealed manifest, since a compromised
  charter with an open channel can keep pulling more, where a manifest-only one
  can only leak what it already had. Decided per-charter at charter time, same
  kind of call as the tier-routing policy itself, not a separate mechanism.
  General shape (a real config system, not just this one flag) still open.
- Memory review UX: I can read, edit, and veto everything the agent writes about me
  — the memory is the most personal artifact in the system.
- Self-maintained behaviour file: the agent keeps its own notes on how I like to
  work (the JARVIS.md idea) and updates them from corrections.
- Self-improvement log: every correction/mistake logged (improve.jsonl idea) and
  periodically distilled into behaviour changes.
- Code of ethics enforced by identity framing, with a lightweight constitutional
  check as audit layer (ringmaster design, rethought for one agent).
- Humaniser / style layer: the agent writes in *my* voice when drafting as me.
- Clients with an explicit mode switch: a visible way to tell a client "you're
  talking to work-me now" rather than persona selection always being inferred.
  Surfaced 2026-07-13 discussing how persona scoping actually gets selected per
  run (seams c/d); not needed yet, revisit once personas themselves are real.
- Context-aware verbosity: depth of explanation adapts to what it knows I know.
- Confirmation-gate pattern for all system actions (v0.1 already needs this).
- `/btw` interrupt channel: inject a thought mid-task without cancelling it.
- Last-mile fine-tuning: LoRA a local model on my own writing/patterns so the
  personal-ness is in the weights, not just the prompt — endgame for local models.
- Life operating layer: one tool wrapper per service (Home Assistant, Jellyfin,
  Spotify, calendar...), screen control filling API gaps — "turn the Masters on in
  the basement."
- Mojo as "an OS for all your stuff" as IoT proliferates: the Vessel/tool split
  worked out in vessel-primitive.md already gives this the right shape for free —
  dumb devices (locks, bulbs, sensors) are tools the first mate calls, never vessels
  themselves, so there's no ceiling on how much "stuff" one Jarvis can orchestrate.
  Real market validation already exists, not just a hunch: Home Assistant
  (open-source, local-first, actively growing) already solved the hard, unglamorous
  part — device/protocol integration across a fragmented ecosystem (Zigbee/Z-Wave/
  Matter/vendor clouds) — as the explicit alternative to cloud-locked Amazon/Google/
  Apple smart-home platforms. The differentiator isn't reinventing that integration
  work, it's the persistent judgment-capable agent layer on top, which Home Assistant
  doesn't have natively. Practical implication: lean on Home Assistant as the
  integration layer (the wrapped service in the Life operating layer idea above)
  rather than building IoT protocol support from scratch.
- Collaborative dev partner with honest uni mode: real pace, real understanding,
  nothing produced I can't explain.
- Dynamic project planning: agent as proactive PM for any kind of project, not a
  to-do list.
- Voice input (Whisper-class, local).
- Watchdog-rollback updates: deploy, health-check, auto-revert — blue-green safety
  without blue-green cost on personal hardware.
- Chaff for mercenary traffic: if there's spare budget on frontier model
  subscriptions, deliberately mix in synthetic/noise requests to defeat pattern
  inference — even if no single request leaks real content (decoy cargo
  already handles that), a pattern across many real questions to a mercenary
  could still reveal what's actually being worked on through traffic analysis
  alone. Real precedent: Tor and similar systems already inject cover traffic
  for exactly this reason. Optional hardening, contingent on spare resources —
  not something the base model needs to function.

## Circus (fleet era)

- PC-as-server first: the daily-driver PC becomes the ringmaster before any
  dedicated hardware exists; laptop joins as second machine.
- MojOS installer, at circus time: explicit create-vs-join branch — first
  machine creates a circus, every machine after points at an existing one and
  joins it. Installer-level decision, not just a Ringmaster-side one.
- No central account system for that join — sovereignty rules it out anyway.
  Syncthing-style device pairing is the precedent: "create" generates a keypair
  for that Ringmaster plus a shareable join token/QR code; "join" means having
  that token and using it to authenticate directly against that Ringmaster's
  address. The "account" is just possession of the token — same shape as
  Tailscale auth-keys, no third party involved.
- That same token should also transparently set up Tailscale under the hood —
  the user never needs to know Tailscale exists. Mainstream usability means the
  networking layer disappears; "join circus" is one token/QR code and it just
  works, not a thing you configure.
- Resource pooling: route inference to the best GPU in the circus; storage to
  capacity. Concrete lead: exo (Apache-2.0, actively maintained, p2p
  auto-discovery, shards a model across heterogeneous devices by real-time
  topology/bandwidth) is architecturally the right shape for this — blocked for
  now by Linux GPU support only existing via Apple Silicon/MLX, worth
  revisiting once that lands. Petals looked similar but is stagnant (~3 years
  no release) and built for public volunteer swarms anyway — skip it.
- Fleet-wide shared namespace (the log, the roster) over 9P: the protocol is
  already in the mainline Linux kernel as `v9fs`, used today for QEMU/WSL
  folder-sharing — no need for Plan 9 itself, just the protocol as a building
  block for unifying resource access across the fleet. Strongest concrete lead
  from the SSI research pass (see devlog).
- Slurm as a reference for the task-routing policy's actual algorithm
  (resource matching, backfill scheduling) — not adopting Slurm itself (it's
  daemon-only, built for shared research clusters, wrong interaction model),
  but its scheduling concepts are worth designing the router against.
- Consensus-based flagship: for people without a dedicated always-on machine,
  use a consensus algorithm (Raft/Paxos-style) across several intermittently-on
  owned machines so they jointly agree on canonical state, instead of requiring
  one device be designated always-on. Lets someone pool whatever they already
  own rather than committing a specific machine as the permanent anchor. Not
  needed for the current single-flagship design — a later accessibility feature.
- Thin clients (phone, watch) as direct windows into the server — also proves the
  agent API is client-agnostic.
- Version-negotiated agent↔server protocol from day one of circus work — mixed
  fleets are the normal case on personal hardware.
- Heata-style waste heat: serious home compute should heat the water. Design for it
  someday.
- Shared `~` across the fleet: any directory can be flagged shared or vessel-local,
  synced fleet-wide if shared. Syncthing looks like the right existing mechanism
  (no cloud, per-folder opt-in, versioned conflict copies on real divergence) rather
  than reinventing sync — same category of call as adopting Tailscale for the mesh.
  Exact flagging mechanism (fixed directory convention vs. per-folder declared rule)
  undecided, figure out when building it.

## Collectives / framework (far horizon)

- The formal collective model: positions independent of holders, constitutions,
  every AI with an accountable human owner, recursive at every scale (Unix-style
  primitive — full thinking in git history).
- Armada: the fleet-metaphor name for a collective — multiple sovereign fleets
  (each still one Captain, one first mate, no exceptions) joining for a shared
  purpose without any fleet losing individual ownership. Sharpens the existing
  "Unix-style primitive" line above: a fleet is the file, an armada is the
  folder — it references/organizes fleets without owning or absorbing their
  contents, same as a directory entry pointing at an inode rather than
  duplicating it. A collective can have its own role-filling AIs (e.g. a shared
  coordinator role), but each still traces back to one accountable human, same
  invariant as everywhere else — two people never share custody of one first
  mate.
- Accountability as the criterion for human-in-the-loop: if someone must answer for
  a decision, a human makes it (the IBM 1979 line).
- Chefs and recipes: agents provide judgment, skills provide deterministic
  structure — the probabilistic/deterministic bridge.
- A formal spec language for agents (.mojo files): typed contracts, NL bodies —
  specs as shareable deliverables. (Naming hazard: Modular's Mojo language exists.)
- Runtime-composed deliverables: same content, surface adapted to how the recipient
  learns — the network flywheel when both sides run Mojo.
- Two-way probing protocol: when does the AI probe vs execute — needs real design.
- Software dev practice as a general work superpower: agents run Agile/ADRs/kanban
  for any kind of work, not just code.
- Honest matching: longitudinal record of how you actually work turns "do I fit
  this role?" into a data question — sovereignty guard against it becoming a caste
  system.
- Community "cloud map" of a craft's known territory — navigable depth, expanded by
  practitioners.
- Cross-implementation fleets: seams k (machine ↔ machine) and n (machine
  admission) being real standardized contracts, not just internal plumbing,
  means a machine running one compliant fleet-manager implementation can join
  a fleet whose sibling machines run a different one — the same seam pair is
  what would let independently-built fleets actually federate into a
  Collective/Armada, surfaced during the Fleet manager piece pass
  (2026-07-12), not pursued now.

## Ephemeral Commons

- sops-nix for all secrets, own hardware and rented alike — no reason to treat
  local as trusted-by-default when the encrypted-at-rest version costs nothing
  extra.
- Mercenary status should cover a full Claude Code (or Codex) session, not just
  single questions — since decoy cargo is a live property on individual cargo,
  not a one-shot prompt substitution, mojo-agent can proxy the whole session:
  serve the decoy version of each file/cargo as the CLI reads it, translate its
  output back onto real cargo as it comes back. No new mechanism, just the first
  mate interposing for the session's length instead of once per prompt. Real
  hope stays that open-source models get good enough that this isn't necessary
  at all — but coding is the sharpest case against that today, since Claude
  Code/Codex are genuinely the best tools for it right now.

## Product / org

- The mojo-package idea: your entire AI as a Nix flake, portable across
  machines — vision.md's "every house has a server" implies you eventually move
  house and the server changes underneath you; Nix is how you take your AI with
  you to the new one instead of rebuilding it. Same packaging then doubles as the
  on-ramp for Ephemeral Commons — beef up the resource spec and the identical
  flake stands up mojo-agent on rented compute instead of a new house's server.

- Sustainability if it ever needs money (never by closing the core or centralising
  data): pre-built home servers, setup + support, AI-house partnerships ("one bed,
  one bath, a server"). Thinking in git history (`raw/docs/vision/monetisation.md`).
- Enterprise angle from internship observations: determinism/probabilism boundary
  design, migration, vendor lock-in — companies need the framework's discipline.
  (`raw/docs/vision/pain-points.md` in git history.)
- The objections doc: honest answers to "why would this work" — worth reviving as a
  public FAQ once there's a working system (`raw/docs/vision/why-this-works.md` in
  git history).
- Essay series: craft problem → sovereignty → collectives. Blog + professors +
  forums; essays stand alone, project attached only when asked.
- Docs maintenance needs a fresh plan once this structure has been lived with.
