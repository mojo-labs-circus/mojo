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

## mojo-agent

- The brain, staged: plain markdown memory first; consolidation ("dream mode"),
  semantic recall, and the condensation principle (keep everything as compact as
  integrity allows) when files stop being enough.
- Memory review UX: I can read, edit, and veto everything the agent writes about me
  — the memory is the most personal artifact in the system.
- Self-maintained behaviour file: the agent keeps its own notes on how I like to
  work (the JARVIS.md idea) and updates them from corrections.
- Self-improvement log: every correction/mistake logged (improve.jsonl idea) and
  periodically distilled into behaviour changes.
- Code of ethics enforced by identity framing, with a lightweight constitutional
  check as audit layer (ringmaster design, rethought for one agent).
- Humaniser / style layer: the agent writes in *my* voice when drafting as me.
- Context-aware verbosity: depth of explanation adapts to what it knows I know.
- Confirmation-gate pattern for all system actions (v0.1 already needs this).
- `/btw` interrupt channel: inject a thought mid-task without cancelling it.
- Last-mile fine-tuning: LoRA a local model on my own writing/patterns so the
  personal-ness is in the weights, not just the prompt — endgame for local models.
- Life operating layer: one tool wrapper per service (Home Assistant, Jellyfin,
  Spotify, calendar...), screen control filling API gaps — "turn the Masters on in
  the basement."
- Collaborative dev partner with honest uni mode: real pace, real understanding,
  nothing produced I can't explain.
- Dynamic project planning: agent as proactive PM for any kind of project, not a
  to-do list.
- Voice input (Whisper-class, local).
- Watchdog-rollback updates: deploy, health-check, auto-revert — blue-green safety
  without blue-green cost on personal hardware.

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
  capacity.
- Thin clients (phone, watch) as direct windows into the server — also proves the
  agent API is client-agnostic.
- Version-negotiated agent↔server protocol from day one of circus work — mixed
  fleets are the normal case on personal hardware.
- Heata-style waste heat: serious home compute should heat the water. Design for it
  someday.

## Collectives / framework (far horizon)

- The formal collective model: positions independent of holders, constitutions,
  every AI with an accountable human owner, recursive at every scale (Unix-style
  primitive — full thinking in git history).
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

## Product / org

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
