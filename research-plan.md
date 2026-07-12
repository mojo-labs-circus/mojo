# Research Plan & Tracker

*How the Mojo System Interface gets worked out, and the living status of doing
it. Re-derived from [anatomy.md](anatomy.md) on 2026-07-11: the anatomy's
pieces and lettered seams are the organizing structure now, and everything in
here is keyed to them. Distinct from [devlog.md](devlog.md) (the raw, dated
reasoning trail) and `msi.md` (the draft standard itself, which doesn't exist
yet, on purpose). Nothing in this file is true by virtue of being written down.
The Status column is the only thing that means anything, and most of it says
Open on purpose. The prior-art notes carried into the seam rows below were
earned in earlier sessions (see devlog) and are inputs to the walk, not
outcomes of it: a seam walked honestly can overturn any of them.*

---

## The method, taken from how POSIX actually works

POSIX.1 is the model, taken seriously rather than as a slogan:

- **The normative content is interfaces only.** POSIX never defines what a
  kernel or a scheduler is; it defines observable behavior at a boundary, one
  interface page at a time. Nobody conforms to a description of a piece, they
  conform to the contracts the piece sits behind. For the MSI that means:
  anatomy.md's piece prose is informative, the seam contracts are the
  normative spec, and **a piece's real definition is the union of the
  seam-sides it must implement.**
- **Conformance is against profiles, not the whole document.** POSIX has
  option groups; the MSI has ten conformance profiles, each a named bundle
  of seam-sides. §0 gets written at assembly time out of what actually
  landed, never guessed up front.
- **Rationale is non-normative and kept separate.** The why of each decision
  goes in an appendix, distilled from the devlog, not mixed into the
  contracts.
- **It codified existing practice and was drafted iteratively.** POSIX wrote
  down fifteen years of running Unix; OSI wrote its spec first and lost to
  TCP/IP's running code. So: prior art checked before anything is invented,
  sections drafted the session their seam lands, and the assembly pass at the
  end is a coherence read, not a writing project.

## The target: msi.md

**MSI-1** (the first version, the way POSIX-1 was; iterated openly from
there). One file at the repo root until it outgrows that:

- **§0 Conformance.** RFC-2119 language. Ten profiles: Client, Router, Harness,
  Model endpoint, Sandbox, Credential Broker, Provenance, Kernel, Memory
  Provider, Fleet Manager. Each profile lists the seam-sides
  its implementations must satisfy. Written during assembly.
- **§1 Base definitions.** Vocabulary, plus the small set of file-like data
  primitives the identity is built from.
- **§2 The identity schema.** Seam m, the flagship: the data shape, travelling
  permissions, provenance fields, history.
- **§3 Seam contracts.** Every MSI-posture seam, one section each.
- **§4 Adopted-standard bindings.** What adopting the model wire format, MCP,
  SKILL.md, and A2A actually binds a compliant piece to: pinned versions,
  required surface, what the MSI layers on top.
- **Appendix: Rationale.** Non-normative.

## The plan: three phases

**Phase 1 — the piece pass.** Walk the pieces one at a time. For each: get the
anatomy.md prose right (a clear reader should know what the piece is and
isn't), write down exactly which seam-sides its profile owns (the Pieces table
below), and surface what a builder still couldn't answer. Every real gap found
is a seam question and gets recorded in that seam's row, never as more anatomy
prose. A piece that surfaces zero new questions is done; one that surfaces five
just found the real work. If a question has no seam to live in, the anatomy is
missing a seam, and that's a finding, not a filing problem.

**Phase 2 — the seam walk.** One seam at a time, study-then-decide: open with
a briefing on the seam's named prior art, land the answers, draft the msi.md
section the same session. The old nine-leg walk order is superseded; the
proposed order below is dependency-based and revisable.

**Phase 3 — assembly.** Write §0's profiles out of the landed seam-sides, run
a cross-seam contradiction check, read msi.md end to end once for coherence.
That's MSI-1 done, and [roadmap.md](roadmap.md)'s Next (the first system,
released runnable) starts there.

### Working rules (carried forward, still binding)

- **Timebox rule.** A seam that resists landing after one real session gets a
  provisional answer plus an explicit open-question flag, never an indefinite
  Deciding. MSI-1 is complete-coverage-thin on purpose; depth comes from
  versioned iteration once something runs.
- **Minimal-core rule.** MSI-1's mandatory core for any seam is the smallest
  set of obligations that is actually correct, not the most capable one.
  Anything additive (richer transports, new content types, new capabilities)
  is a named, explicitly deferred extension point, never folded into the
  mandatory core to make MSI-1 look more complete than it is.
- **Definition of done, per seam.** The contract is written well enough that a
  stranger could build a compliant piece against it without ever talking to
  me. Anything less isn't Decided.
- **Draft as you go.** When a seam lands, its msi.md section gets drafted the
  same session, while the reasoning is fresh.
- **Nothing already thought is decided.** The `dockets-research/` files, the
  old plan's leans, and every note carried into the rows below are inputs. A
  seam walked honestly can overturn any of them. Only Decided means decided.

**Currently on: phase 1 (piece pass) — in progress.** Client covered
2026-07-11; Peripheral retired 2026-07-12 (collapsed into Tools, no piece,
no profile, no seam); Harness covered 2026-07-13; Model endpoint covered
2026-07-12; Tools covered 2026-07-12. Next unchecked step is 1.6, Memory
provider. *(Updated in place as phases and seams complete; the Status
columns stay the real truth.)*

### Proposed seam walk order (phase 2, revisable)

1. **e, f, h, l (adopted seams).** Cheap, mostly confirmation of what each
   adoption binds us to, and it puts §4 down first. The momentum leg. Flag
   carried: how A2A composes with the federation seam gets revisited at k.
2. **a (owner verification).** Small, and everything else's permissions
   bottom out here; do it before anything that needs "the owner said so."
3. **m (data shape).** The core deliverable and the longest walk. Drafts §1
   and §2. Then **s (memory access)** immediately after: its answer depends
   on m's shape, and its first candidate (collapse into f, providers as MCP
   servers) was already sized up during the adopted-seams leg.
4. **j (enforcement).** Authority as data; the other half of m's travelling
   permissions. Hardest conceptual seam, scheduled right after its
   dependency for exactly that reason.
5. **i (run contract).** Needs m (the fragment's shape) and j (what
   capabilities are handed).
6. **d + g (run assembly, triggers).** The router's two seams; needs i so it
   knows what it's assembling.
7. **p (secret custody), q (sandbox), r (content trust).** Each needs j; none
   needs the others.
8. **c + b (client contract, proactive delivery).** Needs m (what a client is
   handed is fragment-shaped) and g (delivery is the output half of a
   trigger).
9. **k + n (federation, machine admission).** The distributed-systems seams,
   last on purpose: everything single-machine is settled first, and these
   are the hardest rows in the tracker.

---

## Tracker: pieces (phase 1)

The Owner and the identity are listed for coverage but carry no profile: the
owner is a person represented by a credential (seam a's subject), and the
identity is data, not a piece; nothing to conform, only a shape to keep (§1/§2's
subject).

| Piece | Profile | Seam-sides owned | Status | Notes |
|---|---|---|---|---|
| Client | Client | a (owner side passes through it), b (receives delivery), c (its contract) | Covered | Piece pass done 2026-07-11: no session of its own, store-shaped between runs, live only for the duration of a run, needs no AI/harness in the loop to qualify. Checked against real prior art (OpenClaw, Hermes, Letta, Khoj, Matrix, MIME, Kitty) |
| Harness | Harness | e, f, i (run side), q (executes inside), h (reads skills), s (queries providers) | Covered | Piece pass done 2026-07-13: prose tightened (adopted-whole framing, scoped "manages context" to a harness's own live in-run budget), seam-sides confirmed unchanged. Checked against real prior art (OpenAI Agents SDK, Claude Agent SDK, CrewAI, LangGraph, smolagents, OpenClaw's Agent Runtime, Hermes-agent, Letta). Real findings landed on i (no private filesystem, incremental write-back not session-end, subprocess lean, OpenClaw's handoff-object shape), d (trust-tiered selection ties to the existing candidate-transparency flag, OpenClaw's `supports()` predicate as the lean for capability declaration), j (mercenary-tier harnesses are ordinary capability-narrowing, no new mechanism), m (personas confirmed as the mercenary-tier identity mechanism, ideas.md's decoy-cargo sketch carried forward) |
| Model endpoint | Model endpoint | e (serves) | Covered | Piece pass done 2026-07-12: prose broadened to include vendor-hosted endpoints alongside self-hosted (vLLM/Ollama/llama.cpp), trust tier (owned/rented/mercenary) kept invisible to the piece itself, same precedent as Harness's mercenary-tier harnesses (Clarke confirmed the scope call). Checked against real prior art (vLLM, Ollama, llama.cpp current server docs). Real findings landed on e (single-model-per-instance vs. multiplexed-many-models-behind-one-endpoint is a genuine architectural split, vLLM/llama.cpp vs. Ollama, resolved by the wire shape itself, nothing to invent). Renamed from "Model" 2026-07-12, closing the last piece/profile name divergence (same call as Client/Harness) |
| Tools | — (no profile) | f | Covered | Piece pass done 2026-07-12: a Tools instance is one MCP server, exactly parallel to a Model endpoint being one instance; several serve one machine at once (confirmed against Claude Desktop's real `mcpServers` config, which runs several independent servers side by side). Real finding: unlike Model endpoint, a run isn't pointed at one Tools instance, it needs several tool capabilities live at once, so seam f's binding is a roster, a subset of the machine's available servers assembled per run, not a single pick — carried into d and f below. Prose tightened to state this; the retired-Peripheral ground (hardware and software tools are the same MCP shape) re-confirmed, not reopened |
| Memory provider | Memory Provider | m (derives from the data), s (serves runs) | Open | |
| Sandbox | Sandbox | q (provides placement) | Open | |
| Router | Router | d, g | Open | |
| Kernel | Kernel | i (hands the run its contract), j, q (places actions) | Open | |
| Credential broker | Credential Broker | p | Open | |
| Provenance | Provenance | r | Open | |
| Fleet manager | Fleet Manager | k, n | Open | |

**Retired 2026-07-12 (piece pass, 1.2): Peripheral.** Not a piece, no profile,
no seam. Two independent real systems checked (OpenClaw's computer-use/browser
control, Hermes-iOS's camera/mic/health/location/notifications) both route
device capability through the same general tool-calling path as everything
else, one MCP server, no separate contract. The kernel already gates every
consequential action regardless of piece (seam j), so seam o had nothing left
to contract once declaration and invocation-checking are already covered by f
and j. Continuous sensor telemetry (location history, health metrics) isn't
even a live call, it's data landing in storage and queried later, m/s's
concern, not a peripheral one. Ten pieces, ten profiles, ten seams now. Prior
art gathered for the retired seam folded forward: Matter/HomeKit's typed
capability declaration ("clusters"/"characteristics") noted on f as the
richer schema shape to check MCP's tool schemas against; iOS/Android's
static-manifest-plus-runtime-prompt split and the W3C Permissions API's dead
`revoke()` noted on j as prior art for capability risk tiers and the
revocation question.

**Resolved 2026-07-11 (was a flag: the harness ↔ memory provider binding had
no seam letter):** it's seam **s** now, in anatomy.md and the tracker below.
The router picks providers per run, so a harness has to be able to talk to
whichever provider it is handed; without a uniform surface that's a
harness×provider compatibility matrix, the exact thing the standard exists to
prevent. Same reasoning that gave models seam e and tools seam f. Whether the
walk lands an MSI-defined envelope or collapses s into f (providers as MCP
servers) is the walk's call; the seam exists either way.

## Tracker: seams (phase 2)

Status: **Open** (not yet walked), **Deciding** (actively being worked),
**Decided** (landed, drafted into msi.md, reasoning recorded). Posture from
anatomy.md: **MSI** (the standard's own work), **Adopt** (existing standard,
consumed), plus anatomy's third posture, deliberately-not-standardized, which
shows up here as explicit leave-open notes inside rows rather than whole rows.

| Seam | Connects | Posture | Status | Prior art carried forward, and the open questions |
|---|---|---|---|---|
| a | Owner ↔ any client | MSI | Open | The login of the system: how a key-pair/DID-shaped credential proves "the owner said so." Prior art: what a Unix uid mechanically is (referenced by files and processes, itself neither); Plan 9's per-namespace identity; W3C DID/VC specs (unverified); SAIHM's seed-derived post-quantum keypair that never leaves the holder's machine (worked instance, not adopting the protocol, devlog 2026-07-10). Open: root of trust, where does credential #1 come from at install; where keys live day to day; groups/Collectives deferred past MSI-1 (gated) |
| b | System → owner | MSI | Open | That a compliant client can receive an unsolicited delivery; urgency policy is the owner's data. Prior art: ntfy/push conventions; Matrix push gateways; plain email as the degenerate case. The output half of g: a trigger firing is only useful if the result reaches the owner somewhere |
| c | Client ↔ system | MSI | Open | What a client is handed, may cache, must never hold authoritatively; a client holds no session of its own, reads/writes against the identity between runs, live only for the duration of a run addressed to it; includes how a client is pointed at the right one of the owner's machines when more than one exists (folds in here, not a separate seam). MSI-1 scope, minimal-core rule applied: core covers discrete request/response plus async delivery only; real-time continuous-stream clients (voice, phone, eventually video) are a named, explicitly deferred capability extension, not designed in MSI-1. Prior art, checked 2026-07-11: Matrix's device model + `/sync` (one account, many device_ids, clients reconcile against server state, not a permanent push session, confirms the store-shaped reading); MIME `multipart/alternative` (content offered in several representations, a client renders whichever it understands, no central registry of client types needed); Kitty's graphics-protocol query/response as an alternative capability-negotiation shape (client probes, terminal replies only if supported, silence means no); OpenClaw's real architecture (Gateway resolves sessions and dispatches to an ephemeral per-message Agent Runtime, channel adapters hold no state of their own, close match to this seam's shape); Letta's agent/conversation/session split (durable memory server-side on the agent, a session is a disposable client-side connection holding only transient turn state, closest match found); Khoj (self-hosted mode is structurally close to Mojo's own single-machine shape; its Obsidian plugin is real precedent for a non-conversational, direct-identity-editing client). Open question surfaced by OpenClaw: its Command Queue serializes messages per session to prevent state corruption when multiple inputs target one in-progress conversation; c has no answer yet for what happens when two clients address the same live run at once. Flagged 2026-07-12 (Client piece pass, prompted by the borrowed/foreign-hardware case: a friend's laptop, a work machine): Clarke's lean is that caching may not be worth allowing at all, not just scoped down, since a device the owner doesn't own is exactly where cached state is most exposed; not decided, real candidate answer for c's real walk alongside the two-clients-one-run question above |
| d | Task → a run's assembly | MSI | Open | Inputs (task, roster, constraints, budget) and output (a full piece-set, chosen again for every sub-run); selection quality is competitive, hard limits are the owner's policy data, never router properties. Prior art: Slurm's resource-matching concepts (reference, not adoption); model-router products; the closed-vendor pass found nothing resembling a task-aware router (devlog 2026-07-10). Scheduling/priority under contention parked here (AIOS's context-interrupt, snapshot/resume mid-generation, is real working precedent when this comes up). Doing the selection call by hand first; a trained router is implementation, not contract. Flag carried, 2026-07-11 (outside review): authority already sits with the kernel, not the router (anatomy.md's existing split holds), but a router with no formal authority can still become a de facto black box if its selection isn't inspectable; the contract should require a router's decision to expose which candidates existed and why one was chosen, and stay overridable by the owner's policy data, not just correct. Finding, 2026-07-13 (Harness piece pass): trust-tiered harness selection (a chartered open-source runtime vs. a scoped-down mercenary-tier one; real precedent, ideas.md's decoy-cargo/session-proxy sketch, named against Claude Code/Codex specifically) is a concrete real case of this same selection-transparency obligation, not a new one; to every piece downstream of the choice a mercenary-tier harness looks like an ordinary harness, only d (selection) and j (capability grant) know the difference, and router+kernel jointly own enforcement across every trust axis (model, harness, and whatever comes later), scoped to the degree the owner's policy wants. Open question, not yet answered: how a harness advertises optional capabilities (e.g. guaranteed structured output, sub-agent spawning, live streaming) so the router can pick among candidates without guessing; no real SDK checked (OpenAI Agents SDK, Claude Agent SDK, CrewAI, LangGraph, smolagents) has a third-party-consumable format for this, each just documents its own flags, and the capability set itself will keep changing, arguing against a fixed enumeration if MSI-1 answers this at all. Finding, 2026-07-12 (Model endpoint piece pass): Clarke flagged a live question this row doesn't answer yet, whether per-run model selection is one shot (a single model handed at assembly, fixed for the run) or can hand a run more than one endpoint of the *same* modality to choose among per step (a cheap model for routine steps, a stronger one for a hard one) — distinct from the already-settled cross-modality case (chat vs. embedding vs. image vs. speech, which the Model endpoint piece prose already covers as separately-swappable endpoints). Left open for d's real walk. Either answer makes it a router+kernel job (assembly grants the set, enforcement checks each call against it), harder than single-model assembly but the same shape as the sub-run-gets-its-own-selection recursion already in this row, not a new mechanism class. Two real candidate shapes surfaced: adopt MCP's own already-proven two-layer pattern (a static manifest for cheap pre-launch selection, a live initialize-style handshake at invocation to confirm), or defer the whole mechanism per the minimal-core/timebox rules, router selection in MSI-1 stays coarse (whichever harness is configured), richer matching waits for real implementations to learn from. Research reported (2026-07-13, single pass, see verification note below): OpenClaw is real, already-shipped precedent for something like this, each harness reportedly implements a `supports(context) => {supported, priority} | {supported: false}` predicate, core reportedly calling it during selection (explicit model/provider config wins first, then `supports()`across registered harnesses, then a fallback), with debug logging reportedly exposing the chosen harness, the selection reason, and every candidate's result, the exact selection-transparency shape this row already wanted, if accurate. Only one actual declared capability flag was reported anywhere checked (`authBootstrap: "harness"`, said to be added ad hoc for one integration's real need). Verification note (2026-07-13): a second, deeper pass independently confirmed the repo (github.com/openclaw/openclaw), its MIT license, and, via a real GitHub issue, that it genuinely swaps between independent real backends (an embedded default runtime, OpenAI's Codex CLI, Anthropic's Claude CLI, ACP) — that general finding is solid, doubly sourced. It could NOT independently locate the`supports()` interface, the exact priority order, or the `authBootstrap` flag in actual source; those specifics are single-sourced from web content, not verified code, and should be treated as a lead, not a fact, until someone actually clones the repo and greps it. Letta separately deprecated its own declarative `tool_rules` system in 2026 explicitly to avoid inhibiting frontier capabilities, a second independent real signal (unrelated to the OpenClaw specifics above) against upfront declarative capability machinery. Lean, not yet Decided, resting on the confirmed general shape rather than the unverified specifics: MSI-1 adopts something like OpenClaw's approach rather than inventing a schema, a context-dependent predicate per harness, plus making candidate-exposure and selection-reasoning a required part of d's contract instead of optional debug output; no capability enumeration needed since it's a predicate, not a manifest, and something like it is already proven in production rather than purely speculative |
| e | Harness ↔ model endpoint | Adopt + envelope | Open | OpenAI-compatible baseline; how optional capabilities (structured output, reasoning traces) are declared rather than assumed; endpoints modality-typed (chat, embedding, image, speech), never assumed chat-only. Walk records the pinned surface. Finding, 2026-07-12 (Model endpoint piece pass): checked vLLM, Ollama, and llama.cpp's current server docs against the modality-typed-endpoints claim, confirmed accurate (llama.cpp's own server lists them as distinct paths, matching OpenAI's real shape). Surfaced a real architectural split not previously in this row: vLLM and llama.cpp run one model per server instance (multi-model means separate processes behind a proxy, confirmed on vLLM's own docs and forums), Ollama runs one server multiplexing many models, swapped per request via a `keep_alive`-governed load. Resolved by the wire shape itself, not a new mechanism: `model` is always a per-request field regardless of which shape sits behind the endpoint, and `/v1/models` (real, standard, implemented by all three) already answers discovery; the walk doesn't need to invent either. Also confirmed, same session: the Model endpoint piece's scope includes vendor-hosted frontier APIs (Claude, GPT) as ordinary, capability-narrowed instances, invisible to the piece itself, the same pattern as d/j's mercenary-tier-harness finding — this is why step 2.1 already planned to read the Anthropic Messages API alongside the OpenAI-compatible surface, and matches vision.md's existing "hired help, minimum fragment" framing of frontier models (vision.md:154-160) |
| f | Harness ↔ tools | Adopt | Open | MCP, pinned version; enforcement stays with the kernel, not the protocol. Tool discovery is MCP's own listing (the old PATH-equivalent question dissolves here unless the walk proves otherwise). Carries the retired Peripheral piece's ground: device/hardware capability (camera, location, appliances) is just a tool, checked against two real systems (OpenClaw's computer-use path, Hermes-iOS's camera/mic/health/location/notifications all served from one MCP server, devlog 2026-07-12). Matter/HomeKit's typed capability declaration ("clusters"/"characteristics") is the richer schema shape to check MCP tool schemas against for device-facing tools. Finding, 2026-07-12 (Tools piece pass): a Tools instance is one MCP server (confirmed against Claude Desktop's real `mcpServers` config, several independent servers run side by side on one machine); a run's binding to f is a roster, not a single pick, a harness needs several tool capabilities live at once, unlike a model or sandbox binding. Open question for the real walk: how the router assembles that roster and how f's contract exposes it to the harness |
| g | A trigger → a run | MSI | Open | How a run fires with nothing else running: the router watches the clock, outside events, idle time; downtime maintenance is just a run; triggers themselves are data (m's reused-shape catalog verifies that). Prior art: cron/at; systemd timers; iCalendar RRULE as the recurrence grammar; Erlang timers; Letta's sleep-time compute, checked 2026-07-11 (memory reorganized and consolidated during idle periods by a background run, not a continuously-alive foreground agent) as real precedent that downtime-maintenance-as-a-run is the right shape for continuity, not a reason to keep a runtime alive between engagements |
| h | Skills → harness | Adopt | Open | SKILL.md as the on-disk shape (agentskills.io); skills live in the identity and travel with it, read straight off the data, never a per-run binding. Walk records what adoption binds |
| i | Kernel → harness | MSI | Open | The run contract: what a run is handed (identity fragment, capabilities, task), what it owes back (learnings, in the data's shape), how a live run is interrupted or killed. Prior art: exec/argv/env as the analogy; Erlang/OTP supervision and let-it-crash (likely the real process-model precedent, not Unix or seL4); Agent libOS's AgentProcess (state + children + budgets + checkpoints + explicit capabilities) and checkpoint fork/restore/commit (thin evaluation, one author). Folded in from the old plan: the fragment's shape is m's business, who may mint one is j's; uniform I/O; supervision vs. the run's own error/retry (where that line sits is the open question); write-back obligation (incremental vs. session-end still open). Finding, 2026-07-11 (Client piece pass): the feeling of continuity across a gap, knowing what was just being discussed, is a memory-quality problem, not a reason to keep a run alive between engagements; a run's starting fragment must be able to carry enough recent context (recent logs, standing summaries) for this to work, what counts as enough and how it's assembled is m/s's business, but i's contract has to guarantee the fragment can carry it. Explicit leave-opens to record: planning strategy, and a harness's own live in-run context-budget management — harness internals, the market's problem. Finding, 2026-07-13 (Harness piece pass): a harness holds no private filesystem or staged copy of the identity; it reads and writes against the shared data directly (through whichever memory provider it is handed, seam s) throughout a run, the same way every other consumer does. This resolves the incremental-vs-session-end question above: incremental, not batched at the end, a private copy merged at run-end would just reintroduce the concurrent-write-reconciliation problem m's design (concurrent writes reconcile at the data, never inside one provider) already exists to prevent. Real precedent checked (OpenAI Agents SDK's `Runner.run()`, Claude Agent SDK's `query()`, LangGraph) invokes a harness as an in-process library call, not a subprocess or network service; Clarke's lean runs against that grain on purpose, a run should be its own OS-level process, matching the ephemeral/killable claim and the exec/argv/env analogy already carried here, though it means adopting a vendor SDK as a Harness costs a subprocess wrapper. Not yet Decided, a steer for the walk. A process boundary buys killability and crash isolation only, never the security boundary, that stays j's regardless. Real precedent for the handoff object's contents (2026-07-13, from OpenClaw): before a harness runs, core resolves everything into one object, provider/model, auth, context budget, transcript, workspace/sandbox/tool policy, streaming callbacks, worth checking against when i is walked |
| j | Kernel ↔ everything consequential | MSI | Open | The permission model: granting, narrowing, revoking, auditing, offline operation. Direction picked (not designed): seL4-flavored capabilities, not Unix ambient DAC (the documented road not taken). Prior art: seL4's Capability Derivation Tree + Revoke (subtree revocation, the candidate mechanism, check whether Capsicum's no-revoke fd model is actually sufficient instead); WASI's handed-capabilities model — the portable direction, no ambient access, what you weren't given isn't there; Capsicum (FreeBSD-only, so shape-precedent, never the mechanism); Landlock/macOS sandbox as per-OS hardening backends underneath, never the contract; UCAN/Biscuit/macaroons as the portable policy-token family (must round-trip with m's travelling permissions); AgenticOS's capability + intent-check as complementary layers, and unreachable-by-construction rather than runtime-gated; kernel.chat agent-os and OpenFang gates as source-level checks. Audit trail folded in: what the kernel records per mediation, records presumably memory-shaped (m verifies); "what did it do and who allowed it" is a vision.md commitment. Carries the retired Peripheral piece's ground (devlog 2026-07-12): iOS's Info.plist and Android's manifest both declare capability access statically and gate the risky ones with a runtime prompt, Android explicit about tiering ("dangerous" vs. "normal") in a way j's model doesn't yet; the W3C Permissions API's `revoke()`, specified then pulled, is a real shipped standard that tried programmatic revocation and failed, worth checking against before assuming CDT+Revoke solves it cleanly. Finding, 2026-07-13 (Harness piece pass): mercenary-tier harnesses are a concrete real case of this seam's capability-narrowing machinery already, not a new mechanism, the kernel simply grants a narrower capability set by construction; the harness itself never knows it's mercenary-tier, an ordinary harness from its own seat, the distinction lives entirely at d (selection) and j (grant) |
| k | Machine ↔ machine | MSI | Open | One identity across N machines: ordering, conflict, partition, live-task handoff (who's driving). Prior art: Plan 9's 9P for addressing something remote through the same namespace as something local (load-bearing); Erlang distribution; capability handoff over the wire (Unix fd-passing, seL4 IPC transfer, Zircon channels passing capabilities in messages, Mach ports, UCAN-style delegation — all unverified); transport reuses Tailscale/WireGuard underneath, seam-only, nothing persists there. Fleet coherence is the hardest row in this file: git (explicit merge, human-visible) vs. CRDTs (Automerge/Yjs, automatic convergence) are opposite answers to the same question, and the semantic-merge problem (two branches of belief state that both claim true and contradict) sits under both. Revisits the A2A-composition flag from l |
| l | System ↔ outside agents | Adopt | Open | A2A, pinned version; different owners, opaque, no trust extended. The one real question: how it composes with k (machine-to-machine inside one fleet is capability-mediated; agent-to-agent across owners extends nothing) |
| m | Providers ↔ the data | MSI | Open | The flagship seam: the standard data shape, everything derived rebuildable from the data alone, history so a bad write rolls back, per-unit permissions travelling in the data (they are what cuts a persona's view), concurrent writes reconciling at the data, never inside one provider. Carried from the old File-side rows: the core file-like primitives (identity, types, naming, namespace; Plan 9's per-process namespaces the load-bearing namespace precedent; does a unit ever have more than one name); content mutation over time — retraction (cheap, common, keeps history) vs. purge (rare, deliberate, actual erasure), with git's object model (immutable objects + moving refs) and Plan 9's Venti (content-addressed but single-timeline, no branches) as the two real precedents; concurrent same-machine writes. Carried from the old schema rows: the memory unit's shape (Portable Agent Memory, Engram, W3C AI Agent Memory Interop CG — checked adapt-before-build, all unverified); persona format (nothing standardized found); policy shape (UCAN/Biscuit/macaroons/Cedar, the enforcement semantics being j's half); budgets; provenance fields (shared with r); the scoped-view fragment (what a run or an outside model is handed, minimum content, stand-ins for real values); write-back landing in the shared shape; and the reused-shape catalog — verify per object, never assert, that a trigger, a routing policy, the roster, a task's checkpointed state, and an audit record are all just memory objects. Retrieval/embedding engines deliberately leave-open (providers compete); the harness↔provider binding question is flagged in the Pieces table. `dockets-research/*.md` are pre-anatomy input material for this walk, nothing more. Flagged 2026-07-11: whether the schema should name three sub-shapes explicitly (data about the owner; the counterpart's own operational identity, its own accounts, credentials, skills, and operational history; and the relationship/provenance data bridging the two) rather than the current undifferentiated list. Credential broker and kernel already don't care whose name is on an account or secret, only owner policy gates action, so this reads as a schema-naming question to settle when m is walked, not a new mechanism. Finding, 2026-07-13 (Harness piece pass): personas (already defined, a scoped view over the same memory cut by per-unit permissions) are confirmed as the mechanism for a mercenary-tier harness's faked/proxied identity access, no new schema needed. ideas.md already has a working sketch worth carrying into the walk: decoy data served per-object as a low-trust harness reads it, translated back onto the real object on write, proxied for the whole session length rather than per-prompt, named explicitly against Claude Code/Codex as the motivating case |
| n | New machine → owner's set | MSI | Open | Admission (hardware attestation is one admissible mechanism) and later revocation of a machine's trust. Prior art: Tailscale/WireGuard machine-key identity as the likely reference mechanism (kept out of anatomy.md the way vLLM is just an example; posture stays MSI, it's not an open protocol the way MCP is). Overlaps a's root-of-trust question: machine #1 and credential #1 are the same bootstrap moment |
| p | Broker ↔ anything needing a secret | MSI | Open | New row, 2026-07-11: the old plan never asked this anywhere. How secrets are stored, scoped, and injected into exactly the call that needs them without entering model context; custody, not permission (that's j). Prior art to gather when walked, candidates unverified: OS keychains/keyrings, HashiCorp Vault's dynamic-secrets model, sops/age, pass, systemd-creds |
| q | Kernel → sandbox | MSI | Open | Where an allowed action executes and what it can see while running; placement, not permission. Prior art: containers/isolated processes; WASI instantiation-time handle-passing; the per-OS sandbox family (Landlock, Capsicum, macOS sandbox) as hardening backends a placement can use, never the contract; another of the owner's machines or a remote service as placements, which ties into k |
| r | External content → a run | MSI | Open | Trust-tagging before content reaches the model: source, authorship, what vouches for it, so a stranger's words are never fed as if they were the owner's. Prior art: C2PA content credentials; Sigstore (keyless signing + transparency logs); Ed25519-signed manifests (OpenFang, shipped). The tag's on-disk fields are m's business; the tagging behavior and its single-instance-per-machine guarantee are this seam's |
| s | Harness ↔ memory provider | MSI | Open | New seam, minted 2026-07-11 (see the resolved flag above the Pieces table). What a run asks of a provider and what every compliant provider must accept and return: query in, sourced results out, so a harness can talk to whichever provider it is handed. Live candidate answer to check first: collapse into f, providers are just MCP servers exposing retrieval tools; if MCP's surface can't carry what retrieval needs (result provenance for r, view scoping cut by m's travelling permissions), it's an MSI envelope instead. Retrieval internals (how a provider indexes, embeds, ranks) stay deliberately open, providers compete there; whatever path a write takes, it lands in the data's shape, which is m's business |

## Precedent library

Trimmed to a pointer list; the load-bearing caveats now live in the seam rows
above. Depth per system stays in the devlog and gets re-earned when a seam is
walked.

- **Unix/POSIX** — runtime target, default vocabulary, and the method itself.
  Feeds a, i, j, m.
- **seL4** — capabilities, CDT + Revoke, no ambient authority. Feeds j.
- **Plan 9** — per-process namespaces, 9P, remote-through-local addressing.
  Feeds k, m.
- **Erlang/OTP** — cheap isolated processes, supervision, let-it-crash. Feeds
  i, k.
- **Capsicum / WASI / Landlock** — capability discipline on real userspace
  systems; WASI is the portable one. Feed j, q.
- **UCAN / Biscuit / macaroons / Cedar** — portable, attenuable authority as
  data. Feed j, m.
- **git object model / Venti** — immutable history, retraction vs. purge,
  branching. Feed m, k.
- **CRDTs (Automerge, Yjs) / Syncthing** — convergence without coordination.
  Feed k.
- **Matrix / XMPP** — one identity, many simultaneous clients. Feed c, b.
- **C2PA / Sigstore** — shipping provenance standards. Feed r.
- **cron / systemd timers / RRULE** — trigger grammar. Feed g.
- **Agent-era literature** (AIOS, AgenticOS, Agent libOS, kernel.chat,
  OpenFang) — contemporaneous and unproven, mechanism ideas only, never
  hardened precedent. Feed d, i, j.
- **Closed comparators** (Alexa+, ChatGPT, Apple Intelligence, Meta AI) — what
  welded-shut looks like, checked 2026-07-10. Feed c, o.

### Explicitly excluded — inherited from the real OS underneath

Memory management, device-driver internals, the raw networking stack
(Tailscale/WireGuard), boot/init. Listed so the tracker stays honestly
complete, not because they need Mojo-side design.

## Naming

The formal spec is the **Mojo System Interface**, a direct echo of POSIX's own
full name (Portable Operating System Interface), not a claim of literal POSIX
compliance. Versions are numbered the same way: MSI-1 first, iterated openly.
Plain functional register per [naming-conventions.md](naming-conventions.md);
deliberately not "MojOS Standard," which would collide the abstract standard
with MojOS, the concrete NixOS product.

## Deliberately deferred

- §0 profile granularity: written at assembly from what landed; the ten
  named profiles are the working set, not a commitment. Whether Tools
  graduates from no profile to a named profile of its own is a live
  question, hinging on what seam f's walk (step 2.2) finds MSI has to add on
  top of raw MCP.
- Backup / disaster recovery: the all-machines-lost case. The shape being
  plain files means export is copying, so it's operational, not a seam — but
  if walking m proves it needs a defined export envelope, it gets a row then.
- Groups / Collectives: gated past MSI-1 per the sequencing discipline; the
  "N holders of one capability" question is noted at seam a for when it
  ungates.
- Implementation (repos, services, SDKs): blocked on the system being
  defined, not on this file.
