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

- **Model endpoint – Tools: no.** Tried: a tool that internally calls an LLM
  to do part of its own work (a "summarize this page" tool). That call sits
  inside the tool's own opaque implementation, the same principle already
  established for Tools (MCP, adopted as-is, nothing inside a tool is
  standardized, only the Harness↔Tools boundary). Flagged, not resolved: no
  system checked gives a tool its own model-endpoint grant distinct from the
  Harness's; a tool-scoped model budget or credential is a real open
  question if it ever surfaces, no evidence either way today.
- **Model endpoint – Memory provider: yes, two-way.** A provider embeds
  content at index time and embeds queries at retrieval time by calling an
  embedding-typed endpoint directly, distinct from whatever chat model the
  Harness is using and not mediated by Harness at all. Verified against real
  prior art (fork, 2026-07-14), two independent systems: Letta keeps
  `embedding_config` fully separate from `llm_config` in its own docs
  (Agent/LLM Config/Embedding Config are three distinct settings), and a real
  Letta issue (#3210) exists precisely because archival memory sometimes
  ignores the configured embedding model and falls back to OpenAI's default,
  a bug that only makes sense if the two are architecturally separate calls;
  Khoj's docs describe a bi-encoder "search model" that builds document/query
  vectors, explicitly separate from the "chat model," re-indexing required if
  you switch it.
- **Model endpoint – Sandbox: no.** Tried: vendor-hosted code execution
  (Anthropic's hosted code-execution tool, OpenAI's Code Interpreter
  equivalent), where the vendor's own infrastructure runs the sandboxed
  process and returns the result inside the same API response. Real and
  current (Anthropic's docs, `anthropic-experimental/sandbox-runtime`), but
  opaque behind the modality-typed endpoint boundary, the same
  outside-Mojo's-boundary logic already used for Credential broker/password
  manager, not a crossing this project models. Flagged, not resolved: is a
  vendor-bundled, ungated execution surface compatible with a conformant
  endpoint at all, since it bypasses Kernel enforcement entirely? A real
  conformance-profile question for whenever Model endpoint's own seams get
  walked.
- **Model endpoint – Router: no.** Decomposes into the already-established
  Router→Harness grant (one-time, at run assembly) plus the already-settled
  Harness↔Model-endpoint crossing above; re-confirmed, not just assumed, by
  the same five-system finding already cited there (the session's model
  choice is config/org-policy only, never a live per-run negotiation).
  Checked one real counter-pattern before accepting this: LiteLLM's Router
  does live health checks, latency-based routing, and per-deployment
  cooldowns against model endpoints, a genuine continuous liveness signal.
  But that logic sits inside the Harness's own outbound call path (a
  library/proxy the harness's client points at, load-balancing across
  redundant deployments of the same logical endpoint), not a separate Router
  piece re-contacting Model endpoint after assembly. Doesn't override the
  finding, but worth a second look at step 3 classification since it shows
  the live-routing pattern is real somewhere in the stack.
- **Model endpoint – Kernel: no.** Already stated directly in the
  Harness–Model-endpoint entry above: whether a particular call is allowed at
  all isn't a separate mechanism, it's one more consequential action that
  seam already covers. Confirming, not re-deriving.
- **Model endpoint – Credential broker: yes, one-way (broker → point of
  call), conditional on the endpoint requiring authentication.** Distinct
  from the already-settled Harness–Credential-broker "no" (which only tested
  a tool hand-off scenario): here the scenario is authenticating the outbound
  call to a vendor-hosted or authenticated self-hosted Model endpoint.
  pieces.md's own broker definition already names "a model's context" as one
  of three places a secret must never sit, alongside "a harness's hands" and
  "a sandboxed action's own view" — this crossing was already implied by the
  piece definition, just not yet verified until now. The secret goes to
  whatever code makes the outbound HTTP call, injected as an auth header at
  the point of transmission; the model itself never receives it as input, no
  more than a person sees their own front-door key as one of the words on
  their to-do list. OpenFang's already-cited quote reads as being about
  exactly this case on closer read: "API key resolution from vault returns
  `Zeroizing<String>`, meaning the secret exists only in memory long enough
  for transmission" describes the vendor-model-API-key pattern specifically,
  injected at the point of the outbound call, never resting statically
  anywhere. Conditional, not universal: a local, unauthenticated endpoint
  (vLLM or Ollama on the owner's own machine, no `--api-key` set) has no
  secret to hand over at all, so this crossing simply doesn't fire for that
  deployment, it doesn't change the "yes" for any endpoint that does require
  auth.
- **Model endpoint – Provenance: no.** Tried: does the model's own completion
  get tagged by Provenance before Harness receives it, a direct exchange? No,
  Model endpoint has no provenance-shaped concept, the same pattern already
  established for it having no client- or owner-shaped concept either.
  OpenFang's taint labels for "untrusted agent outputs" attach at the point
  content re-enters the run, which is the already-settled Harness–Provenance
  crossing (one-way, Provenance → Harness, content arrives already tagged),
  not a new one.
- **Model endpoint – Fleet manager: no.** Tried: a self-hosted endpoint on one
  machine (vLLM on the owner's desktop) being reachable by another machine's
  Router. That's Fleet manager keeping a shared registry in sync, data read
  later by each machine's own Router, the same "no" shape as Owner–Router
  (data at rest, not a live protocol), not a live Fleet-manager↔Model-endpoint
  call; the model endpoint itself has no fleet-shaped concept.
- **Model endpoint – Identity: no.** Tried: a personalized or fine-tuned
  model whose weights live in identity storage, loaded directly by the
  endpoint, the Harness–Identity precedent for Skills. Not found in any of
  the five systems checked: all five personalize through context/memory/
  persona injection at call time (the existing Harness↔Model-endpoint path),
  not fine-tuned weights. Flagged as speculative only, worth revisiting if
  local fine-tuning or LoRA-in-identity ever becomes a real adopted
  technique.

**Flagged for later, not resolved here:**

- Tools – Model endpoint's tool-scoped grant question (above) is speculative
  only, no real system evidences it either way.
- Sandbox – Model endpoint's vendor-bundled execution surface is real and
  current, and raises a genuine conformance question for Model endpoint's own
  profile once that gets written, not resolved here.
- Router – Model endpoint's "no" holds, but LiteLLM's live health-checking/
  latency routing shows a real live-liveness pattern living inside Harness's
  outbound call path. Worth a second look when Harness gets classified at
  step 3, since it's a live signal this row's "no" doesn't have anywhere else
  to record.

**Row closed 2026-07-14.**

---

## Tools

- **Tools – Memory provider: no.** Tried: a tool doing retrieval calling out
  to a memory provider mid-call. Any tool that does retrieval is itself a
  Memory-provider implementation, a distinct, already-profiled piece, not a
  generic Tool calling one. Verified (fork, 2026-07-14): MCP's only
  server-initiated callback is `sampling` (a server asks the *client* to run
  a model completion, explicitly "no server API keys necessary" because it
  routes through the client), no equivalent memory-callback exists in the
  spec; Letta and Khoj's memory-retrieval APIs sit in their own access path,
  architecturally separate from generic tool-calling, the same split already
  established in the Client/Harness–Memory-provider rows.
- **Tools – Sandbox: no.** Decomposes into the already-settled
  Harness–Sandbox crossing: Harness dispatches the tool call into a sandbox
  and gets the result back; Sandbox decides nothing itself (placement, not
  permission), so Tools has no independent channel to it. Whatever a tool
  does with its own internal execution is opaque, the same logic as Model
  endpoint–Sandbox's vendor-hosted-execution finding.
- **Tools – Router: no.** Verified (fork, 2026-07-14): MCP spec docs and AWS
  Bedrock AgentCore's Gateway-target docs both put `tools/list` discovery and
  the connection handshake inside "the gateway/runtime layer" or "the agent
  runtime" itself, not a separate scheduler component. Router only grants
  which tools are configured, once, at assembly (already Router→Harness);
  Harness does the live connection and discovery itself.
- **Tools – Kernel: no.** Already covered by Harness–Kernel: enforcement is
  entirely harness-side (Claude Code's docs, already cited: rules "enforced
  by Claude Code, not by the model"). No system found where an MCP server
  itself calls back to a kernel-like piece for authorization; MCP's OAuth
  resource-server token validation is caller authentication, a different
  mechanism from Mojo's consequential-action gating.
- **Tools – Credential broker: no**, decomposes into Sandbox–Credential
  broker. Corrected 2026-07-14, resolved when Sandbox's own row was walked:
  first pass read this as a genuine disagreement between OpenFang (secret
  substituted into the sandboxed execution at egress) and OpenClaw (secret
  resolved into the MCP server process before it spawns), different target,
  different timing. Tie broken by Mojo's own already-written definitions
  rather than forcing a read of the two disagreeing systems: Credential
  broker's own rule (pieces.md) names exactly three places a secret must
  never rest — "a model's context, a harness's hands, or a sandboxed action's
  own view" — Tools isn't a fourth. Sandbox's own definition is "wherever a
  permitted action actually executes," so a tool's call running *is* Sandbox,
  not a separate resting place. OpenFang's pattern (placeholder in the
  sandbox, real secret substituted only at the network boundary) is the one
  that actually satisfies Mojo's own rule; OpenClaw's spawn-time injection
  reflects an architecture that doesn't cleanly separate "the tool's process"
  from "a sandbox boundary" the way Mojo's split assumes, not evidence
  against the crossing. See Sandbox–Credential-broker below for the settled
  entry.
- **Tools – Provenance: yes, one-way (tagged at the point Tools returns
  fetched content).** Resolves the flag left in Harness's row: when a tool
  fetches external content (a web search, a scraped page, an API response),
  it gets tagged untrusted/external right at the moment the tool hands it
  back, not upstream or downstream of that boundary. Verified (fork,
  2026-07-14): OpenFang's taint system keys labels by source, with
  `ExternalNetwork` a named category propagated "from ingestion to output,"
  ingestion for fetched content being exactly the tool-return moment;
  Anthropic's own prompt-injection guidance independently wraps tool-result
  content in delimiters as untrusted at the same boundary. Two independent
  sources naming the same point.
- **Tools – Fleet manager: no.** Same shape as every other Fleet manager pair
  so far: data at rest, read by Router, no live cross-machine call. No system
  checked coordinates tool availability live across machines; OpenClaw
  explicitly lacks cross-device sync (already cited in Harness–Fleet
  manager). A device-specific tool is a static config fact Router reads at
  assembly.
- **Tools – Identity: no.** Tool registration/config reads as owner policy
  data, the same "written through a Client, read later as data" shape as
  Owner–Router, unlike Skills. No system checked stores a tool registry as
  identity-owned memory that Harness reads directly; OpenClaw's plugin/env
  config reads as app-level config, not personal identity data.

**Row closed 2026-07-14, Credential broker entry corrected 2026-07-14 after
Sandbox's row resolved the disagreement (see above).**

---

## Memory provider

- **Memory provider – Sandbox: no.** Tried: provider background work
  (indexing jobs, embedding batches) running inside a Kernel-granted,
  Harness-dispatched sandbox. Verified (fork, 2026-07-14): Letta's
  sleep-time agents and Mem0's async background jobs both run as the
  provider's own internal process, never as a permitted action placed into a
  sandbox. Same shape as Model endpoint–Sandbox and Tools–Sandbox: a
  service's private internal execution isn't a modeled crossing.
- **Memory provider – Router: no.** Tried: an external scheduler triggering
  consolidation or reindexing the way Router assembles a run. Verified
  (fork, 2026-07-14): Letta's sleep-time agents run "in the background
  between conversations," server-triggered internally, not Router-assembled;
  Mem0's async memory writes happen inline in the provider's own
  architecture. No system found where an external Router-shaped piece
  schedules a provider's background work as a run.
- **Memory provider – Kernel: no**, decomposes into the already-settled
  Harness–Kernel crossing, better-evidenced on a second look than the first
  pass found. OpenFang's own capability manifest declares `memory_read` and
  `memory_write` as capability types (`MemoryRead(String)`,
  `MemoryWrite(String)`) in the exact same declaration system as tool
  capabilities, checked by `openfang-kernel`'s RBAC — the "memory... bypass
  the scheduler entirely" line already cited elsewhere (Harness–Identity) is
  about the *scheduler* (Router's prior art) being out of the loop, a
  different OpenFang component from the kernel, not evidence the kernel gate
  is skipped too. So Harness checks the capability before ever calling into
  the provider, same gate as everything else, not a separate
  provider→kernel call.
- **Memory provider – Credential broker: yes, one-way (broker → point of
  call), conditional on the backend requiring auth.** Distinct outbound call
  from the already-settled Model endpoint–Credential-broker (that's the
  embedding model call; this is the vector-store connection itself).
  Verified (fork, 2026-07-14): Pinecone is SaaS-only, no self-hosted option,
  always requires an API key; self-hosted Weaviate/Qdrant can run with or
  without auth, cloud-hosted requires it. Same conditional shape as Model
  endpoint's row: fires only when the storage backend is authenticated,
  never for a local, unauthenticated store.
- **Memory provider – Provenance: open, not resolved here.** No real system
  checked documents what happens to a trust/taint tag across a memory
  write-then-read round trip, laundered to trusted on write or carried
  through to retrieval. A real, not just a disagreement-between-systems,
  gap: prior art has no answer either way (fork, 2026-07-14), so per
  AGENTS.md's own rule this stays an open question, not an asserted finding.
  Recorded below for a real decision, not decided in this pass.
- **Memory provider – Fleet manager: no.** Same shape as every other Fleet
  manager pair: data at rest, read by Router, no live cross-machine call.
  Verified (fork, 2026-07-14): no system found does live cross-machine
  provider-instance selection; Obsidian/Khoj vault sync is file-level
  background sync, not a routing decision between provider instances.
- **Memory provider – Identity: yes, two-way.** *Read side* is directly
  evidenced: Khoj's `fs_syncer` module "discovers and reads files from
  configured sources" straight from the vault to build its index, no
  intermediary — the provider's own access to the underlying store, distinct
  from the already-settled Client/Harness–Memory-provider pairs (those are
  callers *using* the provider, not the provider's own read of the data it
  indexes). *Write side is Mojo's own deliberate design choice, not current
  practice*, and worth stating plainly as exactly that rather than dressing
  it up as prior art: pieces.md's rule that a provider's index must be
  rebuildable from identity data alone, writes landing in the data's
  standard shape rather than trapped in any one provider's private state,
  doesn't match any of the five systems checked (fork, 2026-07-14) — OpenFang
  and Letta's archival store both *are* the canonical store, not a derived
  index written back to a separate identity layer. The reason to keep it
  anyway: it's the same anti-lock-in principle the hotswap demo tests for
  Harness and Model endpoint, applied to Memory provider — swap the
  provider, the data survives, because the data was never the provider's to
  keep.

**Flagged for later, not resolved here:**

- Memory provider–Kernel's general gate holds (above), but real systems show
  a live, currently-unaddressed gap one level finer: per-document or
  per-agent memory access control. Letta has an open issue titled "Memory
  governance — policy enforcement for stateful agent memory" (#3320), and a
  real paper (FragFuse, arXiv 2606.15609) documents memory-based
  access-control bypass. Not solved by the Harness–Kernel decomposition,
  since that gate answers "can this harness touch memory at all," not "can
  it touch this specific fact."
- Memory provider–Provenance (above): genuinely unresolved, no prior art
  either way. Leaning, not decided: tags should survive the write rather than
  reset to trusted on landing in memory, the same "propagates from ingestion
  to output" principle OpenFang already uses for tool-fetched content
  (Tools–Provenance), treating a memory write as one more sink along that
  path rather than a boundary that launders trust. The reset-to-trusted
  alternative is a real memory-poisoning path: untrusted content summarized
  into memory would later be treated as the assistant's own established
  fact. Needs an actual decision, not asserted here as settled. **Merged
  2026-07-14 with Provenance–Identity's own open flag (Provenance's row,
  below): Identity is this piece list's actual canonical store, Memory
  provider is explicitly the derived, rebuildable index over it, so this is
  the same question restated one layer down, not a second independent
  unknown. One decision answers both; don't resolve one and leave the other
  inconsistent.**

**Row closed 2026-07-14.**

---

## Sandbox

- **Sandbox – Router: no.** Decomposes into the already-established
  Router→Harness grant (Router grants a sandbox option once, at assembly,
  per pieces.md's own definition of Router granting "a set of each service...
  to draw from") plus the already-settled Harness–Sandbox dispatch.
  Verified (fork, 2026-07-14): OpenClaw's docker-exec dispatch, already
  cited there, comes from the Agent Runtime, never the Gateway. Same shape
  as every other X–Router "no" in this pass.
- **Sandbox – Kernel: yes, one-way (Kernel → Sandbox), distinct from
  Harness–Kernel.** Not a decomposition this time, a genuine second,
  resource-side enforcement layer. Verified (fork, 2026-07-14): OpenFang
  runs "capability gates and WASM sandboxing at the kernel level, not via
  LLM instructions... tool code runs inside WASM with dual metering (fuel +
  epoch interruption), file operations are workspace-confined, subprocesses
  are env-cleared and timeout-enforced," explicitly framed as defense-in-
  depth alongside, not instead of, the dispatch-time capability check.
  Matches, independently, the Claude Code quote already cited in
  Harness–Kernel's own entry: "sandbox restrictions prevent commands from
  reaching resources outside defined boundaries, even if a prompt injection
  bypasses Claude's decision-making" — the same system naming two distinct
  layers, dispatch-time (Harness–Kernel) and resource-side (Sandbox itself
  enforcing the boundary Kernel configured). Kernel sets the boundary (what
  paths, what network egress) at instantiation; the sandbox runtime enforces
  it continuously for the execution's duration.
- **Sandbox – Credential broker: yes, one-way (broker → point of call),
  conditional on the action needing an external credential.** Resolves the
  disagreement carried forward from Tools' row (corrected there, see
  above): pieces.md's own Credential broker definition names exactly three
  places a secret must never rest — "a model's context, a harness's hands,
  or a sandboxed action's own view" — Tools isn't a fourth. Sandbox's own
  definition, "wherever a permitted action actually executes," makes a
  tool's call running *this* place, not a separate one. Verified (fork,
  2026-07-14): OpenFang's pattern (a placeholder stored in the sandbox,
  substituted with the real secret only at network egress, when the action
  initiates an outbound request) is the one that satisfies Mojo's own rule
  directly, the real secret exists only at the outbound boundary, never
  resting in the sandboxed process's own view.
- **Sandbox – Provenance: yes, one-way, tagged at the point execution output
  exits the sandbox.** A real, separate crossing from Tools–Provenance, not
  the same one under a different name: Tools tags what it fetches from
  outside; this tags what the execution itself produces, before either
  reaches Harness. Verified (fork, 2026-07-14): OpenFang has a distinct
  `SystemCommand` taint category (shell commands) alongside
  `ExternalNetwork`/`ExternalInput` (the category behind Tools–Provenance),
  "environment variables are wiped before executing skills or shell
  commands, then selectively injected via an allowlist," data tagged by
  source with checks performed "at sinks like shell, net_fetch, or
  agent_message."
- **Sandbox – Fleet manager: no.** Same shape as every other Fleet manager
  pair: registry data, read by Router at assembly, no live cross-machine
  call at execution time. Verified (fork, 2026-07-14): Letta's Constellation
  environment selection, already found in Harness–Fleet-manager's own entry
  to be sandbox-instance selection at assembly rather than a live call,
  covers this pair too, no counter-evidence found.
- **Sandbox – Identity: no.** Environment/execution config lives as
  deployment config, not identity-owned personal memory. Verified (fork,
  2026-07-14): Hermes (`~/.hermes/config.yaml`, `TERMINAL_ENV`) and Letta
  (`TOOL_EXEC_DIR` env var) both keep this as operator/deployment
  configuration. Same shape as Tools–Identity's "no," app-level config,
  unlike Skills.

**Row closed 2026-07-14.**

---

## Router

- **Router – Kernel: no**, decomposes into Router–Identity below plus the
  already-settled Harness–Kernel. pieces.md's own text is explicit: "hard
  limits are the owner's policy data, which a router obeys. They are never
  properties of the router itself" — Router obeys config data, it doesn't
  make a live call to Kernel to check whether assembly is allowed. One
  counter-signal flagged, not enough to overturn this (fork, 2026-07-14):
  OpenFang's manifest bundles "scheduler" and "RBAC, budget tracking" under
  the same `openfang-kernel` crate, suggestive of a real pre-spawn
  rate-limit/budget gate, but no public doc confirms it's a live check
  distinct from config-reading. Belongs to step 3, not decided here.
- **Router – Credential broker: no, speculative only, no evidence either
  way.** Checked (fork, 2026-07-14): Khoj's OAuth setup is for harness/tool-
  level integrations, not scheduler trigger-polling. No system checked has a
  scheduler holding its own credential to poll an external trigger source
  (webhook, monitored inbox); real trigger types found in practice are
  clock-based (no credential) or inbound-request (receiving, not
  authenticating outbound). Same flagged-but-unevidenced treatment as
  Tools–Model-endpoint's tool-scoped-grant question.
- **Router – Provenance: no**, decomposes into Harness–Provenance. Router
  decides whether and how to assemble a run; it doesn't interpret the
  substantive content of what triggered it. That content's trust-tagging
  happens once it's inside the run, already covered by the settled
  Harness–Provenance crossing.
- **Router – Fleet manager: open, not resolved here.** pieces.md already
  asserts this directly ("keeping several machines' routers from stepping on
  the same trigger is the fleet manager's job, not the router's own"), a
  live peer-coordination problem distinct from every other Fleet manager
  pair in this pass (all of which are one-time config reads, not real-time
  coordination). But it's the first pair in the whole existence pass with
  zero real-system verification available, not a disagreement between
  systems, an absence: none of the five checked (fork, 2026-07-14) run
  personal multi-device fleet coordination at all (OpenClaw has no
  cross-device sync; Letta's Constellation is cloud-multi-tenant, many users
  sharing infra, not one owner's several personal machines). The general
  distributed-systems pattern (leader election, Kubernetes CronJob's
  single-active-controller, Quartz Scheduler clustering) is real engineering
  practice but not evidenced in any comparison system here. Left open rather
  than asserted from Mojo's own text alone, per AGENTS.md's grounding rule;
  decided when Fleet manager's own row is walked, which is itself likely
  Horizon-scoped (informative layer, not MSI-1 normative text) rather than
  part of the first draft, per making-the-standard.md's Scope and growth
  section.
- **Router – Identity: yes, one-way (Identity → Router).** Router reads its
  own trigger/schedule/budget/policy config straight from identity-owned
  data, the same shape as the already-settled Harness–Identity (Skills read
  directly, no intermediary). Reasoned primarily from Mojo's own
  already-committed definitions: pieces.md lists "the owner's policies and
  budgets" explicitly under what Identity holds, and Router's own definition
  says it "obeys" that data. Weakly evidenced externally (fork, 2026-07-14):
  Khoj's automation config storage doesn't document where it lives relative
  to the user's own data, inconclusive rather than confirming. Flagged with
  the same honesty standard as Model endpoint–Credential-broker: already
  implied by the piece definition, not yet independently verified.

**Row closed 2026-07-14.**

---

## Kernel

- **Kernel – Credential broker: yes, one-way (broker checks authorization
  before releasing a secret), evidence sourced outside the five canonical
  comparison systems.** None of OpenClaw/Hermes/Khoj/Letta/OpenFang's public
  docs specify whether the broker independently re-checks authorization
  before injecting a secret, versus trusting that a disallowed action never
  reached that point. Verified against a broader real-world agent-security
  catalog instead (fork, 2026-07-14): Warden ("agent presents JWT/SPIFFE
  SVID, Warden injects real credentials per-request, agent never holds
  secrets"), Riptides ("kernel-space interception... on-the-wire injection...
  via SPIFFE workload identity"), the "phantom token pattern." All separate
  the authorization decision from the calling application, the same shape as
  Sandbox–Kernel's resource-side defense-in-depth layer. Flagged honestly: a
  strong, repeated pattern, but off-roster sourcing, not one of the five
  systems this pass otherwise checks against.
- **Kernel – Provenance: yes, one-way (Provenance tag → Kernel decision),
  genuinely new, not a decomposition into Harness's passive receipt of
  already-tagged content.** Directly confirmed (fork, 2026-07-14): OpenFang's
  own docs state the kernel "can enforce policies like 'tainted content
  cannot trigger tool calls' at the kernel level, not just as a suggestion to
  the LLM." A taint tag actively gating a permission decision, a second real
  crossing alongside the already-settled Harness–Provenance one, the same way
  Sandbox turned out to be a real exception to the usual X–Kernel
  decomposition pattern rather than folding into it.
- **Kernel – Fleet manager: no**, decomposes into Kernel–Identity below plus
  Fleet manager's already-described job of keeping identity data (which
  includes policy) synced across machines. pieces.md's own text, "all of an
  owner's machines answer to one shared policy," reads as machines sharing
  synced identity data, not Kernel making a live call to Fleet manager.
  Distinct from Router–Fleet-manager's live trigger-claiming problem (left
  open in Router's row): policy propagation can tolerate eventual
  consistency, no live coordination need found or expected, so this one
  doesn't carry the same unverified-live-coordination flag.
- **Kernel – Identity: yes, two-way.** Read side is solid (fork, 2026-07-14):
  three real systems store permission/policy config as plain owner-authored
  files, not a separate database or cloud service — Claude Code
  (`settings.json`), OpenFang (capability manifests, declared per-agent),
  Codex CLI (`config.toml`'s `approval_policy`) — same shape as Router
  reading trigger config from Identity. Write side (the audit trail:
  pieces.md's "records what happened, so 'what did it do, and who allowed
  it' always has an answer") matches Identity's own definition almost
  verbatim ("a record of who wrote what under what authority"), but is
  weakly evidenced externally, none of the three systems checked publicly
  document their permission logs living in the same plain-file location as
  user data specifically. Flagged with the same honesty standard as
  Router–Identity's write side: implied directly by Mojo's own definition,
  not yet independently confirmed.

**Row closed 2026-07-14.**

---

## Credential broker

- **Credential broker – Provenance: no**, decomposes into the already-settled
  Kernel–Provenance (Kernel gates on trust tags) plus Kernel–Credential-
  broker (the broker executes custody only after Kernel has already
  decided). Verified (fork, 2026-07-14): real credential-broker writeups
  (TRM Labs, Infisical's Agent Vault) describe release as purely caller-
  identity/policy-gated, "the broker swaps the placeholder for the real
  credential as the request leaves," with no content-trust-tagging layer
  inside the broker itself. Custody and injection are orthogonal to
  trust-tagging; that decision already lives one layer up.
- **Credential broker – Fleet manager: no**, same decomposition shape as
  Kernel–Fleet-manager, not Router–Fleet-manager's live trigger-claiming
  problem. pieces.md's "custody is exactly the kind of fact two instances
  must never disagree about" reads as a strictness requirement, not a
  live-coordination one. Verified (fork, 2026-07-14): HashiCorp Vault's own
  docs describe writes forwarding to a single primary cluster, replication
  to secondaries "near real-time... but not instantaneous," secondaries
  stalling reads briefly to mask staleness rather than requiring live
  locking. Single-primary-writer, eventually-consistent-but-strict is a
  well-evidenced real pattern, not an open question the way Router's was.
- **Credential broker – Identity: no.** Backend/location config (which
  vault, which keychain) lives as separate deployment/system config, not
  identity-owned personal data, same shape as Sandbox–Identity's and
  Tools–Identity's "no." Verified (fork, 2026-07-14): OpenClaw's vault-
  provider config (`SecretRef` definitions, exec/env/file provider setup)
  lives in `openclaw.json`, explicitly separated from the secret values
  themselves ("you can back up `openclaw.json`... without including the
  actual secret values"). No system checked stores broker backend selection
  as identity/personal-data-shaped content.

**Row closed 2026-07-14.**

---

## Provenance

- **Provenance – Fleet manager: no**, decomposes into whatever syncs the
  tagged record itself, same shape as Kernel–Fleet-manager and
  Credential-broker–Fleet-manager (eventual consistency, not Router's live
  single-claim problem). A trust tag is just another field on a record; once
  written, it travels with the record through whatever replication mechanism
  already carries that record across machines, no separate live-coordination
  protocol needed for the tag specifically. Reasoned by analogy (fork,
  2026-07-14) to the general metadata-travels-with-data pattern, not a
  direct taint-specific finding, since none of the five systems run a
  personal multi-device fleet (the same absence already noted for Router's
  still-open Fleet-manager pair) — but nothing here suggests exactly-once or
  liveness semantics the way trigger-claiming does, so this sits at the same
  confidence tier as Kernel's and Credential broker's clean decompositions,
  not Router's open flag.
- **Provenance – Identity: real crossing, open, not resolved here, and the
  same underlying question as Memory provider–Provenance's own open flag,
  restated one layer down.** Identity is this piece list's actual canonical
  store (Memory provider is explicitly the derived, rebuildable index over
  it), so whether a trust tag survives being written durably is more
  fundamentally an Identity-level question; Memory provider's version is
  downstream of whatever the answer is here. Two data points found (fork,
  2026-07-14), neither from the five canonical systems, sharpening the
  question rather than settling it: US Patent 11374966 exists specifically
  because the default failure mode is tags getting silently dropped on
  serialization to storage ("re-serialized into a purely character-based
  string representation, causing all taint information to be lost");
  Microsoft Purview's sensitivity labels are a shipped counter-example in an
  adjacent domain (data classification, not prompt-injection specifically)
  proving persistent metadata is achievable, labels "remain permanently
  attached to the file, even if it leaves the M365 environment." None of
  OpenClaw/Hermes/Khoj/Letta/OpenFang document this for their own memory.
  Merged with Memory provider's flag rather than tracked as two independent
  unknowns (see that row's "Flagged for later," corrected 2026-07-14): one
  decision answers both.

**Row closed 2026-07-14.**

---

## Fleet manager

- **Fleet manager – Identity: yes, two-way, well-evidenced.** Unlike
  Router–Fleet-manager (still open), this is Fleet manager's actual reason
  to exist, not another piece's crossing decomposing through it: pieces.md's
  own definition already says Fleet manager "coordinates the set so one
  identity stays coherent across all of it: sync, conflict, partition, and
  who is driving a live task." Fleet manager both reads and writes Identity
  data as the mechanism keeping multiple machines' copies consistent, a
  genuine two-way relationship, not one side owning a static fact the other
  polls. Verified (fork, 2026-07-14) against real prior art at two scales:
  Matrix (already this project's own cited protocol precedent) does exactly
  this shape for several devices belonging to one identity, live event-driven
  device-list sync pushed to remote servers, plus State Resolution v2, an
  explicit algorithm for merging conflicting state and healing a network
  partition without any device's history silently rolling back. Closer to
  personal scale: Obsidian Sync merges concurrent edits with Google's
  diff-match-patch, falling back to "last modified wins" or conflict files
  for non-text; CRDT-based alternatives (Yjs-based Local Sync) do zero-
  conflict automatic merging with no manual step. Three real,
  independently-arrived-at approaches to the same problem.

**Flagged for later, not resolved here:**

- pieces.md's own Fleet manager definition bundles two genuinely different
  distributed-systems problems into one sentence: "sync, conflict,
  partition" (data consistency, this pair, real evidence) and "who is
  driving a live task" (mutual exclusion / leader election, a different
  mechanism, locking rather than merging). This pair only closes the
  data-merge half. It does not resolve Router–Fleet-manager, which stays
  open exactly as recorded there: live, exactly-once trigger-claiming,
  unverified against any of the five comparison systems. No interaction
  found with the merged Memory-provider/Provenance tag-persistence flag
  either, an orthogonal question (trust-tag semantics, not sync mechanics)
  that happens to also touch Identity.

**Row closed 2026-07-14.**

---

## Existence pass complete

All 78 pairs across the 13 nodes (Owner, Client, Harness, Model endpoint,
Tools, Memory provider, Sandbox, Router, Kernel, Credential broker,
Provenance, Fleet manager, Identity) are resolved as of 2026-07-14. Three
items carried forward as genuinely open rather than forced, per
seam-method.md's own instruction to check against real systems before
treating anything as settled:

- **Router – Fleet manager**, live trigger-claiming, unverified against any
  of the five comparison systems (none run a personal multi-device fleet).
  Decided when Fleet manager's own seam gets walked at steps 3-7, itself
  likely Horizon-scoped per making-the-standard.md.
- **Memory provider–Provenance / Provenance–Identity (merged)**, whether a
  trust tag survives a durable write or resets to trusted, no prior art
  either way, a real decision still needed, not an existence-pass fact.
- **Tools – Model endpoint**'s speculative tool-scoped model grant, and
  **Router – Credential broker**'s speculative trigger-source credential:
  both flagged no-evidence-either-way, lowest-stakes of the open items.

Next per seam-method.md: step 2 (the seam-worthy filter) across all
surviving crossings, then steps 3-7 one seam at a time per session
(structural classification, load-bearing problems, shape match,
substitutability stress test, recording into seams.md as `settled`).

---
