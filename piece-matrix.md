# Piece × piece existence pass

The actual working data for [seam-method.md](seam-method.md)'s step 1: for
every pair of the 13 nodes in [pieces.md](pieces.md) (the 11 pieces plus
Owner and Identity), does a real fact or action ever need to cross at all?
Stress-tested per pair — a concrete scenario tried and either surviving or
broken — not pattern-matched from a general rule, even when a general rule
ends up explaining several pairs at once.

`piece-matrix.html` is a rendering of this file for looking at, not the
working copy. It's stale right now (still the empty grid from before this
pass started) and gets regenerated from here later, when there's something
worth looking at, not edited directly while this pass is running.

Legend: **yes** (a real crossing exists), **no** (checked, nothing crosses,
with the scenario that was tried and why it doesn't hold), *(open)* (exists
but what exactly crosses, or which piece owns the other side, isn't settled
yet).

Order: each node against every node listed after it, so no pair is checked
twice. Node order matches pieces.md: Owner, Client, Harness, Model endpoint,
Tools, Memory provider, Sandbox, Router, Kernel, Credential broker,
Provenance, Fleet manager, Identity.

---

## Owner

- **Owner – Client: yes, two-way.** *Verification* (Owner → Client): something
  has to tell this specific human apart from an impostor; that fact has
  nowhere else to live. *Delivery* (Client → Owner): whatever needs to reach
  the human — an ordinary reply or an unprompted push — has to actually land
  on one of the owner's live surfaces (phone, watch, workshop speakers, an
  always-on email address); Client is the owner's window in both directions,
  not just an input funnel, so this genuinely belongs to this pair, it
  doesn't relocate. First pass wrongly pulled delivery out of this pair
  entirely; corrected after Clarke caught it. Checked against real systems
  (fork, below) before accepting: holds in all five. Worth carrying forward —
  not every Client instance has the same liveness. A terminal or app is only
  live while open; an email address (or a phone number) is effectively
  always reachable. That's real variation within one piece type, not a
  reason to split it, but it'll matter once Client gets classified for real.
- **Owner – Harness: no.** Tried: a mid-task confirmation prompt ("are you
  sure?"). Real, but Harness has no channel to a human of its own — it can
  only push the question out through whatever Client the run is attached to,
  and the answer comes back the same way. Decomposes into Harness–Client and
  Client–Owner, never a direct Harness–Owner crossing. Flags Harness–Client
  as a real pair to check properly when we get there (does a run talk back
  to its client mid-execution, or only hand off a final result).
- **Owner – Model endpoint: no.** Tried: owner prompts a model directly. Even
  then it's formatted and passed by a Harness; the model never sees "the
  Owner" as a party.
- **Owner – Tools: no.** Tried: owner hands a tool a credential directly.
  That's custody, the Credential broker's job; the tool never talks to the
  Owner.
- **Owner – Memory provider: no.** Tried: owner directly edits their own
  memory data. Even `cat`-ing a file happens through a terminal or file
  browser, which is a Client by definition. No exception found.
- **Owner – Sandbox: no.** Tried: owner watches a sandboxed action execute
  live (a terminal session). Still perceived through whichever Client that
  terminal is. Sandbox itself has no owner-facing contract.
- **Owner – Router: no.** Tried: owner sets an explicit schedule or trigger.
  Written through a Client, read later by Router as data. No live
  Owner–Router protocol.
- **Owner – Kernel: no.** Tried: owner revokes a permission in an emergency.
  Done through some administrative Client surface; Kernel acts on the
  written revocation, never talks to the Owner directly.
- **Owner – Credential broker: no.** Tried: owner types a password into a
  vault at setup. Corrected 2026-07-13 (caught working the Client row): that
  vault/keychain UI isn't a Mojo Client at all. Client is defined as the
  surface for reaching the assistant; a password manager or OS keychain is
  substrate the owner already has, entirely outside the Mojo piece boundary,
  the same as buying a hardware token or installing disk encryption. So
  there's no crossing here at all, not even Owner–Client — the broker just
  reads afterward from wherever the owner's secrets already live, no live
  exchange with anything Mojo-shaped at setup time.
- **Owner – Provenance: no.** Tried: owner tags their own content as trusted.
  That's a data fact (content originating from an owner-authenticated
  Client), not a live Owner–Provenance exchange.
- **Owner – Fleet manager: no.** Tried: owner admits a new machine (approves
  on phone, scans a QR code). The action is Owner-using-a-Client telling
  Fleet manager to trust a device; the crossing is Client–Fleet manager, not
  Owner–Fleet manager directly.
- **Owner – Identity: no.** Same as Owner–Memory provider: even "direct"
  access is through some Client.

**Structural note, checked not assumed:** every no above reduces the same
way, because Client is *defined* as the owner's only reach into the system —
nothing else in the piece list has a channel to the Owner as a person, only
to owner-authored data. That's a reusable shortcut, but each pair above still
got its own concrete scenario before relying on it, since a shortcut that
turns out wrong once (the way the old flat seam list did) is worse than
useless.

**Flagged for later, not resolved here:**

- Harness–Client: does a run interact with its client mid-execution, or only
  at handoff and final return? (surfaced by Owner–Harness)
- Delivery itself is Owner–Client (see above), but *deciding* a message needs
  to go out, and *picking which* of the owner's several live clients it
  should reach, is upstream of that and still open: Router, since it
  "watches the triggers"? Kernel, since it flags "consequential"? Both, for
  different reasons? That decider needs a roster of the owner's live/
  reachable clients and either picks one or broadcasts — a real discovery
  problem. Belongs to that decider's row against Client, not Owner's.
- Verified against real prior art (fork, 2026-07-13): OpenClaw, Hermes, Khoj,
  Letta, OpenFang all mediate 100% of user-facing interaction through one
  channel/UI/API-shaped layer; none give the user a live protocol with any
  backend component directly. Khoj's scheduled automations deliver by email,
  the one case that looked different — but that's just an always-live Client
  instance, i.e. the roster/discovery question above, not a different kind of
  relationship. No system found breaks the "Owner only ever touches Client"
  claim.

**Row closed 2026-07-13.**

---

## Client

- **Client – Harness: yes, two-way.** *Request* (Client → Harness): once
  Router assembles a run and hands it to a Harness, the task/context that
  started it has to actually reach the assigned Harness. *Live
  back-and-forth* (both ways): streamed partial output, mid-run clarifying
  questions, and the final result travel directly between Client and Harness
  for the run's duration, not proxied through Router token-by-token, since
  Router's own job ("assembles the run") reads as a one-time setup action,
  not an ongoing relay. Resolves the flag left open in Owner's row: yes, a
  run talks back to its client mid-execution, not only at final handoff.
  Verified against real prior art (fork, 2026-07-13): OpenClaw's Gateway
  resolves sessions and dispatches to an ephemeral per-message Agent Runtime,
  channel adapters holding no state of their own (Gateway = Router's
  one-time dispatch, Agent Runtime = Harness, adapter = Client); Letta's
  session model is the closest match, "a session is a disposable
  client-side connection holding only transient turn state," the live
  connection sits client/agent-side, not scheduler-mediated.
- **Client – Model endpoint: no.** Tried: a client rendering raw model
  output directly, no intermediary. Even in a bare terminal, that stream is
  relayed by Harness, which is what actually calls Model endpoint; Model
  endpoint has no client-shaped concept, only whichever Harness called it.
- **Client – Tools: no.** Tried: client invoking a tool directly as a
  shortcut (e.g. a UI button that "runs a script"). Still goes through
  Harness's tool-call loop; Tools has no notion of a client, only of its
  caller and what Kernel permits that caller.
- **Client – Memory provider: yes.** First pass called this "no" (Memory
  provider is granted to a Harness by Router, a bare Client never holds
  one) — wrong, caught by Clarke: Memory provider's real value is
  retrieval, indexing, and ranking, not raw file access, and a client built
  specifically for that (a semantic notes browser, a memory editor) would
  want that capability directly, no agent run required, writing back
  through the provider so edits land in the data's standard shape rather
  than as raw file writes. Verified against real prior art (fork,
  2026-07-13), not just plausible architecture: Khoj's docs state its
  search "operates independently from the chat features," a distinct API a
  client calls without touching the agent path; Letta exposes `GET
  /v1/agents/{id}/archival-memory/search` as its own documented external
  API, separate from the agent's own tool call for the same retrieval;
  Mem0 is explicit its client library is for use "in client applications
  outside of agent contexts," `client.search()` needs no agent at all.
  Three independent systems, same pattern. (Checked and rejected as a
  fourth data point: Obsidian's Smart Connections plugin bundles its own
  private local index rather than calling a separate provider service —
  that's the raw-file/private-index pattern, not this one.)
- **Client – Sandbox: no.** Tried: client watching or driving a sandboxed
  execution live (a terminal streaming command output). Still relayed
  through Harness over the same live Client–Harness connection above;
  Sandbox has no client-facing surface of its own.
- **Client – Router: yes, one-way (Client → Router).** Tried: owner types a
  message into a terminal right now. Router's own definition names "an
  owner's direct request" as one of the triggers it watches for, alongside
  the clock, outside events, and idle time. That trigger has to actually
  reach Router live for a fresh request to be assembled into a run without
  waiting on a poll cycle. No return crossing: once Router assigns a
  Harness it steps back, and the reply path is Client–Harness (above), not
  Router relaying anything back to Client itself.
- **Client – Kernel: no.** Tried: a direct-access Client (Obsidian-style)
  writing straight to identity files, checked against Kernel's "every
  consequential action... passes through here" claim. Already settled
  elsewhere (devlog, 2026-07-13 latest, the userspace-tampering question):
  the owner editing their own data directly is ownership, not an attack, so
  there's no live permission check here at all, not even a
  trivially-passing one. What actually guards this path is sandbox
  isolation, tamper-evidence, and encryption at rest, none of which are a
  live Kernel exchange.
- **Client – Credential broker: no.** Tried: owner types a password into a
  vault at setup. Corrected from a first-pass reasoning error (caught by
  Clarke): that vault/keychain UI isn't a Mojo Client at all. Client is
  defined as the surface for reaching the assistant; a password manager or
  OS keychain is substrate the owner already has, entirely outside the Mojo
  piece boundary, the same as buying a hardware token or installing disk
  encryption. So there's no crossing here at all, not even Owner–Client —
  the broker just reads afterward from wherever the owner's secrets already
  live, no live exchange with anything Mojo-shaped at setup time. Same fix
  applied to Owner row's version of this reasoning above.
- **Client – Provenance: no.** Tried: content pasted or forwarded into a
  Client (owner copies in a webpage paragraph, forwards someone else's
  message) — does that get a different trust tag than the owner's own typed
  words, and does that tagging happen through a live Client–Provenance
  exchange? First pass waved this off too fast; Clarke pushed back and
  asked what prior art actually does before accepting an answer either way.
  Verified (fork, 2026-07-13): no real system tags provenance below the
  message/turn boundary. Anthropic's own prompt-injection guidance wraps
  *tool-result* content in delimiters as untrusted, drawing the line at the
  tool-call boundary, not inside what the user typed; the published
  instruction-hierarchy work (Anthropic/OpenAI/Google) ranks system > user
  > retrieved content as three tiers, with "user" as one tier regardless of
  what the user chose to include; Microsoft's Spotlighting/Prompt Shields
  tags external/retrieved documents specifically, never sub-spans of the
  user's own input. The real, standardized boundary is that the owner is
  accountable for whatever they put in their own turn, full stop — which is
  the Tools/fetched-content-vs-Client boundary this project already models,
  not a finer one. Nothing to standardize below that; left for the
  community to build finer-grained schemes on top once this publishes, not
  invented here.
- **Client – Fleet manager: yes, two-way.** Carried forward from Owner's
  row, which already named this pair directly: "the crossing is
  Client–Fleet manager, not Owner–Fleet manager directly." Scenario: owner
  approves a new device (scans a QR code, confirms on a phone client); that
  approval is submitted straight to Fleet manager, which is what actually
  admits the device into the trusted set. Return leg: a confirmation
  (device admitted) or a live status query ("is this identity already
  driving a task elsewhere") for a handoff-aware client.
- **Client – Identity: yes, two-way.** The direct-access flavor of Client
  named in pieces.md itself ("direct access to the identity itself... the
  way Obsidian touches a vault") reads and writes the identity's plain-file
  storage straight, no Memory provider, no Harness, no Kernel in between.
  Verified against real prior art (fork, 2026-07-13): Khoj's Obsidian
  plugin edits vault files directly, no Khoj indexing/retrieval layer sits
  in that path.

**Flagged for later, not resolved here:**

- Client isn't one shape, it's three, each hitting a completely different
  piece and nothing else: conversational (→ Harness only), raw-file (→
  Identity only), and provider-mediated (→ Memory provider only). No pair
  above needed more than one of these to hold. Worth deciding at
  classification time (step 3) whether this is one piece with typed
  capability variants — the same pattern Model endpoint already uses for
  modality (chat/embedding/image/speech) — rather than three genuinely
  separate pieces. Leaning toward variants, not a piece-list split, but not
  deciding that here.
- Client–Router's request path: is the "owner's direct request" trigger
  delivered by a live call from Client to Router, or does Client only ever
  write a request into a queue/inbox (arguably Identity- or Router-owned
  data) that Router polls? Marked yes/one-way above on the live-call
  reading; the queue reading would make this crossing look completely
  different (data at rest, not a live protocol) — worth settling once
  Router gets classified for real.

**Row closed 2026-07-13.**

---

## Harness

- **Harness – Model endpoint: yes, two-way.** The run calls the model
  directly and gets a completion back, live, per call; nothing sits between
  the two for the actual exchange. Verified against real prior art (fork,
  2026-07-13): OpenFang's `openfang-runtime` bundles "agent loop, 3 LLM
  drivers" inside the same runtime component, the driver call sits inside
  the loop, not a separate service; Hermes Agent's "agent loop... processes
  user input, selects tools, executes actions" calls the model inline the
  same way. No system found interposes a broker between the runtime and the
  model backend for the call itself.
  Whether *this particular call* is allowed at all isn't a separate
  mechanism from Harness–Kernel below: it's one more consequential action
  that seam already covers. Real prior art splits by granularity, not by
  system: the primary session's own model choice is config/org-policy only
  in every system checked (Claude Code, Codex CLI, OpenFang, Hermes,
  OpenClaw), consistent with a run's grant being scoped once at spawn, not
  re-checked per token. But a **sub-run** requesting a model is a different
  moment, and Claude Code has a real, live, per-call gate for exactly that:
  `Agent(model:opus)` is a documented permission rule, evaluated through the
  same deny→ask→allow pipeline as tool calls — because spawning a sub-run
  with a model choice is itself modeled as a tool call (the `Agent` tool),
  not a new kind of check. Codex CLI has no live gate at either level;
  enterprise policy precedence is documented as real but its enforcement
  point isn't, flagged rather than guessed.
- **Harness – Tools: yes, two-way, gated.** The run calls a tool directly and
  gets a result back, live, per call, but the call passes a permission check
  on the way. Verified against real prior art (fork, 2026-07-13): OpenFang's
  `tool_runner.rs` runs an `allowed_tools` check inline in the loop, before
  dispatch, the runtime calls the tool directly, checked, not routed through
  a separate broker service; Letta's client-tool flow is a live round trip
  too, "the agent attempts to call the tool, the server sends an approval
  request; your application then executes the tool... sends result back,"
  still runtime-initiated, gated by an approval step rather than a broker. No
  intermediary tool-gateway piece found in either.
- **Harness – Memory provider: yes, two-way.** Distinct from the
  already-verified Client–Memory-provider pair (that one is a client calling
  retrieval directly, no run involved): this is the run itself calling
  retrieval live, mid-task. Verified against real prior art (fork,
  2026-07-13): OpenFang states directly that "memory and skills access
  bypass the scheduler entirely... the runtime accesses these through shared
  references" to `openfang-memory`; Letta's agent loop calls its own memory
  tools (`memory_replace`, `archival_memory_search`) live during the run,
  core memory pinned in-context, archival memory queried by tool call from
  inside the loop itself.
- **Harness – Sandbox: yes, two-way.** The run dispatches a permitted action
  into a sandbox and gets the result back live, as part of its own tool-call
  step. Verified against real prior art (fork, 2026-07-13), three of five
  systems directly: OpenClaw's docs state "sandboxed tool calls execute
  inside a Docker container via `docker exec`... the Gateway process always
  stays on the host; only tool execution moves into the sandbox," dispatched
  from the Agent Runtime's own tool-call step, Gateway never in the path;
  Hermes documents an execution-environment abstraction (local/Docker/
  SSH/Singularity/Modal) "utilized by the terminal_tool and file tools,"
  dispatched from inside the agent's own tool-execution path; Letta's client
  tools "run on the local execution environment," server tools "run in the
  Letta server's remote sandbox," both dispatched and returned from the same
  tool-call step in the loop. Three independent systems agree: direct
  dispatch, live result, no broker in between.
- **Harness – Router: yes, one-way (Router → Harness).** Carries forward the
  Client–Harness row's finding: Router assembles the run once and hands it
  off, then steps back; it doesn't stay in the loop for the run's duration.
  Verified against real prior art (fork, 2026-07-13), a third independent
  system now, not just the two already cited for Client–Harness (OpenClaw's
  Gateway/Agent Runtime split, Letta's session model): OpenFang states this
  outright, "the scheduler operates independently from individual agent
  loops. Once an agent begins execution, the scheduler hands off
  responsibility; the agent loop runs to completion or guard limits without
  ongoing scheduler involvement." Three systems agree on the same shape.
- **Harness – Kernel: yes, two-way, live, per-action.** Verified against
  real prior art (fork, 2026-07-13) across four systems, not one: OpenFang's
  agent loop has a capability check as an explicit step in its own sequence
  ("tool-call detection → capability check → approval gate → tool
  execution"), *and*, redundantly, `tool_runner.rs` independently enforces
  an `allowed_tools` list at the execution boundary itself, "so the LLM
  cannot hallucinate tool names outside the agent's capability grants."
  Claude Code is a second, cleaner data point for the same shape: its docs
  state plainly "permission rules are enforced by Claude Code, not by the
  model... instructions in your prompt or CLAUDE.md shape what Claude tries
  to do, but they don't change what Claude Code allows," and separately,
  "use both for defense-in-depth: permission deny rules block Claude from
  even attempting to access restricted resources [and] sandbox restrictions
  prevent commands from reaching resources outside defined boundaries, even
  if a prompt injection bypasses Claude's decision-making" — a documented
  system explicitly distinguishing "the model" (no enforcement authority)
  from "the harness's own permission engine" (which does the checking),
  which is the Harness/Kernel split itself, just built in-process rather
  than as a separate networked piece. Codex CLI corroborates independently:
  a live `approval_policy` check plus OS-level sandbox modes plus an
  optional third layer (`approvals_reviewer`) for side-effecting calls
  specifically. Kubernetes' admission control is a fourth data point in the
  same direction: the caller submits the request and auth+admission run
  inline as part of that same submission, not silent resource-side
  interception, a rejection comes back synchronously. seL4 (pure
  resource-side, no loop-initiated check at all) is the outlier among four
  real systems checked, not an even split. anatomy.md's own map already
  carries a two-label split here ("run contract" next to Kernel, separate
  from "enforcement contract") that lines up with this convergent finding.
  Exact layer count and where each sits stays step 3's job, not settled
  here, but a real, live, per-action crossing, and real systems mostly
  agreeing it's more than one layer, both hold.
- **Harness – Credential broker: no.** Tried: the run asks the broker for a
  secret to hand to a tool itself. Verified against real prior art (fork,
  2026-07-13): OpenFang states the runtime "never holds raw secrets in
  plaintext," credentials are wrapped in `Zeroizing<String>` end to end, "API
  key resolution from vault returns `Zeroizing<String>`, meaning the secret
  exists only in memory long enough for transmission," and secrets are
  redacted from logs — injection happens at the point of use, matching
  pieces.md's claim exactly ("a real secret never sits in... a harness's
  hands"). The general secrets-manager pattern (Vault dynamic secrets, AWS
  STS) agrees: injection at the call site, never held by the orchestrating
  caller.
- **Harness – Provenance: yes, one-way (Provenance → Harness).** Content
  arrives at the run already tagged; the run doesn't call out to a tagging
  service mid-thought. Verified against real prior art (fork, 2026-07-13):
  OpenFang's taint-tracking system is architecturally the same mechanism
  under a different name — "tainting happens at data ingestion points,
  external networks, user input, vault secrets, and untrusted agent outputs
  each receive corresponding labels," and "taint checks execute before tool
  invocation, making this a preventive rather than detective control." Tags
  attach where content enters the system, and the loop consumes
  already-labeled data, gated by lattice rules, rather than calling a tagger
  live. Note: this pair only covers Harness *receiving* tags. The actual
  tagging of a tool's fetched content (Tools ↔ Provenance) is a separate
  crossing, not yet checked, belongs to Tools' own row.
- **Harness – Fleet manager: no.** Tried: a run in progress hands off live
  to a harness on another machine. Verified against real prior art (fork,
  2026-07-13, two rounds): no system examined has the agent runtime itself
  relocate between peer machines. First round found OpenClaw explicitly
  lacks cross-device sync (open issues #47871, #38878). Second round checked
  the apparent counter-example directly and it doesn't hold up the way it
  first looked: Letta's Constellation reads like live task handoff, but its
  own docs are explicit that "the environment only determines where tools
  run" — the agent's reasoning loop lives permanently in Constellation's
  cloud and never relocates; machines are selectable execution targets for
  the *next tool call*, i.e. Sandbox instances, already covered by
  Harness–Sandbox above, not a Harness–Fleet-manager crossing at all.
- **Harness – Identity: yes, one-way (Identity → Harness).** pieces.md
  already states this directly: Harness "reads Skills... straight from the
  identity the same way it reads any other data, not as a per-run binding,"
  bypassing Memory provider for this one category. Verified against real
  prior art (fork, 2026-07-13), not just Mojo's own doc: OpenFang confirms
  the same split structurally — "memory and skills access bypass the
  scheduler entirely... `openfang-skills` supplies the skill registry that
  the loop consults during execution," a distinct, direct channel from the
  runtime, separate from the general `openfang-memory` service. Letta's core
  memory (pinned directly in-context, agent-editable) versus archival memory
  (queried via tool call) is a similar two-tier split, though Letta's direct
  tier is working memory, not a skills-file equivalent specifically, worth
  noting as a partial match rather than an exact one.

**Flagged for later, not resolved here:**

- Harness–Kernel's exact layer structure (how many checks, where each sits,
  which piece initiates which) is real but genuinely varies across real
  systems. Belongs to step 3 classification, not the existence pass.
- Harness–Fleet manager's "no" is solid on the runtime-relocation question,
  but it surfaced a real open design question for whenever Fleet manager's
  own seams (k, n) actually get walked: should the MSI foreclose a
  company's hosted cloud service as a legitimate Fleet-manager
  implementation, or should that be an owner's choice, the same way hiring
  a frontier model is a choice, as long as the contract still guarantees
  portability and no lock-in regardless of topology? The older research
  pass archived in research-plan.md (2026-07-12) reads as foreclosing
  external cloud custody outright ("what's foreclosed is an external,
  cloud-authoritative single point the way Constellation is") — that framing
  is now considered too strong and needs revisiting when that row is
  walked, not edited into the archive now.
- Constellation's own shape (permanent cloud-side state custody, machines as
  selectable execution targets) doesn't map cleanly onto Fleet manager *or*
  Identity as currently defined. A real gap worth surfacing when Fleet
  manager's row is walked, not resolved here.
- Tools ↔ Provenance (the actual tagging mechanism for fetched content) is a
  real, separate crossing, not yet checked. Belongs to Tools' own row.

**Row closed 2026-07-13.**

---

## Model endpoint
