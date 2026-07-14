# Devlog

*One entry per session, newest first — not per day. A single date can carry more
than one entry if more than one session happened on it. Messy is fine — this is
where the reasoning trail lives.*

---

## 2026-07-14 — Scope and growth section sharpened, three additions from an outside session

A separate conversation (not this repo) independently reached the same
inversion landed earlier today and added three things worth keeping, folded
into making-the-standard.md's Scope and growth section after discussion:
a concrete elimination test for which pieces belong in MSI-1 (if the piece
disappeared, would the hotswap demo still run? if yes, it's not MSI-1);
deferred scope reframed explicitly as room left for whoever shows up at
publish, not scope lost; and the growth loop named directly, the idea
backlog already in ideas.md/vision.md publishes alongside the minimal spec
and each idea moves through visible states (idea, researched, prototype
exists, multiple independent implementations exist, candidate document,
standardized) so the queue is legible enough for someone else to pick up a
rung. Everything else in that outside conversation was already covered by
today's earlier landing; no other document changed.

Pushed further into the org/repo question itself: not now, shape isn't
settled, but before publish the whole org needs rework, not just the
`mojo` repo's internal structure. Landed, not yet acted on: mojos leans
further into NixOS specifically (a personal system he builds his own
implementation on top of), which goes past roadmap.md's current Horizon
line about Nix as package manager, not NixOS, worth reconciling later, not
now; mojo-agent as a standalone repo likely goes away entirely, which
isn't a new call so much as this session's own adopt-don't-build
conclusion finally reaching the repo layer; ringmaster gets archived; every
doc across the org needs reframing for its post-publish job as the
community hub. Recorded in ideas.md's Product/org section rather than
acted on, since none of it is decided enough to execute yet. Also surfaced:
Clarke has no mentor yet for this project specifically and wants Claude
leading and teaching more actively on calls like this one, not just
executing on request (saved to global memory).

Corrected: restated making-the-standard.md's "kernel/identity/memory glue"
build list back to Clarke as settled. It isn't; that section already sits
in tension with the same doc's own Scope and growth hedge that piece
selection is a build-time call, not a paper one. Not fixed in the doc
itself this session, just flagged, since which piece actually resolves it
isn't decided either. Ringmaster: already archived on GitHub by Clarke,
directly, not through this session. And the portability question from
earlier in this session got answered directly: the reference implementation
and whatever a stranger tries out at publish has to run on any Linux
distro, not just his own machine, no exception for however deep the
personal NixOS build goes. Matches roadmap.md's existing "Nix, not NixOS"
line already, so nothing there needed changing, just confirmed.

Closed the night on a good compression of the whole day's work, from a
separate conversation: MSI-1 doesn't have to be the digital counterpart
standard, it has to make one possible, proving identity/memory survive a
harness swap without claiming to deliver the whole vision in one draft.
Not new mechanism, everything under it (three-layer publish, the growth
loop, the elimination test) already landed earlier today, but the line
itself was sharp enough to land verbatim as the closing thought of Scope
and growth in making-the-standard.md. Also filed a name, not a mechanism,
for the informative-layer-plus-backlog once the org rework is real: "MSI
Horizon", into ideas.md next to the rest of tonight's org thinking.

## 2026-07-14 — How standards actually get made, researched end to end; the plan inverted (build before the document); seam-method amended; standards-research.md and making-the-standard.md land

The handoff question from last session, run as its own thread: not "is
seam-method.md right" but how have standards actually been made,
historically, decomposition through document through validation through
publication through ongoing help. Three research passes (findings, with
sources and flagged unverified items, in standards-research.md) against
POSIX, Kubernetes' CRI/CNI/CSI and admission webhooks, LSP, OCI, Matrix,
XMPP, Linux, IETF/W3C process, and the failure cases (OSI, CORBA).

The headline, and it's unambiguous: no successful standard published its
document before running code existed, and the trigger for publishing an
interface was almost always a concrete second party arriving (rkt for CRI,
Red Hat/Codenvy for LSP, appc for OCI). Extraction from a working system is
the dominant modern pattern. POSIX, the one convergence case, still invented
at every seam where real systems' semantics collided under one name
(termios, O_NONBLOCK, pax), and had a procurement stick (FIPS 151) this
project doesn't. Matrix, the one a-priori design, was built by veterans and
still paid years of spec revisions; its own stated method is now ours: build
an implementation first, prove the protocol works, then spec it. LSP shows
the bill for publishing with one dominant implementation and no conformance
machinery: VS Code's behavior silently became the spec (UTF-16 offsets,
field semantics reverse-engineered by every other client). And the real
done-gate everywhere serious: two independent, interoperable
implementations, per feature (IETF RFC 2026, W3C CR exit). A first
implementation proves implementability; only a second, independent one
proves the document carries the interface.

What changed because of it, all discussed before writing:

- **The ordering inverted.** The old plan had the finished document gating
  the build; that's the OSI shape. New sequence in roadmap.md and
  making-the-standard.md: existence pass to build-ready (not complete),
  verify build inputs, build the first system, extract the MSI-1 draft from
  what runs, publish draft and release together, and "done" means a
  stranger built a second implementation from the text alone. The September
  framing is wiped; progress is scope, not calendar.
- **seam-method.md amended, three ways**, from the prior-art check of the
  six problems (which held overall: the six are contract questions where
  the fallacies of distributed computing are channel physics): liveness
  widened to cover partial failure (outcome-unknown, what forces idempotency
  into contracts, not derivable from "may be down"); independent
  administration named as the root fact under versioning, trust, and
  discovery; step 5's shape match now has to respect boundary physics
  (latency, round trips, the CORBA lesson).
- **Harness decision**: the first harness will be a LangGraph-style runtime,
  not Claude Code or Codex CLI. Flagged with it: since I assemble that
  harness, the hotswap demo must swap to an independently-built harness
  before publish, or the draft becomes "whatever my harness does", LSP's
  failure exactly.
- **OpenFang hypothesis banked, not asserted**: the kernel and the
  identity/memory layer might be findable in OpenFang. Its internals were
  the undocumented ones in the Harness-row research, so this is the first
  verification task of the build-inputs step, checked before the kernel
  plan hardens.
- **Honest answers to "can one person do this, how long"**: one person can
  reach the publish moment; the build scope after adoption is schemas, a
  small policy daemon, per-harness shims, glue, and the hotswap demo (Linux
  0.01 was 10,239 lines, and Linus had POSIX handed to him; the survey work
  here is that half of the job). One person cannot pass the done-gate, by
  definition; publishing exists to attract the person who does. No dates;
  publish when the hotswap demo is reproducible by a stranger and the draft
  describes only exercised seams.

Closed the session on the scope question: extracting MSI-1 from what runs
means it's minimal, so does every piece go in? No, and the record is
comfortable with that (POSIX.1-1988 was just the system interfaces, OCI
added specs as scope earned them, the hourglass theorem is the theory for
why small first versions win). The first system gets minimized harder than
the anatomy: Fleet manager is definitely out of the first build, and which
pieces form the minimal base is a build-time decision, made once the
adoption survey shows what's actually available, not a paper decision now.
The floor: the draft covers enough seams that the hotswap demo is
specifiable from the text alone. The vision still gets across at publish
through three layers that never mix: informative material carries the full
map, the by-implementer-role structure reserves named slots without
half-specifying them (normative text for an unexercised seam is the
OSI/CORBA move), and the MSC-style change rule is the door
designed-but-unbuilt pieces come through later, working implementation
attached. Fleet manager stays in scope for the family, its own document,
normative only when a system actually runs across two machines. Landed as
making-the-standard.md's Scope and growth section, plus a line in
roadmap.md's build step.

Files: standards-research.md (new, findings archive), making-the-standard.md
(new, the plan, the document item 2 called for), roadmap.md rewritten,
AGENTS.md Current stage rewritten, .claude/rules/msi-research-sessions.md
updated (build-ready finish line, both gates answered), research-plan.md's
Currently-on pointer updated. msi-steps.md untouched and still frozen,
pending rewrite or retirement next housekeeping session.

Next session: verify the OpenFang extraction hypothesis and the
LangGraph-style harness's interception surface; then the existence pass
resumes at Model endpoint's row under the new finish line.

---

## 2026-07-13 (yet later) — Harness's row closed, "discuss before write" made explicit, Fleet manager scoped in (sequenced last, not cut), AGENTS.md gets a standing prior-art rule

Continued the existence pass onto Harness's row, ten pairs. Opened the same
way Client's row almost did last time: took a fork's prior-art findings and
went straight to writing them into piece-matrix.md. Stopped twice in one
session before it landed. First catch: reasoning from anatomy.md's own prose
and treating it as checked, when anatomy.md is Mojo's own thinking, not
prior art — real-system verification is required per pair, not just for the
handful of claims that settle several pairs at once. Second catch, sharper:
even *real, cited* research doesn't get written straight into the tracked
docs. The actual shape is a loop, research comes back, gets discussed, gets
iterated on, sometimes through several more rounds of follow-up research,
and only then gets written. Both corrections are now standing rules, added
to AGENTS.md (a new "Grounding in prior art" section, stating the POSIX
comparison explicitly) rather than left as one-off fixes.

Four research rounds later, the row closed with real findings, several of
them corrections to the first pass:

- **Harness–Sandbox** started with only Letta confirmed and OpenFang's
  internals undocumented; a second round found OpenClaw and Hermes both
  document the same direct-dispatch shape explicitly, three of five systems
  now, gap closed.
- **Harness–Kernel** turned out to be the most load-bearing pair on the row.
  Real systems don't agree on single-checkpoint vs. defense-in-depth, but
  the disagreement resolved toward Mojo's own instinct rather than away from
  it: OpenFang, Claude Code, and Codex CLI all independently converge on
  multi-layer enforcement, and Claude Code's own docs state outright that
  permission rules are "enforced by Claude Code, not by the model" — the
  exact Harness/Kernel split Mojo draws, just usually built in-process
  rather than factored out as its own piece. seL4's pure resource-side
  model is the outlier among four systems checked, not an even split.
- **Harness–Model-endpoint gating** resolved into something more precise
  than expected: no system gates the *primary* session's model choice
  live, it's config/policy scoped once at spawn everywhere checked. But
  Claude Code has a real, live, per-call gate specifically for *sub-run*
  model requests (`Agent(model:opus)`, checked through the same pipeline as
  tool calls, because spawning a sub-run is itself a tool call in its
  implementation) — meaning this was never a separate mechanism from
  Harness–Kernel's already-established live check, just that seam covering
  one more category of consequential action.
- **Harness–Fleet-manager** stayed "no," but the citation for it was wrong
  on the first pass. "No real system does live cross-machine task handoff"
  turned out too strong; Letta's Constellation looked like a counter-example
  until checked directly, and it isn't one: the agent's reasoning loop lives
  permanently in Constellation's cloud and never relocates, machines are
  just selectable Sandbox targets for the next tool call. So the "no" holds,
  for a cleaner reason than first claimed.

That last thread pulled in a real scope conversation, not a research
correction: is Fleet manager (cross-machine coherence) actually in MSI-1's
scope, given it's the piece with the thinnest prior art and the rest of the
existence pass has already shown real systems mostly don't attempt it?
Landed, after actually rereading research-plan.md's already-banked
2026-07-12 Fleet-manager pass rather than assuming it was thin: it isn't.
Plan 9's 9P, Letta's Constellation, Apple Continuity/Handoff (real precedent
that "who's driving" is architecturally separate from bulk sync), Signal's
linked-device model, and git-vs-CRDT for conflict resolution are all
already-banked, real findings, not a blank slate. Fleet manager stays in
MSI-1's scope, full stop, not deferred to a later Mk (roadmap.md already
commits to this by September regardless: laptop now, PC then, one identity
across both). What changes is sequencing, not scope, matching an old
archived candidate walk order that already said the same thing on its own:
federation and admission get walked last, after the single-machine seams
they depend on (data shape, enforcement, run contract) are settled, because
the contract can't be written well any earlier, not because it matters
less.

One design point corrected mid-conversation, flagged on the Harness–Fleet-
manager entry rather than silently fixed: the 2026-07-12 research read as
foreclosing an external, company-run cloud service as a Fleet-manager
implementation. Clarke's read: that's too strong. A company's hosted
service should be a legitimate implementation an owner can choose, the same
way hiring a frontier model is a choice, as long as the contract still
guarantees portability and no lock-in regardless of topology. The MSI
standardizes the contract, not the topology. Left as an open flag for
Fleet-manager's own row, not fixed in research-plan.md now, since
research-plan.md is frozen archive, new findings get cited directly in
piece-matrix.md/seams.md where they're actually used, not written back into
the old file, and some of what's already in there may need correcting later
too, a separate future cleanup, not tonight's.

Real open question, unresolved on purpose: the existence pass is now
running much deeper per pair than msi-research-sessions.md's own text
describes ("can run in bulk, many pairs in one sitting, since they're
mostly grounding in anatomy.md's own already-written text"). That
description no longer matches how the work is actually happening, now that
every pair gets checked against real systems and discussed before writing.
Worth a rule update, and worth deciding whether every remaining row gets
this same depth or whether structurally-simple pairs (ones a shortcut
already covers) get a lighter pass. Not decided this session.

33 of 78 pairs now settled (Owner, Client, Harness). 45 remain across nine
rows: Model endpoint, Tools, Memory provider, Sandbox, Router, Kernel,
Credential broker, Provenance, Fleet manager.

---

## 2026-07-13 (later still) — Client's row closed: caught writing conclusions to the file before checking them, three real findings (Memory provider flips to yes, Provenance grounded instead of hand-waved, the vault/keychain thing was never a Client), one more piece-list-adjacent flag banked

Continued the existence pass onto Client's row. Opened by drafting all
eleven pairs solo and moving straight to a file edit — got stopped hard.
Clarke's correction: the existence pass has to actually be walked together,
pair by pair, not decided alone and presented as a fait accompli for
sign-off after the fact. That's a process rule now, not just a one-off
correction: results get proposed, Clarke checks them for coherence and
pushes back or signs off, *then* they get written down. Nothing from the
aborted first draft made it into piece-matrix.md; the row that actually
landed was rebuilt through real back-and-forth.

**Client–Memory provider flipped from no to yes**, caught by Clarke, not
found solo. First pass reasoned Memory provider is only ever granted to a
Harness by Router, so a bare Client can't hold one — Clarke pushed back with
a concrete case, a client built specifically to be good at editing memory,
no agent run needed. Checked against real precedent rather than accepted on
architecture-reasoning alone (Clarke's explicit ask: verify all of it, don't
invent): Khoj's docs state its search "operates independently from the chat
features"; Letta documents `archival-memory/search` as its own external API,
separate from the agent's own tool call for the same thing; Mem0 is built
explicitly for use "in client applications outside of agent contexts." Three
independent systems, same pattern, so it held. (Checked and rejected as a
fourth data point: Obsidian's Smart Connections plugin bundles its own
private index rather than calling a separate provider — that's the raw-file
pattern, not this one.)

**Client–Provenance got waved off too fast on the first pass, then actually
checked.** Clarke's pushback: what about copy-pasted content, does the owner
typing something versus pasting someone else's words in need a different
trust tag, and what does real prior art actually do about it, since the
project's whole discipline is standardizing what exists, not inventing what
sounds clean. Checked rather than guessed: no real system tags provenance
below the message/turn boundary. Anthropic's own prompt-injection guidance
wraps tool-result content in delimiters at the tool-call boundary, not
inside what the user typed; the published instruction-hierarchy work ranks
system > user > retrieved content as three tiers, "user" staying one tier
regardless of what the user chose to include; Microsoft's
Spotlighting/Prompt Shields tags external/retrieved documents specifically,
never sub-spans of a user's own input. Real answer: the owner is
accountable for whatever they put in their own turn, full stop, which is
the Tools/fetched-content-vs-Client boundary the project already has.
Nothing finer to standardize here, on purpose — left for the community to
build on top of once this publishes, not invented now to look complete.

**The vault/keychain thing was never a Client, on either row.** Owner's row
(closed last session) had reasoned "owner types a password into a vault at
setup" reduces to Owner–Client, since a local secrets UI counts as a Client.
Clarke caught this while reviewing the Client row: a password manager or OS
keychain doesn't reach the assistant, it's substrate the owner already has,
same as buying a hardware token, entirely outside the Mojo piece boundary.
So Owner–Credential broker's "no" verdict was right, but for the wrong
reason as written — fixed in both rows. Real lesson: a "settled" row from a
prior session isn't immune from a hole a later row's walk exposes, exactly
what AGENTS.md already says about the piece list, now confirmed true of
matrix rows too.

Landed for Client, all verified against real prior art, not asserted: yes
to Harness (two-way, live for the run's duration, OpenClaw/Letta), yes to
Router (one-way, the trigger), yes to Memory provider, Fleet manager, and
Identity; no to Model endpoint, Tools, Sandbox, Kernel, Credential broker,
Provenance.

**Flagged, not resolved:** Client isn't one shape, it's three — conversational
(→ Harness), raw-file (→ Identity), provider-mediated (→ Memory provider) —
each pair above only ever needed one of them. Reads like a conformance-type
split (the way Model endpoint is one piece with typed modalities), not a
piece-list problem, but not deciding that here. Also open: whether Client's
trigger to Router is a live call or a queue Router polls, which changes
what that crossing looks like once Router gets classified for real.

Next: Harness's row, a fresh session per the existence-pass pacing.

---

## 2026-07-13 (late night) — the existence pass actually started: anatomy.md split into pieces.md, the matrix moved out of HTML into a plain md working file, Owner's row closed, and the method itself amended mid-pass for prior art and for not writing ahead of agreement

First real session working seam-method.md's step 1 rather than tooling
around it. Opened by rereading anatomy.md, seam-method.md, seams.md, and the
still-empty piece-matrix.html cold, then walking Owner's row as a first
attempt, and got corrected twice in a row in ways that reshaped how the rest
of this pass has to run, not just what Owner's row says.

**First correction: stop citing the old letters as evidence.** First pass at
Owner's row leaned on "this is already seam a" as if a prior label were
proof a crossing was real. Clarke stopped this outright: the whole point of
redoing seams from scratch is that the old eighteen-letter list might be
wrong, so it can't be used to check itself. Redid the row from raw existence
reasoning only. Landed on: wipe the old letters everywhere rather than let
them bias reasoning quietly, but don't lose them, since a correct old
answer should be allowed to reappear on its own merit, not be pre-empted by
erasure. They didn't need saving anywhere new: anatomy.md was clean at
`d35483b`, so that commit is the checkpoint to diff against once this pass
is done, to see what reappears and what doesn't. Stripped the lettered
annotations out of anatomy.md's map and deleted the old seam table entirely;
[seam-method.md](seam-method.md) and [seams.md](seams.md) already didn't
depend on the letters.

**Mid-correction, Clarke's own idea: split pieces out of anatomy.md.**
Watching the letter-strip happen, Clarke proposed pulling anatomy.md's whole
piece-by-piece section into its own file, [pieces.md](pieces.md), the same
move already made for seams when research-plan.md's flat table stopped
fitting. Agreed it was the right call and well-timed rather than premature,
since the piece list is explicitly revisable per AGENTS.md and a focused
file makes that a small diff instead of surgery on a big narrative doc.
Moved the full piece descriptions into pieces.md verbatim; anatomy.md is now
the union, intro, map, and "what survives, what swaps," pointing at
pieces.md and seams.md rather than duplicating either. Updated every
forward-pointing reference across the repo (AGENTS.md's current-stage
pointer, roadmap.md, seam-method.md, seams.md, the session rule file);
historical mentions in devlog.md and research-plan.md's archive were left
alone, they're accurate records of what existed when they were written.

**Second correction: piece-matrix.html itself was the wrong home for the
work.** Attempted an edit to the HTML file's embedded JS arrays mid-pass;
Clarke stopped it: encoding actual reasoning as JS array literals inside an
HTML file wastes tokens on presentation nobody's looking at yet, and all
real work should happen in plain markdown, full stop, not just for this
file. Moved the actual matrix data into a new [piece-matrix.md](piece-matrix.md);
piece-matrix.html now stays stale, a rendering to regenerate from
piece-matrix.md later when there's something worth looking at, not touched
during the pass itself.

**Then walked Owner's row for real**, stress-testing each verdict with a
concrete scenario instead of pattern-matching off a first read, per Clarke's
explicit ask ("don't want things to slip through the cracks"). Landed on:
Owner only ever crosses with Client, ten other pairs all no, each with its
own tried scenario (a mid-task confirmation prompt for Harness, handing a
tool a credential directly, typing a password into a vault at setup, and so
on), because Client is *definitionally* the owner's only reach into the
system, nothing else in the piece list has a channel to the Owner as a
person, only to owner-authored data already sitting in Identity.

**Third correction, twice in one exchange: Owner–Client isn't one-way, and
I'd bundled two different facts and then mishandled the split.** First
pass called Owner–Client "verification, full stop," with "proactive
delivery" written off as bundled-in noise to relocate elsewhere. Clarke
caught two separate things wrong with that. First: delivery is real, and it
still has to reach the owner through *some* client, so the actual live
question is which of the owner's several clients gets picked, not whether
delivery happens at all, exactly the "outward reach" half of what Client
already is (his own framing: Jarvis contacting Tony through his phone, his
workshop speakers, his watch). Second, once that landed: I'd overcorrected
by pulling delivery entirely out of the Owner–Client pair, when the actual
human-facing leg of it (the phone actually ringing, the text actually
landing) is exactly Client's outward-facing half and belongs to this pair
after all. What's genuinely a separate, still-open question is upstream of
that: which piece decides a message needs to go out at all (Router, since
it watches triggers? Kernel, since it flags consequential actions? both?),
and how that decider picks from, or broadcasts across, the owner's roster of
live clients. Fixed: Owner–Client is real and two-way, verification one way,
delivery the other, both belonging to this one pair; the decider-and-roster
question stays parked for Router's and Kernel's rows against Client.

**Fourth correction: verify the row against real systems before calling it
closed, not just from the piece definitions.** Clarke's framing: MSI
standardizes what real systems already do, it doesn't invent structurally
clean claims and assume they hold. Forked a check of the "Owner only ever
touches Client" claim against the same five digital-counterpart-shaped
systems already used for the piece-completeness pass (OpenClaw, Hermes,
Khoj, Letta, OpenFang), specifically for whether any of them let a user
touch a backend component directly, or ran proactive delivery through
something other than the same channel/UI layer the user already uses. Held
clean in four of five; Letta has no proactive-delivery concept to check at
all, worth stating plainly rather than guessing. Khoj's scheduled
automations deliver by email specifically, the one case that looked
different at first, but that's just an always-live Client instance (unlike
a terminal or app, which is only live while open), the same roster/discovery
question already parked, not a break in the claim. Noted the liveness
variation within Client as a real thing to carry into its eventual
classification, not a reason to split the piece.

**Fifth correction: stop writing conclusions into the working file ahead of
actual agreement.** Had written all ten of Owner's "no" verdicts into
piece-matrix.md right after reasoning through them, before Clarke had
actually signed off, while he was still pushing on Owner–Client. He stopped
before I could compound it further. Standing process from here: reason
through a pair out loud first, write it into piece-matrix.md only once
Clarke's actually said it's right, not just when he's stopped objecting to
something else.

**Landed as a real amendment to seam-method.md, not just a one-off fix**:
prior art now runs throughout the procedure, not deferred to step 5's shape
match. A structural shortcut used to settle several pairs at once (like
Client being Owner's only reach) gets checked against the standing prior
art, the five digital-counterpart systems for how real systems actually
shape this kind of relationship, and research-plan.md's deeper archive for
protocol shape, before it's accepted, not asserted from anatomy.md's prose
alone and only rescued with research afterward if it starts to feel shaky.

Owner's row is closed in piece-matrix.md: one real two-way crossing, ten
no's, each with its own tried scenario, checked against prior art, corrected
twice by Clarke along the way. Client's row is next, started (section header
only, no pairs walked yet). Session ends here on Clarke's request, for fresh
context next time rather than carrying this one further.

## 2026-07-13 (night) — seam-method.md got a persistence question and a cross-cutting object registry, both requested by Clarke reviewing the method cold

Clarke reviewed seam-method.md as written (not as a continuation of the
evening session's outside-conversation discussion, he came at it fresh) and
raised three points. First, a missing structural question: step 3's
authority question ("who owns the shared fact") silently assumes a fact
exists, and breaks down for crossings like Harness to Model where nothing
survives the interaction. Second, a prediction that walking step 5 across
every seam will reveal a handful of shape families rather than eighteen
unrelated seams, correctly identified as not a method gap, step 5's six-shape
taxonomy already sets that up, nothing to add until seams actually get
shaped. Third, a warning that pairwise analysis (piece-matrix.html is
literally a grid, one cell per pair) can't represent an object that's really
one thing with several bilateral faces, a Grant issued by one piece, checked
by a second, revoked by a third, showing up as three cells each capable of
passing step 6's substitutability test in isolation while the schema drifts.

That third point is the same object-model idea from the evening session's
outside conversation (Task, Run, Grant, Capability, Event, Artifact, Identity
Fragment, Trust Tag, Secret Reference), landing independently, this time
from Clarke rather than the outside conversation, and phrased as a concrete
failure mode rather than a proposed replacement axis. The evening session had
already judged the idea genuinely strong but oversold as a replacement for
piece-to-piece work, correct as a second axis composed with it, gated on
surviving m's "verify per object, never assert" discipline before adoption.
First build of this pre-filled a candidate list (a shorter seven-object
guess at the outside conversation's nine), Clarke caught it immediately: he'd
thrown out those seven as an illustrative guess, not a considered list, and
the registry pre-filling itself from a guess is exactly the mistake
piece-matrix.html's own "nothing assumed answered" rule exists to prevent.
Corrected: the registry table starts genuinely empty, a row only gets added
the first time an actual seam's "What crosses" field names something for
real, and `confirmed` now requires a second independent seam hitting the
same name, not just one. Nothing pre-filled, same discipline piece-matrix.html
already enforces.

Concrete edits: seam-method.md's step 3 gained a fourth question (does a
fact survive this crossing or does nothing remain), step 7's recording
instructions now check "what crosses" against the registry too. seams.md
gained the registry section, a placeholder for two future rollups (by
matched shape, by shared object, not the same grouping and a seam can sit in
both), and the entry template gained Persistence and Shared object fields.
No seam has been walked yet under the updated method, still just tooling
before seam-method.md's step 1 actually runs.

## 2026-07-13 (evening) — the outside conversation landed and got critiqued rather than adopted, seams rebuilt from first principles instead of patched, a real seam-finding method derived and locked in seam-method.md, and research-plan.md demoted from tracker to archive

Opened with the outside conversation Clarke had flagged as incoming (2026-07-13,
later still's handoff). It proposed three matrices in sequence (authority, who
owns the fact; information flow, what crosses; interaction pattern, protocol
shape, only derived last) and, more centrally, that seams should be found by
defining a shared object model first (Task, Run, Grant, Capability, Event,
Artifact, Identity Fragment, Trust Tag, Secret Reference) and deriving seams
from which pieces create/read/update/replicate/validate which objects, rather
than walking piece-to-piece pairs directly. It also flagged what it called a
missing Identity-to-Kernel seam: who defines what a capability string like
`email.send` actually means.

Ran it through a real critique rather than adopting it wholesale. Most of it
turned out to already be landed in anatomy.md verbatim (the "who decides"
framing is just Kernel's own paragraph; "memory as filesystem not service" is
already the Memory provider piece's "Obsidian and your vault" line; "data as
the swap test" is already m's flagship-seam framing), useful as outside
confirmation the organizing principle reads clearly cold, not new work. The
missing-seam claim didn't survive: j's row already picked seL4-style
capabilities specifically to dodge the RBAC-registry problem being described,
and the actual residual question (who declares the vocabulary a capability
cashes out to for a given tool) is very likely already answered by the
registration-vs-negotiation split found in the 2026-07-13 (later) session's
CRI/CNI/CSI-vs-LSP research, just never cross-referenced onto j. Flagged as a
cross-reference to add during the real walk, not a 14th seam. The two ideas
that survived scrutiny: the matrix-sequencing order is a real upgrade over
piece-matrix.html's current one-axis status legend, and the object-model idea
is genuinely strong but oversold as a replacement for piece-to-piece work
rather than a second axis composed with it, and its object list needs to
survive m's own already-standing "verify per object, never assert" discipline
before being adopted, the same discipline that already caught trigger/
routing-policy/roster/checkpointed-state/audit-record almost getting treated
as five separate shapes instead of one.

Clarke's actual call, once that landed: scrap the patch-the-old-list approach
entirely. Take the piece list as given, zero seams, and have the method
actually taught and derived from first principles rather than inherited from
either the old eighteen letters or the outside conversation's framing. This
is not a deviation from AGENTS.md's own standing plan, it's exactly what it
already called for ("expected to mostly get discarded and rebuilt fresh...
seeded by an outside conversation"), just executed as genuine derivation
instead of direct adoption of what the conversation seeded.

The teaching arc, run as real back-and-forth, not a lecture dump. First
question: strip away everything a same-process function call gets for free
(same memory, same version, same author, same trust, always running) and name
what breaks. Clarke's own first pass named real things, mixed together: shape
agreement (his signed/unsigned int example), a namespace concern he wasn't
sure about, versioning across "different language versions," and function B
containing malware. Sorted into six named problems rather than corrected
outright: shape and vocabulary as genuinely distinct (his `pull()`/`get()`
example is vocabulary, not shape), his namespace instinct renamed to discovery
(how does one side even find the other, distinct from namespace collision,
which is a same-process problem that mostly disappears once two things are
separate processes), his versioning sharpened from "different language
versions" to protocol skew, two live versions of an interface coexisting
forever once more than one implementation exists in the wild, not just
sequential spec releases, trust kept as-is (his malware line, a completely
different axis from the other four, about belief and permission rather than
understanding), and liveness added as the one he missed entirely: "the other
side isn't running right now" is a normal state across a seam with zero
equivalent inside one process, where a function either runs or the whole
program is dead.

Tried a worked-example comparison next as a check, Kernel-Sandbox versus
Fleet-manager-to-Fleet-manager against the six problems, meant for Clarke to
run himself. He refused outright, and was explicit about why: not "stop
teaching me," specifically "stop assigning homework," he'll learn from
watching the reasoning happen rather than doing the exercise himself. Real
interaction-style correction, worth carrying forward for future teaching
moments in this project, distinct from wanting less depth, he pushed back
again one turn later when the correction got over-read as "wrap up faster."
Ran the comparison directly instead. Kernel-Sandbox: same machine, so
discovery and liveness are near-zero; shape and vocabulary carry the real
weight because Sandbox is meant to be swappable (container, isolated process,
another machine) and all of them have to satisfy the same placement contract;
trust turned out asymmetric and non-obvious, not "is this actor legitimate"
but "does this sandbox actually deliver the isolation it claims," a
truthfulness problem rather than an authentication one, and it connects
directly to the already-open declarable-isolation-strength question sitting
in p and q's rows. Fleet-manager-to-Fleet-manager came out close to the exact
mirror image: shape, vocabulary, discovery, and versioning all carry real
weight (peer state, cross-implementation interop, machines rejoining after a
partition, rolling upgrades), trust means "is this still one of my machines"
rather than legitimacy or truthfulness, and liveness isn't just a factor,
partition tolerance is the entire reason the piece exists. Generalized from
the two into three structural facts that determine which of the six problems
actually bite for any given pair, derived rather than handed down this time:
locality (same machine/process versus across machines), cardinality (how many
live instances actually sit on each side at the moment of this specific
crossing, not how many vendors build the piece type, which is always plural
by design and so never discriminates anything), and authority (one side owns
the shared fact versus both sides being peers).

Taught the purpose layer last, since Clarke asked explicitly to dwell on "what
seams are trying to do" before moving to method. Landed on: a seam is
specifically a piece's outward-facing surface, not any internal interaction
(Harness's own insides, deliberately unstandardized so harnesses can compete,
is anatomy.md's own existing example of the not-a-seam case), and the bar it
has to clear is blind interoperability, independently-built implementations
working correctly together on first contact with zero coordination, the same
bar TCP/IP, POSIX, and HTTP actually clear in practice. Tied this directly to
"every Mojo piece designed to be outcompeted": that promise is only real if
the boundary a competitor has to hit is written down to that same standard,
otherwise the only way to build a competitor is reverse-engineering whatever
the reference implementation happened to do. The mechanism for finding where a
seam belongs and whether it's specified correctly is the substitutability
stress test: swap either side for a hypothetical competing implementation,
does the other side still work, blind, against only what's written down. A
failure diagnoses one of two things, an incomplete contract (something crossed
that never got written down) or a wrong boundary (the two "pieces" weren't
really separable, redraw the piece list itself), and this is exactly, if never
named as such before, the test that already caught seam q and seam l each
hiding two real relationships under one letter.

Locked all of it into a seven-step procedure: existence pass across every
piece pair (grounded in anatomy.md's own text, most pairs a fast no); a
seam-worthy filter (does MSI actually want independent implementations to
interoperate here, mostly yes by design in this system, kept explicit anyway
for edge cases like Owner, a person, never swapped); structural
classification (the three drivers above); working out the load-bearing
problem subset from those three answers; matching the load-bearing profile
against the six-shape taxonomy already researched over the prior two sessions
(static shared format/OCI-POSIX-base-definitions, one-boundary-many-operations
open-caller-set/POSIX syscalls, one-caller-many-providers live session
negotiation/LSP-DAP, one-orchestrator-many-vendored-plugins live per-call
negotiation/CRI-CNI-CSI, peer replication with deterministic conflict
resolution/Matrix-git, lease-and-grant capability authorization/seL4-AWS-STS);
the substitutability stress test; and recording the result.

One real correction landed mid-write, caught by Clarke before it went in a
file: the first pass at the cardinality driver was phrased as "one canonical
implementation versus plural," meaning vendor count, which is meaningless
here since every MSI piece is designed to have competing implementations by
construction, so it never discriminates anything. The actual, discriminating
version is runtime cardinality at the moment of a specific crossing, how many
live instances are reachable right then, already implicit in anatomy.md's own
"one live instance per machine" language for the system-control pieces against
the explicitly plural service pieces, and already the exact axis
piece-matrix.html's own broadcast-cell legend encodes. Fixed in seam-method.md
before it was written, not after.

A second real correction, same exchange: research-plan.md shouldn't keep
holding per-seam findings going forward, its table format assumes every seam
is the same uniform shape, a POSIX-style flat list, the exact assumption two
sessions of research already proved wrong. Landed the split Clarke asked for:
[seam-method.md](seam-method.md), the procedure, new file; [seams.md](seams.md),
the tracker, new file, one flexible entry per seam tagged by its matched shape
instead of uniform table columns, empty and ready for the existence pass;
research-plan.md demoted to a research archive, its real prior-art findings
(Vault/STS, seL4, LSP, POSIX, the digital-counterpart research) kept and now
cited from seams.md entries rather than duplicated, its old seam table and
proposed walk order marked as archived input rather than the live plan. A
PostToolUse formatter hook reflowed seam-method.md's markdown right after it
was written, confirmed to be a pure line-wrap, no content change.

Closed the loop before Clarke ended the session (lunch): AGENTS.md's file
guide and Current stage section rewritten to point at the new files and name
the concrete next step; roadmap.md's numbered work list updated the same way,
explicit that seam-method.md is not the still-unstarted piece/seam-to-standard
document (item 2, a distinct later thing that decides how a seam/piece map
becomes one buildable spec, not how individual seams get found and shaped);
`.claude/rules/msi-research-sessions.md` rewritten wholesale, steps 1-2 of the
new procedure can run in bulk across the grid in one sitting, steps 3-7 stay
one seam at a time per session, the same one-seam-per-session discipline the
old rule had, and a `settled` seams.md entry explicitly does not trigger
drafting msi.md, that's still gated on item 2. Confirmed, when Clarke asked
directly, that msi-steps.md itself needs no change and stays frozen, unchanged
from the standing rule before this session. Next session's actual first move,
not yet started: seam-method.md's step 1, the existence pass across
piece-matrix.html's grid.

---

## 2026-07-13 (latest) — piece list stress-tested against five real digital-counterpart systems plus a second agent's independent read, no 14th piece found, a real durable-state correction landed (only Identity owns state), the userspace-tampering question it raised answered in layers, and Identity confirmed to stay one matrix node

Picked up with Clarke's own sequencing for the session: settle the piece list
is actually complete first, then work the matrix for seams, then start on the
standard-shape document. This entry is the first part only, the matrix pass
itself didn't start yet.

Checked the 13-node list two ways before touching a single cell. Against
anatomy.md's own accounting (1 reach + 1 run + 4 services + 5 system-control +
Owner + Identity, Host OS excluded) it already balances. Against real
precedent (Kubernetes' major components, Matrix's actors) nothing extra fell
out either. Clarke then asked for the harder version: check against the real
digital-counterpart systems this project already treats as real prior art
(OpenClaw, Hermes, Khoj, Letta), not just general-purpose distributed systems,
and added a fifth mid-session, OpenFang, which he'd heard was some kind of
kernel for agent systems but hadn't verified himself. Forked five parallel
research agents, one per system, each told to inventory the system's real,
current, whole-architecture components from actual docs/repos (not
recollection) and map every one onto the 13-node list, flagging anything that
didn't fit.

Verdict across all five: no 14th piece. OpenFang turned out to be a real
project (RightNow-AI/openfang, "Agent Operating System," disambiguated from an
unrelated embedded-bootloader project of the same name) and Clarke's hunch was
right that it's kernel-shaped, though its `openfang-kernel` crate actually
fuses Router, Kernel, and budget-tracking into one component, real evidence
that bundling is an implementation choice, not evidence Mojo's split is wrong.
Real findings worth carrying forward, dumped raw at the top of
research-plan.md rather than integrated properly, since Clarke was explicit
that whole file is getting rewritten once the standard-shape document lands
and there's no point polishing something about to be redone: OpenClaw's
Pairing & Device Trust System is a real trust tier between a bare Client and a
Fleet-admitted machine that anatomy.md doesn't currently name (open question,
not resolved: does kernel-permission-plus-sandbox-placement already cover it);
OpenFang has active egress/taint-tracking, a real gap distinct from both `p`
(ingress custody) and `r` (ingress trust-tagging), likely `j` or `q`'s scope
rather than a new piece; and OpenFang runs a separate wire protocol for its
own agent-mesh distinct from A2A, independent third-party precedent for the
k-vs-l split already landed last session.

Clarke then pasted in a second AI agent's independent read of anatomy.md,
asked for it cold rather than pre-framed. It converged with the five-system
check (no glaring missing piece, "architecture feels surprisingly complete")
but raised four real challenges, walked one at a time rather than accepted
wholesale:

- **Provenance as a piece vs. a schema field.** The other agent's challenge:
  maybe trust-tagging is just metadata on the data, and "Provenance" should
  fold into Kernel, the way Kubernetes' admission control lives inside the
  API server's pipeline rather than as its own top-level component. Real
  precedent, taken seriously rather than waved off. Weighed against
  anatomy.md's own laundering argument (content often arrives with no trust
  metadata at all; something has to assign it consistently or a run could
  route to the laxest tagger) and Clarke's own call: keep it standalone.
- **A missing registry/discovery piece.** Resolved rather than left open: the
  Khoj fork's own finding already showed a real system's "state management" is
  just plumbing holding the roster, not a piece, and every adopted service
  protocol (MCP's `tools/list`, an OpenAI-compatible `/models` endpoint)
  already carries its own discovery handshake. Discovery is already
  distributed into `e`/`f`/`s`'s adopted standards; Router just reads the
  resulting roster as data via `d`. Not a gap, one line for `d`'s row later.
- **Router might be two pieces** (Trigger Manager vs. Run Planner, the way
  Kubernetes split controller-manager from the scheduler over time). Real,
  and the other agent's own instinct, flag it, don't split yet, is the right
  call: doesn't change today's node count, parked as a watch-item for `d`/`g`'s
  real walk.
- **The durable-state lens**, the sharpest of the four. Its table (Fleet:
  yes, Kernel/Broker/Router: maybe) got checked against anatomy.md's actual
  text rather than left as a guess, and it doesn't survive as written: Broker
  explicitly doesn't own storage ("doesn't own the storage; it scopes and
  injects") and Router doesn't record anything (Kernel does, per `j`).

That last point led somewhere bigger. Clarke asked directly whether Kernel and
Fleet manager should keep their own private durable state at all, given the
whole point of the system is that everything is swappable except the data.
Anatomy.md turned out to already answer this, just not consistently: Identity's
own description already lists "a record of who wrote what under what
authority" as part of the data, literally Kernel's audit ledger, and line
72-77 already states "among the singular pieces, only the kernel writes...
[Fleet manager and the others] all read the data to do their jobs; none of
them author it." That second line already forecloses a private Fleet-manager
membership store: admission is just a permission event, Kernel-authored into
Identity's shape like anything else. Extended the same logic to Kernel itself
by analogy by `m`'s own already-stated rule for providers ("providers derive,
they never own... any write it accepts lands in the data's standard shape"):
if Kernel kept a private ledger, swapping kernel implementations would strand
the system's entire permission history outside the one thing that's supposed
to be portable, breaking the "package up the data, drop it into a different
assembly, still the same assistant" invariant for the single most
security-critical record in the system. Landed: nothing but Identity owns
durable state. Kernel and Fleet manager are exactly as swappable as Router or
Broker; Kernel is licensed to *write*, not to *own*. One loose end flagged
rather than resolved: Kernel's own audit-write into Identity is presumably a
privileged internal operation that doesn't need to re-check itself against its
own enforcement, the same non-issue a journaling filesystem has writing its
own journal, but worth naming explicitly in `m`'s row rather than silently
assumed.

Clarke then asked the obvious next question: if the data is just plain
userspace files, what actually stops direct tampering, bypassing Kernel
entirely by editing the files by hand? Anatomy.md already commits to not fully
solving this, on purpose: "Host OS... every piece above, the kernel included,
is ordinary software running in OS userspace. No kernel modules, no custom
OS." Clarke's own instinct, that a later, more mature MSI might eventually earn
real kernel-space enforcement (merged into Linux, or its own thing) once
there's enough behind the project for someone to build it, was affirmed
directly as a legitimate future-MSI direction rather than a gap MSI-1 fails to
close. For what MSI-1 can do without a real kernel, landed a three-layer
answer built entirely from pieces already in the anatomy, no new mechanism
invented: sandbox isolation (`q`) is the actual first line, since the real
threat isn't the owner editing their own files (that's ownership, not an
attack) but a harness or sandboxed action bypassing Kernel, and if a sandbox
never exposes Identity's raw files the problem shrinks to "can something
escape its sandbox," already modeled; tamper-evidence via content-addressing
and hash-chaining, git's object model (already `m`'s own precedent) and
OpenFang's real Merkle hash-chain audit trail found in this session's own
research, catches tampering after the fact rather than preventing it; and
encryption at rest keyed through the broker (`p`) so a raw file edit without
the key produces garbage, a real connection between `p` and `m` worth naming
explicitly rather than leaving implicit.

Finally, landed the identity-split node count Clarke had guessed would be at
least 3, and pushed back on that guess rather than just confirming it. The
test that mattered: the matrix tracks relationships between pieces, and
Identity isn't a piece, so sub-shapes only matter to the matrix if some other
piece's actual seam relationship to Identity differs by sub-shape. Checked
directly rather than assumed: Credential broker's own row already says it
"don't[s] care whose name is on an account or secret, only owner policy gates
action," Kernel's permission-gating path is the same regardless of what it's
gating, Provenance tags incoming content before it's filed anywhere, and
Memory provider's whole plurality argument depends on the data staying one
shape underneath multiple views. Nothing changes seam-wise, which matches what
the original 2026-07-11 flag already said and had been read past: "a
schema-naming question to settle when m is walked, not a new mechanism." Not a
new mechanism means not a new relationship, means not a new node. Landed:
**Identity stays one node on the completeness matrix.** The sub-shape count
itself (the original 3: owner data, the agent's own operational identity,
relationship/provenance; a candidate 4th, task/project working context, from
Hermes's Context File System, probably not real evidence, it reads like
ordinary Tools/Sandbox file access bundled into a memory subsystem for
implementation convenience, versus Letta's MemFS, a stronger candidate since
it's explicitly framed as persistent memory and personas would plausibly need
to scope it differently than personal history) stays open, parked for `m`'s
real schema walk, not resolved here.

Session closed here at Clarke's request, piece list settled well enough to
move on (not frozen: it can still move if seam work exposes a real hole, the
way it already did once for seam l). research-plan.md's "Currently on" line
and AGENTS.md's "Current stage" section both rewritten to match. Next
session: the seam list itself (anatomy.md's eighteen letters) is expected to
mostly get discarded and rebuilt fresh rather than re-walked in the old order,
seeded by an outside conversation Clarke is bringing in. Explicitly not yet
the gated phase-3 walk, since the standard-shape document (item 2, how a
piece/seam map becomes a real standard) hasn't been touched this session at
all and still blocks it.

---

## 2026-07-13 (later still) — piece-matrix.html landed in the repo, seam l resolved into existing pieces without a new one via a real A2A spec check, and walking concrete examples split "why do two owners' systems talk" into four shapes instead of two

Picked up the blank 13-node matrix from the handoff. Clarke had a second copy
of it from earlier the same day, better designed (warm paper/terracotta
palette, a five-state legend: known/broadcast/ambiguous/gap/none, vertical
sticky column headers, a findings section under the grid) and asked to save
it into the repo rather than keep rebuilding it from scratch each session,
the actual point being to stop burning tokens regenerating the same
artifact. Merged the two: kept the better version's design and findings
section wholesale, corrected the node order to match anatomy.md's own
Pieces section order rather than the earlier draft's spatial grouping
(Router had been placed third, right after Client, by diagram habit; the
document itself puts it after every service). Landed as `piece-matrix.html`
at the repo root, the same pairing anatomy.html already has with anatomy.md.
One finding folded into it directly: checked whether Fleet manager's own
flagged diagonal cell (the same-machine-grid-has-nowhere-for-k/n problem
found last session) generalizes to Kernel, Router, Credential broker, and
Provenance too, since all five are singular-per-machine. It doesn't:
anatomy.md is explicit that every other system-control piece's cross-machine
coordination routes through Fleet manager by design, not piece-to-piece
(Router's own prose: "not the router's own"). Clarke confirmed directly,
"all things shared cross-machine should be done through fleet-manager
because that is its one job," so this is landed, not just leaned on.

Clarke then proposed folding seam l (system ↔ outside agents) into Fleet
manager and renaming it, since Fleet manager's job could be read more
broadly as "connect to anything that isn't local." Real tension surfaced
before accepting it: k/n's whole point is maintaining one trusted identity
across the owner's *own* machines; l is explicitly the opposite, "different
owners, opaque, no trust extended," per anatomy's own Outward paragraph.
Recalled, not yet verified, that Matrix keeps federation (its own k/n)
structurally separate from bridging to non-Matrix networks (Discord, IRC),
and XMPP has the same split historically (s2s vs. gateway/transport
components) — real precedent, if it held up, against merging a
trust-extending piece with a trust-refusing one just because both are drawn
as outward arrows on the same diagram edge.

Forked a real check of the actual A2A spec (a2a-protocol.org) rather than
trust that recollection, testing a sharper hypothesis first: l might not be
one relationship at all, it might split by direction, outbound (a harness
delegates a task to another owner's agent, structurally a protocol variant
of f, tool-calling) and inbound (another owner's agent reaches Mojo first,
structurally a fifth instance of g's "outside events" trigger kind, no new
piece needed either way), unless A2A's discovery mechanism forces something
always-on the way Router or Fleet manager are but Harness isn't. Verdict
came back clean against the real spec text: Agent Cards are static JSON
metadata, §8.3 defines what one must contain, never that it must be served
by a live, always-reachable process, and §14.3's `.well-known` hosting
convention is explicitly optional. §3 names Client/Server as per-exchange
roles, not fixed identities, matching the outbound/inbound split exactly.
The task lifecycle (direct response, polled Task objects, SSE, webhook push)
never requires a standing daemon either, Mojo-as-server can process
asynchronously and call back later. The only genuinely "always-on"
requirement is the mundane one any web server has, something listening at a
stable address, and Router already is that. Same bug shape as q, one letter
hiding two real relationships: l dissolves into an outbound seam-side on
Harness and an inbound seam-side on Router, no new piece, no rename. A
sub-case fell out along the way: a bare capability query (fetch my Agent
Card, nothing more) doesn't need a run at all, it's data-serving, the same
shape as a Client reading the identity with no live session.

Clarke pushed one step further: if talking to another machine outside a
harness is already a real thing (k/n, Fleet manager to Fleet manager, no run
involved, ever, for the owner's own fleet), is task-delegation and
data-exchange actually the only two reasons two owners' systems would ever
talk? Walked concrete examples rather than reasoning abstractly: booking a
table (delegation), asking a friend's agent for a recommendation (feels like
data, structurally still one request-response task, no different shape from
booking), two coworkers' agents keeping a shared project timeline in sync
(not a task at all, ongoing, bidirectional, needs write reconciliation, A2A
has no mechanism for this whatsoever), a family wanting standing trust
between their assistants so they're not re-verifying every exchange (not a
task and not data, establishing that a relationship exists in the first
place), and an open capability lookup (no relationship, no run, just
discovery). Four categories fell out, not two: discovery (no run, no
relationship), one-shot task/query (l itself, zero standing trust, both
"delegate a task" and "give me data" collapse into this one shape since a
data request is just a task with an informational payload), relationship
formation (negotiating that an ongoing partial-trust relationship exists
between two owners at all, the cross-owner cousin of n, weaker by design,
scoped and revocable rather than full membership, no seam or piece anywhere
yet), and ongoing shared state (only possible once a relationship exists,
not request-response shaped at all, k/Matrix-shaped peer state agreement
across a partial-trust boundary). Clarke's own read: the last two are
literally what a Collective is, not adjacent to the idea, the actual
substance of it.

Ran an extensibility check before parking those two, since Clarke's
instruction was explicit, only solve them now if MSI-1 can solve them well,
otherwise make sure MSI-1 doesn't foreclose them later. m's per-unit
permissions, j's grants, and r's origin-based tagging all read as
principal-agnostic already in how they're already written, none hardcode
"owner-internal only," so extending "principal" to a future Collective
member looks additive, not a rework. k's write-reconciliation is the one
real open leap, whether the mechanism that converges one owner's own writes
across their own machines generalizes to reconciling two different owners'
writes into a shared Commons, left unverified on purpose. The one actual
foreclosure risk found was wording, not architecture: if l's row asserted
"no trust extended" as an absolute property of A2A rather than a
description of the no-standing-relationship case specifically, it would
contradict the standard's own text the moment a relationship ever formed.
Fixed by scoping l's row narrowly on purpose, landed directly in
research-plan.md rather than parked, since it's a correction to existing
content, not new design. The relationship-formation and shared-state finding
itself went to ideas.md's Collectives section, next to the existing Armada
and cross-implementation-fleet notes, explicitly gated behind the
Collectives phase, not designed further here.

Clarke's explicit call on scope, worth recording verbatim in spirit: leave
research-plan.md's own meta-status framing (the "Currently on" line, phase
bookkeeping) to drift rather than housekeeping it this session, since it's
all going to change again once the next two real milestones land, dialing
in the anatomy's pieces and seams for real, and the overall shape of the
standard itself (the four-shapes taxonomy from the prior session). That
housekeeping instinct got corrected mid-session rather than followed.
Session paused here at Clarke's own request, and his read on the pacing
change itself: taking this slower and making sure the standard actually
gets built properly is the better path than the faster version phase 2 was
running on before it got suspended.

---

## 2026-07-13 (later) — POSIX checked for real and confirmed a real but partial precedent, four more real standards surveyed, the seam list reframed as several genuine shapes wearing one template, a piece-completeness matrix started from scratch, and the project's own stated current phase corrected to match

Picked up the handoff: check anatomy.md's pieces against real POSIX
structure, and resolve whether a seam is a strict two-piece pairing or
POSIX-syscall-shaped, using j, p, and q as the test case, before touching
phase 2's steps again. Went and read the actual Open Group Base
Specifications rather than working from recollection. XSH (System
Interfaces) turned out to be a single flat alphabetical list of 1123
functions, no grouping by which subsystem answers a call anywhere in the
document; XCU (Shell & Utilities) is the same shape, a second, separate flat
list, not a subdivision of XSH. Conformance runs through Option Groups,
named bundles of related functions an implementation opts into as a whole,
real precedent for the ten conformance profiles already in research-plan.md,
but a conformance-grouping concept, not a grouping of the interface list
itself. And POSIX's own spec text never names "the kernel" or any other
component at all, only observable behavior at the process-facing boundary,
confirming research-plan.md's method section was already right on this
point before any of today's research happened. One correction worth
recording: "a piece's real definition is the union of the seam-sides it
implements" is a reasonable derived corollary of how real Unix ecosystems
achieve swappability (independent implementations satisfying the same
interface contract), not something lifted directly from POSIX's own
document structure, which has no Pieces table at all. Doesn't change the
piece list, just how honestly the method section should describe where the
idea actually comes from.

The 1:1 question resolved cleanly against that evidence. XSH is one
boundary (process ↔ kernel) decomposed into many individually named
operations, with an open, unenumerated caller set, not organized by which
process is calling. j and p are already written this way in anatomy.md's
own Connects column ("Kernel ↔ everything consequential," "Broker ↔
anything needing a secret"), just never formalized as a legitimate seam
shape or decomposed into actual named operations the way XSH decomposes
into read/write/open. The scarier version of the original doubt, that p
might really be an operation hiding under j's one boundary, doesn't survive
contact with phase 1's own already-completed work: the Kernel and
Credential broker piece passes independently checked *different* real
mechanisms for j and p (seL4/AWS IAM/STS/Kubernetes admission webhooks for
j; Vault/STS/Claude Code's sandbox proxy/OpenClaw's sentinel swap for p). If
p were really j's clothes, that research would have found the same
mechanism twice; it didn't. q, on the other hand, turned out to be an
ordinary attribution bug, not a shape problem: anatomy.md's Connects column
says "Kernel → sandbox," but Kernel's own row and Harness's own row (via "q
(executes inside)") both already claim it, two different real relationships
(kernel authorizing placement; a harness's action executing once placed)
wearing one letter, exactly the shape seam l's gap already was.

That reopened the bigger question from a different angle: is POSIX even the
right precedent for the *whole* seam list, or just being force-fit onto
something it doesn't actually cover, since POSIX standardizes one monolithic
kernel's boundary and MSI has to standardize several independently-swappable
pieces built by different vendors, none of which exist yet. Forked four
parallel research passes to check real alternatives without filling this
session with scrape noise: Kubernetes' CRI/CNI/CSI, the Language Server
Protocol and Debug Adapter Protocol, the Open Container Initiative's
multi-spec structure, and Matrix's federation API alongside XMPP's older
server-to-server shape. All four came back with real, sourced, structural
findings, not vibes.

CRI/CNI/CSI is the inverse of POSIX's own shape: one caller (the
orchestrator core), many independently-vendored implementers, live
capability negotiation (CSI's `GetCapabilities` RPCs, CNI's capability
dicts) rather than POSIX's static compile-time conformance. Real cautionary
tale from CNI's own history: staying deliberately thin without a declared
extension mechanism didn't keep it thin, the ecosystem bolted on external
daemons and CRDs instead, producing a documented lifecycle mismatch (a
synchronous plugin call depending on an async daemon that might be
mid-crash). LSP/DAP is the tighter fit for the session-scoped, one-caller
many-swappable-providers seams (e, f, s, h): a real, shipped `initialize`
handshake exchanging typed `ClientCapabilities`/`ServerCapabilities`
objects, unknown fields ignored for forward compatibility, servers
registering capabilities dynamically mid-session, directly answering
research-plan.md row d's open question (how a harness knows what a given
service instance supports) with a solved, shipped mechanism rather than
something MSI has to invent. Between LSP and CRI/CNI/CSI, a real seam
d/e/f/s question surfaced: registration (is a provider instance available
on this machine at all, CRI/CNI/CSI's territory, probably d's job) and
per-run capability negotiation (LSP's territory, e/f/s's actual job) had
been treated as one layer and are probably two.

OCI's image-spec, runtime-spec, and distribution-spec are genuinely
separate, independently-versioned specs (image-spec 1.0 in 2017, runtime-spec's
reference implementation in 2021, distribution-spec proposed in 2018), real
precedent for msi.md possibly not being one file, but the split emerged from
reconciling an already-forking real ecosystem (Docker's format versus
CoreOS's competing appc/rkt format), not from a clean-sheet design decision,
worth being honest about since MSI has no existing fork to reconcile, the
case for splitting now is foresight, not urgency. Matrix's Server-Server
(federation) API is kept as a wholly separate spec from its Client-Server
API on purpose, real precedent that k and n (federation, admission) are a
genuinely different kind of spec, peer replication of shared state via a
deterministic State Resolution v2 algorithm (not automatic CRDT
convergence, closer to the git side of the git-vs-CRDT question already in
k's row) rather than client-calls-provider or piece-boundary-with-operations.
Honest and useful: Matrix is a decade old and well-funded and is still
actively re-engineering state-resolution performance in 2025 (Project
Hydra), real evidence k is hard, not a reason to avoid modeling it properly.
XMPP's server-to-server shape turned out structurally different, not just
older: it routes messages, it doesn't replicate shared state, so it only
matches k/n if Mojo's fleet problem turns out to be message-routing rather
than state-convergence, and anatomy.md's own framing (one identity,
sync/conflict/partition) says it's the latter.

Checked one more thing before calling shape A settled: Kubernetes'
admission control and RBAC, the closest thing inside Kubernetes to j's own
shape (every consequential action checked against policy). Verdict:
supplements, doesn't replace. RBAC turned out to be identity-based ACLs
looked up centrally, the exact shape already rejected for j in favor of
capabilities, with a documented real failure mode (over-permissioned roles,
hidden escalation paths). Admission webhooks confirmed, with real numbers
(a default 10s timeout, a documented 30s global admission budget clusters
actually blow through), that a live round-trip per check is a genuine
operational cost, not a rumor, the same cautionary example already sitting
in j's row. Two real details worth keeping regardless: the mutate-then-validate
two-phase admission lifecycle (a request can be narrowed before the final
accept/reject, something XSH never had to model since it has no "narrow"
verb) and Kubernetes' tiered audit-log design (Metadata/Request/RequestResponse).

Landed, provisionally, on four real shapes the eighteen seams sort into
(fixed-known-deal with an open caller set, POSIX-XSH-and-seL4-shaped;
negotiate-first between one caller and many swappable providers,
LSP-and-CRI/CNI/CSI-shaped, itself splitting into a registration layer and a
per-run capability layer; peer agreement between instances of the same
piece across machines, Matrix-shaped; and a fourth category that isn't a
conversation at all, a shared data format, POSIX-XBD-shaped, m's own
existing home), plus an "adopted vs. invented" modifier that can stack on
any of the four rather than being a fifth shape.

Then Clarke pulled the session back hard, and rightly: confusion was real,
this is genuinely unfamiliar territory, and the actual goal, a community
forming around MSI and helping build both the standard and its
implementations, only works if the standard is something a stranger can
build against without ever asking either of us a question. Getting the
shape taxonomy theoretically right means nothing if it's not actually
understood. The rest of the session became deliberate ground-up teaching
rather than more research: why a standard is needed at all (so independently-built
pieces can be swapped without every builder needing to ask); the four
shapes, one at a time, using non-jargon analogies (a function call; a text
editor plugin system that has to ask a language backend what it supports
before using it; a group chat with no admin where everyone still has to
agree; a shared file format nobody "calls"), checked for understanding
before adding the next one, rather than delivered as one dense brief.

Built a 13-node piece-by-piece completeness matrix (an HTML working
artifact, not yet a file in this repo) so Clarke had something concrete to
look at instead of holding eighteen letters in his head, and so the
seam list's completeness could actually be checked rather than assumed. The
first version, pre-filled mechanically from anatomy.md and research-plan.md's
own existing text, surfaced five real findings just from the act of
building it: Router, Kernel, Credential broker, Provenance, and Fleet
manager all read the data per anatomy.md's own top-level system-control
section, but only Memory provider and Harness own an explicit seam-side
into it, a real, previously-unasked question; only the kernel writes the
data per anatomy.md's own second system-control rule, and no lettered seam
actually covers that write path; b, c, d, and g all name "system" or "a
trigger" instead of an actual piece, unlike every other seam, never
flagged before; q's already-known second-party bug and l's already-known
zero-attribution gap, now visible as literal blank or ambiguous cells
instead of devlog footnotes.

Clarke asked to wipe it and rebuild slower, and to actually question
whether the 13 nodes themselves were right rather than assumed. Answering
that honestly surfaced a real flaw in the tool itself, not a filling-in
gap: k and n (federation, admission) are relationships between a piece and
another machine's copy of itself, and a same-machine, piece-by-piece grid
has nowhere to put that, since the diagonal (self-pairs) is normally just
blanked out as not-applicable. Found by trying to build it, not spotted in
advance. Fixed by marking Fleet manager's own diagonal cell as an open
question rather than hiding it, not by pretending the grid's shape was
already right. Host OS's absence from the matrix was confirmed correct but
worth stating out loud rather than silently missing (the same
observable-behavior-not-mechanism move from the POSIX research earlier in
the session, applied a second time); l's target, a different owner's system
entirely, was confirmed to have no node yet, open whether it needs one on
the edge of the grid or stays off it on purpose. Board rebuilt fully blank,
with "not looked at yet" and "checked, no relationship" given genuinely
different visual treatment, since the first version had quietly conflated
them.

That fed directly into a real architecture discussion on k/n's
primary-vs-peer question, prompted by Clarke asking whether the earlier
per-machine-instance rule (settled during Router's and Fleet manager's
piece passes) actually forecloses an owner's own machine acting as a
designated primary. Re-checked what was actually settled versus assumed:
only an *external* cloud-authoritative point (Letta's Constellation) was
ruled out; an internal primary-among-the-owner's-own-machines strategy was
explicitly left open in k's own row, Signal's linked-device model already
named as the closer precedent. Worked through the real tradeoff: a
designated primary would make convergence far simpler (no genuine
multi-writer conflict, matching how a primary sidesteps needing anything
like Matrix's State Resolution), but Mojo's own offline requirement means
even a primary-mode fleet needs some independent-operation fallback for
when the primary itself is unreachable, so a real reconciliation mechanism
probably can't be avoided entirely, just exercised less often.

Clarke then asked whether MSI would need to fully specify both a primary
algorithm and a peer algorithm, since anything that could silently diverge
between compliant implementations has to be exact, not implementer's
choice, a rule that fell out of this same conversation (mode *selection* is
safe as owner policy data, same shape as Router's existing hard-limits
pattern; the reconciliation *mechanics* underneath aren't, because divergent
algorithms across vendors would silently corrupt the identity's literal-
equivalence guarantee, the one thing this whole project promises
unconditionally). Real POSIX precedent exists for defining several exact,
selectable strategies (real-time scheduling policies, `SCHED_FIFO` /
`SCHED_RR` / `SCHED_OTHER`), but a cheaper alternative was floated and not
yet verified against real precedent: one convergence algorithm (needed
regardless, for offline resilience) with a tunable priority weighting,
where "primary" is just that weighting set heavily toward one machine
rather than a structurally separate mechanism. Clarke's own follow-up,
asking why you'd ever want a separate primary mode if the weighted version
covers it, led to splitting what "primary" was actually doing into two
unrelated jobs: data-conflict tiebreak (dissolves into the weighted
setting, no separate mode needed) and admission authority (probably
dissolves too, into an ordinary use of seam a, the owner authenticated from
whichever device they're using, not a special machine at all). Not landed,
a strong lean, flagged for real verification next time, not something to
treat as decided from one conversation.

Clarke separately pushed back on weighted tiebreaks specifically, leaning
toward git-style explicit merge conflict resolution instead (surfacing
conflicts rather than silently resolving them), which is one of the two
options k's own row already names; not resolved, correctly left open rather
than picked under momentum. Checked whether Signal's actual admission
mechanism (QR-code scan) is the right one to adopt, prompted by Clarke's
own instinct to question it: no, not literally, since it assumes a screen
and a camera present together, which breaks for the headless server case
anatomy.md already wants to support; the asymmetric, inherit-don't-negotiate
shape underneath it probably still holds, the specific mechanism doesn't
transfer. The bootstrapping worry Clarke raised (how do two machines agree
on a mode before they're synced) resolved the same way: mode agreement
isn't symmetric peer negotiation at all, it's resolved asymmetrically at
admission, the joining machine inherits it as part of what it's handed, the
same shape as Signal's new-linked-device inheriting state rather than
negotiating from zero information; machine #1 has no real version of this
problem since there's nothing to coordinate with until a second machine
exists.

Clarke's own work-laptop example (wanting a new machine admitted to the
fleet but scoped to only work-relevant data) gave real teeth to an already-open
question sitting unanswered on seam n's row, whether an admitted machine
gets the full data immediately or a scoped subset, and surfaced a further,
not-yet-resolved distinction worth carrying forward rather than deciding on
the spot: whether machine-level scope and persona-level scope are the same
mechanism or two independent, intersecting restrictions (a work laptop
staying work-only even if a casual persona were selected on it). Clarke
flagged confusion about this specific point; it needs re-explaining fresh
next time, not assumed understood from this session.

Also surfaced, connected to the primary/peer discussion rather than
resolved: anatomy.md's identity already names "policies" as one of its five
data slices (memory, personas, policies, budgets, provenance); fleet
coordination mode is the first genuinely concrete content anyone has found
for that slice. Clarke's own instinct that Nix's declarative module system
(typed options, mode-gated sub-options) might be real precedent for how
"policies" gets structured was validated as a strong, real candidate worth
a proper research pass, explicitly deferred to seam m's own walk rather
than designed mid-conversation about k, since m is the long, flagship seam
and this session was already deep in k.

Closed by correcting the project's own stated current phase to match
reality rather than leaving it stale: AGENTS.md's Current stage section,
roadmap.md's Now section, research-plan.md's "Currently on" line, and a new
redirect note at the top of msi-steps.md all now say the same thing,
neither phase 2 nor phase 3 is where a session works right now, the actual
current work is finishing the piece-completeness matrix and working out how
anatomy becomes a real standard, both argued out loud with Clarke rather
than assumed, before msi-steps.md gets rewritten and its phases resume.
Nothing from this session is written into research-plan.md's actual seam
rows yet, on purpose, per Clarke's own instruction to get both foundational
things down correctly before writing anything else.

---

## 2026-07-13 (earlier) — Phase 1 closed, anatomy.html rebuilt as a real diagram, seam l turned out to have no owner anywhere, phase 2 (seam pass) drafted then suspended on a bigger question: is a seam even the right unit

Closed phase 1 (step 1.13). Coherence read of anatomy.md end to end: clean,
no edits needed, no stray peripheral/seam-o references, no em dashes,
piece count and seam-letter set internally consistent. Two small
research-plan.md loose ends from the peripheral retirement, unrelated to
the coherence read itself, fixed in passing: the Closed-comparators
precedent's stale "Feed c, o" repointed to f (peripheral's real successor),
and seam p's identity-vs-machine-scoped-custody question, which only lived
on p's own row, cross-referenced onto k and n's rows too.

Then anatomy.html, which turned into most of the session. First pass just
caught it up to anatomy.md: cut the whole retired Peripheral band and seam
o, dropped Jarvis (the persona name, not a public term per
naming-conventions.md) in favor of what anatomy.md actually says, added
content anatomy.md had gained since the last sync (the digital-counterpart
definition, the two system-control invariant rules, the identity's
rollback/history paragraph, Router's fourth trigger kind, Tools absorbing
the retired Peripheral's hardware examples), fixed a real content bug where
the credential broker's html prose still said it "stores" secrets when the
piece pass had already corrected that to mediates-not-stores.

Clarke's read on that pass: still wrong in ways that mattered more than
sync. "Swappable piece, profile: X" labels on every box when everything is
swappable by the same one rule stated once at the top, that's exactly the
kind of per-piece restatement the piece-pass writing convention already
forbids in anatomy.md's own prose and I'd reintroduced it in the html.
Piece names had drifted from what anatomy.md actually calls things
(Windows/Client, Agent runtime/Harness). The digital-counterpart definition
needs to come before the sentence that uses the term, not after. And the
diagram itself, a stack of labeled section dividers with no real lines
connecting specific boxes, wasn't actually a diagram.

Rebuilt it as a real node-and-edge graph: compact piece cards with stable
ids, an SVG layer computing actual connector lines from live
getBoundingClientRect() positions (so it stays correct at any width, not
hand-placed coordinates), full prose detail moved to cards below the
diagram. First version tried to route around the crossing-lines problem by
regrouping pieces spatially (clustering harness next to kernel/router/
provenance) and by hand-routing long edges through margin lanes and
same-row arcs. Real bugs surfaced testing it in a real browser (playwright,
screenshotted, not just trusted): a genuine NaN in the path data from
`rectIn()` never returning a `.right`/`.bottom` the lane math read, and
z-index ordering that put lines behind pieces when Clarke wanted them in
front (lines only show on hover now, so nothing behind them stays hidden).

Bigger catches, both real modeling gaps, not cosmetic: seam q was drawn as
kernel-to-sandbox only, but Harness's own row in the Pieces table lists q
among what it owns too ("executes inside"), same shape as its other three
service seams (e, f, s), so it needed its own line, not just kernel's.
And seam l (agent-to-agent) had been drawn Harness-to-other-agents by
analogy to how it calls tools over MCP. Checked against research-plan.md
for real: l has zero piece attribution anywhere, not itemized on any
piece's row and not covered by a blanket clause the way j and p each have.
Every other seam in the diagram traced to something real; l didn't. That's
what the piece pass exists to prevent and nothing had done the equivalent
check for seams.

Once Clarke said lines could cross through pieces on hover (since only the
relevant ones show at all, the earlier margin-routing complexity was
solving a problem that no longer existed), the diagram simplified a lot:
dropped the arc/bus routing, went back to the plain four-row categorical
layout anatomy.md's own map already uses (harness alone, services, system
control, identity), all edges straight lines, and made j (kernel touches
everything consequential) and p (broker touches anything needing a secret)
real fanned-out edges instead of a background-tint zone, since a zone
had been Clarke's actual complaint, seam p effectively wasn't visible as a
seam at all.

That led straight into the real thread of the session: seam l's gap meant
no one had ever checked seam-to-piece attribution the rigorous way the
piece pass checked pieces. Drafted a new phase 2 (seam pass) for
msi-steps.md and research-plan.md, renumbering the old phase 2 (seam walk)
to phase 3 and assembly to phase 4. Worked out what "prior art" even means
for a seam rather than a piece: for the four adopted seams (e, f, h, l) it's
the real spec itself, read for which component actually makes the call, not
analogy. For the MSI-posture seams there's no protocol to check, that's the
point of MSI posture, but the relationship still exists in every real
system, just permanently wired rather than swappable, so prior art means
reading how it's actually built where it's bolted in.

Tested that method on two live questions rather than just stating it.
Skills scoping (does a run see every skill, or a subset): turned out not to
be seam h's question at all, h is just the adopted SKILL.md format, full
stop; which skills a given run can see is an ordinary instance of m's
per-unit permissions cutting a scoped view, enforced the same way any other
memory read is. Persona setup and selection (how does the owner define a
work persona, how does a run end up bound to the right one): setup is an
ordinary owner-authorized, kernel-written permission change (a, then j,
landing in m), selection is the router's ordinary assembly job (d, handed
to the harness as the identity fragment via i), no new mechanism either
way, though d's row picked up a real candidate finding, persona might need
to resolve before the rest of the roster since it could gate which
tools/models are even eligible, not decided, flagged for the real walk.
Findings recorded on h, m, and d's rows.

Clarke also proposed replacing the vague "walk pairs that plausibly
interact" missing-seam-hunt step with an actual exhaustive matrix, every
piece against every piece (13 nodes, Host OS excluded as substrate, roughly
13x13), checked as a real table in research-plan.md rather than a mental
pass, so completeness is checkable later instead of trusted.

Then, thinking through why j and p don't fit the piece-pair shape cleanly,
a bigger question opened: is a seam even the right unit at all? POSIX's
real precedent, recalled, not yet verified against the actual spec text,
is that a kernel boundary isn't organized by which piece is on the other
end, it's one boundary (process to kernel) decomposed into many
individually named operations (read, write, open...). If that's the right
model here, q and p might not be separate top-level seams at all, they
might be two already-half-found named operations under j's one boundary,
and the current 18-letter list could be the wrong unit entirely. Not
something to decide on a recollection mid-session, and not something to
decide in a context this loaded either.

Landed: suspend phase 2 as drafted rather than run it on a possibly-wrong
assumption. Added a client-mode-switch idea to ideas.md (an explicit way to
tell a client "you're talking to work-me now" rather than always inferring
persona) while it was fresh, since it came up discussing persona selection
and isn't needed yet. Handoff written for a fresh session to check pieces
against real POSIX structure too (Clarke's bet is they'll hold, but
asserted, not verified, same as everything else in this project), and to
actually resolve the seam-shape question against the real spec text before
any seam work continues.

---

## 2026-07-12 — Fleet manager (1.12) covered: cut a restatement bug in the prose, Letta's Constellation is real precedent and a real counter-example at once, phase 1 down to the close-out step

Piece pass on Fleet manager, seam-sides k and n. Prior art first: OpenClaw has
no fleet concept at all, two open, unshipped GitHub feature requests confirm
the gap directly rather than by absence (#47871 node awareness, #38878
multi-device shared memory). Khoj is a centralized hub, one server, many thin
clients, not peer machines, consistent with its running role as the
centralized-cloud-authoritative counter-example everywhere else in this
tracker. Letta's Remote Environments is the real find: an actual shipped
product where the same agent moves mid-conversation between machines and its
memory follows it, exactly the "who's driving" shape anatomy.md's Fleet
manager paragraph already describes. But it's brokered through Constellation,
Letta's own external cloud coordination service, which makes it simultaneously
real precedent that the shape works and a real counter-example to anatomy's
one-instance-per-machine rule. I initially flagged that as an open question;
Clarke caught it as already-settled ground, the same per-machine/no-anchor
scope Router's 2026-07-14 pass already nailed down for the whole
system-control class, and "Flagship" is retired pre-reset vocabulary, not a
door to reopen. The distinction that survived: what the rule forecloses is an
*external* cloud-authoritative single point the way Constellation is; a
leader-election or primary-designation strategy run among the owner's own
already-live per-machine instances is still open to implementations, Signal's
primary-device-among-peers being the closer real precedent than Constellation
for that internal strategy.

Adjacent real standards landed on k and n's rows: Signal's linked-device
model (primary device mandatory, others join by scanning its QR code,
owner-authenticated, no third party approves; revocation induces a key
ratchet and a newly-linked device doesn't inherit past history — admission
and revocation are not symmetric operations, a real gap in how the piece's
prose had been treating them as one clause); Tailscale's device approval (an
admin approves a new node; deauthorizing and deleting are two distinct,
differently-final actions; node-key revocation near-instant); Apple
Continuity/Handoff as the closest real precedent for "who's driving"
specifically, a lightweight activity-description signal kept architecturally
separate from bulk data sync (iCloud) — real evidence it's a different kind
of problem from sync/conflict/partition even though the piece's prose named
them in one sentence.

The prose fix itself: "two fleet managers on the same machine disagreeing
about who else is in the fleet is exactly the kind of fact that can't have
two answers" was a near-verbatim restatement of the general rule already
stated once at the document's top ("who is in the fleet" is literally that
section's own example). Cut, replaced with the same offline-resilience
reasoning Router's and Kernel's paragraphs already give for the identical
one-instance-per-machine fact. Same bug shape as the restatements Router's
pass removed 2026-07-14; worth watching for on the remaining pieces.

Two open questions landed on seam n's row rather than resolved here: who
authorizes a new machine's admission, and whether an admitted machine gets
the full data immediately or a scoped subset. Real find surfaced while
routing those: seams k and n together are also the cross-implementation-fleet
interoperability contract, a machine running one compliant fleet-manager
implementation joining a fleet whose sibling machines run a different one.
Clarke's reaction was immediate, real stakes for a future Collective/Armada
made of independently-built fleets, not something either of us had connected
before. Gated behind Collectives per the project's own sequencing rule, so it
went to ideas.md rather than any further work this session.

Phase 1's piece pass is now down to its close-out step, 1.13: read anatomy.md
end to end for coherence, confirm every seam row holds the questions phase 1
raised, sync anatomy.html, move the "Currently on" line to phase 2.

---

## 2026-07-12 (later) — Provenance (1.11) covered: prose confirmed as-is, two of three inherited prior-art citations turned out to be real-standard-wrong-fit, the actual working precedent found elsewhere

Piece pass on Provenance, seam-side r. Forked the research (C2PA, Sigstore,
OpenFang's signing, and whether the digital-counterpart systems have any
content-trust mechanism at all) to keep the scrape noise out. The prose
itself needed nothing: "trust-tags content... where it came from... whether
it arrived from the open web" was already origin-based and mechanism-agnostic,
a piece pass that finds the prose right the first time.

The real work was in the prior art the row inherited before this pass ever
checked it. Two of its three citations turned out to be genuine mismatches,
not weak precedent, wrong subject matter entirely. C2PA, checked against the
actual spec, tags media authenticity at the point of creation, was this
image AI-generated, who made it, not whether an agent should trust a piece of
ingested text; no assertion in a C2PA manifest is even mandatory, so content
from a bad actor just ships with no manifest at all. Sigstore/SLSA is
software-supply-chain attestation, this build came from this commit, not
content trust. Both real, shipped standards, just answering a different
question than this seam asks. OpenFang's Ed25519 signing, also inherited into
this row, turned out to be a straightforward miscite: it signs capability and
identity manifests, tamper-evidence for tools, already correctly credited to
seam j's row from the Kernel pass, never content.

The real fits came from where the row didn't have citations yet. DeepMind's
CaMeL (2025) is the load-bearing find: a taint-tracking interpreter that
attaches unforgeable origin tags to data as it enters the system and gates
sensitive actions when tainted data reaches them, tag-here/enforce-at-the-gate
as two cooperating mechanisms, matching this project's own Provenance/Kernel
split independently. Anthropic's own shipped tool-content containment
(a classifier inspects tool return values before they enter context) is real,
current, working precedent for active inspection specifically. OpenAI's
Instruction Hierarchy, shipped as message-role privilege, confirmed real but
confirmed insufficient alone: a coarse role can't tell a web search result
from the owner's own file if both arrive under the same role, necessary
background, not a full answer. OpenClaw's own issue tracker names this seam's
exact shape (issue #7707, "Memory Trust Tagging by Source," open,
unimplemented, source-tiered trust with decay) without having shipped it,
this project's running pattern again, the digital counterparts recognize the
problem and haven't built the piece. Hermes, Letta, and Khoj: no mechanism
found, consistent negative.

Clarke's actual scope call, the substantive decision this session: keep
MSI-1's mandatory core to mechanical origin-based tagging only, tied to which
fetch or tool path content crossed in on, and treat active content inspection
(Anthropic's shape) as a legitimate richer tier implementations can compete
on, not something invented into the mandatory core. Consistent with his
standing instruction for this phase: minimal correct core, park real
questions rather than resolve them here, get the piece prose and seam-sides
right so MSI-1 and a first, known-inadequate reference implementation can
actually ship and pull in outside help, depth comes from iteration once
something real exists. Decay/reassessment policy (OpenClaw's ask) parked the
same way, owner-policy-data territory, not a seam-mandatory obligation, for
seam r's real walk to actually settle. C2PA and Sigstore's citations were
also a small scope decision: kept in the row rather than dropped, honestly
flagged as checked-and-mismatched, real standards just answering a different
question, a different case from kernel.chat's outright removal since these
two actually exist.

No gap without a seam to live in; both real questions landed inside seam r's
own row, parked for step 2.14. Provenance done for this pass; next unchecked
step is 1.12, Fleet manager.

## 2026-07-12 — Credential broker (1.10) covered: mediates, doesn't store, matching Memory provider's derive-never-own shape; two distinct real mechanisms found, scoping vs. keeping plaintext out of hands entirely

Piece pass on Credential broker, seam-side p. Forked the research (OS
keychains/keyrings, HashiCorp Vault, sops/age, `pass`, systemd-creds, plus
the usual four digital-counterpart systems) to keep the scrape noise out.
Real finding, the useful kind a piece pass exists to surface: two genuinely
different mechanisms were hiding under one seam-row line. Scoping/expiry of
what the secret even is (Vault's dynamic secrets, a fresh uniquely-scoped
credential minted per lease and auto-revoked; AWS STS's short-lived, scoped
credential triple) is real and load-bearing for "custody, not permission,"
but both still hand the live secret back to the caller as an ordinary return
value. Keeping the plaintext out of the consumer's hands at all is a
separate, harder property, and the best real precedent for it, Claude Code's
own sandbox proxy and OpenClaw's sentinel mechanism, independently converged
on the same shape: the consumer only ever holds an opaque placeholder, and a
component outside its own memory swaps in the real value only at the
network/adapter boundary, right as the call leaves. OpenClaw's own docs
honestly flag theirs as same-process protection only, a real, weaker
guarantee than Claude Code's cross-process sandbox boundary. Letta adds a
third variant, redaction in both directions, substituted in and scrubbed
from tool output on the way back, with secrets bound to agent identity
rather than machine. Confirmed negative precedent throughout: OS
keychains/keyrings and sops/age/`pass` all hand plaintext straight back to
the requester, encrypted-at-rest only, no injection concept, useful as the
baseline the piece has to go beyond and nothing more. Hermes and Khoj are
real negatives too, plaintext env vars, no scoping, no broker concept at
all, the same pattern this project keeps finding for those two systems
across every system-control piece.

Clarke's catch, and the actual substantive finding of the session: the
prose said the broker "stores" the secrets, and that's wrong. Every real
positive precedent (OpenClaw's `SecretRefs`, Claude Code's proxy reading
from an external vault) treats the broker as a mediator reaching into
wherever the secret actually lives, an OS keychain, a vault, a hardware
token, never a second copy of its own. That's exactly the Memory provider
shape (derives, never owns) landed two piece passes ago, applied to a
different piece. Rewrote the prose around that: the broker mediates between
wherever secrets actually live and the specific call that needs one, and
widened "never sits in a model's context or a harness's hands" to include
"a sandboxed action's own view," the third real consumer the research
surfaced.

Also caught myself mid-session conflating phase 1 (piece pass) with phase 2
(the real seam walk): I opened by asking Clarke to decide whether injection
needs a declarable strength tier and whether broker custody should be
identity-scoped rather than machine-scoped, both real seam-p-contract
questions, but decisions for step 2.12, not this step. Corrected, and both
landed as open questions in seam p's row instead of forced decisions.
Credential broker done for this pass; next unchecked step is 1.11,
Provenance.

## 2026-07-14 (later) — Kernel (1.9) covered: contract + enforcement + placement-authorization confirmed as one honest piece, a self-referential gap in the piece's own prose fixed, OpenFang upgraded to real reference-implementation material

Piece pass on Kernel, seam-sides i (hands the run its contract), j
(enforcement), q (authorizes placement, doesn't place). j's row already
carried heavy mechanism-level prior art from the Sandbox and Router passes
(seL4, AWS IAM/STS, Kubernetes admission webhooks, the OpenRouter/LiteLLM/
Hermes/OpenClaw budget findings), so this pass's real job was narrower: does
bundling those three seam-sides into one piece hold up against real systems,
in the same spirit as Router's trigger/assembly check, and does the piece's
own prose actually say what it means.

Forked the research to keep the scrape noise out. Verdict: confirmed honest
bundle. The Linux kernel is the strongest match, verified against real
mechanism, not just the loose analogy anatomy.md already draws: runc and
containerd only configure namespaces, cgroups, and seccomp once at process
launch, via kernel syscalls; every syscall after that is checked in-kernel,
continuously, via LSM hooks and the installed seccomp filter, for the life
of the process. CVE-2024-21626 (a real runc fd leak exploitable in the
window before seccomp took effect) is direct evidence this is a gap in the
kernel's own enforcement window, not proof of some separate enforcing
component picking up the slack. seL4's capability invocation fuses creation
and access-checking cleanly (i+j), but has no placement concept whatsoever,
a real, named limit on how far that precedent reaches toward q.

Checked all four digital-counterpart systems by name, at Clarke's insistence
that they get equal depth to the OS-kernel checks, not a token pass:
OpenClaw has a genuine, separately-documented policy-cascade layer
(global→provider→agent→session→sandbox) plus exec-approval allowlists,
explicit that skills carry no permissions of their own, a real positive
match verified against its own security docs. Hermes has a real
gateway/sandbox-host split enforced via SSH ForceCommand, `cap_drop: ALL`,
and env-var allowlisting at the exec boundary. Letta has RBAC, but only at
the API-endpoint level, not per-action in-run enforcement, a real partial
match, a gradient rather than a clean yes or no. Khoj is a genuine negative:
no first-party enforcement layer exists, and its own community discussion
explicitly flags self-hosted agent security as unsolved. Checked Kubernetes
as the real counter-example, the same role it played in Router's pass:
scheduler, admission controllers, and kubelet are genuinely three separate
components there, but that split costs a persistent control-plane (etcd,
the API server) to stay coherent, a cost single-machine Mojo doesn't pay, so
it reads as a real documented tradeoff, not a refutation of the bundle.

Real find, upgrading something already in the tracker rather than adding
something new: OpenFang (real repo, real docs, not marketing copy)
independently converges on this exact bundle under a literal
`openfang-kernel` component: agents declare required tools, the kernel
enforces them, capabilities are immutable after creation and enforced
kernel-side via capability gates and a WASM sandbox, with the same component
also handling budget and placement metering. That's i+j+q, arrived at
independently, under the same name. Upgraded from this project's prior
"unproven, mechanism ideas only" characterization to a genuine candidate for
scaffolding the kernel's first reference implementation, still young, not
production-hardened, but real. Clarke had a lead that kernel.chat might have
become Microsoft Agent Framework; checked, no connection found, MAF is real
(GA'd April 2026, the Semantic Kernel + AutoGen merger) but nothing ties it
to kernel.chat specifically, more likely a false memory triggered by
Semantic Kernel's own name. kernel.chat dropped from j's row and the
precedent library, no such product exists. Scoped whether MAF itself was
worth a real research pass and decided against it without spending session
time: nothing surfaced suggesting it's a capability-enforcement system
rather than an orchestration framework, so it's not obviously this seam's
business, and speculative research into an unscoped lead isn't what a piece
pass is for.

One real prose gap, caught by Clarke, not the research: Kernel's own line,
"every consequential action... passes through here and is checked," read
literally, has the kernel gating its own reads and writes of the data it
enforces, which can't be right, flagged and left open during Router's pass.
My first fix attempt scoped it to "a run takes," which was too narrow;
Clarke's catch: it isn't just runs, it's every other system-control sibling
(router assembling, credential broker injecting, provenance tagging, fleet
manager admitting a machine) and a client writing straight to the data too,
the notes-app case. Landed as "every consequential action any other piece in
the system takes," covering all three, excluding only the kernel's own
bookkeeping, and actually catching the piece prose up to what seam j's row
already said ("Kernel ↔ everything consequential") rather than narrowing
anything. Second, smaller fix: the Pieces table's q-side read "places
actions," which contradicts the piece's own prose, the kernel authorizes
placement, Sandbox does the placing; tightened to "authorizes placement."

No new seam-question gaps surfaced beyond what i, j, and q's rows already
carry. Clarke's call: Kernel is done for now.

## 2026-07-14 — Router (1.8) covered: watching triggers and assembling a run confirmed as one real piece, system control's read/write split named once at the top, two corrections landed in vision.md

Piece pass on Router, seam-sides d (task → a run's assembly) and g (a
trigger → a run). Started with the real question a piece pass exists to ask:
is bundling "watches triggers" and "assembles a run" into one piece an
honest reading of real systems, or an artificial merge nobody else does.
Checked Apache Airflow's scheduler (its own docs: one required, always-on
component "handles both triggering scheduled workflows, and submitting Tasks
to the executor to run") and systemd (PID 1, watches timer/socket/path/D-Bus
triggers, resolves the unit's dependencies and resource limits on
activation). Both real, mature, widely-deployed systems land on the same
two-jobs-one-piece shape Router already had. Checked Kubernetes as the real
counter-example (CronJob controller and scheduler are separate components) to
confirm the merge is a genuine design choice, not the only option, before
accepting it.

Forked the digital-counterpart research (OpenClaw, Hermes, Letta, Khoj) to
keep the raw scrape noise out of the main thread, three questions: does a
missed trigger catch up on restart, is scheduling per-device or centralized,
can more than one run be in flight at once. OpenClaw and Hermes independently
converge on the same shape, gateway-local scheduler, persisted job state,
tick loop, a separate concurrency lane for triggered runs so they don't
starve behind a live conversation. Khoj is the real outlier, scheduling runs
server-side through Khoj Cloud, one centralized authority rather than
per-device. Letta's sleep-time compute turned out structurally
uncomparable, it's memory-consolidation work coupled to the primary agent's
lifecycle, not a scheduled trigger with a due time to miss.

Clarke's catch, mid-discussion: none of this is about picking an
implementation mechanism, that's exactly the competition the MSI is supposed
to leave open. The job was to find out what the standard actually has to
require. Re-sorted the three findings on that basis:

- **Liveness/catch-up is a real seam question.** OpenClaw's own issue
  tracker shows the failure mode in the wild, trigger state living only in
  the running process gets lost across restarts and stale locks jam future
  ticks, contradicting its own docs' claim that this was a deliberate
  non-issue. Landed as an open question on g's row: a trigger's evaluation
  state has to be recoverable from durable, data-shaped storage, not
  router-process memory, or restart behavior isn't something two compliant
  routers can be expected to agree on. Whether a trigger record needs an
  explicit owner-set catch-up policy (systemd's `Persistent=true` is real
  precedent) or silent-drop-on-restart is an acceptable default stays open
  for g's real walk.
- **Scope (per-machine, not flagship-only) wasn't actually open.** Anatomy.md
  already says system control is singular per machine, and Kernel's own
  paragraph already gives the reasoning, offline resilience, each machine
  keeps enforcing even disconnected from the rest of the fleet. Router
  follows the identical pattern for the identical reason, and cross-machine
  coordination (two machines' routers reacting to the same fired trigger) is
  explicitly the fleet manager's problem, seam k, not Router's own — noted
  as a concrete instance on k's row for whenever that seam gets walked.
  "Flagship" isn't live vocabulary in this document anymore; that was
  pre-reset terminology.
- **Concurrency isn't a Router-specific question at all.** The
  roster-plus-per-action-kernel-mediation mechanism already established
  across d/j/q/s during the Sandbox pass is what makes concurrent runs safe
  regardless of how many a given router allows at once; nothing new needed
  here. The one real keeper: Hermes disables cron-management tools inside a
  cron-triggered run specifically to block self-triggering loops. Not a new
  mechanism either, it's a second real data point for j's existing
  budget-enforcement finding (the OpenClaw $200 runaway-retry incident),
  not a new one.

Prose landed two real cuts and one fold-in. Cut: "every grant is made per
run; which instance actually handles a given action is resolved live,
through the kernel" — already stated once, under "Services are plural."
Cut: "a run can spawn sub-runs, each of which gets its own grant,
recursively" — already stated once, under "Runs are ephemeral." Fold-in: "or
the owner asks for something" stops being a separate clause and becomes g's
fourth trigger kind, alongside the clock, outside events, and idle time — a
direct request isn't a different mechanism, just a different trigger source.
Router's own paragraph also picked up the per-machine reasoning every
sibling system-control piece's paragraph already states for itself
(kernel: offline enforcement; broker: custody can't disagree; provenance:
laundering through the laxest tagger; fleet manager: fleet membership can't
have two answers) — Router's prose was the one missing it.

Clarke's question about how Router reads owner policy surfaced a bigger
finding than Router itself: every system-control piece reads the data, only
the kernel writes it (grants, narrows, revokes, records — literally
Kernel's own paragraph, just never named as the general rule). Applies to
all five, not just Router, so it went into anatomy.md's top-level "System
control is singular" section as a second rule sitting inside the first,
rather than getting restated five times across five paragraphs. One real
loose end flagged rather than resolved: Kernel's own line, "every
consequential action... passes through here," read literally, would have
Kernel gating its own reads and writes of the data it enforces, which can't
be right. Left for j's real walk, recorded on j's row.

Two corrections landed in vision.md, both Clarke's catch, not something the
piece pass surfaced on its own. First: vision.md's claim that no
implementation of a kernel or memory-layer analog exists was wrong on its
own terms, real enforcement logic and real memory stores already run today,
just buried inside monolithic products (Claude Code's own sandbox, Mem0,
Zep, Letta, Obsidian plus its community plugins, all already checked during
earlier piece passes) and never factored out as a swappable piece behind a
published contract. Rewritten: Mojo's reference implementations get built
against those buried implementations, codified rather than invented from
nothing, the same discipline POSIX used against real Unix systems. Second,
broader correction: it isn't just kernel and memory layer that need a first
Mojo-built reference. The tracker's own Posture column shows most seams are
MSI, not Adopt, meaning Router, Sandbox, the credential broker, provenance,
the fleet manager, and Client sit entirely behind not-yet-invented contracts
too, same situation as kernel and memory layer, nothing existing can conform
to a contract that doesn't exist yet. Rewritten to say Mojo builds the first
reference implementation of every piece behind an MSI-posture seam, kernel
carrying the most weight of any of them as the enforcement layer, not the
only one built from scratch.

One thing discussed and deliberately left alone: whether a good-enough
kernel could become the de facto locked-in piece the way the Linux kernel
is for most real Unix-like systems, competition converging on one dominant
implementation even though the standard never mandates it. Judged already
covered by vision.md's existing "someone smarter than me can and should
build a better one" framing, plus it's a natural consequence of the kernel
being the single most competitive piece to build well, not a gap that needs
new prose.

## 2026-07-13 (later) — Sandbox (1.7) covered, and a bigger cross-cutting mechanism landed alongside it: every service is granted as a roster, resolved per-action through the kernel's existing gate, budget enforcement is a real kernel obligation not an optional extra

Started as a straight piece pass on Sandbox, prior art gathered first
(Claude Code's own sandbox, the most load-bearing find since it's literally
what this session runs under: Seatbelt on macOS, bubblewrap+socat on Linux,
filesystem default-deny outside the working dir and session temp, network a
domain allowlist enforced by a proxy, credentials injected as sentinels and
swapped for real values only at the proxy boundary, a working answer for how
q and the credential broker interact; E2B's always-remote Firecracker
microVMs; WASI's capability-handle model, no ambient `open()`, only
`openat()` against handles passed at instantiation; Landlock as a real
unprivileged default-deny backend; and the digital-counterpart set,
OpenClaw's opt-in mode/scope/backend sandbox config plus a real SSH
remote-placement backend with its own host-key auth, Letta's
no-built-in-sandbox third-party-provider pattern, Khoj's total absence of
execution isolation, Hermes's Docker-as-whole-boundary). Verdict on the piece
prose: holds as written, the container/isolated-process/other-machine/
remote-service category list is real, and "what it can see" stays a single
clause on purpose, the finer filesystem/network/secret split belongs in seam
q's contract, not anatomy prose.

That surfaced Clarke's real question, though: an OS-portability check (the
piece prose already correctly keeps Landlock/Seatbelt/bubblewrap out, those
are backends, not the contract, nothing to fix there) led into whether "one
sandbox per run" is even the right granularity, since a single run's
different turns plainly want different things. Clarke floated three
candidate mechanisms: the router deciding per step in advance, sub-run
spawning, or handing the run a set (a roster) with the pick made live. Worked
through the first and ruled it out on the architecture's own terms, the
router assembles once, before the run starts, and would need to already know
the run's steps to pre-bind them, but planning strategy was already carved
out as harness-internal (seam i). That leaves roster-vs-sub-run, and they
answer different kinds of "different": sub-run for a genuinely separate unit
of work, roster for the same ongoing task reaching for a different instance
mid-stream. The roster shape isn't new, either, f already found it for
Tools; the proposal was just to recognize it as the general answer, not a
Tools-specific quirk, made non-arbitrary by the observation that j's kernel
already mediates every consequential action individually, not per-run, so
resolving "which granted instance" per action is that same gate doing one
more job, not a new mechanism.

Stress-tested against real capability/permission systems before accepting
it: seL4's capability invocation is a local kernel-table check with an
explicit fast-path built specifically to avoid a round-trip on this exact
hot path (Liedtke, "IPC performance is the master"); AWS IAM/STS session
policies are evaluated locally per call against the document attached to the
credential; MCP itself, already adopted at f, negotiates once per session
then dispatches cheaply per call to whichever open session owns the tool.
The rejected alternative, live round-trip to a decision-maker before every
action, is real too (Kubernetes admission webhooks) and is documented as a
known operational cost, reached for only when a decision can't be
precomputed, never a default. Then checked against the actual class of
system Mojo is standardizing, not just infra, per Clarke's correction: Hermes
ships almost exactly the proposed mechanism, a main model plus 11
independently-overridable slots, each with its own fallback chain, swappable
mid-session with memory intact, including fully automatic swaps; OpenClaw
supports it via explicit rebinding rather than continuous resolution; Khoj
is the real negative case, one model fixed per agent, "switching" means
picking a different agent, which maps onto sub-runs, not roster resolution.
Hermes also named a real cost honestly, a mid-session model swap resets the
prompt cache, confirming swapping isn't free and the contract shouldn't
pretend otherwise.

Clarke's follow-up sharpened it further: why does the kernel check the
roster at all, and does a roster entry need tags. Separated two jobs that
were getting conflated, granting a roster is provisioning, an optimization
surface for whoever picks; checking against it per action is enforcement,
the kernel not trusting the picker regardless of who or what made the pick,
same reason Unix enforces file permissions in the kernel instead of trusting
a process to only open() what it should. That split the tags question
cleanly too: capability tags (what an instance can do) are pure
selection-layer metadata the kernel never needs to understand; cost tags
(what an instance consumes) cross into enforcement, because budget is
already owner policy data, a hard limit, and metering spend against a hard
limit is the kernel's job. Checked against real systems: OpenRouter and
LiteLLM both keep budget enforcement structurally separate from capability
routing and genuinely hard-blocking (OpenRouter's `limit_usd` returns a 402,
LiteLLM ships a named "Agent budgets" feature); Hermes enforces a hard
iteration budget the same separated way but has no dollar-cost enforcement
at all; OpenClaw has no spend cap whatsoever and has a documented real
incident, a runaway retry loop burning $200 in one session. Every system
that skipped hard budget enforcement got burned for it, landed as a real
argument that budget enforcement belongs in MSI-1's kernel obligations, not
left optional.

Landed, Clarke confirmed: every service is granted as a roster at assembly,
resolved per-action through j's existing gate; capability tags serve the
picker, cost tags serve the kernel's budget enforcement; mid-run swaps carry
a real, sometimes nontrivial cost (Hermes's cache-invalidation case) that the
contract must name rather than hide. Recorded as a finding across d (resolves
its own open multi-endpoint-per-step question), e (the swap-cost caveat), f
(cross-reference, this is where the roster shape was first found), j (the
load-bearing mechanism and the budget-enforcement obligation), q (Sandbox
gets the same roster treatment as everything else), and s (reinforces Memory
provider's existing plurality finding). Sandbox's own open questions, not yet
answered, recorded in q's row rather than resolved here: whether isolation
strength should be a declarable capability tag, whether null/host placement
is a legitimate placement MSI-1 must express, whether placement nesting
needs an explicit no.

One more pass followed once the seam findings were recorded: Clarke caught
that Sandbox's own prose still had it backwards, "where an allowed action
runs... is the sandbox's" reads as if the sandbox itself decides, when a
sandbox is the boundary chosen among, not a chooser, the harness reaches for
one and the kernel confirms the pick was actually granted. That fix
generalized immediately: Model endpoint's and Tools' prose already carried
their own versions of the roster-and-optimal-use point (independently, from
their own earlier piece passes), so the same content was sitting in three
places at once. Pulled it out to the one spot the document already uses for
system-wide invariants ("Services are plural," alongside the swappable-piece
rule at the top), stated once: a run is granted a set per service, not
handed one, a harness resolves which instance a given action uses at
whatever granularity the task calls for, a service decides nothing itself,
and the kernel checks every pick against the grant the same way it checks
everything else consequential. Clarke confirmed Memory provider fits the
same shape too, with its own real instance of "different granularity": not
just different provider instances, different views and scopings over the
same data (vector vs. grep) picked per need. The four service paragraphs
trimmed down to only what's actually piece-specific once the general
statement covered the rest (Model endpoint keeps modality-typing, Tools keeps
the protocol-vs-kernel distinction, Memory provider keeps the
derives-never-owns reasoning and the view/scoping point, Sandbox keeps the
category list and the decides-nothing correction). Router's own paragraph,
a few lines below Sandbox, was left saying "picks which model endpoint...
which sandbox" as single picks, a direct contradiction sitting right next to
the corrected text; synced its wording to match (grants a set, resolution
happens live through the kernel) without doing Router's actual piece pass,
no new prior art, no seam confirmation, no Pieces-table flip, that stays at
1.8 in full.

Edits made: anatomy.md's "Services are plural" bullet and matching ASCII
diagram line rewritten to state the roster-plus-per-action mechanism once;
Model endpoint, Tools, Memory provider, and Sandbox prose trimmed to
piece-specific content only; Router's wording synced for consistency
(not its full piece pass). research-plan.md's Pieces table Sandbox row
flipped to Covered; seam rows d, e, f, j, q, s gained this session's
findings; "Currently on" line advanced to 1.8 (Router); msi-steps.md's 1.7
checked.

---

## 2026-07-13 — Memory provider (1.6) covered: derives-never-owns confirmed as a deliberate rejection of the real AI-memory market (Mem0/Zep/Letta all store-own, only Obsidian's plugin ecosystem actually derives), curation is a two-tier problem (live incremental write-back already handled by seam i, deep consolidation is an ordinary triggered run needing no new "overseer" piece), and seam s gets a real POSIX lean (VFS operation-table over ioctl's freeform-arg wart)

Piece pass, worked with Clarke end to end. Prior art checked split cleanly
into two camps. **Derive-only**: Obsidian's vault (canonical plain files) plus
its plugin ecosystem — Smart Connections (vector semantic search) and Smart
Relations both build indexes explicitly advertised as rebuildable from the
vault, no separate database of record, several such plugins running over one
vault at once. This is the only strong real match for what the piece's prose
already claimed (the Obsidian analogy), and it isn't even an AI-memory
product. **Store-owning**: Mem0, Zep, and Letta (already surveyed for other
pieces) are all the opposite — Zep's own "buy vs build" post describes Mem0
as a vector index plus KV store that IS the memory, no canonical layer
underneath; Letta's memory blocks live inside Letta's own agent state. The
dominant pattern in the actual AI-memory market is exactly what Mojo's
derives-never-owns rule forbids. Landed as an explicit, acknowledged
divergence rather than something to paper over: MSI-1 is choosing the
Obsidian shape on purpose, against the market's dominant real pattern, not
because no one else does memory servers, but because sovereignty (your data
survives switching the app) is the actual point and store-owning kills it by
construction.

That surfaced the real question: is this even buildable given real
engines assume they own their store? Worked through what "adapt the prior
art" concretely means. Extraction (LLM reads conversation, produces facts)
and retrieval (embedding search, graph traversal, ranking) are two different
things bolted together in every camp-two product. Retrieval is reusable
regardless of camp, pure implementation detail. Extraction is the part that
assumes ownership, and it turns out extraction was never this piece's job in
the anatomy to begin with (the prose only ever listed retrieval, indexing,
search) — it's a run's normal output, already covered by seam i's existing
"learnings, incremental, not batched" finding. So a Mem0-shaped provider
becomes buildable by splitting it: its vector/graph structure stays private
and genuinely rebuildable (re-embed, re-run the same extraction logic against
canonical facts); what has to change is that its extracted facts get written
back through the normal channel into the shared shape instead of living only
in its own DB.

That led into a second real question, Clarke's: if extraction/curation isn't
the provider's job, and it's real cognitive work, shouldn't it be its own
competitive axis rather than baked into whichever harness happens to be
running today's task? First pass landed on pushing all synthesis into a
deferred idle-triggered run (Letta's sleep-time compute as precedent,
already cited on seam g). Clarke pushed back correctly: that contradicts
seam i's own already-decided incremental write-back, Jarvis has to be
learning while you're talking to him, not just during downtime. Corrected to
two tiers, not one replacing the other, matching Letta's own real shape more
closely than my first pass did (core-memory live edits AND separate
sleep-time consolidation, not one or the other): live/incremental capture
(already decided, seam i, nothing new needed) plus a separate deep
consolidation pass (resolving contradictions, finding patterns, pruning —
work that live conversation pressure works against) that's still the real
competitive axis, still swappable, still no new piece needed. Clarke then
asked whether the consolidation pass needs an always-on "overseer" process
given it should fire more often than pure idle-time. Answer: no, Router
already is the overseer (always on, watches triggers, fires a run) — a
consolidation run is just another triggered run, and cadence (idle, periodic,
event-count-based) is router-implementation tuning, not a new mechanism.
No real prior art found for a genuinely continuous always-computing memory
daemon in any of the six systems surveyed; flagged as unproven, not built.

Landed, with an explicit callback: this whole tiered shape (live edits plus
periodic reorganization) is what Letta/MemGPT and, loosely, biological sleep
consolidation already converge on independently — Mojo isn't inventing new
memory architecture, it's taking a pattern that keeps getting reinvented and
refusing to let it stay welded to one vendor's box.

Second real finding, on seam s specifically: does locking the harness↔provider
envelope kill what makes a provider useful, the same worry already resolved
for e and f? Checked two real POSIX precedents. `ioctl(fd, request, arg)` is
the literal match (fixed outer call, arbitrary inner meaning) but is a
well-known design wart exactly because the free inner part lets every driver
invent an undocumented mini-protocol, quietly defeating the interoperability
the uniform call appears to promise — the specific failure this seam exists
to prevent. The VFS operation-table pattern (ext4/NFS/tmpfs implementing the
same fixed set of typed operations, freedom living underneath each one, not
inside one opaque call) is the healthier precedent and the new lean for s's
real walk: a small fixed set of typed operations (search/get/write) with real
defined shapes, provenance and permission-scoping mandatory on all of them,
rather than one MCP-tool-name free-for-all. Not yet Decided, a lean for step
2.7.

Edits made: anatomy.md's Memory provider prose rewritten (front-door framing
added: a run reads and writes through whichever provider it's handed, never
touching the data directly); research-plan.md's Pieces table row flipped to
Covered, seam s/g/m rows gained the findings and flag above; msi-steps.md's
1.6 checked, "Currently on" line advanced to 1.7 (Sandbox).

---

## 2026-07-12 (later) — Tools (1.5) covered: instance = one MCP server, a run gets a roster not a pick; piece names collapsed onto profile names (Window→Client, Agent runtime→Harness, Model→Model endpoint); the whole swappable/profile tag scheme simplified; Client confirmed to run on unowned hardware, caching flagged as maybe-not-at-all

Started as step 1.5 (Tools) and detoured into two real structural findings
before landing back on Tools itself. Both detours were legitimate piece-pass
work, not scope creep: they surfaced while actually reading the doc closely
for the Tools pass and changed content Tools' own prose depends on.

**Cardinality audit: three lifecycles were never four.** Walked every piece's
instance count (one vs. many, per-machine vs. fleet-wide) against the intro's
existing Runs/Services/System-control split and found Client had no bucket at
all — not per-run like a service, not singular-per-machine like system
control, just never named. The diagram already drew this right (the WINDOWS
box sat outside the "one of the owner's machines" box from the start); the
prose never caught up. Fixed: the split is four lifecycles now, a new
**Reach** bucket for clients (many, owner-scoped, not bound to any one
machine). Separately, Fleet manager was the only system-control piece
without its own explicit "one live instance per machine, because X"
sentence, even though its whole job is cross-machine coordination — the
worst piece to leave that implicit. Added the sentence (same
disagreement-must-not-happen reasoning as Kernel/Broker/Provenance, plus the
offline-resilience angle: each machine needs its own instance for the fleet
to stay coherent when one machine drops). Also caught a plain miss while in
there: Router's prose listed what it assembles per run (harness, model,
memory provider, sandbox) and left out tools, even though the diagram's own
binding line already included it. Fixed.

**The swappable/profile tag scheme was carrying dead weight.** Clarke asked
what `[swappable: X]` in the diagram was actually doing. Answer: "swappable"
is the system-wide invariant, already stated once at the top of the document
("every piece is a replaceable implementation") — repeating it on every box
and every piece's parenthetical violates the document's own rule about
stating cross-cutting facts once. Dropped it everywhere. That left `profile:
X`, which turned out to be carrying a second question: why do piece names
and profile names differ at all? Dug through the devlog rather than
guessing: profile names (Client, Harness, Router, Kernel, Sandbox...) were
coined 2026-07-09, in the "borrow POSIX verbatim or don't name it at all"
naming pass, before anatomy.md existed. Piece names (Window, Agent runtime)
were coined 2026-07-10, when anatomy.md itself got written and the project
moved onto plain names — and most pieces landed on the same word both times
by coincidence (Router/Router, Kernel/Kernel...). Window/Client and Agent
runtime/Harness didn't, purely because nobody went back and reconciled them.
naming-conventions.md's own illustrative list already said "window" and
"agent runtime," confirming the divergence was never a decision, just debt.

Clarke's call, explicit: collapse to the industry-standard word every time —
"stick to all the words that are actually used in the industry... that's
what makes this a standard built on what exists and also more easy to
adopt." Harness and Client win (both real terms of art; "harness" was
already winning in practice, used all over research-plan.md's own prose).
**Renamed piece-wide: Window → Client, Agent runtime → Harness**, swept
through anatomy.md (prose, diagram, seam table), research-plan.md (Pieces
table, seam table, dated findings' own session-name labels), msi-steps.md,
naming-conventions.md's canonical list, vision.md, interoperability.md,
README.md, and roadmap.md. Left two things alone on purpose: vision.md's and
anatomy.md's own poetic "a window into the same one identity" lines, which
use "window" as an ordinary English metaphor, not the piece name; and any
direct quotation of another project's own proper-noun component (OpenClaw's
"Agent Runtime" is their name for their thing, not ours).

**Model/Model endpoint checked against the same question, then closed the
same way.** Same 2026-07-09 origin as Client/Harness, so on the surface it
looked like the same bug. It isn't quite: "Model" is genuinely ambiguous
between the weights (not Mojo's concern) and the serving interface (the
actual conformance surface — a compliant piece is judged by its wire API,
not which weights sit behind it), and the piece's own prose already does
that disambiguating work ("wherever inference actually runs... any endpoint
speaking the standard wire shape is interchangeable"). Window/Client and
Agent runtime/Harness never had that job, they were pure duplicate
phrasings. Raised whether to close this last gap anyway, matching the same
"stick to industry words" principle (model endpoint is itself real
vocabulary — SageMaker, Azure ML, vLLM/Ollama docs all use it), and gave
Clarke an actual recommendation rather than leaving it open: take the rename
anyway, because leaving Model as the sole exception means the naming rule
needs an asterisk ("piece name = profile name, except Model, because
disambiguation") instead of holding with no exceptions at all — the cost is
a marginally heavier word in a few high-frequency spots (Router's assembly
list, seam e), the benefit is one rule instead of a rule-plus-footnote.
Clarke: "yeh do it." **Renamed piece-wide: Model → Model endpoint**, same
sweep as Client/Harness (anatomy.md prose, diagram, seam table;
research-plan.md's Pieces table, seam e's row, every dated finding's own
piece-pass label; msi-steps.md). This closes the tag question completely:
every piece's parenthetical is now just its bucket word (`reach`, `the run`,
`service`, `system control`), the `profile: X` tag is gone from every piece
paragraph with no exceptions, and the diagram's four `[profile: X]` bracket
lines were deleted outright rather than just de-"swappable"-d, since every
box title now matches its profile name (case/pluralization only) and the
canonical piece↔profile mapping already lives once, in "## The seams"' own
sentence. Tools keeps its own special case, below (no profile at all, not a
name mismatch).

**Tools (1.5), the actual step, landed once the above was settled.** Real
prior art checked: Claude Desktop's own `mcpServers` config, which runs
several independent MCP servers side by side (brave-search, filesystem,
puppeteer, sequential-thinking) — real, shipped confirmation that a Tools
instance is one MCP server, exactly parallel to a Model endpoint being one
instance, and that several can serve one machine at once the same way the
intro already claims generically for services. The real finding, surfaced by
Clarke asking directly how a harness actually gets linked to "the multiple
services that have the tools": unlike a Model endpoint, a run isn't pointed at *one*
Tools instance. A harness legitimately needs several tool capabilities live
in one run at once (search and filesystem and calendar together), so seam
f's binding is a roster — a subset of the machine's available servers
assembled per run — not a single pick. Model endpoint's own version of "maybe it's
actually a set, not a singleton" question is still open at d (whether a run
can get handed more than one same-modality endpoint); Clarke: "eventually a
roster will probably be right [for models too], each with their own
capabilities and permissions, but leave that for later." Nothing written for
models this session, flagged as already-open at d.

Tag simplified to `(service; no profile)`: "consumed format: MCP" was
smuggling two facts into one tag (no MSI profile, and MCP as the adopted
protocol) when only the first is actually piece-specific — MCP-the-protocol
is already stated in seam f's own row and in Tools' very next sentence, and
no other adopted-standard piece gets a tag naming its standard. Prose
rewritten to state the roster point plainly; the retired-Peripheral ground
(hardware and software tools are the same MCP shape) re-confirmed against
the same evidence, not reopened — Clarke's "whether physical peripherals
fit" question resolved by the instance-granularity answer itself, a camera
is just another MCP server in the same roster as everything else, no special
case needed. research-plan.md: seam f's row gets the roster finding plus an
open question for the real walk (how the router assembles the roster, how
f's contract exposes it); Tools' Pieces-table row flipped to Covered.

**Housekeeping:** msi-steps.md 1.5 ticked (relabeled "Tools (no profile)");
research-plan.md's "Currently on" line updated, next unchecked step is 1.6,
Memory provider.

**Client confirmed to work on unowned hardware, a real gap in its prose
found in the process.** Clarke's question: can a client run on a friend's or
work machine, e.g. a work laptop connecting back to the home system on a
scoped "work" persona? Mechanically yes, and it's a clean validation of why
personas exist at all (authenticate as owner via seam a from anywhere, get
served whichever persona's scoped view applies, no new mechanism needed).
But Client's prose only covered two cases, "one of the owner's machines" or
"a thin device that runs nothing else," and a borrowed work laptop is
neither. Fixed: the piece now states the actual defining trait plainly, a
client can run on any reachable device, ownership doesn't matter, since it
holds nothing authoritative locally to begin with. Surfaced a real open
question this creates for seam c: what a client may cache locally matters a
lot more on hardware the owner doesn't control. Clarke's lean, not decided:
caching might not be worth allowing at all, not just scoping it down.
Flagged on seam c's row for its real walk (2.16), alongside the
already-open two-clients-one-run question.

Also reconfirmed in passing: peripherals-as-tools still holds (unchanged),
and Model endpoint's own "maybe it's actually a roster too" question is
still open at d, not lost, not resolved this session.

## 2026-07-12 — Model (1.4) covered: vLLM/llama.cpp vs. Ollama's real architectural split resolved by the wire shape itself, vendor APIs confirmed in scope as mercenary-tier instances, mid-run same-modality model swapping flagged open for d

Checked vLLM, Ollama, and llama.cpp's current server docs against the Model
piece's existing claims before touching prose, per the phase 1 discipline.
Modality-typed endpoints held up (llama.cpp's own server lists chat,
embeddings, etc. as distinct paths, matching OpenAI's real shape). Found one
real thing not yet in the tracker: vLLM and llama.cpp each run one model per
server instance (confirmed on vLLM's own docs — "each vLLM instance only
supports one task" — and its forums, where multi-model means separate
processes on separate ports behind an external proxy); Ollama is the
opposite, one server multiplexing many models, loaded or swapped per request
via the `model` field and kept resident by `keep_alive`. Resolved without
inventing anything: `model` is already a per-request field in the wire shape
regardless of which architecture sits behind the endpoint, and `/v1/models`
(real, standard, implemented by all three) already answers "what's back
there." Landed as a finding on seam e's row, not a piece-prose change.

Real scope question surfaced and put to Clarke directly, since it touches
the piece's boundary: does the Model piece include vendor-hosted frontier
APIs (Claude, GPT) as an ordinary, capability-narrowed instance, or are they
handled by some separate mechanism not yet named anywhere? Argued for
inclusion on three grounds — yesterday's Agent runtime precedent (mercenary-
tier harnesses stay invisible to their own piece prose, the trust tier lives
entirely in d and j), step 2.1's own plan to read the Anthropic Messages API
alongside the OpenAI-compatible surface (only sensible if vendor endpoints
are in scope), and vision.md's own existing language for frontier models as
"hired help... handed the minimum fragment of context... never given access
to the system itself" (vision.md:154-160), which already describes exactly
this mechanism. Clarke confirmed: vendor APIs included, same
invisible-to-the-piece pattern as the harness case — the standard doesn't
get to decide models for people, self-hosted and sovereign by design, never
forced, and proper scoping lets mercenary-tier vendor access work safely
alongside it. Model prose rewritten accordingly (anatomy.md): dropped "local
or self-hosted," which read as excluding vendor APIs outright, in favor of
"wherever inference actually runs," self-hosted hardware the owner controls
or a vendor's hosted API, with no trust-tier language in the piece prose
itself, matching how Agent runtime's prose stayed silent on mercenary tier.

Also confirmed, not a design change: pointing a self-hosted server at
better/rented hardware, and a runtime on one machine calling a model server
on a different one, both already hold. Model is a separate piece connected
over seam e's wire, never assumed co-located with the runtime; the router
already picks "which model... on which machine" per run (anatomy.md), and
vision.md's owned/rented ladder already covers a rented machine as
"genuinely one of your machines for the length of the rental." Nothing to
add for either.

One real gap did surface from Clarke's own follow-up, left open rather than
resolved on the spot: the existing "a runtime may use different endpoints
for different steps of the same run" only clearly covers cross-modality
switching (chat vs. embedding vs. image vs. speech). Whether a run can also
swap between two models of the *same* modality mid-run (cheap model for
routine steps, stronger one for a hard step) is a live, unanswered question
about whether router assembly is one-shot per run or can hand a run a
candidate set to choose among per step. Flagged into seam d's row for its
real walk; either answer makes it a harder router+kernel job (assembly
grants the set, enforcement checks each call against it) but not a new
mechanism class, the same shape as the sub-run-gets-its-own-selection
recursion d's row already carries.

## 2026-07-13 — Agent runtime (1.3) covered: subprocess lean, write-back corrected off a false "at the end" framing, mercenary-tier harnesses land as a router+kernel decision, capability-declaration gap resolved against OpenClaw's real `supports()` predicate

Opened with real prior art before touching prose, per the phase 1 discipline.
Checked OpenAI's Agents SDK (`Runner.run()` vs. the raw Responses API is a
real, already-shipped version of the exact runtime/wire-format split
anatomy.md draws, their own docs name the seam explicitly), Claude Agent SDK
("the same tools, agent loop, and context management that power Claude
Code," bundled as one adopted-whole unit, matching the piece's framing almost
word for word), and CrewAI/smolagents/LangGraph as three genuinely different
internal shapes for the same job, real evidence that runtime internals are
actually competitive, not quietly converged. Two real gaps surfaced: every
precedent checked invokes its runtime as an in-process library call, never a
subprocess or network service, and nothing in the tracker picked a side; and
no SDK has a capability-declaration format a third party could consume.

Clarke's calls on both, argued not assumed:

**Subprocess.** His lean: a run should be its own OS-level process, on
purpose, against the grain of what the vendor SDKs actually do. Ties to real
precedent already sitting in the tracker (exec/argv/env, Erlang/OTP's cheap
isolated processes) and to anatomy.md's own existing "ephemeral... killable"
claim, not invented on the spot. Flagged for the future walk: a process
boundary buys killability and crash isolation, not the security boundary,
that's still j's job regardless of how i lands.

**Context management isn't one thing, and my first framing of it was
wrong.** Asked whether context management could be split out as a separate
concern. Real answer: part of it already is. What crosses a run boundary
(recent history, standing summaries, what a run needs to feel continuous) is
already i/m/s's business, the Window piece pass found this on 2026-07-11.
What's genuinely harness-internal is the live, in-run token-budget
management, and standardizing that would gut the exact competition the piece
exists to enable. Tightened anatomy.md's Agent runtime prose to say this
precisely instead of a blanket "manages context."

**Write-back: corrected off a wrong assumption, twice in the same edit.** My
first draft of the tightened prose said the MSI defines what a harness owes
back "at the end," implying a private staged copy reconciled at run-end.
Clarke caught this immediately, wrong and unnecessary at once: a harness
holds no private filesystem, it reads and writes against the shared data
directly throughout a run, the same way every consumer does, and a
private-copy-merged-at-the-end model would just reintroduce the exact
concurrent-write-reconciliation problem m's design (concurrent writes
reconcile at the data, never inside one provider) already exists to prevent.
Resolves seam i's standing incremental-vs-session-end open question:
incremental. Also a lesson in the piece-prose discipline itself: the wrong
framing was smuggling a seam-i timing mechanism into piece-defining prose
that never needed to assert it either way. Fixing the wrong claim and fixing
the over-explaining turned out to be the same edit.

**Mercenary-tier harnesses land as a router+kernel decision, invisible to
every other piece.** Clarke connected this to older ground: the
owned/rented/mercenary trust ladder from the pre-2026-07-08 naming era
(Keel/Billet-era devlog, machine trust specifically) generalizes cleanly to a
second, independent axis, harness trust, alongside the existing mercenary-vs-
chartered distinction at seam e for models. His call: to every piece
downstream, a mercenary-tier harness is an ordinary runtime, only d (which
candidate gets picked, and why) and j (what capability set it's actually
granted) know the difference, and router+kernel jointly own enforcement
across every trust axis, scoped to the degree the owner's policy wants.
Neither seam needed a new mechanism, d's existing selection-transparency
flag and j's capability-narrowing model already covered it, this just names
a real case of what they already do. Personas confirmed as the
identity/memory side of it: a mercenary-tier harness gets a persona built for
exactly that purpose rather than real access. Found the concrete prior sketch
already sitting in ideas.md (decoy cargo, served per-file/per-object,
translated back onto the real data on write, proxied for a whole session
length, named explicitly against Claude Code/Codex).

**Capability declaration, the harder gap, resolved in two research passes.**
First pass (vendor SDKs, orchestration frameworks) found nothing, no
third-party-consumable format anywhere. Clarke pushed back correctly: that
vendor-SDK vocabulary isn't even the right place to look, since Mojo's
mainline target is open-source community harnesses, closed vendor SDKs run
mercenary-tier only. Second pass found real precedent in OpenClaw
specifically: each harness implements a `supports(context) => {supported,
priority} | {supported: false}` predicate, core calls it during selection
(explicit config wins first, then `supports()` across registered harnesses,
then a fallback), and debug logging already exposes the chosen harness, the
reason, and every candidate's result, real shipped precedent for exactly the
selection-transparency shape d's row already wanted. Only one actual declared
capability flag exists anywhere checked (`authBootstrap: "harness"`,
OpenClaw, added ad hoc for one integration's real need). Letta's own 2026
deprecation of its declarative `tool_rules` system, explicitly to avoid
inhibiting frontier capabilities, is a second independent real signal against
building declarative capability machinery upfront. Landed lean: MSI-1 adopts
OpenClaw's shape (a `supports(context)` predicate per harness, mandatory
candidate-exposure and selection-reasoning as part of d's contract) rather
than inventing a capability schema, small, proven, not speculative. Also
surfaced OpenClaw's "prepared attempt" object (provider/model, auth, context
budget, transcript, workspace/sandbox/tool policy, streaming callbacks
resolved into one object before a harness runs) as real precedent worth
checking against when seam i is actually walked.

**A third pull on OpenClaw, for later, not for MSI-1 itself.** Once the
capability-declaration question landed, Clarke wanted more: OpenClaw isn't
just prior art for the standard, it's a real MIT-licensed codebase
(github.com/openclaw/openclaw) he can actually read and adapt when writing
Mojo's own router reference implementation, well after MSI-1 is drafted. A
GitHub issue on the repo confirms it swaps between real independent backends
(an embedded default runtime, OpenAI's Codex CLI, Anthropic's Claude CLI, and
ACP, Agent Client Protocol, as a generic adapter), stronger evidence than an
internal abstraction, proof the pattern works for genuinely separate,
independently-built agents plugging in as harnesses. Structure at a glance:
`src/` is the core app, `packages/agent-core/` is where the context-budget
work actually lives (confirmed via commit history), `packages/acp-core/` is
the external-CLI adapter. Exact file paths for `supports()` and the prepared-
attempt object's schema weren't pinned down remotely, real reference-
implementation work, deferred on purpose until that stage actually starts.
One caution logged here rather than treated as fact: some specific numbers a
web-scrape pass returned (star/fork counts) read as inflated and are
unverified, worth a real look (`gh repo view` or a clone) before ever citing
them anywhere that matters.

**Self-correction, same session:** Clarke asked directly whether the prose
and seam updates were still right after all the research, and checking
honestly turned up a real overclaim. Seam d's row stated the `supports()`
predicate's exact signature, the selection-priority order, and the
`authBootstrap` flag name as confirmed findings, but they were only ever
reported by the first research pass (web search, likely docs-level content,
not source), and the second, deeper pass explicitly could not verify them
against real code, it only confirmed the repo, its MIT license, and, via a
real GitHub issue, that OpenClaw genuinely swaps between independent real
backends. That general shape is solid and doubly sourced; the specific
mechanism details are not, and research-plan.md's seam d row now says so
directly rather than implying both were checked equally.

**Piece pass result:** Agent runtime's prose and seam-sides list (e, f, i, q,
h, s) both held, tightened not rewritten. Four seam rows carry real findings
out of this session (i, d, j, m), all leans, not yet Decided, that's phase
2's job.

**To resume:** next unchecked step is 1.4, Model (Model endpoint).

## 2026-07-12 — Peripheral (1.2) retired: not a piece, collapses into Tools

Picked up 1.2 mid-research, exactly where the last session left it: five raw
prior-art sources gathered, not yet synthesized with Clarke.

First pass at synthesis proposed a device-bound test (a peripheral is
whatever's bound to one of the owner's own machines, a camera vs. a generic
network-reachable tool). Clarke corrected this immediately: Jarvis has to be
able to reach anything in Tony's world, appliances, cameras, a suit, not just
compute the owner is sitting at. That killed the proximity framing, and my
replacement guess (physical-world sensing/actuation vs. software) didn't land
either, Clarke flagged it as confusing rather than clarifying, and redirected
to the right question: check whether Peripheral needs to exist as a separate
thing at all, against how OpenClaw and Hermes actually do it, rather than
theorizing a boundary from scratch.

Checked real running code rather than reasoning further. OpenClaw's
computer-use (browser control, desktop control, OS-level access) already goes
through its general tool-calling path, no separate contract, carried over
from the prior session. New this session: **Hermes-iOS**
(github.com/dylan-buck/Hermes-iOS), a real shipped companion app for
NousResearch's Hermes-agent. Its own README: "This app gives Hermes camera,
mic, health data, location, and notifications. All available in one MCP
tool." Mechanically, an iPhone connector streams camera/location/health/
motion data into a local SQLite DB, and Hermes queries it through 8 tools on
one MCP server (`get_user_location`, `get_health_summary`,
`query_sensor_data`, etc.), the identical mechanism as any other tool call.
Two independently-built real systems now converge on the same answer: no
separate wire protocol or declaration format for peripherals exists in
practice. It's tools.

Mechanical reasoning for why this holds up, not just "two examples agree":
the kernel already gates every consequential action the same way regardless
of which piece it came from (seam j is universal), and MCP already handles
declaration and invocation (tool listing + call). Seam o's job, "declare what
a peripheral offers, check every invocation," was already fully covered by f
and j combined. There was nothing left for a separate seam to contract.

Secondary finding surfaced by the Hermes-iOS evidence: some of what looks
like peripheral access isn't a live call at all. Location/health/motion
stream continuously into storage and get queried later
(`query_sensor_data`), structurally a memory/provider concern (m/s), not an
invocation. So "peripheral" data splits cleanly across two seams that already
exist, live actuation/sensing is a Tools call (f), passive telemetry is
memory data (m/s). No third mechanism needed anywhere.

Clarke's call, explicit: collapse Peripheral entirely, and framed why this is
the right posture for a first version, not just this one piece, "we're
trying to standardize what already exists... POSIX didn't invent new pieces
of a Unix system." If people want to add (or remove) pieces after MSI-1
ships, that's what versioning is for, but the first best effort stays as
close to prior art as it can get.

**Retired: the Peripheral piece, the Peripheral profile, seam o.** Ten pieces,
ten profiles, ten seams now, down from eleven. anatomy.md: the Peripherals
box and seam-o arrow cut from the diagram, the Peripherals piece paragraph
removed, Tools' prose gained a line that hardware and software tools are the
same shape (a camera and a search API are both just an MCP server), Kernel's
prose and the "eleven pieces" line updated, seam o's row removed from the
seam table. research-plan.md: Peripheral's Pieces-table row removed, Kernel's
seam-sides list dropped "o", seam o's tracker row removed, a Retired note
added recording the reasoning and where the two useful scraps of prior art
went (Matter/HomeKit's typed capability declaration folded into seam f as a
schema-richness check for device-facing tools; iOS/Android's manifest-plus-
runtime-prompt split and the W3C Permissions API's dead `revoke()` folded
into seam j as prior art for capability risk tiers and the revocation
question); the proposed seam-walk order renumbered; every stray "eleven"
became "ten". msi-steps.md: 1.2 checked off with a retired annotation, phase
3.1's profile count corrected.

Side discussion worth keeping: what "profile" actually means, since Clarke
asked directly whether it's just POSIX's word for "piece." It isn't. A piece
is anatomy.md's structural concept (what a part of the system is); a profile
is the conformance bundle POSIX would call an option group, the seam-sides an
implementation must satisfy to count as compliant. Most pieces get exactly
one profile, but not all: Tools has real MSI content (§4 pins an MCP version)
but, per seam f's current unwalked state, nothing additive layered on top of
raw MCP, so there's no separate profile to bundle yet, provisional, not
settled, pending step 2.2. Skills goes further, it isn't even a piece: no
separate running component exists anywhere, SKILL.md files just live inside
the identity's data and the Harness reads them directly. The identity itself
is the inverse case, not under-defined but the most detailed thing in the
whole document (§1, §2, seam m is called the flagship seam precisely because
it's memory provider's route to that data); its conformance target is a
schema, not a profile, because it's data, not swappable software. Tools stays
a piece regardless of its profile status, because it's a running service in
the same architectural category as Model endpoints, Memory providers, and
Sandboxes (chosen fresh per run, its own box in the diagram's SERVICES row),
unlike Skills which never rises to piece status at all. Recorded here rather
than in research-plan.md because it's a concept clarification, not a new
finding or open question.

**To resume:** next unchecked step is 1.3, Agent runtime (Harness). No
special phrasing needed, msi-steps.md's own session-start instructions
already cover it.

## 2026-07-11 — Window (Client) piece pass landed; msi-steps.md rewritten to research-first; Peripheral (1.2) opened, prior art gathered mid-session

Long single session, phase 1 of msi-steps.md. Also found, unrelated, at
session start: `interoperability.md` sitting modified in the working tree,
reformatted by an editor (hard-wrapped paragraphs collapsed, relative links
rewritten to absolute `file:///home/clarkehines/...` paths). Flagged to
Clarke; confirmed intentional, left untouched.

**Window (Client), 1.1, done.** Opened with the anatomy.md prose read aloud
and two questions (does a window's "never a separate copy" claim need to say
anything about caching credentials; what does the thin-device-vs-machine
distinction actually imply). Clarke redirected before answering either:
define the piece properly first, and check real prior art before guessing.
That redirect turned out to be the right call and reshaped the whole
session, see the process-rewrite entry below.

Checked real prior art rather than reasoning from memory: OpenClaw's Gateway
(session resolution + dispatch to an ephemeral per-message Agent Runtime,
channel adapters hold no state of their own, close structural match to
Mojo's own Router/Runs split) and its Command Queue (serializes messages per
session, a real open question Mojo hasn't answered — what happens when two
windows address one live run at once); Hermes-agent's "persistent session"
framing versus its actual hibernate/wake-on-demand mechanism (closer to
ephemeral-plus-store than the marketing suggests); Letta's agent/
conversation/session split (durable memory server-side, session as a
disposable client-side connection holding only transient turn state — the
closest match found, arrived at independently); Khoj, which proved two
things at once: self-hosted mode is structurally close to Mojo's own
single-machine shape, and its Obsidian plugin is real production precedent
for a non-conversational, direct-identity-editing window. Also checked
Matrix's device model + `/sync`, MIME's `multipart/alternative`, Kitty's
graphics-protocol capability query, and real-time voice-agent architecture
(Twilio-style telephony bridges, VAD-driven barge-in) to understand why a
phone-call window is a genuinely different transport shape from a
terminal's.

Landed definition: a window is a pure access surface, holds no session of
its own, store-shaped between runs (git/IMAP-shaped, not a live process to
attach to), live only for the duration of a run addressed to it, and does
not require an agent runtime or conversation to qualify — a direct identity
editor counts too.

Side question answered along the way: does "feels continuously alive"
(Jarvis knowing he's been talking to Tony) require an actual architecture
change, a runtime that never sleeps? No — traced this to a memory-quality
problem, not a run-model problem. Letta's "sleep-time compute" (memory
consolidated during idle periods by a background run, not a live foreground
agent) is real precedent that this is the right shape, and it's what seam
g's existing "downtime maintenance is just a run" line was already
anticipating. Landed as a finding on seam i: a run's starting fragment must
be able to carry enough recent context for continuity to work; how much and
how it's assembled is m/s's business.

Adopted the **minimal-core rule**: MSI-1's mandatory core per seam is the
smallest set of obligations that's actually correct, not the most capable
one; anything additive (real-time streaming for windows, specifically) is a
named, explicitly deferred extension point. Real-time/continuous-stream
windows (voice, phone, eventually video) were on the table as a possible
seam-c extension and got deliberately cut from MSI-1 scope rather than
designed, per this rule.

Two real corrections from Clarke on how piece prose should read, now baked
into msi-steps.md so they don't have to be relearned per piece: never define
a piece by what it isn't or by contrast with a sibling (that content belongs
in the sibling's own prose — the wake-word/Peripheral boundary got cut from
Window's prose for exactly this reason, to be written into Peripheral's
prose instead when its own turn comes); never restate the system-wide
"every piece is a swappable implementation" invariant per piece, it's
already said once at the top of the document.

Edits made: anatomy.md's Window prose rewritten; research-plan.md gained the
minimal-core working rule, seam c's row rewritten with the prior art above
plus the open Command Queue question, seam g's row gained the Letta
sleep-time-compute citation, seam i's row gained the continuity finding;
Window's Pieces-table status flipped to Covered; msi-steps.md's 1.1 checked.

**Process rewrite.** msi-steps.md's phase-1 procedure got reordered to match
what actually worked: check real prior art first, only then write or fix the
piece's prose (as the abstract contract any compliant implementation must
satisfy, never as a description of how one existing system happens to
behave), then confirm seam-sides against that same prior art, then
gap-hunt, then flip status. Also corrected on a first pass at this rewrite:
don't hardcode example systems (Matrix/MIME/IMAP were Window's finds, not a
template for every piece); anatomy.html sync moved from per-piece to once,
at phase 1's close (step 1.13).

**Peripheral, 1.2, opened, not landed.** Current anatomy.md prose flagged
for the same sibling-comparison problem Window's had (the "a phone is
usually both a window and a peripheral host... two different jobs"
sentence); to be cut and replaced once Peripheral gets its own proper pass,
with the wake-word/ambient-sensing example folded in as a native example of
what a peripheral senses through, grounded in Peripheral's own research
rather than carried over from the Window conversation. Seam o remains the
only seam-side. Prior-art research was fired off but interrupted before
synthesis or write-up, so nothing is filed into research-plan.md yet,
deliberately — raw material for the next session, not yet reviewed with
Clarke:

- iOS's permission model: declared via `Info.plist` usage-description
  strings, granted through runtime prompts (Apple's own docs; OWASP MASTG's
  note contrasting this with Android's approach).
- Android's permission model: declared in the manifest; "dangerous"
  permission groups additionally require a runtime prompt, "normal" ones
  don't.
- The Web Permissions API (W3C spec, MDN, Chrome docs): query/request model
  for capabilities like camera and geolocation. Notable real-world dead end
  worth flagging for seam j as much as o: a programmatic `revoke()` was
  proposed and then removed/disabled by default — a real, shipped standard
  that never actually solved revocation.
- Matter/HomeKit: devices declare capabilities as "clusters"/
  "characteristics" — the closest real structured-capability-declaration
  precedent for what "a peripheral declares what it offers" should
  mechanically look like.
- OpenClaw: its own docs literally use the word "peripherals" for
  device-level capabilities, and separately grants full computer-use
  (browser control, desktop control, OS-level access) through what looks
  like its general tool-calling path rather than a separate contract — real
  evidence worth checking properly next session for whether Peripheral
  should stay its own seam or collapse into Tools/MCP, the same kind of
  question s once weighed against f.

**To resume:** msi-steps.md's own session-start instructions already say to
read the last devlog entry then find the first unchecked step, so no special
phrasing is needed next time, "let's continue the anatomy piece pass" (or
similar) is enough. Lands on 1.2, mid-research, not mid-decision: the prior
art above needs synthesizing with Clarke before anatomy.md's Peripheral
prose gets touched.

## 2026-07-11 — "Jarvis" split into persona vs. category term, "digital counterpart" adopted, why-nobody's-building-this landed in vision

Also not a research session. Clarke brought back a conversation with a
separate outside model, this time about why nobody else appears to be
building MSI, plus a naming question the same conversation surfaced: what do
you actually call a Jarvis-class system.

**The naming problem was real, not just style.** "Jarvis" was doing two jobs
at once across anatomy.md, vision.md, and README.md: the generic category
term for what the standard defines ("Jarvis-class system"), and the proper
name of Clarke's own instance (vision.md's "Mine is called Jarvis, and the
name is the spec"). naming-conventions.md already drew this distinction in
principle (persona built on the data vs. the data itself) but never resolved
the category-term overload, and "Jarvis-class" riding as a technical term in
docs meant to be plain-language and stranger-legible was also borrowed
Marvel IP doing spec work it shouldn't have been doing.

**Resolved by splitting, not renaming.** "Digital counterpart" is the
category term now; it wasn't imported from the outside chat so much as
formalized from a word this project's own docs already reached for
organically (vision.md's "a persistent counterpart," "a counterpart that
knows you," devlog's own "persistent-counterpart arc"). "Jarvis" stays
exactly where it was as Clarke's own instance's name, product branding
alongside MojOS and mojo-agent, untouched at vision.md's "Tony Stark's
JARVIS" passage and the "measure is the name" section. A formal,
POSIX-style definition of digital counterpart went into anatomy.md's opening
("the term this document and the MSI are written against, the same way
POSIX had to define 'operating system' before it could define anything
else"), matching identity/memory/permissions vocabulary already established
rather than inventing new terms like "authority relationship." Mechanical
swap followed through anatomy.md, vision.md, and README.md, and mirrored
into the org-level profile README (mojo-labs-circus/.github, separate repo,
condensed register kept as before) in a local clone, not yet pushed, pending
Clarke's go-ahead on a push to a public repo.

**On why nobody else is building this: most of the outside chat's reasons
were kept, one was cut, nothing was pasted in wholesale.** Kept: the
incentive argument (nobody at a platform company gets paid to make their
platform replaceable), the timing argument (standards arrive after the
first product generation, not during it, already vision.md's own
POSIX/TCP-IP point), the delayed-reward funding problem, and the sharpest
one, that most companies still treat the model or the app or the network as
the asset rather than the accumulated identity, which restates this
project's actual thesis as a market observation rather than adding a new
one. Cut: "most builders don't think in systems," judged as self-flattery
wearing an independent-cause costume when it's really the incentive
argument again, and everything in the source chat character-reading Clarke
personally, which isn't market analysis and doesn't belong in a project doc
regardless. Also cut as redundant: the Linux/Kubernetes "people join for a
seam they care about, not for you" point, already made by vision.md's
existing Linux-driver analogy in "Why every builder benefits." New content
landed in vision.md's "Who's building this," a paragraph on why the gap is
still open (incentive plus timing plus the wrong-asset problem) and a second
tying to the existing falsifiable-core test: three honest explanations for
why nobody's built this (missed it, too early, wrong), assuming the first
one is the trap, and the only real test is whether identity actually
survives the swap.

## 2026-07-11 — outside review on vision: adoption argument written, "lose the standard too" sharpened, router flag carried

Not a research session (phase 1 stays untouched). Clarke ran vision.md and
anatomy.md past a separate outside model and brought the conversation back in
to fold in what held up.

**The critique's read on anatomy was mostly confirming**, and its one real
suggestion, decompose the Router because it's accreting too much authority,
turned out to already be answered: anatomy.md's Router prose already puts
authority in the kernel (seam j) and leaves the router only selection (seam
d), with hard limits explicitly "the owner's policy data, which a router
obeys... never properties of the router itself." No anatomy change. The real
risk isn't a monolithic router, it's an opaque one: a router with zero formal
authority can still function as a de facto OS if nobody can see why it picked
what it picked. That's a seam-d contract requirement, not a piece-list
change, and got carried into research-plan.md's row d as a flag: a router's
decision should have to expose its candidates and reasoning, and stay
overridable by policy data.

**Adoption argument written into vision.md**, new section "Why every
builder benefits" after "A Linux for AI." The move: standards win on removed
cost, not elegance, and the cost removed is different per audience, not one
pitch reused five times. Runtime builders stop having to also build
identity/memory/permissions/sync. Memory providers get every runtime for
free off one contract instead of an N×M integration matrix (the MCP
mechanism, one layer down). Model vendors lose the user relationship but
keep getting hired as the best model for the job, an AWS-on-Linux trade, not
an existential one. Enterprises are the real wedge: portable context solves
a switching-cost problem procurement already has a name for, and the
kernel's audit trail answers a compliance question they already have to
answer. Open source gets eleven real seams to build into instead of one
project to fork. Underneath all five: MSI turns a vertical market (whoever
welds the most together first wins the user) into a horizontal one (whoever
builds the best single piece wins that layer), and almost everyone not
currently winning the vertical race benefits from that shift.

**"Losing" pushed one level further than it was written.** The mission
section already said the idea winning matters more than Mojo's
implementation winning; that was still implicitly about the pieces, not the
standard itself. Added to the closing section, "An argument, not just
software": the standard itself is meant to be outcompeted too, by anyone who
gets identity portability and piece decomposition righter than MSI does. Not
resignation, confidence that the shape existing is the actual goal and MSI
is one route to it, not the destination. Landed on: it doesn't matter whose
name is on the standard, just that it exists at all.

**Same edits carried into the org-level profile README** (mojo-labs-circus/
.github, separate repo), condensed for its punchier register rather than
pasted whole. The opening pitch and "What Mojo is" both got the same
one-sentence extension: a better standard replacing MSI counts as winning
too. A new section, "Why this instead of a monolith," gives the condensed
per-audience adoption case (runtime builders, memory providers, model
vendors, enterprises, open source) right where a stranger landing on the org
page would actually ask "why would I use this." Everything else in that
README (the problem, roadmap, who's building this, philosophy summary) was
checked and left alone — still accurate, not touched by this session's
changes.

**Then the org-README's own Philosophy section got a real fix, on Clarke's
call that the original was thin and wrong, not just missing something.**
"Open source by necessity" as a blanket line collides with philosophy.md's
"Allow, don't mandate": the standard itself never requires open source or
local, only that identity data stays in the portable shape. What's actually
open by necessity is Clarke's own implementation, because a sovereignty
claim specifically needs to be provable rather than promised ("A proof, not
a promise"). Rewrote the section to keep those two claims separate instead
of collapsed into one, and added a second paragraph carrying the
wanting-to-lose belief with its actual reasoning (credit vs. existence pull
apart, this project already knows which one it picks), not just a
cross-reference. Two pushes to that repo this session; the first one landed
a version that was still too thin, caught and fixed in the same sitting.

**philosophy.md itself got the matching gap closed.** The belief that Mojo
losing, even the standard itself losing to something better, is success had
only ever been written down as strategy, in vision.md. It's actually
personal, the same non-ownership the agent is designed to practice toward
its user (surface, challenge, never decide) pointed back at the project that
built it, so it earned its own section: "Wanting to be outcompeted," placed
after "Right over easy," before "The honest tension." Also fixed a small
drift while in there: "The honest tension" still said "I'm a first-year
student," left over from before this year finished; matched it to vision.md
and the org-README's "just finished the first year of a CS degree."

## 2026-07-11 — seam s minted, steps tracker created, ready to draft MSI-1

Same sitting as the re-derivation entry below; this session closed the gaps
that entry flagged and set up the drafting workflow.

**Readiness call: yes, MSI-1 drafting starts now.** The anatomy holds to the
best of what's knowable up front, the seams are enumerated, and the prior art
is unusually rich — this is POSIX-side standardization of things that already
exist (MCP, A2A, SKILL.md, OpenAI-compatible serving, capability systems,
CRDTs), not OSI-side invention. One habit locked in against the OSI failure
mode: every seam gets walked against reality (the actual spec documents, real
code, real memory files on disk), never summaries of summaries. After MSI-1:
reference implementations, stitched into the first real Mojo system.

**Seam s minted (was the unlettered memory binding).** Runtime ↔ memory
provider. The router picks providers per run, so a harness must be able to
talk to whichever provider it's handed; without a uniform surface that's a
harness×provider compatibility matrix, the exact thing the standard exists to
prevent — same reasoning that gave models seam e and tools seam f. 19 seams
now, a–s, in anatomy.md, anatomy.html, and the tracker. First candidate to
check when walked: collapse into f, providers as just MCP servers exposing
retrieval tools; if MCP can't carry result provenance (r) and view scoping
(m's travelling permissions), it's an MSI envelope instead. Deliberately not
decided in this session: whether writes pass through a provider at all —
only that wherever they pass, they land in the data's shape (m).

**On the primitive question (do we still define a Docket-like unit, or is
the primitive just files?):** both, at different layers, and that's already
the anatomy's position ("plain files, built on a small set of file-like
primitives the MSI defines"). The bytes are ordinary Unix files, greppable
and portable — that's the runtime contract and it's non-negotiable. But POSIX
files don't carry what a remembered fact needs: identity that survives
rename, history and retraction, provenance, travelling permissions. So the
MSI's unit is a format-plus-conventions on top of files — the way a git
object is "just a file" in .git but the real primitive is the commit, or an
Obsidian note is "just markdown" but the frontmatter is load-bearing. No new
filesystem, no VFS, no kernel object. Its exact shape and its name are step
2.6's job (the m walk); "Docket" itself is dead as a public name per
naming-conventions.md.

**msi-steps.md created** — the procedural tracker, written so a cheaper
model can run sessions cold: find the first unchecked step, do only it,
per-step instructions and non-negotiable guardrails (ask Clarke before
anything architectural, timebox rule, Decided means a stranger could build
from it). research-plan.md stays the why-and-status; msi-steps.md is the
what-next. Session rules and AGENTS.md updated to point at it.

**Next session: step 1.1**, the piece pass, starting with Window — prose
check, seam-side check (a, b, c), gap hunt.

## 2026-07-11 — anatomy critique triaged; research plan re-derived from the anatomy, POSIX-style

Two things in one session: an outside model's critique of anatomy.md got
triaged, and the research plan got rebuilt around the anatomy's seams.

**The critique mostly confirmed the spine** (piece/data split, router vs
runtime, kernel as enforcement, providers derive). Its one challenge, that
"identity" is a weak name for the data box, was half right for the wrong
reason. Renaming the data is off the table: "identity" is the load-bearing
thesis noun across vision.md and philosophy.md, and it's philosophically apt
(what makes X the same X over time is memory continuity, which is exactly the
claim). The real collision was local: anatomy.md also used "identity" for the
owner's key-pair/DID login, a completely different thing three lines away in
the same diagram. Fixed by renaming the owner's side to **credential** in
anatomy.md and anatomy.html. The data keeps its name.

**Method locked: however POSIX actually works is how the MSI gets made.**
Concretely: normative content is interfaces only, so a piece's real
definition is the union of the seam-sides it implements, and anatomy.md's
prose is informative, not normative; conformance is against profiles (bundles
of seam-sides), never the whole document; rationale is a separate
non-normative appendix; and POSIX codified running practice iteratively, so
draft-as-you-go stands and the assembly pass is a coherence read. First
version is **MSI-1**, the way POSIX-1 was, iterated openly from there (the
"Mk1" label in older docs means the same thing).

**research-plan.md rewritten, remapped rather than wiped.** The old file's
structure was dead (nautical vocabulary, the superseded nine-leg walk) but
its prior-art capital was earned, so every old row was mapped onto the
anatomy's lettered seams and carried forward inside the seam rows. The new
plan is three phases: piece pass (prose + seam-side list + gap hunt per
piece), seam walk (study-then-decide per seam, drafting msi.md as each
lands), assembly (§0 profiles from what landed, contradiction check,
coherence read). Walk order re-proposed dependency-first: adopted seams, then
a, m, j, i, d+g, then p/q/r, o, c+b, with k+n last. Session rules
(.claude/rules/msi-research-sessions.md) rewritten to match.

**The remap surfaced two real gaps, which is the method working:**

- **Seam p had no row anywhere in the old plan.** Trust/enforcement covered
  permissions and Connection covered the wire, but "how does a real secret
  get injected into exactly one call without entering model context" was
  never asked. Row created, candidates to gather when walked.
- **The runtime ↔ memory provider binding has no seam letter.** The map's
  per-run bindings read "(e) model · (f) tools · memory · (q) sandbox" and
  memory is the unlettered one; seam m is providers ↔ data, not runtime ↔
  provider. Either provider APIs get a lettered seam or they're deliberately
  unstandardized and only the data shape makes providers swappable. Flagged
  in the new Pieces table, to be resolved during phase 1's memory-provider
  pass.

**Where this leaves things:** currently on phase 1, the piece pass. The
lifecycle pass of 2026-07-10 already did most of the prose; what's left is
the per-piece seam-side check and gap hunt, and the two flags above are its
first two work items.

## 2026-07-10 — looked at the SAIHM protocol, not adopting

Sidebar, not a session. Clarke asked whether the SAIHM protocol could help
the identity work. Checked the actual IETF draft rather than the marketing
site: it's an Independent Submission, single author, six-month expiry, so it
carries no more weight than a blog post, and it's tightly coupled to a
specific blockchain project (COTI chain, a governance token, on-chain audit
anchoring, two of its eight tools are token-vote plumbing). Verdict: not a
standard, one vendor's proposal, same tier as the other memory-schema
candidates already sitting in research-plan.md (Portable Agent Memory,
Engram, W3C AI Agent Memory Interop CG).

Stripped of the chain and token layer, a few mechanisms are real and land on
open seams: identity as a post-quantum keypair derived from a seed that
never leaves the holder's machine (a worked instance of the key-pair/DID
identity anatomy.md already commits to), per-cell encryption keyed off that
identity, cryptographic erasure by destroying the key rather than queuing a
delete, and revocable sharing contracts as one answer to the open "N holders
of one capability" question for Collectives. Not added to the research plan;
Clarke was just intrigued, not proposing adoption. Worth a look if the
memory-schema seam gets walked for real.

## 2026-07-10 — lifecycle pass on the anatomy: runs, services, system control; the router leaves the run; memory providers go plural; fleet manager named

Started as "are we happy with the anatomy or do we keep hunting for more
pieces." The piece list held. What didn't hold was the lifecycle story, and
pulling on one loose thread (seam g: who watches the triggers when nothing is
running?) unravelled the whole "runtime pieces assembled per run" box.

**The router was misfiled.** It sat inside the ephemeral run box, but the
router is what chooses what a run is made of — it can't live inside the thing
it assembles, and something has to be awake watching the clock when no run is
live. The router is persistent now, and trigger-watching is its job. That
closes the seam-g gap without inventing a separate scheduler piece.

**Three lifecycles, not two.** I first framed it as agent-invocation
(ephemeral) vs overarching system (persistent), but the middle didn't fit
either bucket. The split that landed:

- **Runs** — ephemeral. One agent runtime working one task. The harness is
  the only piece born and killed with the run.
- **Services** — plural. Models, tools, memory providers, sandboxes. Several
  of each kind can serve one machine at once; what's per-run is the binding,
  not necessarily the process.
- **System control** — singular. Router, kernel, credential broker,
  provenance, fleet manager. Exactly one live instance of each per machine.

**The sorting rule** fell out of arguing the individual cases: if two live
instances disagreeing about a fact would itself be the security hole (a
permission, a secret's scope, a trust tag, fleet membership), the piece is
singular. If several simultaneous readings of the same data are safe, it's
plural. Provenance was the deciding case: if a run could pick its own trust
tagger, untrusted content gets laundered through the laxest one. Same shape
as two brokers disagreeing about a secret. Both moved to system control.

**Memory providers go plural, and seam m was wrong as written.** The doc
said "concurrent writes reconcile inside one provider." But the whole point
of the identity being a standard substrate is that several memory systems
can serve it at the same time — a vector-heavy one beside a grep-style one,
per-run pick. So there is no "one provider" for writes to reconcile inside.
Corrected rule: providers derive, they never own. Writes land in the data's
standard shape and reconcile at the data; anything a provider builds must be
rebuildable from the data alone. That makes the rebuildability rule
load-bearing instead of just a nice property.

**Fleet manager is a new piece.** Nothing owned seams k and n; the kernel
paragraph gestured at "one shared policy" but named no mechanism for
admission, revocation, sync, or who's-driving handoff. That's a distinct
concern from enforcement, so it's a distinct piece, same logic that split
router from kernel. Tailscale is the likely reference-implementation pick
there (WireGuard-keyed machine identity basically for free), kept out of the
doc the same way vLLM is just an example under Model. Note for later: the
piece's posture is MSI-defined, not Adopt — Tailscale isn't an open protocol
the way MCP is.

Anatomy.md and anatomy.html both rewritten to this structure. Eleven
conformance profiles now (Fleet Manager joins), still 18 seams. Skills
clarified as data the runtime reads, not a running piece.

**Where this leaves the plan:** the piece-by-piece pass is real but shallow
on purpose — right name, one-paragraph scope, lifecycle bucket, nothing
about internals. This session was that pass, mostly. Depth goes into the
seams: a piece's true definition is the union of the seam contracts that
touch it, so the seam-by-seam prior-art walk is where the real work is, and
re-deriving research-plan.md from the anatomy is still the next move.
Reference implementations stay later.

## 2026-07-10 — six Jarvis-class codebases read for real pieces we hadn't named: four new pieces, four new seams, personas go plural

Session started as "research the anatomy of the welded-shut monoliths" and
went through two real corrections before it produced anything trustworthy.

**First correction: the premise was wrong.** Hermes and OpenClaw, the two
named up front, aren't closed monoliths — both are open-source, self-hosted
agents, closer to what Mojo is building than to a walled garden. Swapped in
the actual closed comparators (Alexa+, ChatGPT with memory/connectors, Apple
Intelligence, Meta AI) for that half of the question. Real finding from that
pass: every closed vendor welds windows to one vendor's own machine identity
(nobody lets a third party build a compliant client), and none has anything
resembling a live task-aware router — model choice is either fixed or a
static per-user default everywhere.

**Second correction: "monolith" doesn't mean closed, it means welded.**
Clarke's actual ask was to read the real source of OpenClaw and Hermes and
inventory what pieces they'd built internally, welded together or not — not
compare feature surfaces. First pass at that was still too narrow: checking
source code against a pre-guessed candidate list (five things found from
OpenClaw/Hermes) instead of doing a ground-up inventory. Caught and corrected
mid-flight, including sending correction messages to three already-running
research forks. Ground-up, the method held: read the actual directory trees
and cite file paths, list everything found, map onto the anatomy after, not
before.

**Six systems, read for real, not from docs:** OpenClaw, Hermes Agent, Letta
(MemGPT), Khoj, ElizaOS, GAIA. Same five candidate concerns checked in every
one, but instructed each time to also report anything wholly new. What came
back, confidence-ranked by how many independent codebases built it without
coordinating:

**Four new pieces, each built independently by most or all six systems and
named nowhere in the old anatomy.** A device-capability surface (camera, GPS,
contacts, screen control) that every system splits from its conversational
client — OpenClaw's node-host, Khoj's operator `Environment`, ElizaOS's
`native/plugins/`, three separate teams landing on the same two-jobs-not-one
split. Named **Peripheral** in the doc (not Actuator — camera and GPS are
sensors, not actuators, so that name only covered half the piece; not
Device — collides with "machine"). A **credential broker**, distinct from the
kernel: five of six systems build a dedicated subsystem for secret
storage/scoping/injection, separate code from the permission check itself.
**Sandbox**, distinct from tool-availability policy: where an action executes
(container, host, remote service — Khoj even externalizes it to a network
sandbox) is a different question from which actions are allowed at all, and
every system keeps them as separately-built concerns. **Provenance**: only
strongly confirmed in two of six (OpenClaw's `quarantine-health.ts`,
ElizaOS's whole `evidence/` subsystem) but real and separately engineered
where it exists, and a genuine specialization axis under Clarke's own
decomposition philosophy — more pieces, more competition, when the piece is
real.

**Refinements to existing seams, not new pieces.** Model endpoints are
modality-typed in practice (Khoj: separate swappable configs for chat,
embedding, image, speech) — seam e updated so a compliant Model profile
doesn't assume chat-only. Scheduled runs (seam g) broadens from "clock → a
run" to any trigger: clock, external event (GAIA's dedicated `triggers/`
system, separate from its clock scheduler), or idle time. Run contract (seam
i) gets live interruption/cancellation, missing before (GAIA's
`interruption.py` — the current framing only covered before/after a run, not
control during). Data shape (seam m) gets per-unit permissions (Letta's
`read_only` memory blocks — a global kernel grant isn't the only place
permission can live) and in-provider conflict reconciliation, motivated
twice over: GAIA needed it inside one memory engine even without federation,
and it's the direct consequence of runs now being explicitly concurrent.
Machine admission (seam n) gets hardware attestation as one admissible
mechanism (ElizaOS's TEE + revocation list).

**Design decisions made in conversation, not found in any codebase.** Seam
d's router has zero precedent across all six systems, confirmed six times
over — and the reason isn't that nobody needed it, it's that nobody else has
enough hot-swappable pieces for a router to have a job. Mojo is the first
design in the set that creates the condition seam d exists to serve; kept
exactly as designed, not softened. Pushed further in discussion: the router
doesn't just pick a harness and a model, it assembles the *whole* per-run
piece-set — including which memory provider, which sandbox — and that
assembly is chosen fresh per run, recursively per sub-run, with runs treated
as ephemeral, killable, and concurrent by default. Half of this turned out to
already be implied by "assembled per run" in the original diagram; made
explicit rather than left to inference. Personas go from an open question to
decided: an owner can hold as many as they want, each a voice plus a scoped,
permissioned view over one shared memory — not separate memory stores. This
wasn't invented in the session; naming-conventions.md already drew the
identity-is-data / persona-is-built-on-it line, the research just gave the
mechanism (per-unit permissions) that makes plural personas concrete instead
of aspirational. And "dream mode" — an idle-triggered memory consolidation
pass Clarke remembered wanting before this project's reset — was real:
found in git history (commit `7880305`, the old `jarvis-spec.md`, pre-reset).
Same idea, confirmed, folded into seam g's idle-trigger case in plain
language; the colorful name stays out of anatomy.md itself per the naming
policy, not lost.

**What actually changed on disk:** anatomy.md and anatomy.html both — the
ASCII map and its HTML rendering gain a Peripherals band and a Sandbox /
Credential Broker / Provenance row inside the runtime pieces; four new seam
letters (o, p, q, r) bring the ledger from 14 rows to 18 and conformance
profiles from six pieces to ten; seams d, e, g, i, m, n reworded for
granular/recursive routing, modality-typed models, any-trigger runs, live
interruption, per-unit permissions with in-provider conflict handling, and
attestation-as-admission; the identity section rewritten for plural personas
over one memory. No decisions here are final in the sense research-plan.md
uses the word — this is anatomy work, feeding the plan, not the plan itself.

---

## 2026-07-10 — the anatomy becomes the first-class document: leg 1 walked, the walk itself overturned, anatomy.md lands, and the public docs get rewritten in plain language

Session started as "begin leg 1 (adopted seams)" and ended somewhere much
bigger: the research plan's walk order is superseded, the project has a new
first-class document — [anatomy.md](anatomy.md) — and vision/README/roadmap/
naming-conventions are rewritten against it.

**Leg 1's study happened and is banked, not landed.** Four parallel research
passes on the adopted seams, all against the real current documents, not
training memory: MCP's current spec is `2025-11-25` (JSON-RPC 2.0, stdio +
Streamable HTTP; crucially, the spec itself disclaims enforcement — consent and
safety are explicitly the host's job, which is exactly the hole the kernel
exists to fill). A2A is 1.0.0 under the Linux Foundation (Agent Card discovery,
task lifecycle, three interchangeable transports; auth is transport-level only,
authorization explicitly agent-defined — it's a protocol for talking to
strangers, extends no trust, so it composes with the federation seam rather than
competing with it). SKILL.md has genuinely broad multi-vendor adoption (25+
clients) but no version numbers — an adoption note has to pin a fetch date, not
a version. Model endpoint: after I caught the research drifting toward frontier
vendor APIs (wrong center of gravity — the system runs on open-source serving),
a second pass on the actual OSS stacks: TGI is dead (archived March 2026), the
real landscape is vLLM + SGLang + llama.cpp + Ollama, and all four genuinely
converge on the OpenAI-compatible Chat Completions wire shape as their primary
interop surface. Fragmentation is concrete and lives at exactly two edges:
grammar-constrained/structured output and reasoning-trace exposure. So that
seam's shape is "adopt the wire baseline, define a capability-declared envelope
for the two unstable edges — a router may use a declared capability, must never
assume one." None of this got marked Decided; it's input for when the re-derived
plan reaches those seams.

**The walk got challenged and lost.** First round: I noticed adopted seams
punt to unbuilt legs (skills→memory schema, A2A→federation, model→invocation),
proposed dissolving leg 1 into per-adoption checkpoints before their consuming
design legs — a 12-leg walk. Second round, the real one: the tracker grew
accretively (the row dates show it), and an accreted list can't prove its own
completeness. The fix isn't reordering the list — it's drawing the whole system
first and deriving the list from the drawing, the way POSIX's writers could see
a running Unix before they wrote anything. Drew it as an artifact (three
revisions with feedback): every piece, every seam labeled a–n, treatment
color-coded. The drawing immediately earned its keep by finding four seams
nothing tracked: owner verification (the system's login — Unix has PAM, we had
nothing), machine admission (root-of-trust only covered machine #1), live-task
handoff (data coherence was owned, action coherence wasn't — two machines, one
task, who's driving), and conversation continuity (probably memory-shaped,
never verified).

**Anatomy corrections that reshape rows, from talking it through:** windows
are a swappable piece like everything else (profile: Client), and a window runs
*on* a machine or on a thin device — the machines themselves are not windows.
The memory provider split out of the identity entirely — the Obsidian cut: the
vault is not the app; providers compete on retrieval/indexing and anything
derived must be rebuildable from the data alone, or portability silently dies.
The owner is a human represented by something key-pair/DID-shaped so "the owner
said so" is checkable. Leans recorded, not decided: kernel instance per
machine, logically one per owner's set (enforcement must work offline; policy
must not fork); router proposes, kernel disposes; trigger firing is the
ground's job (the schedule is data, the firing is an act of authority).

**The naming policy landed.** Plain functional names in everything
public-facing — POSIX names nothing thematically; themes live in products, not
standards, and Urbit is the cautionary tale. The nautical register survives as
the analogy we think with, not terminology anyone must learn. One precision
gain from the demotion: the identity is the *data*; the first mate — the
persona, the "Jarvis" — is what's built *on* it. The old three-register
naming-conventions.md is rewritten; the old scheme is in git history.

**What actually changed on disk:** anatomy.md (new, first-class — the concrete
"what Mojo is," seams a–n, the what-survives/what-swaps split) and anatomy.html
(its visual rendering, kept in-repo at Clarke's call); vision.md rewritten
plain (same ideology, anatomy-anchored, audit/accountability given real weight —
"what did it do and who allowed it" as the thing that makes the system
adoptable beyond enthusiasts); README.md rewritten around the anatomy;
roadmap.md's Now is anatomy → derive plan → walk it; naming-conventions.md
rewritten to the new policy; AGENTS.md points at anatomy.md first.

**Second pass, same session, after outside-reader feedback on the new
vision:** Collectives reframed hedge-first (the recursion is a bet the
primitives have to earn, said in the first sentence instead of disclaimed in
the last — and if it needs new machinery, that's the bet failing, out loud);
MojOS reframed personal-first ("personal, not central" — my OS, offered
because the same defaults probably help anyone, with "runs on any Linux
distribution" stated as a design constraint of the standard). Asked whether
Nix's rollback story belongs in the anatomy: no — the anatomy stays
mechanism-free and any-distro — but the property underneath it does: the
identity data is the one irreplaceable thing, so the data itself must keep
history and a bad write must be rollbackable, whatever sits above or below.
Added to the memory-provider piece and seam (m) in both anatomy.md and
anatomy.html. And corrected my own framing in the same pass: Nix is not
MojOS-only — the package manager runs on any distro, and it's the planned
assembly/deployment story for the first system (parts pinned, installs
reproducible, upgrades rollbackable); MojOS is the same idea taken to the
whole machine. The standard itself never requires Nix. vision.md ("Nix, and
MojOS") and roadmap.md's Next say so now. Org-level README rewritten too, via
`gh` (MCP creds were failing): anatomy linked, identity-is-data framing,
plain names, same Collectives/MojOS reframes, and its roadmap now names the
ends-public step — the MSI and first system posted online to make the case
and invite better implementations.

**Third pass — the org README rewritten from scratch, not patched.** The
patched version still read like the old idea: a prophecy up top, then
product-tour sections (Jarvis, Nix-and-MojOS, Collectives) describing things
that aren't what's being made. The org is about one thing now: making
Jarvis-style systems interoperable, Linux-style, so the open-source community
can build them correctly together — no lock-in, local and sovereign if
wanted, scaffolding others implement against. New shape: the lede carries the
whole idea; "The problem" is the weld + sovereignty-is-ownership-plus-
portability + the two unstandardized seams being exactly the moat; "What Mojo
is" folds the Jarvis claim in as the falsifiable core instead of a section;
"How this gets built" owns the honesty that our implementations are first and
crude on purpose, with Nix/MojOS as how *we* build ours (documented, never
required — implementers do it however they want); Collectives is no longer a
section at all, just one hedged line in the roadmap's Then ("we think the
primitives will support it; we won't claim it until we've tested it"). The
home-server future stays in vision.md where it belongs — the org front door
now sells the mission, not the prophecy.

**Fourth pass — vision.md gets the same mission-first rewrite.** Rebuilt from
first principles, same vibe, better laid out. New order: "The mission" opens
(the chatbots → agents → persistent-counterpart arc, the weld, and what
interoperable-Jarvis-systems-grown-like-Linux actually means) → "What's at
stake" (the three gaps, kept — they earned their place) → "What Mojo is"
(interface-history argument, the standard, the two built pieces with the
audit story, the distro proof, identity/character, hired help vs rented
machines) → **"A Linux for AI", new section** carrying today's side-
conversation thinking: Linux won infrastructure not the desktop and that's
the shape of the bet; individuals bootstrap it the way hobbyists
bootstrapped the PC; companies adopt it for the same reason they adopted
Linux (portable data, auditable enforcement, no irreplaceable piece — not
charity); existing beats displacing; and the endgame is neutral stewardship
of the MSI — light the spark, hand over the fire. → "The future, if it
works" (the home-server prophecy, demoted from the opening but kept whole,
with Collectives' hedged paragraph living there) → Nix/MojOS → who's
building → the five tests → the closer, all substantially as they were.
The Nix/MojOS section then got its specifics on request: flakes (an assembly
is a lockfile, reproducible bit for bit), generations (every swap atomic and
one command from undone — a hotswap system needs swaps reversible in
practice, not just permitted), Home Manager (the same model at exactly the
user-level layer Mojo's pieces live in, any distro, no root); MojOS as the
whole-machine version — NixOS's single declarative configuration, boot-
selectable system generations, and impermanence (root wiped every boot, only
declared state survives), which makes "the data is the only thing that
persists" mechanically true rather than conventional.

**Fifth pass: the de-slop edit.** Clarke's words: make the project not look
like a slop factory. Full style pass over every current public doc (anatomy
md+html, vision, README, roadmap, naming-conventions, philosophy, AGENTS,
org README): every em dash removed, sentences shortened, the stacked
rhetorical constructions thinned out. Content unchanged everywhere;
philosophy especially got style-only treatment since its ideas predate today.
AGENTS.md also got its stale nine-leg walk paragraph replaced with the
anatomy-first sequencing, and the no-em-dash rule is now written into its
conventions so future sessions hold the line. Devlog history left alone on
purpose: it's the trail, and editing it would be rewriting history. ideas.md,
interoperability.md, and research-plan.md skipped too, since the latter two
are due for supersession by the plan rewrite anyway.

**Sixth pass: philosophy.md catches up to the newest position.** One real
content gap found when checked against today's reframe: the doc read as if
open source and sovereignty were requirements of the whole project. The
newest position is the Unix/Linux one, now written in as a new section,
"Allow, don't mandate": the interface is the commons and what people build
against it is their business; the MSI requires exactly one thing, that the
identity data stays portable so nobody can be trapped; closed implementations
can compete behind the same seams, they just can't hold anyone hostage.
"A proof, not a promise" got its open-source claim scoped to match: open
source is a logical necessity wherever sovereignty is claimed, not a
condition of compliance. And "Sovereignty is an axiom" gained the standard's
form of the axiom: sovereignty must always be available; walking through the
door stays a choice. Everything else in philosophy.md (people, the dark path,
craft, the sensei, right over easy, the honest tension) holds unchanged.
Those beliefs predate today and survive it.

**Seventh pass, same session, on Clarke's direct question: does the standard
actually stop lock-in on identity data, or just allow sovereignty?** Honest
answer, worked through and now written into "Allow, don't mandate": no, not
by itself. A closed kernel can write data in the standard shape (so it looks
compliant) while quietly telemetering a copy elsewhere, and MSI compliance is
a contract shape, not a runtime proof the shape is honored. The kernel is the
thing meant to catch unauthorised copying; if the kernel itself is the bad
actor there's no kernel of the kernel checking it, the same reflexive-trust
problem every capability system has. What actually holds: compliant and
trustworthy are different claims, and a closed piece can have the first
without the second, permanently, since you can't verify what it's doing.
Grounded against email/IMAP as precedent: portable data and freedom from
lock-in aren't the same thing, Gmail's export doesn't get you its search
quality back. Clarke's call, matching Linux exactly: don't force openness or
local-first, hope enough people understand the trade to choose it, and the
interoperability goal stands either way.

**Deliberately not done this session:** perfecting the anatomy (next session —
it needs one more hard pass before it's trusted as the map); re-deriving
research-plan.md from it (the session after — current tracker content is input,
not casualty); rewriting `.claude/rules/msi-research-sessions.md` (goes with
the plan rewrite).

Session started as "audit research-plan.md's shape" and landed as a full
restructure of it, driven by three corrections that all point the same way.

**The Linus/POSIX lesson, taken seriously.** Asked what Linus actually did:
he didn't write a standard first — his famous Usenet post was him asking where
to *find* the POSIX documents, and POSIX itself codified fifteen years of
already-running Unix. The committee-writes-spec-first pattern is OSI, and
TCP/IP's running code killed it. Doesn't change the lifecycle (requirements →
standard → system → iterate), but it sharpens what Mk1 has to be: a thin,
complete, adoption-heavy first pass written to be built against immediately.
Encoded into the plan as three working rules — a timebox rule (a row that
resists landing after one real session gets a provisional answer plus an open
flag, never an indefinite Deciding), a per-row definition of done (a stranger
could build or adopt a compliant piece without talking to me — the bar the
2026-07-09 session already named), and draft-as-you-go (each part's msi.md
section drafted the same session its rows land, so the tracker being covered
*is* the draft being assembled — no big transcription phase at the end).

**Prior-art posture corrected: adopt-or-adapt everywhere, including the two
built pieces.** I flagged that even the kernel and memory have prior art, and
that's right — the repo itself found it (kernel.chat's `agent-os`, OpenFang's
gates, Capsicum/WASI on the kernel side; Portable Agent Memory, Engram, the
W3C CG on the memory side) but vision.md and interoperability.md still said
"nothing to adopt"/"nothing to consume." Overstated. The defensible claim is:
no accepted *standard* exists at those two seams — prior art exists and gets
checked adapt-before-build; Mojo designs the residue. Both files got one-line
fixes to say exactly that.

**The tracker's shape didn't match the spec it's building — fixed.** The
audit findings, all now in the restructured research-plan.md:

- **The MSI-1 target structure is now stated up front** and the tracker maps
  onto it one-to-one: §0 Conformance (RFC-2119, compliance profiles — one per
  swappable piece: Memory Provider, Kernel, Harness, Client, Router, Model
  endpoint — a piece is compliant against a profile, never against the whole
  document; this is what makes hotswapping testable), §1 Base definitions
  (Part A: primitives), §2 Schemas (Part B: First mate), §3 Contracts (Part
  C), §4 Adopted standards (Part D), plus a non-normative Rationale appendix.
- **Reachability had no rows anywhere** despite vision.md naming it as a seam
  a Jarvis-class system needs and test 1 depending on it. New Part C section,
  two rows: the channel/client contract (what a window into the First mate
  must implement — Matrix/XMPP flagged as the "one identity, many clients"
  precedent, unverified) and proactive delivery (the output half of Time's
  trigger row).
- **The adopted seams had no rows at all** — MCP, SKILL.md, A2A, and the model
  APIs were "settled" in prose but the adoption decisions were never actually
  walked or recorded anywhere. New Part D, four rows; each records what
  adopting actually binds Mojo to. Skill format moved there from the First
  mate section (it's an adoption call, same kind as MCP).
- **Two more First mate rows**: the scoped view / mission fragment (the shape
  of what a mercenary is handed — load-bearing for the whole mercenary model,
  previously tracked nowhere) and a reused-shape catalog (interoperability.md
  claims triggers, rosters, routing policies, task checkpoints, and audit
  records are all "just memory objects" — that gets verified per object now,
  not asserted).
- **An audit-trail row under Trust** — the kernel mediates every consequential
  access and nothing tracked what it records; accountability tracing to a
  human is a vision.md commitment.
- **Treatment column everywhere** (Adopt/Design/Leave open, ? for a lean) so
  the tracker directly answers "what does the first build actually have to
  cover" instead of that living in prose across two files.
- **Dead columns cut**: the four OS columns only survive in Part A where OS
  precedent is the point; Parts B–D carry a Precedent candidates column
  instead — the old format forced "—" noise and hid the real (agent-era)
  precedent. Shell/environment section dissolved into Process/invocation,
  which is its actual spec home. Content-mutation row now explicitly owns
  same-Vessel concurrent writes (two sessions at once), distinct from Fleet
  coherence's cross-Vessel case.
- **New secondary precedents seeded, all flagged unverified**: Landlock,
  UCAN/Biscuit/macaroons (portable attenuable capability tokens — the one
  precedent family for "permissions travelling with the data" no OS provides),
  CRDTs (Automerge/Yjs, the automatic-convergence counterpoint to git's
  explicit-merge answer on Fleet coherence), Matrix/XMPP, C2PA/Sigstore,
  iCalendar RRULE.

**Two decisions made explicitly this session:** draft-as-you-go for the spec
(msi.md created when the first real section lands — no empty skeleton, per the
repo's own convention), and the MVP finish line is a **fully runnable
release** — install docs, pinned parts, the hotswap test reproducible by a
stranger. Forum proof means the published MSI-1 draft plus a system someone
else can stand up, not a demo only I can run. roadmap.md's Next now says so.

Row count went 39 → 47. Nothing moved to Decided that wasn't already — the
restructure is not research, and every new row starts Open.

**Same-session follow-up, pressure-testing the restructure.** Four corrections
out of it:

- **Capsicum can't be the mechanism — it's FreeBSD-only** (the Linux port
  died), which I hadn't registered when it got flagged as "likely the real
  precedent." Kept as precedent for the *shape*, corrected in three places,
  and the design consequence is now written down: portable enforcement has to
  come from what an invocation is handed (WASI-style — what you weren't given
  isn't there), with per-OS sandboxes (Landlock/Capsicum/macOS) as optional
  hardening backends underneath, never the contract itself. WASI is the one
  candidate that runs everywhere Mojo does.
- **New working rule: nothing already thought is decided.** The
  dockets-research files, "direction picked" notes, and Treatment leans are
  inputs to the walk, not outcomes — a row walked honestly can overturn any of
  them, Docket's own shape included. Was implied by the Status discipline;
  now it's stated.
- **Sequencing rewritten as "the walk" — nine study-then-decide legs**, each
  naming the question it answers, what to study (the training), and what
  msi.md section it drafts, so the research is a curriculum that ends in
  decisions, not a checklist. One ordering fix: Captain moved before
  File-side, so Docket ownership fields have a defined owner to point at.
- **Backup/disaster recovery recorded as deliberately deferred** (the
  all-vessels-lost case from book-research.md) — operational, not a seam, but
  now written down instead of silently missing, with a re-entry condition if
  the First mate walk proves an export envelope needs a contract.

**Closed the loop on how the walk actually gets lived with, session to
session.** Real prerequisite check: none for leg 1 (the adoption specs need no
OS background), and OSTEP's persistence/virtualization chapters are the one
worth reading, but alongside legs 3 and 6 rather than front-loaded — the
recurring failure mode for a project shaped like this is reading a whole
textbook before touching the tracker and losing the thread. No separate
sources file: the study material is already named inside each leg's walk
entry, and a second list would just drift out of sync with it. Landed the
session pattern instead — every leg opens with a briefing on its named
precedents before any deciding starts — and a one-line "Currently on: leg N"
marker in research-plan.md so a session after a gap costs a glance, not a
re-read. Both now written into the plan and carried in memory.

**Session-flow discipline, same thread, one leg confined to one session.**
Explicit agreement: work in a session stays on the leg named by the marker —
never start a second leg because there's time left, never reopen a Decided
leg without being asked. A leg spanning several sessions is expected (File-side
especially); the constraint is never more than one leg *per session*, not one
session *per leg* — multiple sessions in a single sitting are fine. Set up the
same way `docs/framework/`'s dead session rules were: a path-scoped rule file
(`.claude/rules/msi-research-sessions.md`, matching `research-plan.md` and
`msi.md`) plus a `SessionStart` hook that greps the marker line and surfaces
it automatically, so the discipline doesn't depend on either of us remembering
to re-read the plan first.

**Session flow made self-enforcing, not just documented.** Two hooks added to
`.claude/settings.json`, both pipe-tested against real repo state before
being written: a `SessionStart` hook that greps research-plan.md's "Currently
on" line and surfaces it as context automatically (no re-read needed after a
gap), and a `Stop` hook that blocks completion if vision.md, philosophy.md,
roadmap.md, research-plan.md, interoperability.md, ideas.md, or msi.md
changed this session but devlog.md didn't — the enforcement side of AGENTS.md's
existing devlog habit, which was previously just a written rule I could forget
to follow. `.claude/rules/msi-research-sessions.md` (the leg-confinement rule
file, mirroring the existing dead `framework-sessions.md` pattern) now points
at both hooks as the automated backstop. Neither hook fires mid-session, so
both need `/hooks` opened once or a restart to pick up — the config-watcher
caveat, not a bug.

Next session picks up leg 1: walk Part D's four adoption rows, cheap and
mostly confirmation, putting the first real section into msi.md.

**Closing thread, not technical — worth keeping anyway.** Talked through the
recurring doubt directly rather than past it: why isn't anyone else doing
this, why should a first-year student trying to do it mean anything. Answer
landed on, not just offered: it isn't literally true nobody else is
working on it (kernel.chat, OpenFang, Portable Agent Memory, Engram, the W3C
CG are all real, already named in interoperability.md's landscape checks) —
the actual gap is nobody's finished stitching the pieces together, which
reframes the whole worry from "am I seeing something nobody else sees" to
"am I willing to do the unglamorous integration work several well-resourced
efforts have stopped short of." The permissive licensing on all three real
candidates (Apache 2.0, MIT) is itself part of the plan, not a side note —
adopt-or-adapt was always meant to include forking the good parts of
competing efforts, not just industry standards like MCP. Landed on the
honest version of the "why me" answer too: the scarce resource this specific
kind of work needs is unstructured time free of monetization pressure, which
a first-year student on summer break has more of than most senior engineers
with a startup runway to justify — not a consolation, an actual structural
fact about who's positioned to do slow foundational work. No file changes
from this thread; keeping the reasoning here so it doesn't only live in a
chat log, per the repo's own habit rule.

---

## 2026-07-09 (later) — MVP scope confirmed small by design; real competitive check finds close-but-incomplete precedent on both halves, sovereignty argument confirmed genuinely rare paired with a system

Second session same day, following straight on from the identity-landing
session below. Two threads: sizing the actual first build, then testing that
whole session's core claim ("nobody's building this") against real search
instead of assumption.

**MVP is small on purpose, and that's a feature of the design, not a
coincidence.** Talked through why "this is a big idea" and "the first working
version is small" are both true at once: the standard is what's big, and it's
written down, not built. The actual first system only needs two genuinely new
pieces of software — a thin capability kernel and a memory shape with
permissions attached — sitting in front of adopted everything-else (model API,
MCP, SKILL.md, an existing harness). Landed the honest MVP bar: not "does it
feel like a product," but "does it pass the four vision.md tests using parts I
didn't build." Also named the standing payoff explicitly: every time a better
third-party kernel or memory implementation gets swapped in later, that's
vision.md's test 3 re-firing on Mojo's own architecture, in production,
indefinitely — a dependency swap as a feature to root for rather than a risk
to dread. And the real gate on "other people start building better pieces"
isn't enthusiasm, it's whether research-plan.md's tracker rows are written
well enough that a stranger could build a compliant piece without ever talking
to Clarke — the actual bar for calling a tracker row done.

**Real competitive check, two forked searches, not a recall of the 2026-07-09
(earlier) landscape check.** First fork: is anyone building the *integrated*
thing — capability kernel + portable identity shape + everything else
swappable, aimed at a Jarvis-class persistent identity? Verdict: every
individual piece now has real prior art, the integration still doesn't.
Closest kernel-side match found: `kernel.chat`'s `agent-os` package
(`isaacsight/kernel`, GitHub, Apache 2.0, real object-capability tokens, not
RBAC-wearing-capability-language the way OpenFang's docs read) — literally
markets itself "POSIX for AI agents," architecturally sits right where Mojo's
kernel does, but it's enterprise orchestration/audit-trail scoped, with no
portable personal-identity concept attached at all. Closest memory-side
match: "Portable Agent Memory" (arXiv 2605.11032, May 2026, single author,
real SDK, cross-model demo) — stronger and more concrete sibling to Engram,
same single-author-preprint stage, no kernel attached. Also surfaced: Linux
Foundation's Agent Name Service (identity as DNS-style discovery, not
memory) and an OpenAI forum thread literally titled "Portable Personal AI
Identity" with zero implementation — noted as a signal the idea is now
surfacing at casual-forum-post level, independent of this project.

Second fork, run separately on purpose because it's a different question:
is anyone making the *sovereignty* argument itself, not just the technical
portability one? More populated than expected, still never paired with a
system. Real finds: iury souza's essay "Memory Portability: Owning what
matters" (Dec 2025, argues AI memory lock-in as a liability, unbuilt); Chris
Riley's Tech Policy Press piece (Aug 2025, portability-as-a-right at the
policy level, stays regulatory-abstract); VERTU's "Personal AI Sovereignty
Manifesto" (genuinely individual-scoped ideological framing, but a commercial
product pitch); GitHub's `sovereign-ai` topic (464 repos, confirmed the
already-known naming collision — "sovereign" there means "runs on my
hardware," not "survives switching products"); Kinic (pitches the exact pain,
"your AI has amnesia," as a blockchain-bridge feature, no rights argument
attached). DMA Article 6(7) is now being applied to AI assistants (Google
proceedings, Jan 2026) but as third-party access (Carterfone-shaped), not
memory portability — no WhatsApp-DMA equivalent exists yet for chat memory.

**Net position, both forks combined.** Every piece Mojo would adopt or build
now has a real, live counterpart somewhere, closer than the earlier
2026-07-09 check found — `kernel.chat` and Portable Agent Memory are both
meaningfully closer precedent than OpenFang/Engram were. The sovereignty
argument is being made piecemeal by several independent, unconnected voices.
What still hasn't happened, anywhere found: pairing the argument with the
system — a capability kernel and a portable identity shape under one
sovereignty-first, Jarvis-scoped standard. The gap is real and still open,
and also visibly narrowing faster than a week's difference would suggest —
named as a live fact, not a reason to change pace. Both new findings folded
into interoperability.md's landscape section alongside OpenFang and Engram,
same discipline (real research, explicit unverified flags, no invented
detail).

**Docs touched this session:** interoperability.md (two new landscape
paragraphs — kernel.chat/Portable Agent Memory, and the sovereignty-argument
survey); README.md (full rewrite, mirroring vision.md's MSI-first framing,
added the missing interoperability.md row to the file table); four project
memories rewritten to match the landed identity
(`project-build-first-reset`, `project-arch-model`, `project-phase-
priorities`, light touch on `project-docs-convention-scope`) plus the memory
index.

---

## 2026-07-09 — the project's identity lands: the MSI is the mission, the system is the proof; vision.md rewritten, tracker restructured, org README rewritten

Started as a handoff to fold the previous session's interoperability findings
into three files, and turned into the bigger thing that handoff was circling:
this is the first time enough is down to say what the project actually *is*,
so the files got rewritten around it rather than patched. Talked it through
before writing anything; the landing, in one breath: **Mojo is a standard —
the Mojo System Interface — for a complete Jarvis-class system, not just an
agent's seams; a POSIX-shaped standard (primitives → schemas → contracts);
Mojo's own system is a distro of it, stitched from existing compliant parts,
existing to prove the standard and to be lived in; every piece Mojo builds is
designed to be outcompeted through the contracts it publishes; built in the
open by one student, honestly, because nobody with resources has the
incentive.**

Decisions made in the talking-through, each one explicit:

- **The standard is the front door, not the system.** Success is the idea
  winning, not the implementation winning — but Mojo still builds its own
  full system (like a distro) because we need to use it ourselves.
- **"Singular kernel" disambiguated: singular per Fleet, as an instance.**
  One gate holds authority inside a Fleet, but the enforcement contract goes
  in the standard and better kernels replacing Mojo's reference is the
  standard working. Anything else would be "everything is swappable except
  the piece we control" — the exact shape of claim the project attacks.
- **Mojo's own implementations are owed no loyalty either.** The two-way
  equivalence split (identity owed literal data equivalence; mind/loop/
  faculties ephemeral, no equivalence, feature not gap) applies to Mojo's own
  kernel and memory too. Committed to in vision.md as the fifth "when it's
  right" test: it survives Mojo, and that counts as winning. No moat, ever,
  including a benevolent one.
- **The memory gap is smaller than it sounded, and that's good.** The
  substrate has prior art everywhere — agents already keep memory in plain
  files, which is why a stitched first system is possible at all. What has no
  prior art is thin: permissions travelling with the data, provenance,
  write-back. Standardizing what the field already does, not asking anyone to
  migrate.
- **Jarvis-class scope, not agent scope.** The field is visibly moving to
  persistent systems (every product surveyed pitches persistence as the
  product now), and a whole-system standard is what justifies Hail, time,
  fleet coherence, and routing being in the MSI at all. Collectives stays
  aimed-second: an extension of the primitives if they're right.
- **The builder honesty is load-bearing, and it attaches to the right claim:**
  can't deliver the *ecosystem* alone (true by definition), can deliver the
  *seed* alone (Mk1 spec, first kernel, first memory, first stitched system —
  the adopt-everything posture keeps it solo-sized). Linus register:
  discipline, not destiny.

What actually changed, all left uncommitted for Clarke's fix-up pass (except
the org README, which had to be a commit —
`mojo-labs-circus/.github@19b0651`; checkpoint of everything prior pushed
first as `8e3314f`):

- **vision.md** — full rewrite to the approved spine. Human opening kept;
  sovereignty gap now carries the portability half ("ownership plus
  portability; either alone is branding"); "What Mojo is" rebuilt around the
  corrected lineage (Unix → PC clones → Carterfone → the web → harness/model
  decoupling → Mojo decoupling identity), the MSI defined with the three-layer
  structure and adopt/design/leave-open, the two built pieces, the
  distro-as-test-suite; "Who's building this, and who it's for" merged
  user-and-builder honesty into one section; fifth test added.
- **interoperability.md** — kernel paragraph made precise (per-Fleet instance,
  contract in the standard, kernel guards the identity the way a kernel
  guards a filesystem); new "anatomy of a system" section (five-part cut
  reconciled into Mojo nouns — the session's Mind/Loop/Faculties/Identity/
  Ground scaffolding didn't survive as a second vocabulary, deliberately);
  the three-way seam classification; the two-way equivalence property;
  OpenFang verified and written up honestly (real, ~18k stars, converging on
  the same adopt-list, memory proprietary + one-way migrate importer = live
  shape-without-seam failure; RBAC-vs-ocap and memory-coupling flags kept
  explicitly unverified; leading fork candidate, not a decision, gated behind
  Mk1); the "smaller than it sounds" memory-gap framing; closing section
  updated now the vision rewrite exists.
- **research-plan.md** — the three-layer spec structure written into The
  plan; every section tagged with its layer; new First mate (schema) section
  (memory schema, retrieval, persona, policy language, budgets, skill format,
  signing/provenance, write-back) — File-side is the primitive, First mate is
  the filesystem-standard built on it; four new Process/invocation rows
  (planning, context management, error/retry, harness/shell boundary); Hail
  promoted from one buried File-side row to its own Connection section
  (addressing, transport, capability handoff over the wire, fleet coherence);
  new Routing/dispatch section with the explicit contract (task + roster +
  constraints in, selection out), lifted out of roadmap.md's Horizon aside;
  sequencing extended to seven steps. Nothing new marked Decided; skill
  format is the only Deciding (candidate: adopt SKILL.md, walk it once
  first).
- **roadmap.md** — Now/Next updated to the stitch-together framing and the
  standard-is-the-project line; Horizon's escalation aside now points at the
  routing tracker row.
- **org README** (`mojo-labs-circus/.github`) — rewritten: distilled mission
  statement up top as the screenshot-able block (the full statement got woven
  through vision.md instead of pasted whole), new "What Mojo actually is"
  section, sovereignty problem carries the portability half, honest "Who's
  building this," mojo-agent's row reworded to match the adopt-posture, and
  the closer: *the world can get its mojo back.*

Open threads left deliberately: whether the mission statement's wording is
final (Clarke flagged it's not necessarily the final draft — it's now prose
in two places, easy to revise); the build repos (mojo-agent, mojos) may not
survive in their current form once implementation shape is real — nothing
renamed or archived yet; OpenFang's two unverified flags need a source-level
check before any fork decision.

---

## 2026-07-09 — interoperability confirmed as the actual mission; full agent anatomy worked out; kernel/userspace split named; OpenFang and Engram checked as real precedent

Long conversational session, starting from Clarke's own framing: Mojo as "Unix for
AI" — breaking the monolith the way Unix broke proprietary OS monoliths, so
people can own a portable AI identity instead of renting a seat in someone
else's product. Corrected a couple of real errors early rather than building on
them: model hot-swapping already exists via APIs, so it isn't the actual lock-in
— the real lock-in is memory/identity welded into a specific product (Pi vs.
Hermes was the concrete example), which is what `interoperability.md` (found
mid-session, untracked, already drafted independently — see below) already
argues. Also floated and retracted, correctly caught by Clarke, a proposal that
Mojo could let switching cost invert in its own favor over time as a retention
mechanism — that's the same shape of lock-in the whole project exists to kill,
just pointed the other direction. Scratched, not carried forward.

**Adoption mechanics.** Walked historical precedent for monopolies actually
breaking open under interoperability pressure: Linux/Unix, PC clones breaking
IBM's own attempted hardware monopoly, Carterfone/the 1984 Bell breakup, AOL's
walled gardens losing to the open web, Docker/OCI unseating VMware's proprietary
format, OBD-II and the Right-to-Repair movement (a close cousin of the
sovereignty argument, already real law in places). Also the honest counter-case:
messaging/XMPP tried this and lost, because none of the three real forces that
make it happen were present — cheaper, a self-interested giant adopting it for
unrelated reasons, or regulation. Notable correction mid-thread: the EU's DMA is
now forcing interoperability onto WhatsApp specifically, years after XMPP lost
on the merits — a live example of force three finishing what the market
couldn't, not just a historical aside.

**Mission.** Landed a draft mission statement tying interoperability directly to
Jarvis rather than treating them as two separate goals — not yet in vision.md,
sitting in this session's chat log until a rewrite pass happens.

**Landscape check, real research not assumption.** Found Engram ("OAuth for AI
memory," SSRN, June 2026, one independent author, reference implementations for
OpenClaw and Nous Research's Hermes already interoperating without shared code)
and reconfirmed Google's A2A (real, 150+ orgs, adopted) alongside already-known
MCP/SKILL.md adoption. Both were already found and logged in
`interoperability.md`'s "why no one's built this yet" section before today —
today's search reconfirmed them independently rather than discovering them
fresh. Flagged a naming collision worth avoiding in public writing: "sovereign
AI" is currently enterprise/national compliance language (Cloudera, Red Hat,
Mirantis), a different claim than personal ownership.

**Full agent anatomy, worked out then reconciled against what already existed.**
Built a five-part decomposition of "what is an AI agent" — Mind, Loop,
Faculties, Identity, Ground — organized by lifetime and ownership rather than
function, initially without checking it against `interoperability.md`'s own
already-drafted Model/Harness/Tools+Skills/Memory-and-identity/Enforcement-
boundary framework. Caught and reconciled once `interoperability.md` was found:
Identity = First mate (which has never gotten its own section of rows in
research-plan.md's tracker the way Docket got File-side — real, concrete gap);
Ground = Vessel plus Trust/enforcement's and Process/invocation's already-
scattered rows, with Hail (currently one buried row under File-side) clearly
carrying more weight than its position reflects now that Fleet coherence
depends on it; Loop = the existing Process/invocation section, needs new rows
(planning strategy, context-management strategy, error/retry policy, the
harness/shell boundary itself) it never had; Faculties = mostly already adopted
(MCP, SKILL.md, A2A), thin treatment intentional; Mind = no existing Mojo noun,
left deliberately unnamed since Mojo has no design stake in it.

Landed a three-way classification for every seam, meant to resolve the
black-box-vs-invent-everything tension directly: **adopt** where real precedent
already exists (models, MCP, SKILL.md, A2A); **design** where nothing exists and
it's load-bearing (Identity's own internals — schema, retrieval engine, persona
format, policy language, budget representation, skill format, signing/
provenance — plus the Identity↔Loop write-back path and Hail's addressing/
consistency); **deliberately left open** where Mojo has no business
standardizing (Loop's internals — planning strategy, critic architecture — the
market's problem on purpose, the same way POSIX doesn't standardize scheduler
internals, only what a process must expose).

**Kernel/userspace, named explicitly.** The enforcement boundary is the kernel
— singular, hardened, capability-based (Capsicum-flavored is the leading
candidate per research-plan.md, not yet decided) — mediating every access,
issuing/deriving/revoking capabilities, and enforcing that anything written
back actually conforms to Identity's agreed shape. Identity itself is the
protected resource, not the kernel — same relationship as a real kernel to the
filesystem it manages access to. Everything else (Mind, Loop, Faculties,
routing/dispatch) is userspace: plural, competing, swappable.

**Checked whether anyone's actually building this — real research, not
assumption.** OpenFang (RightNow-AI, MIT-licensed, ~18k GitHub stars, Rust, 14
crates) is real, serious, and already flagged as an unverified lead in
`interoperability.md` before today. Verified: "Capability Gates" enforcing tool
access, WASM sandboxing, Ed25519-signed agent manifests, MCP/A2A/SKILL.md
support already built in. But its own docs describe the capability gates as
"role based access control" — RBAC and true object-capability security aren't
the same thing, and this needs a real source-level check before trusting it as
a kernel base, not just README language. More importantly: its memory is
proprietary (SQLite plus vector embeddings), and it ships a bespoke,
one-directional `openfang migrate --from openclaw` importer — a live, concrete
instance of the exact shape-without-seam failure `interoperability.md` already
diagnosed, not a hypothetical anymore. A genuinely strong fork candidate for
whenever implementation starts, not a decision made today — implementation
stays behind the gate already agreed: Mk1's coverage has to be real first.

**Final correction, Clarke's, simplifying the whole picture.** "Not owed
identical output" isn't just Mind's property — Loop has it too, and both are
ephemeral. The real split is two-way, not three: Identity is the one place
literal data equivalence is owed across a swap; Mind, Loop, and Faculties are
all ephemeral and swappable with no equivalence guarantee at all, which is the
point of choosing between genuinely different tools, not a gap in the design.

**Left undone, deliberately, for a fresh session:** none of this is written into
`interoperability.md`, `research-plan.md`, or `vision.md` yet. All still sitting
in this conversation.

---

## 2026-07-09 — content-mutation-over-time named as an open tracker row, sourced from a session talked through outside this repo

Pasted in a conversation had with another agent (not Claude Code) working
through the same interoperability thesis from scratch — Linux-for-agents,
shape-vs-seam, git-backed memory. Most of it was ground `interoperability.md`
already covers and exceeds (that draft has real precedent — Capsicum, WASI,
AIOS — the other conversation didn't). One thread wasn't captured anywhere
yet though: that pasted session pushed past plain append-only into git's
actual model — content-addressed immutable objects plus mutable refs — and
caught something real: **retraction and purge have to stay two different
operations.** Superseding a stale claim should be cheap and common and keep
the "the agent used to believe X" trail; actually erasing something (a leaked
secret, a compliance request) is rare, deliberate, out-of-band GC. Conflating
them either bloats the log forever or leaves an audit trail with holes in it.

Checked `research-plan.md`'s tracker before adding anything — there was no
row for how a Docket's content changes over time at all, just the core
primitive/regular-file/directory rows, so this wasn't redundant. Added a new
File-side row and a new Secondary-systems entry for git's object model
(alongside Capsicum/WASI, dated today). Checked the four primaries honestly
rather than assuming git was the only precedent: Plan 9 actually has real
shipped prior art here — Venti/Fossa, content-addressed immutable blocks
archived into nightly whole-filesystem snapshots — closer to hand than
expected, and worth taking seriously precisely because it's a primary
already, not an import. It's a single-timeline archive though, not a
branch-and-merge model, which is the piece git actually adds. Flagged
honestly as unsolved, not glossed over: git's merge works because text diffs
are unambiguous, and reconciling two branches of belief state that both
claim to be true and contradict is a semantic merge problem, not a textual
one — nothing checked so far solves that.

---

## 2026-07-09 — AAIF/Goose checked as a fourth harness candidate; refines interoperability.md's ownership split for context-window compression

**Started from a factual question, not a design one.** Clarke asked what
AAIF's Goose project is. Turned out relevant fast: Goose (built at Block, now
under the Agentic AI Foundation at the Linux Foundation) is a foundation-
governed, general-purpose, any-LLM, MCP-native agent — a much bigger,
better-known data point for the same claim interoperability.md already makes
about Pi. Checked whether it ships its own memory: it doesn't. A feature
request for persistent project memory (issue #4033) got redirected by a
maintainer to third-party discussion instead of built into core, and the
requester's own framing volunteered it "can be built as extensions rather
than core rewrites." Added to interoperability.md's Harness bullet as a
fourth candidate, with a deliberate distinction from Pi: Goose is neutral by
*default* (nobody's shipped the memory standard yet), not neutral by
*design* the way Pi's explicit hand-back is — if a memory MCP extension wins
real adoption first, the same lock-in risk reopens one layer down, at the
extension instead of the harness.

**That surfaced a real, unresolved ownership question: who decides what
survives context-window compression, harness or memory.** First pass assumed
memory should own salience judgment outright, to keep it portable across
harness swaps. Clarke pushed back correctly — interoperability is supposed to
let harnesses specialize, a coding harness and a research harness caring
about different facts from the same history is a feature, not lock-in. Landed
on a split instead of a single owner: the *algorithm* computing salience can
legitimately live in the harness and legitimately differ between them; only
the *output* has to be centralized, written back into memory's own shape
rather than staying trapped in a harness's private context buffer. Portability
test becomes "can a new harness read what an old one wrote," not "would they
have summarized it the same way." Left genuinely open, not resolved: whether
that write-back happens incrementally as compression occurs (matches the
existing in-progress-task-state checkpoint pattern, but means memory's shape
has to tolerate partial/superseded summaries) or only at session end (simpler
shape, real data loss on crash risk). Both additions folded into
interoperability.md rather than left only in chat.

---

## 2026-07-09 — competitor research on Letta/Khoj/OpenClaw/Hermes/Pi turns into the actual interoperability thesis: harnesses can be mercenaries too, shape-plus-seam is the real portability bar, MCP and SKILL.md already solve two of five pieces, and interoperability.md gets written to carry it into the eventual MSI spec and a vision.md rewrite

**Started as a straight competitor look, not a design session.** Clarke asked to
look at Letta and Khoj on GitHub, thinking they looked similar to Mojo. Scraped
both READMEs. Letta (formerly MemGPT) turned out to be the closer technical
precedent and the more instructive architectural contrast: its memory-block
design is genuinely the best prior art for what a First mate's memory needs to
do, but it's welded into Letta's own harness (Letta Code) rather than separable
from it — model-agnostic, harness-locked. Khoj is closer in self-hosted,
multi-surface spirit but is an application you talk to, not a substrate other
agents plug into. Neither separates "agent" from "identity" as two things,
which is the actual thing vision.md bets they should be.

**Extended to OpenClaw, Hermes, and Pi, and found a real three-way split, not
three flavors of the same thing.** Pi turned out to be a genuine harness in
Mojo's sense — an explicit "Agent Harness," no baked memory, no baked
permission system by design, told its own users to sandbox it themselves.
Hermes turned out to have the identical problem as Letta, just louder about
it — "Persistent Memory" is the whole pitch, stored at `~/.hermes/`, welded to
its own loop the same way. OpenClaw turned out to be neither a harness nor a
memory-owning agent — a multi-channel gateway (Discord/Telegram/WhatsApp/
iMessage) with a swappable bundled agent behind it, closer to Vessel/reach
infrastructure than to anything Mojo would run identity through directly.

**Corrected my own use of "mercenary" mid-conversation after real pushback.**
First extended the mercenary category (vision.md's term for closed frontier
models, hired for capability, never given Fleet trust) to Hermes and Letta,
then Clarke asked what there was to gain by hiring them that way — correctly,
there's none. The mercenary trade only makes sense when there's a capability
premium justifying reduced trust; Hermes and Letta run on the same tier of
models anyone can already point Pi at, so there's no premium, just excluded.

**The real finding of the session: harnesses can be mercenaries too, and two
of the most popular ones already are, independent of which model they run.**
Checked Claude Code and Codex's actual telemetry behavior rather than assuming
either "locked in" or "fine." Both are more model-agnostic than expected —
Codex officially supports any Chat-Completions-or-Responses-compatible
provider, Claude Code is reachable through well-established proxy routing.
But Claude Code was found (via reporting on The Register's analysis) to
transmit per-session telemetry regardless of model backend — user ID, session
ID, account/org UUID, email, and reportedly the files read during the
session. Codex's own claim is narrower ("anonymous technical metrics... no
code or sensitive data collected") but a recent release added "deeper
telemetry" and there's a still-open GitHub issue asking OpenAI to formally
commit to a real privacy policy. Conclusion: model-agnostic doesn't mean
sovereign — the harness itself is a second phone-home surface, independent of
whichever model it's calling, and both should get mercenary treatment
(minimum fragment, no Fleet trust) regardless of what they're pointed at.
This sharpens vision.md's existing mercenary definition, which currently ties
the category to closed weights rather than to who the *process* reports to —
worth folding into a future pass.

**Answered "why not just use Hermes" directly, and it's the sharpest question
asked all session.** Hermes exists today, works today, has more features than
Mojo does (zero lines of code so far). The honest answer isn't architectural
purism — it's that open source changes who can audit the code, not who
decides the shape of the data. A year of memory inside Hermes is a year of
real switching cost regardless of license, recreating the exact sovereignty
gap vision.md exists to close, just with a friendlier landlord holding it.

**Clarke landed the actual posture without prompting: the idea has to win,
even if his own build isn't the one people end up using.** Checked this
against philosophy.md rather than treating it as new — it isn't. "Linus built
Linux because he needed it; the ecosystem that grew around it was a
consequence of building something that genuinely worked... build what needs
to exist" is already there, just not yet pointed specifically at the seam
between AI's pieces rather than at the system on top of them.

**Generalized from "swap the harness" to "decompose the whole stack," and
found two of the five pieces already have real, adopted, external standards
worth consuming instead of reinventing.** Tools already have MCP. Skills
already have SKILL.md / agentskills.io (Hermes itself targets it). This
narrows what Mojo actually has to invent from "everything" down to memory/
identity and the enforcement boundary specifically — the one piece nobody
else has built a portable version of, which is also the one piece Mojo has no
choice but to ship a real reference implementation of rather than just a
seam, to prove the interface is satisfiable at all.

**Landed the kernel/userspace framing, then caught it before it overclaimed.**
The capability-enforcement boundary should be singular and hardened (real
precedent: seL4, Capsicum, WASI, all already checked and sitting in
research-plan.md's Trust/enforcement section); everything else — harness,
memory backend, routing policy, delivery channel — is legitimately swappable,
diversity-tolerant userspace. Caught "kernel for AI" before it read as a
literal new-OS claim: research-plan.md already commits to running as ordinary
POSIX userspace software, no custom kernel required — the kernel language is
a legibility frame, not an architecture change.

**Worked out that a shared seam isn't sufficient for "swap and keep your
data," using a real example instead of a hypothetical one.** Hermes ships a
bespoke `hermes claw migrate` importer for OpenClaw specifically — hand-written,
one-directional, breaks if the source format changes. That's what the world
looks like without a shared *shape*, not just a shared API: N systems need
N² bespoke adapters. Get the shape agreed (what a unit of memory and its
permissions actually *are*, independent of implementation) and N systems
interoperate for free, the way ext4 and btrfs are both "a filesystem" despite
being internally unrecognizable from each other, or the way switching email
providers keeps your messages because the message format was never
proprietary to begin with.

**Read philosophy.md, naming-conventions.md, roadmap.md, and ideas.md before
writing anything new, on top of vision.md and research-plan.md already read
earlier in the session.** Same discipline flagged as load-bearing in the
entry below this one — caught nothing this time that contradicted prior
decisions, but confirmed the "idea wins" posture was already in philosophy.md
rather than inventing it fresh, and confirmed the plain-functional naming
register (naming-conventions.md's register 2) before naming the new file.

**Wrote [interoperability.md](interoperability.md), a new root file.**
Explicitly not the Mojo System Interface itself — that name and its rigor
stay reserved for what research-plan.md's tracker builds toward, row by row,
against real precedent. This is the rough draft of the argument for why that
spec needs to exist, a first-pass sketch of its shape (model/harness/tools/
skills external and mostly adopted from existing standards; memory/identity
and the enforcement boundary the one piece actually built), and the honest
case for why nobody else has built it yet. Meant to drive two things not done
tonight: the eventual formal MSI spec, and a future vision.md rewrite that
folds the interoperability argument in alongside the personal-sovereignty one
already there.

**Direct continuation, not a new session — kept going past the first "status
at close" above.** Clarke pushed on the shape/seam distinction with a
concrete case instead of taking it as settled: how does a harness's
in-progress DAG of subtasks survive a crash or a mid-task swap? Good
question to have been asked, because it broke the clean "memory vs.
disposable scratch" binary given a few exchanges earlier — that binary
quietly assumed anything worth keeping was already being written to memory,
without ever saying how. Real answer: it isn't, automatically. A harness has
to actively checkpoint its progress into memory as it goes, or in-progress
task state just dies with the process. Traced this to a row already sitting
in research-plan.md — Process/invocation's "Supervision / fault recovery,"
marked Open, with Agent libOS's checkpoint/fork/restore/commit already
flagged there as the candidate mechanism. Not a new gap, just one this
session's own earlier answer had glossed over.

**That gap turned out to prove something rather than just being a loose
end.** Clarke floated "the harness is in a way the centre piece" — mechanically
true (it's the thing making the calls during a job) but architecturally
backwards if left there, since the whole point of Mojo is that the harness
is the most disposable, least persistent piece in the system. The DAG
checkpoint case is the actual demonstration of that: if a harness can't even
trust its own live work to survive without writing it out to memory, it
holds nothing of its own at all. Memory is the real center; the harness is a
temporary visitor plugged into it for one job's length.

**Walked the rest of research-plan.md's tracker for pieces this session
hadn't named yet, rather than treating model/harness/memory/tools/skills as
the whole system.** Found four more real ones already sitting there: uniform
I/O (Shell/environment section), scheduled/proactive triggers (Time
section), the Vessel-to-Vessel connection layer (Hail/mesh, distinct from
both MCP and OpenClaw-style human-facing reach), and root of trust
(Commission, the one-time bootstrap problem). Then ran the shape-vs-seam
question against each rather than assuming they'd all need the same
treatment — landed on a genuinely satisfying simplification: almost none of
them need a new shape. A schedule, a routing policy, a roster of who's
reachable, a task checkpoint — every one resolves to being just another
memory object, not a bespoke format. What's left needing real design is one
shape (Docket/Book/Cargo, reused everywhere) plus a handful of distinct
seams for the pieces that are pure live behavior and never persist at all —
i/O, connection, fleet-routing queries, scheduling triggers, capability
checks. Much smaller actual surface than "every new piece is a new problem."

**Folded tonight's continuation into [interoperability.md](interoperability.md)
directly** rather than letting it live only in this devlog — the one-shape
insight added to the Shape and seam section, the harness-periphery/memory-
center correction added to the Harness and Memory bullets in The rough
shape, and the four additional pieces (with their shape/seam verdicts) added
as their own paragraph there.

**Clarke's own read on the direction, unprompted, closing the session:**
this is following the same pattern Unix did — design good primitives and the
system follows from them, not the other way round. Matches what actually
happened across tonight's two passes: neither the harness-periphery insight
nor the one-shape simplification were designed in from the start, both fell
out once the actual primitive (memory, shaped correctly) was taken
seriously and pushed on with real cases instead of hypotheticals.

**Second continuation, same session, same night.** Went looking for whether
anyone else is actually building this, rather than assuming the field was
empty. It isn't, and the honest picture is more specific than either "nobody's
doing this" or "someone already beat us to it." A W3C Community Group
("AI Agent Memory Interoperability," proposed 2026-05-18, no chair yet as of
this check) is working the memory-shape problem specifically, with a
blockchain/compliance-first character — post-quantum signatures, public-chain
audit receipts, GDPR/EU-AI-Act crosswalks — a genuinely different set of
instincts than this project's systems/kernel-precedent approach, aimed at the
same target. A paper called Engram ("OAuth for AI memory," already with at
least one commercial implementation) is pitching the same narrow slice
independently. Google's A2A covers agent messaging. Every one of these,
including the W3C group by its own published scope, explicitly excludes
runtime/enforcement/integration as "covered by other forums" — solving one
piece each, assuming someone else does the table. Nobody found is attempting
the POSIX-shaped integration research-plan.md is actually doing. One real
unverified exception surfaced and not yet checked properly: OpenFang, an
open-source "Agent Operating System," 137k lines of Rust, security-layer
focused — unclear whether it's a product, a published spec, or both, fetch
was too large to read tonight. Folded the whole finding into
interoperability.md's "Why no one's built this yet" section rather than
leaving it only here.

**Real, unplanned conversation about the actual size of what this is up
against, worth recording because it's not just morale, it changed how the
project's own stated posture gets tested.** Clarke named a real pattern
across four months: independently arriving at Nix, at interoperability, at
something A2A-shaped, at Collectives, each time before knowing the existing
version already existed — and then hit real doubt on finding OpenFang, a
team with far more resources and experience, working adjacent ground.
Talked through the actual shape of that doubt rather than just reassuring
past it: "beating" a funded team at shipping the most complete product is a
real, honestly-lost race, no point pretending otherwise. But that was never
the race this project set for itself — "the idea wins even if my build
isn't" was already said two exchanges earlier, and this was the first real
test of whether that was genuinely held or just a nice thing to say under no
pressure. Landed on treating the W3C group's current state (proposed seven
weeks ago, unchaired) as the concrete, actionable answer to "I want to be
involved" — a real door standing open right now, not a discouraging finding
— while continuing to build Mk1 regardless, since it's the only way to find
out if the actual abstractions hold up and it's what gives any of this
standing to say something to a group like that at all.

**Status at close.** No code, same as every entry in this phase.
research-plan.md itself still hasn't caught up to tonight — the
Process/invocation runtime-substrate note ("OpenClaw, Hermes, or similar")
still needs Pi named specifically, and nothing in the tracker yet accounts
for MCP/SKILL.md as adoptable standards, the uniform-I/O/time/connection/
root-of-trust pieces getting the same shape/seam treatment interoperability.md
now gives them, or tonight's finding on the W3C group, Engram, and OpenFang.
OpenFang specifically needs a real read next — closest thing found to a
whole competing system, not yet actually checked. Next real design work, per
the tracker's own stated priority, is still Trust/enforcement — checking
Capsicum and WASI closely, queued before this session started and still not
moved. Clarke's heading to bed; next session picks back up on design and
research.

## 2026-07-09 — the definition gets pressure-tested into vision.md for real: Collectives needs many First mates not one, Book/Fleet/Collective turns out to already be fully decided, and the actual rewrite goes out to README and the org profile. research-plan.md hasn't caught up yet

**Direct continuation of the entry below, not a new session** — its "status at
close" was written early; the actual session kept going for most of another
pass. Picking up right where the concrete-definition draft was handed over
for reaction.

**Clarified what "assembled" filesystem view actually meant, because it was
ambiguous enough to worry about.** Not a copy, not synthetic data — real
objects, restricted by *reachability*, not *realness*. Everything outside an
invocation's granted capability isn't faked, it's simply absent
(unreachable-by-construction, AgenticOS's own term, already in
research-plan.md). Leaned toward a FUSE-mediated view over raw bind-mounted
fds specifically because it's the natural place to hang per-access audit
logging and live mid-task revocation — but to whatever's running inside, it
has to be indistinguishable from an ordinary filesystem, real reads and real
writes, or an agent that doesn't know Mojo exists can't function inside it.

**Caught myself about to contradict an already-settled decision and fixed it
before it landed in a doc.** Went back through older devlog looking for
"SSI" and found Single System Image had already been proposed as the closest
lineage, then explicitly walked back two sessions before last: SSI's real
promise is uniform resource presentation everywhere, and Mojo deliberately
doesn't want that — a gaming rig shouldn't leak into the work laptop's view.
What's actually wanted is **federation**: continuity of identity and data,
not uniformity of resources. Tonight's scoped-mount-namespace mechanic had to
be stated as non-uniform per Vessel, or it would have quietly re-introduced
the exact thing already rejected.

**Named a real three-axis decomposition, with one term surviving and one
correctly rejected.** Vessel (machine), an as-yet-unnamed harness layer
(candidate: crewmember), and the model itself are independent choices — hull
size and which-agent-is-driving were wrongly treated as one knob in earlier
drafts tonight; they're two. Checked "crewmember" and "badge" against
naming-conventions.md's actual rule (register-1 nautical names have to be a
real, attested word — no manufactured terms): crewmember passes, badge
doesn't and was self-flagged as a bad example anyway. Floated **rating** — a
real naval personnel term for a sailor's specific qualification — as a
candidate for the model-axis name, explicitly not decided, parked for a real
naming pass rather than locked into anything.

**The subagent idea (First mate delegating to multiple crewmembers) turned
out to land on two rows already sitting Open in research-plan.md, not just
be a nice metaphor extension.** It's the concrete case that makes seL4's
Capability Derivation Tree (revoke a whole derived subtree) matter more than
Capsicum's flatter, revoke-nothing model for the Revocation row — killing a
parent task has to kill every crewmember spawned under it. And Agent libOS's
already-tracked candidate process shape has "children" sitting in it, which
means the precedent anticipated parent/child invocation trees before tonight
gave a concrete reason to need one. Mechanically it's not a new primitive
either way — the same ephemeral-invocation-with-scoped-capability pattern,
applied recursively.

**The real correction of the night, and it came from Clarke, not from me:
Collectives has many First mates, not one.** My own draft definition had
quietly hard-coded "Mojo is one first mate" as if that were the whole
system, when it's only the size-1 case. Checked whether the generalization
actually holds rather than just accepting it — it does, more solidly than
expected: vessel-research.md already has borrowed/rented devices as real,
full Vessels regardless of who owns them, and the capability trust mechanism
never assumed one Captain granting to itself. Held the other side honestly
too: the substrate allowing multiple Captains and First mates doesn't hand
over the actually hard part of Collectives — accountability and consent
across sovereign parties — for free. Recursion fixes the mechanical claim,
not the governance one; vision.md already says this honestly and shouldn't
be made to say more than it's earned.

**Open source, corrected from a license claim to a verifiability claim.**
Not "must be open-licensed," but "must be checkable that it never left your
hardware" — open weights are today's only way to get that, a fact about the
current market, not a permanent architectural rule. Matters because it
changes what the vision document is actually allowed to assert.

**Wrote two genuinely good jargon-free explanations of the whole idea, then
correctly benched both.** Attempted to make the concept legible to a
stranger with zero context — landed on leading with the felt pain (every AI
forgets you, is stuck on one device) before naming the mechanism, analogy
only at the very end for the hardest part (the group case). Caught mid-
stream that this was solving the wrong document's problem: vision.md is read
by people who already have the vocabulary, same as it already uses First
mate/Fleet/mercenary without re-explaining them every time. The jargon-free
version is real work for whenever public-facing copy actually gets written —
not thrown away, just not vision.md's job tonight.

**Clarke independently rederived something that turned out to already be
fully decided, more precisely, in book-research.md.** "Books with books
inside," multiple Captains and First mates recursing into Collectives — all
already landed: Book is one structure parameterized by however many
Captains/First mates/Vessels it holds; a Fleet *is* the atomic case (exactly
one Captain, one First mate), not a separate container that sits inside a
book. Corrected that one piece of phrasing directly. Surfaced a real nuance
tonight's version didn't have yet: every Fleet is guaranteed one
memory-flavored Cargo instance; a Collective can stand up its own shared
memory on top of its members' but doesn't have to — a real design fork for
later, not automatic.

**Mercenary and decoy cargo both corrected against existing research rather
than re-invented.** "Not a Vessel, why would it be" matches
vessel-research.md's already-decided line verbatim — mercenary is "not
infrastructure worked on, an external destination instructions are sent to."
What makes something a mercenary was clarified as *whose hardware it runs
on*, not what kind of software it is — Claude Code is a harness like any
other, it becomes a mercenary specifically because its weights run on
someone else's machine. And "decoy cargo" isn't new either — it's
tokenization/stand-ins, already sitting in cargo-research.md's
trust-graded-access section (the sensitivity judgment, Manifest, Decoy
cargo) and in ideas.md's Ephemeral Commons notes. Referenced fresh tonight,
not invented.

**Before writing anything, read the whole repo properly — philosophy.md,
roadmap.md, README.md, ideas.md, all four dockets-research files,
collective-intelligence-research.md, ephemeral-commons.md, and the org
profile — rather than drafting from tonight's chat alone.** Worth naming as
a real discipline, not just due diligence: two of tonight's corrections
(Book/Fleet/Collective, mercenary/decoy-cargo) only surfaced because of that
read, and would otherwise have been quietly re-invented, weaker, in
vision.md.

**vision.md actually rewritten.** "What I'm building" replaced with "What
Mojo is": Captain/First mate/Vessel/Fleet as the load-bearing vocabulary,
Collectives stated as the same structure recursed rather than a bolted-on
horizon feature, sovereignty restated as structural (enforced from outside
whatever agent is plugged in) rather than behavioral. Commons dropped from
vision entirely — Clarke's own call, it's an idea-stage mechanism
(ephemeral-commons.md), not core vision. MojOS's moods dropped for the same
reason — personal taste, never load-bearing, Clarke's own framing tonight.
"When it's right" went through two real rewrites: first pass mixed
system-level structural tests with behavior-level ones that just duplicated
philosophy.md's own five principles under different names; landed on four
tests mapped directly to what JARVIS the character actually is —
omnipresence, continuity, one identity across any agent/model swap
(the falsifiable core), sovereign data. Behavior stays philosophy.md's job,
explicitly pointed at rather than restated.

**README.md and the org profile both updated to match, and the org profile
had a real bug worth naming, not just a staleness issue.** Its "mojo-agent —
Jarvis itself" section literally named the two as the same thing, and stated
the agent was "built into MojOS from the ground up" — both direct
contradictions of tonight's whole agent-agnostic model, not just outdated
phrasing. Fixed to: Jarvis is the identity, mojo-agent is one reference
agent among any that can plug in. Pushed both repos —
`mojo-labs-circus/mojo` (vision.md, README.md, this devlog) and
`mojo-labs-circus/.github` (org profile). `ideas.md` and `research-plan.md`
had unrelated pre-existing uncommitted changes from before tonight's session
— deliberately left alone, not bundled into this commit.

**Status at close, for real this time: research-plan.md has not caught up
to any of this.** It still walks Unix/seL4/Plan 9/Erlang piece by piece
toward the pre-tonight framing. Most exposed: the Process/invocation
section's rows haven't been updated with tonight's concrete mechanism
(ephemeral crewmember invocations, filesystem-as-API, CDT-style
subagent delegation) even though its own scope note already anticipated the
right shape; the Trust/enforcement rows (Access control, Revocation,
Kernel/syscall boundary) are sitting on exactly the open questions tonight
answered and need those answers folded in, not re-derived later; and the
Identity/users section's gid-equivalent row, flagged as possibly dissolving
under a capability model, may now have a much more direct answer given
Book's already-decided Captain/First-mate/Vessel parameterization than the
row currently reflects. **Next: a real reconciliation pass on
research-plan.md, row by row, to confirm it's actually walking toward the
streamlined Captain/First-mate/Vessel/Fleet/Collective shape landed
tonight — not just structurally similar to it.**

## 2026-07-09 — the mission gets mechanical: memory is the only thing that grows, voice is a seed plus correction, and "agent-agnostic" turns out to mean the filesystem is the API. Chartering falls out of the invocation model for free instead of needing its own design

**Picked up exactly where 2026-07-08 left off** — talking through what Mojo
actually is until it's genuinely clear, no rewriting until it is. Pure
discussion session, nothing implemented until the very end.

**Killed a false dichotomy about what "grows" with you.** Posed the question
as either memory grows, or some persistent agent-character survives
underneath it — a real either/or. It isn't one: an LLM holds no state between
calls, same frozen weights every invocation, so there was never a second
option. Memory is the only substrate capable of persisting at all. "The first
mate growing with you" and "the memory growing with you" aren't two claims
that happen to coincide — they're the same claim. This directly corrects
vision.md's current "What I'm building" language, which still describes
continuity in terms of the agent/first-mate rather than the memory alone.

**Voice/personality is real and wanted — "that's what makes it Jarvis" —
but it's a second, differently-shaped artifact from memory, not a
side-effect of it.** Memory accumulates passively. Voice has to be
authored. Landed on hand-written seed plus correction-driven refinement:
you write a first draft of who Jarvis is, and it only changes when you
actually correct or confirm it in use — which turns out to be the exact
same mechanism already running as this project's own "feedback" memory
type (corrections and confirmations, with a why attached), just applied to
tone instead of task-approach. Explicitly ruled out passive style-mirroring
as a competing idea — copying Clarke's own texture would make Jarvis an
echo, not a character.

**LoRA and system-prompt voice injection split cleanly along the existing
fleet/mercenary line, not "cheap version now, better version later."** LoRA
only works on weights you can fine-tune — your own or chartered hulls.
Mercenaries (frontier models behind someone else's API) are permanently
capped at system-prompt-level voice; there's no future where a hired-out
frontier model gets the deep version. A hired Jarvis is always a thinner
Jarvis, structurally, not by choice.

**Ruled out a translation/rewrite layer for voice** — a second pass that
takes a "raw" answer and restyles it into Jarvis's voice. Two problems: it's
generation restyling generation, so meaning can drift in the rewrite: and it
directly undercuts vision.md's existing "the seam never fully disappears"
principle by manufacturing a persona over top of an output the user never
actually sees. System prompt and LoRA don't have this problem because
they shape generation itself — one pass, one artifact, nothing hidden.

**The sensei framing (philosophy.md) slots directly into the voice
mechanism instead of sitting beside it.** Sensei-as-posture — real
pushback, refuses to let you be passive — is the non-negotiable floor,
written into the hand-written seed. What's tunable is intensity: bluntness,
frequency, when to stay silent, calibrated by the same correction loop.
Pulled a concrete mechanism straight out of philosophy.md's own line ("earn
the right to challenge by knowing you"): challenge intensity should scale
with memory depth, not sit at a fixed setting from day one — thin memory,
generic and rare pushback; deep memory, the specific and cutting kind.

**The actual center of the session: worked out what "agent-agnostic" means
mechanically, not just as a principle.** Since Mojo can't require an agent
to know it exists, the plug interface has to be the one thing every agent
already speaks natively — files, processes, stdin/stdout, environment
variables. So the filesystem *is* the agent API: when Mojo launches an
invocation, it assembles a Capsicum/WASI-style scoped mount namespace from
whatever capabilities are currently held — memory mounted here, permitted
tools mounted there, everything else not hidden but *absent*. The agent
thinks it's just running on a computer. The computer it sees is the user's
identity, scoped by permission. This isn't a new design surface — it
collapses three previously-Open tracker rows (Uniform I/O, Tool/command
discovery, Access-control presentation) into direct consequences of one
decision, and promotes Mount table/namespace from "genuinely-new thread" to
the load-bearing row of the whole tracker. Real cost, stated honestly:
agnosticism means supervision can only ever treat the agent as a black box
— no cooperative checkpointing — which caps how good fault recovery can be
for agents that never opted into Mojo natively. A richer contract stays
possible for a future native reference adapter; the floor has to work for
the ignorant ones or the whole claim is fake.

**Worked out always-on concretely.** Split "always-on Jarvis" into two
different lifetimes instead of one: a small, deterministic, zero-AI Mojo
daemon (capability broker, namespace, Hail) that genuinely runs
continuously because there's no intelligence in it to keep warm, and
ephemeral agent invocations spun up per task and killed after — the
Erlang-shaped process rows made literal, and "first mate holds no state of
its own" made mechanical rather than aspirational, since nothing is lost
when an invocation dies because nothing that mattered was ever inside it.

**Texting/calling the agent falls out of that split for free, with one new
concrete claim.** Phone as a window, not a hull, already existed in
vision.md. An inbound message is a Hail over the existing mesh (already
excluded from Mojo-side design) that wakes an invocation at the daemon.
The new claim: inbound channels have to terminate at Mojo, not at whatever
agent is plugged in — otherwise swapping agents breaks how you're reached,
and the message bypasses the trust boundary on the way in. This is the
first identified case of "an agent's native feature (OpenClaw's own
channel routing) is exactly the part Mojo supersedes, not something to
integrate" — worth a tracker note, not yet added.

**Checked the OpenClaw comparison honestly, with a caveat.** The
daemon-holding-channels pattern is mechanically forced on every always-on
agent product — inference is always per-call, nobody keeps a model
"running," so this isn't a Mojo insight, it's a physical constraint
everyone shares. What differs is ownership: OpenClaw's daemon *is* the
agent, memory and config live inside the product, default session gets
full host access per the 2026-07-08 competitive check. Mojo's actual claim
is narrower and sharper: the gateway shouldn't belong to the agent, the
same way a router doesn't belong to a browser. Flagged explicitly as
reasoned from prior landscape research, not verified against OpenClaw's
actual source — a claim to check for real, not to cite as settled.

**Named the clearest version of the whole thesis, Clarke's own framing:**
harnesses made models hot-swappable, and that's obviously correct now —
nobody would weld a harness to one model. The same move one level up is
agents becoming the swappable part, with Mojo as the layer above that
survives the churn (OpenClaw ceding ground to Hermes, already happening,
is the pain arriving). Sovereignty finally has a literal home in this
picture rather than being bolted onto an agent as a feature: the layer
that owns memory and permissions is definitionally where "yours" lives,
because it's the only part that persists. Held two things honest rather
than just agreeing: agreement isn't the lock, this is Clarke's call, not
Claude's to confirm (corrected three times in the 2026-07-08 session for
exactly this); and the "unoccupied because early vs. unoccupied because
graveyard" risk from that session stays fully open — the only actual test
is the falsifiable core, identity surviving an agent swap on real
hardware, nothing moves.

**Closing insight, and it's a real simplification, not just a good
feeling:** since agent invocations are now ephemeral everywhere, not just
on rented hardware, chartering stops being a separate mechanism to design.
It's the same spin-up/scope/run/kill invocation model already designed for
local execution, pointed at rented compute instead of owned — the only
thing chartering adds on top is one precondition local execution doesn't
need, an attested host, which is a property of the rented hardware's own
confidential-compute stack, not a new mechanism Mojo has to build. Removes
a whole design surface vision.md previously implied was a special case.

**Status at close:** strong, coherent, mechanical account of what Mojo is
— not yet in any doc. Concrete definition being drafted next for
vision.md's "What I'm building" section, still pending Clarke's sign-off
before the rewrite pass starts.

## 2026-07-08 — mojo-agent isn't an agent Mojo builds — it's agent-agnostic scaffolding. A blind outside gut-check on the org, a real competitive-landscape check (OpenClaw, oikOS, Kestrel, SOMI), and Capsicum/WASI/AIOS/AgenticOS/Agent libOS added to research-plan.md all point the same direction: Mojo is the sovereign identity, trust, and coordination layer any agent plugs into, not the agent itself. Pitch has a strong working draft, not locked. Vision, all READMEs, research-plan.md, and roadmap.md still need to catch up, and the mission itself needs more talking-through before any of that rewriting starts

**Started as a blind gut-check, not self-generated.** Fed the org
(mojo-labs-circus) to a separate Claude session with zero context, just to see
how a stranger reads it. It landed on the POSIX-as-inspiration framing
correctly and, unprompted, surfaced a cluster of academic agent-OS papers
(AIOS, "AgentOS," AgenticOS, Agent libOS, Agent Protocol) as related work.
Worth doing again for future big questions — a clean external read caught
things a self-generated pass wouldn't have.

**Ran the literature down for real instead of trusting the summary.** Forked
four parallel research passes. Caught a real error in the blind session's own
citations first — it had conflated two unrelated papers under similar names.
The real picture: **AIOS** (arXiv:2403.16971) is genuinely active — 6k stars,
working code — but its access control is exactly the ambient/Unix-DAC
direction already rejected here; its context-interrupt scheduling
(snapshot/resume a long LLM call mid-generation) is real, useful precedent
though. **AgenticOS** (arXiv:2606.21129) is a three-week-old, code-free
preprint, but technically the sharpest of the four: it argues capability-
holding and intent-checking are complementary layers, not competing ones —
confirms rather than challenges the seL4 direction already picked here, and
its "unreachable by construction" technique (compile undeclared capabilities
out of the address space entirely, don't just runtime-gate them) is a real
requirement now, not just a nice-to-have. The paper actually named "AgentOS"
(arXiv:2603.08938) turned out to be unrelated to either — a weak NL-data-
ecosystem paper, not a security kernel, contributed nothing worth keeping
except a footnote on fault-recovery-via-snapshot. **Agent libOS**
(arXiv:2606.03895) is a solo, unreviewed preprint but has real working code
proving its core claim: tool dispatch is not the trust boundary — a model
seeing a tool doesn't grant it authority to use it, that's checked separately,
centrally. Agent Protocol (LangChain, real and maintained) turned out to be a
weaker fit than it looked — answers cross-call persistence, not the uniform-
I/O or tool-discovery rows it was hoped to answer.

**Added AIOS, AgenticOS, and Agent libOS to research-plan.md's systems list**
as a new tier — "contemporaneous, unproven," explicitly separated from the
four hardened primaries so nothing borrows their authority — with notes
dropped onto the seven rows they actually touch (Access control, Kernel/
syscall boundary, Revocation, Process/execution unit, Supervision/fault
recovery, Scheduling/priority, Tool discovery).

**Real design principle landed, not just cited:** what's visible to an
invocation should scope to currently-held capability (Plan 9 namespace-style),
not a full table gated at call time — my own instinct, independently
confirmed by AgenticOS's Manifest/Weaver split and by the older seL4/EROS
no-ambient-naming doctrine already in the systems list. Landed as a note on
the Access control row, still a principle not a mechanism.

**Pressure-tested whether research-plan.md's actual method — check real
precedent, decide per piece, dependency-sequenced — is the right way to reach
the MSI at all, not just whether tonight's research was sufficient.** Landed
yes, and for a reason worth remembering: it's the direct systematization of a
mistake I keep actually making — Arch, then realizing NixOS already solved it;
now Capsicum, same pattern. The real risk isn't the per-row method, it's
cross-row coherence — do later rows actually get built *from* earlier rows'
chosen primitives, or just independently rhyme with them. Sequencing-by-
dependency already guards most of this; agreed to also watch it continuously
rather than relying on one coherence pass at the very end, with the end-of-
process pass as a second, not sole, safeguard.

**Reopened two rows that were sitting in "Already checked, not gaps"** —
Device-as-separate-primitive-from-Vessel, and Naming/discovery — because both
calls were made before the agent-lit tier existed to check against. Redoing
both from scratch. The "Already checked, not gaps" section itself is gone
now, not left empty.

**Added Capsicum and WASI to the systems list**, likely more directly
relevant than seL4 or EROS to the whole Trust/enforcement section — both are
real, shipping capability systems built on exactly Mojo's own runtime-
contract constraint (capability security on ordinary userspace-on-Unix, not a
clean-slate kernel), not research artifacts. Notes dropped onto Access
control, Revocation, Kernel/syscall boundary, and Root of trust.

**Caught and fixed a real stale-reference bug in vision.md, not just a typo.**
It still linked to and described `standing-orders.md` as "the product of real
design work" (Keel, the permission-ceiling/permission-grant split, a
generational roster) — but that file was explicitly retired the same day as
"the vibes-era draft," and none of those three concepts made it into any
current doc. Confirmed directly: gone on purpose, not lost work to dig up.
Removed the dead paragraph from vision.md.

**The actual pivot, and it's the real center of this session.** Asked
directly: does anything already exist that gives "first mate, not chatbot
with extra steps," and are we about to reinvent something that's already
solved — the same question that's already burned me twice (Arch/NixOS,
Capsicum). Real answer, checked properly, not guessed: the application layer
is genuinely crowded now. oikOS, Kestrel, and SOMI are all real, working,
further along than anything here — persistent memory, and in oikOS's case a
capability-tiered permission model uncomfortably close to what's being
designed here. **OpenClaw** is the big one — 355K stars, 648 contributors,
corporate sponsorship, a real and mature tool-execution/channel-routing agent
that's become the substrate a whole ecosystem builds on top of. It already
solves system-control-via-conversation well. It solves none of what Mojo
actually wants: no OS-embedded primary interface, no declarative/rollback
safety, and its trust model is exactly the ambient one already rejected here
— the default session gets full host access with no sandbox at all.

**From there: mojo-agent might not be an agent Mojo builds at all.** If the
real, unoccupied gap is the OS-embedded identity/trust/coordination layer —
not agent capability, which is now a crowded, fast-moving, well-resourced
space Mojo can't out-build — then the right move is making Mojo agent-
agnostic scaffolding: drag-and-drop whatever agent you want into it, the same
way OpenClaw and oikOS already let you drag-and-drop models. This isn't
actually a new direction, it's an old one finishing: earlier the same day
(above), "the deterministic mechanism is a complete floor with zero AI in it,
First mate sits on top and consumes it, isn't baked into the primitives" was
already locked as a correction to an early overclaim. Tonight just took that
principle all the way to its honest conclusion. It also sharpens the trust
design rather than complicating it: "any agent, including ones with no idea
Mojo's model exists" forces external, structural enforcement (Capsicum/WASI/
seL4-shaped — constrain a process that doesn't know it's constrained) over
cooperative, internal enforcement (AIOS/Kestrel-shaped — only works if the
agent was built against your framework). That's not new scope, it's the
Access control row's already-chosen direction, now with a concrete reason it
has to be that one and not the alternative.

**Worked out what "Nix protections" actually means, and corrected my own
framing mid-conversation.** Nix the tool — not NixOS the distro — runs on any
Unix system (home-manager, nix-darwin, flakes), so Mojo's own declarative/
rollback safety for its own footprint is portable everywhere, not MojOS-
exclusive. NixOS extends that from "Mojo's footprint is safe" to "the whole
system is." MojOS = NixOS plus Mojo's own opinionated defaults (impermanence,
file structure) on top — the deepest host, not the only one, and not the
front door. Also corrected: the "no running software" phase was never a
hardware constraint (being away from my PC) — it was a discipline choice,
interface before implementation. Conflated the two without meaning to.

**Landed the concrete shape: Mojo is services, not a from-scratch OS
requirement.** A sovereign identity and memory that's genuinely local and
never leaves without consent; a capability/trust boundary enforced from
outside whatever agent is plugged in; hardware-aware coordination (Fleet +
Hail) that knows what each machine can actually do and sizes jobs
accordingly — a real, currently-untracked piece worth a research-plan.md row
of its own; and an open plug interface for agents and models, where model-
routing stays the plugged-in agent's job, not Mojo's. MojOS-as-headline was
explicitly rejected in favor of services-as-headline — real developers won't
switch their whole OS for an unproven solo project, but might install
services layered onto what they already run. MojOS stays the best host, not
the pitch.

**Fleet and Collectives survive completely intact** — neither ever actually
depended on a specific built agent, both were always properties of the
identity/coordination layer. If anything Collectives reads cleaner now: a
group where members run different agent software is a more honest model of a
real collective than assuming everyone's running identical tools.
`mojo-agent`'s own future is genuinely open now — might become a reference
plug-adapter instead of "the" agent — not resolved, flagged for later.

**The clearest statement of the whole session, worth keeping close to
verbatim:** "my brain knows everything about my life, that's what makes me
me, I have the context. Just because some days I'm a little slower or not
able to think as powerfully doesn't mean I'm not me. So it's the context and
identity that needs to remain." That's not a new claim — vision.md already
said this about hulls ("the same first mate... regardless of hull," voice and
judgment carried fresh via system prompt or LoRA) — just taken one honest
step further, from different-sized hulls to different agents entirely. I'd
hedged on this mid-conversation, calling cross-agent uniformity "a subtler
claim" than same-agent-different-hull. Wrong to hedge — it's the identical
mechanism, not a weaker version of it.

**Rewrote the public pitch through several real rounds, each with a specific
correction:** first draft over-indexed on the Linux/window-manager analogy
(cut — good material for vision.md's fuller prose, too heavy for a tight
pitch); second draft defined Mojo by negation ("not an agent, not an OS") —
caught as the same "say what, not what-not" rule from earlier the same day,
now generalized past doc-revisions to pitch copy specifically; third draft
lost the sovereignty framing entirely chasing brevity. Landed on:

> Mojo is where your intelligence lives — sovereign, local, and permanently
> yours, no matter which agent or model is doing the thinking. Swap the
> agent, swap the model, and none of it moves: your memory, your history,
> your permissions stay on your own hardware, not anyone else's. What people
> already get on one filesystem, one machine, Mojo grows into something that
> spans everything you own. Scales from one machine, to your fleet, to
> Collectives of people and AIs working together.

Pressure-tested against three real open questions (services vs. MojOS as the
headline, default-agent-bundled vs. pure scaffolding, same-agent-everywhere
vs. different-agents-per-hull) and survived all three unchanged — it was
already right on instinct before any of the three got explicitly settled.

**Status at close:** research-plan.md has a real new systems tier (AIOS,
AgenticOS, Agent libOS, Capsicum, WASI) and two reopened rows; vision.md has
one dead paragraph removed; the pitch has a strong working draft, not locked.
Nothing else public-facing has caught up to the actual pivot yet — README.md
still lists `mojo-agent` as "the eventual implementation," vision.md's "What
I'm building" section still describes mojo-agent as something Mojo builds
rather than scaffolding for whatever gets plugged in, and `mojos`,
`mojo-agent`, and the org `.github` profile haven't been touched at all.
Caught calling the pitch "settled" twice in my own summary right after
writing this entry — it isn't; noted so the pattern's on record, not just
corrected in the moment.

**Next:** keep talking through what Mojo actually is until it's genuinely
clear — the pitch draft above is strong but not locked, and there's more to
pressure-test before it is. Rewriting anything (vision.md, READMEs,
research-plan.md, roadmap.md) waits until that's settled, not before.

## 2026-07-08 — the "walk Unix from scratch" session queued above turns into picking the actual development methodology: seL4 gets checked as a second precedent, Hermes-wrapping gets dropped, the Mojo System Interface gets named, and the whole public-facing org gets rewritten to match

**Started exactly where the previous entry left off** — walking Unix piece by
piece on purpose to find where the model still needs extending — and got
redirected almost immediately: before walking more of Unix, build an actual
inventory of every interface-level piece of the system (files, processes,
users/permissions, IPC, the syscall/kernel boundary, shell/environment, time),
filtered to exclude what's explicitly inherited from the real OS underneath
(memory management, device drivers, the raw networking stack — already settled
as out of scope in docket-research.md). That inventory became
[research-plan.md](research-plan.md): a tracker, not a reasoning file, one row
per piece, scored against however many of Unix/seL4/Plan 9/Erlang-OTP actually
have something to say about it, status Open/Deciding/Decided.

**seL4 got a real research pass, not just a gesture at "capabilities."** Its
whole design divides cleanly from Unix on one axis: Unix is ambient authority
(uid/rwx, checked at open time, no way to revoke an already-open fd); seL4 has
none — holding a capability *is* the authority, and capabilities are trackable
in a Capability Derivation Tree with a real `Revoke` that kills a whole derived
subtree at once. That's a real, working answer to something dockets-research
had flagged as unsolved more than once (how a fleet actually takes back a
role/grant it gave out). Also load-bearing: seL4 has no "process" primitive at
all, only TCB + CSpace + VSpace composed by userspace — independent
confirmation that treating First mate as process-shaped was already the wrong
frame, not just a hunch.

**One real correction, caught by Clarke, not invented alone: process-side and
trust-enforcement are related, not identical.** First pass this session
conflated them because every invocation has to go through the enforcement
layer — but the enforcer has to mediate *every* actor (Sync writing Cargo, a
Vessel joining a Book, a human editing a file directly), not just First mate
invocations, exactly the same way seL4's kernel enforces capabilities for
threads, IPC, and memory alike, not just one kind of actor. Invocation is one
client of the enforcer, not the same primitive as it.

**Second, much bigger correction, again from pushback, not self-generated:
scrap the Hermes-wrapping plan entirely.** The locked 2026-07-05 plan was
build-first — wrap Hermes Agent wholesale, learn what it gets wrong, design
mojo-agent proper later. Argued for keeping it running in parallel with the
design work for real contact-with-reality signal; lost that argument for a
good reason — Hermes was always disposable, and wrapping a black-box
third-party tool teaches you about Hermes's quirks, not about whether your own
capability/invocation design holds up. Landed on treating this like a real
systems-development lifecycle instead: requirements (vision/philosophy, done)
→ the standards document, Mk1 → Mk1 system implementation, built against it →
iterate, versioning both as real use teaches what Mk1 got wrong — explicitly
modeled on how POSIX itself got built (POSIX.1-1988 covered its whole scope
thinly rather than perfecting files and leaving processes blank), corrected
once more when a depth-first reading of "Mk1" was wrong: Mk1 needs a
first-pass answer in *every* row of the tracker, not deep rigor on three
primitives and silence on the rest, since a working system needs some answer
for every piece to run at all.

**Named the eventual spec: the Mojo System Interface** — a direct, honest echo
of POSIX's real full name (Portable Operating System Interface), chosen under
explicit delegation ("do what you think"), deliberately not "MojOS Standard"
since that would collide the abstract policy with the concrete NixOS product.
Wired through `research-plan.md`, `roadmap.md`, `AGENTS.md`, and every
public-facing doc touched this session.

**Rewrote `AGENTS.md`, `roadmap.md`, and `README.md`** to match: Current Stage
now describes the real lifecycle instead of "build-first, meta-work rationed";
roadmap's Now section describes Mk1-of-the-Interface as the active phase, with
no running software during it as an explicit, deliberate tradeoff rather than
an oversight.

**Went to the GitHub org and found it badly out of date — fixed mojos,
mojo-agent, and the `.github` org profile.** mojos' README/AGENTS.md/V0.1.md
were all still describing the dropped Hermes plan as locked and current
(deleted V0.1.md rather than rewrite a Mk1 brief that's premature before the
Interface exists); mojo-agent's status line was stale; the org profile had a
real factual error (said MojOS was "Arch Linux rebuilt around the agent" —
it's NixOS, has been for a while) plus the same stale Hermes-era phase status.
Couldn't rename the org (mojo-labs-circus → recommended mojo-labs-armada,
avoiding "Fleet" since that's already the load-bearing technical term for the
atomic Captain+First-mate+Vessels unit) — GitHub doesn't expose org-login
renaming through the API, only through Settings in the web UI; flagged for
Clarke to do directly, with the follow-up work (local git remote, every
hardcoded org URL across these docs) noted as a consequence once he does.

**The project definition itself went through several real corrections, each
one caught by Clarke, not self-generated:**

- "An operating system" (flat) overclaims — docket-research.md already settled
  that Mojo builds on top of a real OS rather than replacing it; MojOS has no
  kernel of its own. Landed on "architected like an operating system" instead.
- "Built around an agent" (architecturally) contradicts something already
  locked in: the deterministic mechanism is a complete floor with zero AI in
  it, First mate sits on top and consumes it, isn't baked into the primitives.
  True at the product level, false at the architecture level — the two got
  conflated.
- "Use your computer exactly as normal, agent alongside" was correct in
  substance but redundant to say at all, since Mojo is explicitly additive to
  an existing OS — that's already a given, not a claim that needs defending.
- Landed on: "Mojo is a sovereign personal-AI platform, architected like an
  operating system. It adds Jarvis — your own first mate, an AI that actually
  knows you — and lets it work on your machine on your behalf. Scales from one
  machine, to your fleet, to Collectives of people and AIs working together."

**Collectives got real academic grounding, not just vision-doc language.**
Clarke supplied a genuinely converging research lineage — Hybrid Intelligence
(Dellermann et al. 2019), Hybrid Intelligence Teams (Eccles, Oxford 2025),
COHUMAIN (Gupta et al. 2025), and Beckers/Teubner's legal-theory case (2023)
for treating human-AI "hybrids" as their own accountable entity — arguing
independently since 2019 that the collective is the right unit of analysis,
with no one having built the system that treats it as one. Written to
[collective-intelligence-research.md](collective-intelligence-research.md) so
it isn't lost to a chat log, linked from vision.md alongside the concrete pain
point it's also solving: people already coordinate across separate, personal
AI tools today, fragmented rather than unified.

**Re-confirmed, not re-litigated: Mk1's primitives have to hold for
Collectives, structurally, even though Collectives itself stays Horizon.**
Not a new requirement — Book was already deliberately designed as one
recursive structure precisely so Fleet and Collective never need separate
primitives. What's genuinely still deferred is the *policy* on top (governance,
cross-Fleet trust negotiation, constitutions) — the same split already flagged
once for the permission-bits work, now applied one level up.

**"The Circus" turned out to be redundant, not a name worth keeping.**
Questioned directly ("is it still circus??") — it was describing exactly what
Fleet already is (one Captain, one First mate, many Vessels, one continuous
intelligence across machines), just under an older, pre-model nickname.
Dropped as its own concept everywhere; folded into mojo-agent's own
description instead. Same correction applied to the roadmap's Horizon section
more broadly: chartering and Collectives aren't separate future projects that
come after Mojo, they're capabilities of the one system realized through
iteration — rewrote `roadmap.md`'s Horizon section and the org profile's
Roadmap to match, and cut a redundant summary sentence restating this a second
time once flagged.

**Caught twice this session, worth holding onto:** committed language
("settled," "decided," "confirmed") for things that were only ever discussed,
including my own prior-turn conclusions, not just old devlog phrasing — and
explained a current plan by narrating what it replaced ("the earlier Hermes
plan is dropped because...") instead of just stating the current plan
directly. Both saved to memory; both should stop needing a correction.

**Status at close:** `research-plan.md` is the live tracker, most rows still
Open. Four public docs (mojo, mojos, mojo-agent, org profile) rewritten and
consistent with each other and with roadmap.md. Next session's actual work is
still ahead — walking the tracker's rows for real, starting with finishing
file-side (Clarke's call: not done yet) before touching trust/enforcement.

**Context:** a separate conversation (outside this repo, no access to
dockets-research/) walked Unix's file primitive from first principles toward
"how would you extend this to a distributed system," landing on: files are the
tractable part (Plan 9-style location-transparent namespace, not migration),
processes are the hard part, and for a personal-fleet AI identity you can skip
process migration entirely — mount identity/memory as a namespace instead. Pushed
one level up to a collective: `/collective/shared/*` (knowledge, goals, tasks)
plus `/collective/members/<name>/self/*`, recursive, with real multi-writer
concurrency now a genuine problem it didn't have to solve at the personal-fleet
scale. Landed on an open question — event log vs. lock vs. CRDT for the shared
knowledge write path — and stopped there.

**Checked against dockets-research/ and it holds up, mostly as rediscovery.**
The one-primitive/type-as-property move is `docket-research.md`'s starting point.
The collective's recursive member/shared namespace is exactly Book's already-settled
shape (Fleet as atomic Book, Collective as the same structure scaled up, nesting
free). The open conflict-resolution question is already partially further along
in `cargo-research.md` than where the outside conversation left it — human review
for genuine conflicts, explicit rejection of last-writer-wins — though still not
a finished mechanism, and deliberately left open rather than forced shut here too.
Genuinely new, not yet in dockets-research/: the literal mounting mechanism
(synthetic filesystem / Plan 9 9P framing, `/proc`-style live-served content vs.
flat replicated files) — dockets-research has stayed abstract on the mechanism on
purpose, so this is a real implementation-layer thread worth keeping.

**One real correction landed talking it through.** First mate's identity being
"process-side, PID/session-token-like, not Docket-side" (already in
docket-research.md) doesn't mean the content of the identity lives partly on the
process side the way a real Unix process's live memory does — an LLM invocation
is stateless, pure inference over context, so there's no independent runtime
state to hold anything. The process-side token is a pointer to which Cargo to
load, nothing more; essentially all of what constitutes First mate persists
Docket-side, because there's nowhere else for it to be. Sharpens the existing
text rather than contradicting it — worth carrying into whenever the process-side
primitive actually gets designed.

**Next:** new session, walking through Unix from scratch again on purpose, to
find where the model still needs extending.

## 2026-07-08 — standing-orders.md retired to git history; Keel/Billet/Flavor fully stripped from dockets-research/; the naming discipline hardens to "borrow POSIX verbatim or don't name it at all"; Book/Fleet/Collective resolve to one recursive structure; the five-primitive shape of the whole system gets sketched

**Status:** Standing-orders.md committed as-is (preserving the 53-definition
vibes-era first draft) then removed from the working tree entirely — same
treatment the pre-reset `raw/` corpus got. All four `dockets-research/*.md` files
rewritten: Keel, Billet, the Flavor-as-major/minor claim, the uid/gid section, and
Naming history are gone, along with every standing-orders.md reference and old
ticket number. Roughly 480 lines removed, 190 kept or added across the four files.
Verified by grep — zero hits for Keel/Billet/Roster afterward. No "Open threads"
section survives in any of the four files, on purpose: the actual struct-stat work
regenerates open questions as it goes, a maintained pending-list was judged not
worth the upkeep. Nothing in this session moved into a real spec — same discipline
as always.

**Started the actual struct-stat walk (Cargo's `st_ino` first), and it immediately
forced a much harder question than expected: are we inventing Mojo's own nautical
name for every field, or using POSIX's real names directly?** Checked this against
naming-conventions.md's own three-register system — register 1 (nautical) is
explicitly for real entities/actions in the fleet model, register 2 (plain
functional English) is for computed properties and mechanisms. An identity number
is squarely the second thing, and "Keel" was register drift dressed up as a good
fit: nautical-*sounding*, but never actually describing a role or relationship the
way Captain or Charter do. Landed somewhere sharper than either register, though:
if Mojo is going to literally reuse a real POSIX mechanism unmodified, it doesn't
need a name from *us* at all — it's not our vocabulary, Unix already owns it. Only
the things Mojo actually adds *on top* of the real mechanism need naming, and even
then, plainly, not nautically, since they're still pure scaffolding. Confirmed
explicitly: legibility to anyone who already knows Unix is a real, deliberate goal
here, not just a style preference, and worth holding onto "until Mojo becomes a
real thing."

**Applying that to identity specifically: real `st_ino` cannot be the mechanism,
for a precise structural reason, not a preference.** It's scoped to one
filesystem, gets recycled once freed, and changes the instant something crosses
filesystems (`EXDEV`) — none of which survives what Cargo actually needs (the same
identity, stable, across every replica on every vessel holding a copy). Real
validating precedent, since this is the identical problem already solved in
production: Syncthing doesn't use inode numbers either, for exactly this reason —
it mints its own Folder ID. So Mojo's own minted UUID is a second, separate thing
layered *on top* of the real, untouched inode underneath — not a renamed inode,
not a replacement for it. Concretely, this isn't a new metadata file per Cargo
item; it's one entry in whatever a Book already keeps as its own contents-list,
the same way a real directory's dirent table is centralized, not scattered across
each file. Also fixed a real bug in how this was described before: identity mints
at *creation*, not at *joining a Book* — `O_TMPFILE` proves the two are separable
(a real inode, zero names, still fully valid) — "first-join" language was smuggling
in a two-step story where real inode semantics are one step.

**"Billet" got caught on its own etymology and retired.** A billet is historically
an assigned *living space* — spatial, not functional — and Flagship/Consort/Tender
is a functional role, not a place. Real mismatch, not a naming preference. Went
back to first principles on what actually needs distinguishing: plain containment
(does a Docket belong to this Book, what's it called) turns out to need no name at
all, it's structurally just a dirent, same fate as Keel. The functional-role
question is real and separate, and doesn't map onto real POSIX cleanly either way
— major/minor device numbers were the original comparison and don't actually fit
(they exist because *many* device files share *one* driver; each Vessel has
exactly one driver-equivalent, itself, so there's no many-to-one case for a minor
number to distinguish). seL4 capabilities fit much better: an object's identity
held completely separate from external, reassignable, revocable rights over it,
kept in a separate space rather than on the object itself — which is exactly the
shape of "what role does the fleet currently grant this Vessel." Deliberately not
designed further now; deferred to the actual permission-bits/capability pass
rather than solved in isolation.

**That same role-question decomposed into two genuinely separate facts once
pushed on.** (1) How much real compute/judgment a Vessel can actually provide —
this should be *derived* from the hardware/runtime itself (a `/proc`-style live
fact), not declared by anyone. (2) Whether a Vessel currently holds the fleet's
canonical state (is it Flagship right now) — a real, reassignable designation.
This one isn't purely Book-side either: a Vessel needs its own locally-cached
belief about this too (pushed to it at Switch-flagship time), because requiring a
network round-trip before every canonical write would defeat the entire point of
Flagship being an always-on anchor. That reopens a known, previously-unresolved
gap — a partitioned-away old Flagship could keep believing it's still Flagship
after being demoted elsewhere, the classic fencing-token problem — and this time
it got a real answer instead of staying open: yes, self-healing, an epoch/
generation number bumped at every Switch-flagship, no reliance on anyone noticing.

**Does a Fleet's data have to live only on Flagship? No — it lives across all of
them, git-clone-style.** Authoritative and exclusively-located are different
properties; one canonical copy gets designated, but every vessel can hold a full
or partial replica, the same way every git clone has the full history, not just
the remote. Switch-flagship reassigns which copy is canonical, it doesn't move
data anywhere. A Tender is the exception in degree, not in kind — it may hold none
or a partial replica, but that's a capability difference, not a rule that only
Flagship gets the real data.

**standing-orders.md stopped being referenced at all, not even as an after-the-
fact cross-check.** First proposed keeping it as a corroboration signal once
something new got derived — corrected: agreement with something built without
rigor isn't corroboration, it's coincidence, and checking for it either way still
treats the old document as having authority it never earned. Its only remaining
use is as a record of what was generally wanted, useful later once it's actually
being rewritten for real, not as anything to weigh against while deriving the
mechanism now. Concretely retired from the tree this session (see Status above).

**Does Captain or First mate need a Docket? No, and there's a clean structural
reason.** Real Unix actually has a third identity system beyond inode and PID: the
uid/user-account concept, referenced *by* files (ownership) and *by* processes
(who they run as) without being either one itself. Captain maps directly onto
this. First mate stays process-side, already established as needing its own
PID-like identity, explicitly not a Docket. Roster/contents-list membership is
specifically Vessel/Cargo/Book entries — Captain and First mate are referenced by
a Book, not members of it.

**Re-examined what a Vessel actually requires, and the bar turned out lower than
"runs mojo-agent."** Tender already breaks that literal reading — it's thin,
mostly-forwarding, not running real local judgment. The actual floor is just
running whatever lets it participate at all: identity, permission-respecting,
speaks the fleet's protocol. Full AI judgment is a separate, optional,
capability-scaled layer on top of that floor, not the gate itself. This connects
to something bigger worth holding onto: Mojo should work for deterministic
computing generally, not just AI — the fleet-participation layer is a complete,
useful thing with zero AI in it, and AI sits on top to make it better, which maps
directly onto the `mojos`/`mojo-agent` repo split already in place. Pushed on
this once and refined it: the deterministic mechanism working without AI is a
floor, not a boundary — AI is a legitimate participant in decisions made *through*
that mechanism wherever it's available, including decisions about the fleet's own
management (when to actually switch Flagship, triaging conflicts, routing work),
not confined to sitting on top as an external consumer.

**The real structural finding of the night: Book is one general structure,
parameterized by three counts, not a family of different things.** Captains,
First mates, and Vessels are the three axes. The atomic floor case — a Fleet — is
exactly one Captain and one First mate, plus however many Vessels; it's your
machines and your Jarvis, full stop, not also a separate compositional label for
"any book mostly holding vessels" (a real tension flagged mid-session, resolved
this way by the end). A Collective is the identical structure scaled up — more of
each — flat or organized as nested sub-Books, some of which may be atomic Fleets
and some of which may have no individual Captain at all, in which case the owning
identity is nominal/shared rather than empty. A Hold is a Book that's mostly
Cargo — useful because grouping Cargo together is what lets it combine into
knowledge bases and context pieces, both handed to Charters/Mercenaries as a unit
*and* used directly by the agent day to day. Consequence worth being honest about:
the *structure* here is genuinely free — no new primitive needed for Fleet vs.
Collective vs. Armada, the same way infinite directory nesting is free in real
Unix from one recursive definition. The *behavior* across that structure — a
Collective's nominal identity actually getting minted, trust and permissions
enforced between sovereign Fleets that never fully trust each other — is not
free, and lands in the same still-open bucket as the permission-bits work.

Also corrected in passing: home automation devices are not Vessels, and using one
as an example was a mistake — a smart lock doesn't run mojo-agent or any
fleet-participation client, it's a tool the agent calls into, exactly as already
(and correctly) excluded before this session started.

**A definitions file got proposed, drafted, and deliberately not created yet.**
First pass produced nine candidate terms with real definitions; on review, several
felt "weirdly scoped," which turned out to be a fair instinct — Fleet's dual
meaning above and Captain/First mate's negative-only definitions ("not a Docket,
but...") were both real, nameable problems, not vague unease. Decided: only
genuinely, absolutely concrete, straight (non-hedgy) definitions belong in it, and
nothing is concrete enough yet — the actual struct-stat mechanism work is what
will produce real, confirmed content, not a definitions exercise done in
isolation ahead of it. Scope agreed: root-level, all of Mojo, not scoped to
dockets-research specifically. Not written this session.

**Bigger picture, discussed at length and worth keeping.** The full system needs
five pieces at this level of rigor, not one: the file-side (Docket, underway),
the process-side (First mate, still fully undesigned, possibly the hardest of
the five since neither Unix nor seL4 ever had to model judgment-based execution),
the connection between the two (Hail/the mesh), identity/user (Captain, the third
primitive), and the kernel/trust-enforcement layer underneath all of it
(seL4-flavored, already known to connect directly to the permission-bits work).
Worth naming what's deliberately *not* on that list: memory management, device
drivers, the raw networking stack — correctly excluded because Mojo builds on top
of a real OS and real tools (Tailscale/WireGuard) rather than replacing them,
which is itself a sign the scoping is right, not an oversight.

On process: reaffirmed that locking the load-bearing core before building is a
real, evidence-backed bet this session actually demonstrated (Cargo's cardinality,
mojo-package's real scope, Book's required heterogeneity all would have been
breaking changes against running code if gotten wrong first) — with an honest
caveat that this mode has a natural limit, since even Linux itself wasn't preceded
by this level of upfront rigor and the already-agreed principle is to lock the
core, not litigate every leaf-level decision forever.

On timeline, asked directly and answered honestly rather than dodged: the full
five-primitive vision at tonight's level of rigor is realistically months out,
likely into 2027, not a September thing — tonight alone covered maybe 15-20% of
just the file-side primitive. The narrower thing the existing roadmap actually
promises — one Fleet, dogfoodable as a daily driver — is a much smaller slice that
doesn't need Collectives or the general capability system solved first, but even
that is more likely October/November than September 15 once tonight's real depth
is accounted for, unless scope gets cut harder or design time gets capped sooner
to force the pivot into building.

**One real correction surfaced after the above was already written tonight, worth
keeping distinct rather than silently folding in: the process-side primitive
isn't process-shaped in the Unix sense at all, and treating it that way above was
a category error, not just an open question.** There's no actually-persistent
computation to model — each invocation of First mate is genuinely new,
computationally. What got called "identity continuity" earlier tonight isn't a
process fact, it's entirely a context fact: the reason logging into a laptop
feels continuous isn't that any process survived a reboot, it's that the files
did. That kills the "does Mojo need a PID-equivalent for First mate" framing
directly — it doesn't, because there's no process there to give one to. The same
move dissolves the single-vs-multiple-simultaneous-instances question from
earlier tonight too: sameness is just shared access to the same Cargo, not a new
identity-linking mechanism — five concurrent invocations across five vessels are
trivially "the same First mate" as long as they read and write the same
knowledge base.

What's left of process-side design, properly scoped down after this: a much
smaller **invocation** contract, not a rich process abstraction — spin up with a
task plus context access, do work through tools, write results back, tear down.
"Process" itself may be the wrong word to borrow at all here, same
etymology-mismatch shape as Billet not fitting once its own job narrowed. Two
genuinely open pieces remain, smaller than they looked earlier tonight: what
that invocation contract concretely looks like, and whether truly simultaneous
writes to the same Cargo from different vessels need any lighter real-time
coordination beyond eventual git/PR reconciliation. New question raised and
explicitly parked as future work, not to solve now: should Mojo ever run
multiple simultaneous instances of First mate at once, the way Claude itself
uses subagents.

**Next up, not started tonight:** the actual struct-stat field mechanism work —
identity, ownership, `st_nlink`, permission bits, size, timestamps, `st_rdev`,
birth time — per type, side by side, one field at a time, same collaborative
discipline as tonight (propose, check against Clarke's read, adjust, confirm,
only then move on). The four dockets-research files are now lean enough to
actually receive that work without fighting old vocabulary first.

---

## 2026-07-07 (even later still, continued) — Docket lands as the umbrella name; Book replaces Fleet as the directory-type's own name; commands-research/ renamed to dockets-research/ and cleaned into a real current-state record

**Status:** Still no standing-orders.md edits. `commands-research/` is now
`dockets-research/`, with `command-research.md` → `docket-research.md` and
`fleet-research.md` → `book-research.md` (`cargo-research.md` already had the right
name from last time). Every file in it got a genuine cleanup pass tonight, not just
a rename — superseded reasoning (old wrong turns already corrected, resolved naming
debates, redundant restatements) got cut; anything still load-bearing stayed. These
four files are now meant to function as the actual current-state record of the
file-side of the system, not a session diary — the full chronological reasoning
trail for anything trimmed still lives here in devlog.md, that split is deliberate.

**seL4 checked against the file-side shape directly — it doesn't change it, and the
reason why is worth keeping precisely.** seL4 has no file abstraction at all; any
directory/file interface on top of it is built entirely in userspace (Genode is the
concrete, real, already-working example — a POSIX-like VFS running on seL4). What
changes under a capability-based system isn't the data model (files hold content,
directories organize, devices route to drivers — that shape survives untouched),
it's the *access* model: classic Unix trusts ambient identity (whatever uid a
process runs as), seL4 trusts nothing by default and requires an explicit,
unforgeable capability for every single right. Permission ceiling was already
capability-flavored before this discussion started (rationale #10), so there's
nothing here that would force unwinding any of tonight's or previous sessions'
Docket/Vessel/Book/Cargo work. Corrected framing on sequencing too: this isn't a
separate research detour to schedule "later" — it's literally what "doing
permissions properly" already means once the struct-stat pass reaches `st_mode`.

**Actively tried again to break the three-type claim before locking it — nothing
new turned up.** Checked symlink from three fresh angles: cross-Book aliasing (a
reference to a Docket in a different Book) resolves via directory-traversal through
Armada structure, already decided, not symlink-shaped; role-aliasing ("the current
semantic-memory Docket") resolves via the same reassignable Roster-entry mechanism
Flagship already has, not a distinct pointer type; reference-without-copying (is a
Manifest a pointer rather than real content) — no, a Manifest is genuinely real,
scoped content. Vessel, Cargo, Book stands as the type list, with the explicit
caveat that if a fourth genuinely turns up once the deeper per-type work
(especially permissions) gets underway, that's fine, not something to keep forcing
a search for tonight.

**Naming: the umbrella went through one more real round after Command, landing on
Docket.** Widened past Charge (felt wrong on the same instinct as Command) into
Holding (risked colliding with Hold), Complement, Effects — none stuck across all
three types cleanly. Landed on the registration/record theme instead of the
authority/custody theme Command and Charge both tried: what actually unifies
Vessel/Book/Cargo is that they persist, are addressed by stable identity, and are
*constituted* by being recorded — a Vessel is a Vessel as soon as it has a Vessel
Docket, full stop, no billet required, not even an initial one, sharper than the
"existence vs. currently-billeted" fix from two sessions ago. Register and Papers
both fit that theme; Bills got checked and ruled out directly — the watch bill
(#38) is already a real bill in the true nautical-document sense, a genuine
collision, not just an awkward echo. Papers pulled hard for a while specifically
because of how close it is to "file" itself, but the singular-instance awkwardness
never actually resolved ("the devlog.md papers" still reads wrong for one item).
**Docket** won on a real structural test, not just taste: a docket carries its own
identity independent of wherever it's currently filed, which is exactly what Keel
needs — tested against Page specifically and Page failed this cleanly, since a
page's "identity" is purely positional (page 47 means nothing without naming which
book), the opposite of what's needed.

**The directory-type Docket, Fleet's old job, got its own real name too: Book.**
Same double-duty problem Command almost had, one level down — Fleet was both the
structural umbrella and one of its own two flavors (next to Hold). Compartment and
Bay were both floated as neutral replacements; landed on Book instead — genuinely
strong nautical pedigree in its own right (log book, muster book, a ship's account
book), not just a trimmed-down "Docket Book," single-word, and the parent-child
relationship to Docket reads immediately without requiring anyone to already
recognize a specialized term. Checked it against every scale the type needs to
cover — a Fleet-flavored Book (many Vessel entries, like a muster book), a
Hold-flavored Book (many Cargo entries, like a log book), and a Collective (a Book
of Books) — all read naturally.

**One process correction worth keeping distinct from the naming one**: citations to
standing-orders.md item numbers stay, they're genuinely useful — the only actual
issue was #11 specifically, since this whole multi-session thread has substantially
reworked the Vessel-existence criterion it describes and it's now known-stale
relative to tonight's findings. Not a blanket instruction to stop citing numbers,
just not to lean on that particular one as settled ground right now.

**The cleanup pass itself, concretely**: `docket-research.md` got a single
consolidated "Naming history" section replacing four scattered, partially
contradicting naming threads. `vessel-research.md` lost its now-superseded "first
pass, later corrected" scaffolding and its struct-stat table (deliberately not
rebuilding that yet — comes back properly, per type, as its own dedicated pass).
`book-research.md` got its stale mojo-package paragraph actually fixed this time —
mojo-package is the whole portable system, the disaster-recovery/offline-promotion
answer, not the Commission-time bootstrap kit, which still needs its own separate
name, genuinely undecided. `cargo-research.md` lost redundant restatements between
sections that were saying the same thing twice. Every file kept a "Definition
(current)" section at the top (what it is, its flavors, its Unix type match, what
"docket-side persistent data" means for it specifically) and a trimmed "Open
threads" list with only genuinely open items — resolved debates and superseded
framings removed rather than left to accumulate as noise.

**Next up, explicitly not started tonight**: the struct-stat/inode field
walkthrough, per type, against the now-settled Vessel/Book/Cargo names. Handoff
written separately for this, not filed here — same collaborative discipline as the
whole thread: one field at a time, checked against Clarke's read before moving on,
not a solo pass handed back finished.

**Status:** Still no standing-orders.md edits — same discipline as every session in
this thread. `commands-research/` got real updates (`log-research.md` renamed and
rewritten as `cargo-research.md`; `fleet-research.md` and `command-research.md`
updated), but one honest process note: partway through, a characterization of
mojo-package got written into `fleet-research.md` before it was actually settled,
and had to be walked back a message later — that paragraph is currently stale and
needs a fix pass next time, held rather than corrected in the moment, on explicit
instruction to stop writing files mid-discussion and just talk it through instead.
Worth remembering for next time: write less eagerly, especially once something's
visibly still moving.

**The regular-file-type Command went through its full arc this session: Log → Hold
→ Cargo, each rename for a concrete reason, not cosmetic.** Started by actually
interrogating whether "Log" fits regular file at all rather than asserting it —
checked opacity (is it opaque to Fleet-level mechanics the way a real file is opaque
to the OS — yes, Sync just moves bytes, doesn't parse structure), checked
append-mostly-ness (the word "log" already connotes this, same as `/var/log/syslog`
being an ordinary `O_APPEND` file), and ran into a live counterexample that almost
broke it: the actual precedent for this whole idea — git-backed markdown — is,
physically, *this repo*, several files, not one. Resolved the same way a regular
file's own data blocks are scattered across many physical disk blocks without that
making the file directory-shaped: the type is about the presented interface, not the
physical substrate.

Then the real scope question, which mattered more than the interrogation: is this
just agent memory, or the whole shared-data surface — Documents, Projects, Downloads,
the "personal cloud" idea that vessel-research.md had already named as one of
Vessel's four functional groups but never actually mapped to a Command type. Landed
on: same type, decomposed by a real principle rather than by "curated vs. not" —
split into a new instance wherever sensitivity tier, sync/access behavior, or owning
boundary genuinely differs, keep together where none do. Then generalized further,
correcting the same mistake #11 needed correcting for Vessel: existence isn't the
same as being currently promoted/active. Every regular file already has the identity
scaffold; almost all of them just sit at a do-nothing default, the same way a real
inode exists whether or not anyone reads that file again. Renamed Log → **Hold** —
fixes the CS-overload problem ("log" already means something specific and different
in software) and, more importantly, unifies the register with Manifest/Decoy
cargo/full hold, which were already real shipping-cargo terms sitting awkwardly next
to a logbook metaphor. Then Hold → **Cargo** once "every file is potentially this
type" made "Hold" feel wrong for an individual item — a cargo hold is a whole
storage bay, not one crate in it. Hold got reassigned as a flavor-name for a
Cargo-heavy directory instead of the individual-file type's own name. Manifest and
Decoy cargo both read *more* literally after each rename, not less — a real signal
each move was right, not just non-colliding.

**Checked directly whether this is just POSIX with new names — it isn't, and the
real new work is a genuine policy layer POSIX never had to build.** The content
model (opaque byte stream, no imposed structure, no dispatch) stayed completely
unreinvented on purpose. What's actually new: cross-replica identity (wrongly
reached for Tailscale first — that's Vessel's mechanism, a different problem; wrongly
reached for git blob hashing next — backwards, content-derived identity changes the
moment content changes; landed on reusing the same Keel-minting mechanism every
Command type already has, just wider in scope, validated by Syncthing's own Folder
ID doing exactly this in already-running software); trust-graded access via Manifest/
Decoy cargo/the sensitivity judgment instead of binary rwx; conflict resolution as
explicit policy instead of POSIX's non-answer; location-transparent addressing
instead of a path only meaning something on one machine. Explicitly *not* this
type's own contribution: reversibility. That's the MojOS snapshot layer sitting
under everything on the box — confirmed directly, "the beauty of MojOS is that
everything on the system gains reversibility, including all the mojo files."

**Nix's own content-addressed store was floated as the identity/versioning
mechanism and correctly self-caught as a space-complexity trap** — content-addressed
plus "pointer to newest" mints a brand-new store path on every edit, and old ones
only clear via garbage collection, which either deletes the history that was wanted
or, left alone, keeps every version forever. Same tension git already has, and
exactly why git needs periodic repack rather than keeping every blob raw. Git and
Syncthing stay the real mechanism; Nix's actual right role is narrower and genuinely
useful — the impermanence module (already cited in standing-orders.md's own
rationale for MojOS) is the right place to *declare where this type's territory
lives*, not to power what happens inside it. Resulting boundary test: reproducible
from declarative config is Nix's job (system config, packages, secrets via
`sops-nix`); not reproducible from a declaration is this type's job (memory,
projects, documents, media). Walked back an earlier take on secrets needing
*stricter* Hold policy — wrong, secrets isn't this type's problem at all, `sops-nix`
already owns it at the config layer, same place general system config already lives.

**Fleet's Roster is now understood as genuinely heterogeneous — Vessels and Cargo
together, one list, not two.** Same argument that justified grouping Cargo into
directories in the first place (a real directory mixes files and subdirectories
freely, no purity requirement) applies one level up. Not a stretch — required for
internal consistency, since a Fleet already governs both the machines and the data
today and there was never a principled reason to split those into separate
structures.

**Memory vs. "just files the agent uses" resolved as the same thing, not two
categories.** The evidence was sitting in this very repo the whole time —
`devlog.md` (episodic, chronological) sits next to `vision.md`/`philosophy.md`
(semantic, consolidated) inside one ordinary git project, no separate memory folder
anywhere. Landed on: not structurally different types, different lever settings on
the same type (semantic indexing, conflict-routed-to-human-review, sync-priority on
vs. off) — same move as Flagship/Consort/Tender being lever-settings on one Vessel
type. Reinforced by the live industry trend of agents leaning on plain filesystem
access over heavy vector-DB/RAG stacks — the plain filesystem winning as the actual
memory substrate generally. The lever framework itself got flagged as "feeling
weird" and is still genuinely open, not yet resolved.

**mojo-package's real scope, corrected properly this time.** First pass wrongly
narrowed it to just the Commission-time bootstrap kit. Actual intent: mojo-package is
the entire portable mojo-system — everything — and it's specifically the answer to
cases Enlist+Sync+Switch-flagship can't cover: switching Flagship without live access
to the new candidate yet, or the old fleet being unreachable entirely, or wanting a
real offsite backup snapshot of your whole digital life. The narrow Commission-time
bootstrap operation needs its own, different name — not mojo-package — genuinely
undecided, parked.

**The log's single-writer rule loosened — not to no rule, to a different, arguably
better-fitting one.** #24/#25 currently say Flagship alone may write the log
directly, justified by CAP theorem. Reframed: Flagship isn't the only place a write
can *originate*, it's the only place a write becomes *canonical*. Any vessel can
write locally, always, not just when disconnected — it just isn't canon until
synced. This generalizes #27 ("beyond hailing range") from a disconnection-specific
special case into the thing that's always happening, just faster when connected —
which is exactly how git already works, and Cargo is already git-backed. Pulled the
whole git toolkit in deliberately rather than just commit/push/pull: branch as a real
new mechanism for deliberate tentative/exploratory state that isn't meant to become
canon (yet, or ever); PR as the actual concrete implementation of #27's "route to
human review" rather than an abstraction needing its own invented UI — and PRs
already have a well-trodden path from fully-manual review to policy-gated auto-merge,
a real candidate answer to whether human review has to stay the only mechanism
forever. Floated self-hosting this on Flagship via Gitea, then reconsidered toward
**Forgejo** specifically — Gitea underwent a real 2024 governance controversy (a
for-profit entity taking control of the project without full community buy-in),
which is exactly why Forgejo exists as a community-governed fork. Directly on-theme
for a project this concerned with sovereignty, not a random tool swap.

**Bigger picture, worth stating plainly since it kept happening all session: the
actual v1 build strategy is orchestrating proven open-source tools under one
coherent Mojo policy layer, not building bespoke infrastructure.** Tailscale, git,
Syncthing (and its Folder ID mechanism specifically), Nix's impermanence module,
`sops-nix`, Forgejo — every mechanism question this session resolved to "there's
already a real tool for this." Confirmed as the deliberate strategy, not a string of
individually convenient choices: get a working ecosystem together from existing
proven parts first, fix things as real usage reveals what actually needs replacing.

**Asked to explain Unix at the "files, processes, and the rules connecting them"
level of abstraction, then mapped it honestly onto Mojo — including what's still
completely undesigned.** Files split into naming/addressing, access mediation,
process-to-process connection, and the kernel as trusted enforcer underneath all
three. Mapping: files → Command (this whole session, and the two before it). Naming/
addressing → Keel plus location-transparent addressing. Access mediation →
Ownership/Permission ceiling/the sensitivity judgment. Process-to-process connection
→ Hail/the mesh, barely designed. Processes themselves → First mate/the process-side
primitive, flagged back at the start of this whole thread as "still to be designed"
and genuinely still is — this whole three-session arc has been the *file* half of
Unix only. The kernel-as-trusted-enforcer → nothing named yet, and this connects
directly to the seL4/microkernel research thread already flagged as real future work
two sessions ago, not a new tangent. Real conclusion, worth keeping: Mojo's
*structure* is close to 1:1 with Unix's; its *content* deliberately isn't, and
shouldn't be — every place it diverges (cross-replica identity, trust-graded access,
conflict-as-policy, location-transparent addressing) is exactly where Unix never had
to solve for being distributed, multi-trust-domain, or agent-native, which is the
actual interesting work, not a gap to be embarrassed about.

**Single System Image walked back — a real correction to prior-session thinking, not
a new idea from scratch.** SSI got named as the closest research lineage two sessions
ago and reconfirmed this session, then correctly challenged: SSI's actual defining
promise in the literature is uniform resource presentation everywhere — you can't
tell which physical node you're on. Mojo explicitly doesn't want that; a gaming
setup shouldn't show up on the work laptop, that's a feature of the design, not a gap
in it. What Mojo actually wants is narrower — continuity of identity (First mate
feels like the same partner everywhere) and continuity of specific data (Cargo), not
uniformity of every resource. Closer to **federation** — shared identity/protocol/
data continuity while each member stays genuinely, deliberately locally autonomous —
with Nix flakes/modules being the concrete mechanism that makes "shared declarative
base, selectively divergent instances" actually practical, arguably a more
sophisticated answer than classic SSI research (Sprite/MOSIX) ever cleanly achieved,
not just a difference in taste. The Plan 9/Sprite/Amoeba citation in rationale #2
stays valid for the narrower thing it was actually describing — identity/process
continuity across relocation — it just shouldn't be read as claiming the full SSI
promise.

**Sequencing question, genuinely argued through rather than just agreed to.**
Pushed back on "get to full POSIX-completeness before implementing anything" with a
real historical correction — POSIX itself was extracted *from* already-running,
already-diverged Unix implementations, not written speculatively before any of them
existed, so "finish the formal spec first" isn't actually how the precedent being
invoked came to exist. Also named a real, live tension directly rather than letting
it slide: roadmap.md's current build-first-reset stage explicitly rations meta-work
against a September MojOS v0.1/mojo-agent v0.1 deadline, which is in real tension
with "fully spec everything before writing code." Landed on, confirmed: lock the
load-bearing architectural core — type inventory, identity mechanism, the sync/
trust/conflict model, the stuff this very session proved is expensive to get wrong
after code exists (the Cargo-cardinality fix, mojo-package's actual scope, Fleet's
Roster homogeneity, and the Flagship-local-knowledge question below all would've been
breaking changes against running code) — but don't block all implementation on
finishing every leaf-level naming or decomposition decision, which can converge
iteratively alongside real building, the way POSIX itself converged. Not "produce a
finished formal document" — "get the architecture visible enough to start building
against it."

**One concrete stress test worth keeping: does a Vessel need to locally know it's
Flagship, or is that purely Fleet-side?** Purely Fleet-side, queried fresh every
time, doesn't survive contact with reality — Flagship status has to be checked
before every log write, and requiring a network round-trip before each one would
defeat the entire point of Flagship being an always-on anchor. So yes, a vessel
needs a local cached belief, classified **Declared** from its own point of view
(pushed explicitly at Switch flagship time, same shape Ownership already has, not
silently re-derived). Authoritative source stays the Fleet's Roster. Honest,
unresolved gap named plainly rather than papered over: a genuine network partition
could leave an old Flagship still believing it holds the billet after being demoted
while cut off — the classic fencing-token problem any primary-replica system has to
answer, and nothing here answers it yet.

**Naming, still the most open thread of the whole session, deliberately not forced
to a conclusion tonight.** Command itself got reopened, not just Cargo's own name —
its original justification (officer-has-command-of-a-ship, Fleet Command as an
organization) is precisely an authority/organizational metaphor, and it doesn't
stretch to a stored file the way it stretched to Vessel/Fleet/Collective; unlike
POSIX's "file," which was already a loose, low-commitment word before Unix took it,
Command was chosen specifically for how exactly it fit a narrower scope, which makes
it more brittle now that scope has grown, not less. Charge was tried next and also
felt wrong. Opened wide from there — Holding (real "mixed portfolio" precedent, but
risks colliding with Hold, the flavor-name just landed on), Complement, Effects, and
**Register** all surfaced, with Register currently the leading direction: Unix's own
"file" concept is really about persisting as a stable, named, recorded entity, not
about authority or custody, and Mojo's own model already leans on this harder than
Unix does — #11 ties a Vessel's very existence to having been billeted/recorded at
all. Separately, the directory-type Command (currently "Fleet," now also carrying
"Hold" as its Cargo-heavy flavor) needs its own neutral structural name too, same
double-duty problem the umbrella question has one level down — Compartment and Bay
proposed, neither decided. Explicit next step, picking back up from here. — Vessel worked out against POSIX's file/inode model in real depth; Fleet and a new third type, Log, follow from it; AI resolved as process-side, not a Command type at all

**Status:** No standing-orders.md edits tonight, on purpose, same discipline as the
session before it — this was all discussion, now captured properly in a new
[commands-research/](commands-research/) directory (`command-research.md`,
`vessel-research.md`, `fleet-research.md`, `log-research.md`), which replaces and
retires the earlier `vessel-primitive.md`. Explicitly research, not spec — nothing
here is confirmed enough to fold into standing-orders.md yet, same rule as
everywhere else in this repo. Corrected early in the session, worth stating plainly:
"queued correction" or "agreed" language in past devlog entries does not mean
settled — this session re-litigated several things earlier passes had stated too
confidently, and got some of them wrong a second time before landing right.

**The actual exercise: work out Mojo's own "everything is a file" primitive by
testing it field-by-field against POSIX's real `struct stat`/inode model, not a
vague gesture at the phrase.** Landed on a genuine third-order structural finding
partway through: Unix doesn't really have two primitives (file and directory) — it
has *one* (the inode, with type as a property), and directory is just one value of
that property, same scaffold as every other type. Applying that directly: Mojo's
core primitive is one thing, not "Vessel and Fleet as two separate primitives" —
renamed the umbrella **Command**, tested against real naval usage at both scales
("given command of a destroyer" vs. "Fleet Command," and — the sharpest validation —
"Supreme Allied Command" for the no-single-owner Collective case, a better match
than Fleet Command even is). Vessel and Fleet are its first two known types; a third,
**Log**, emerged over the course of the session and is now believed real.

**Vessel resolved as the device-type Command, not just "the generic example."**
Worked through actual Unix device mechanics — major/minor numbers, the device node
being a thin dumb pointer to a real driver doing the actual work, `ioctl` as the
escape hatch for what doesn't fit generic read/write, node-existence being separate
from current hardware presence, and `udev`'s persistent `by-id`/`by-uuid` naming as
already-running real-world precedent for exactly the Keel design already chosen —
then ran a full concrete example (a USB mouse, `/dev/input/eventN`, `EVIOCGBIT`) to
pressure-test the mapping. Also corrected, more than once: existence (Keel) was
wrongly tied to *currently holding a billet* in #11's current wording — an
unlinked-but-open file is still fully a file, and a joined-but-unbilleted vessel
should be a real, still-existing state, not lumped in with hardware that never
joined at all. Ownership was also wrongly treated as gating vessel-hood at all —
corrected: all three existing Ownership values already presuppose full vessel
participation, so Charters and even a borrowed work laptop are real, full vessels for
their duration (the fleet's boundary is "what Clarke works with," not "what Clarke
owns"), while a door lock and a Mercenary genuinely stay outside the model entirely,
for reasons that have nothing to do with ownership.

**Keel's actual mechanics got nailed down properly**: minted by Fleet at first-join
as a UUID, never derived from hardware (a derived identity breaks on component
replacement; a minted one survives a full rebuild because identity was never about
the atoms); never recycled (falls out of UUID-space size, not a rule to enforce);
survives OS reinstall and any hardware change automatically because it's classified
Rooted; and a Captain-to-Captain ownership handoff is a genuine cross-filesystem
`EXDEV` move, not an in-place `chown` — which is exactly why standing-orders.md's
existing "retired, not transferred" line for ownership handoff was already correct,
without anyone having reasoned through why yet.

**uid/gid worked out properly, after one real wrong turn.** First pass conflated
`gid` with "which Fleet a vessel belongs to" — wrong, that's containment (the
Billet, already fully designed), and real Unix `gid` has nothing to do with which
directory a file lives in. Corrected: `uid` is the owning Captain (free today, a
Fleet has exactly one); `gid` is a named set of Captains sharing standing rights,
independent of both ownership and containment, and — like real Unix's own "user
private group" default — is never actually empty, just trivially a singleton group
today. A Collective's lack of a single owner doesn't mean `uid` goes empty either:
it points at the Collective's own nominal identity, following the real Unix
service-account precedent (`root`, `daemon`, a project's dedicated account), the same
shape First mate (#2) already has one level down. Permission bits (`st_mode`'s
owner/group/other) are flagged as the one piece of real, still-open design work that
actually makes group-sharing do anything — parked for the next pass.

**Log is new, and worth being careful about what it actually is.** Started as "the
brain" (already named in standing-orders.md as #25, "the log," just never formalized
as its own type) and initially over-coupled to AI/identity — corrected: a knowledge
store existing and an AI actively tending it are two separate facts, and a Fleet's
Log is guaranteed (First mate always exists) while a Collective's is not (a
Collective can accumulate real shared knowledge without ever standing up its own
dedicated coordinator AI). Maps to the regular-file-type Command; doesn't
recursively contain other Logs the way Fleet contains Fleets.

**AI (First mate) resolved as not a Command type at all — a separate primitive,
still fully undesigned.** Unix's real split between processes and files (different
kernel structure, different identity system — PID vs. inode — different lifecycle
and verbs entirely) maps directly: First mate runs on Vessels and reads/writes Logs,
it doesn't statically hold data the way Command types do. Validating detail found,
not invented: standing-orders.md already keeps Captain (#1) and First mate (#2) in
their own Identity section, ahead of Vessel and Fleet — already correct, without
anyone having reasoned through why until tonight.

**Also**: added an IoT/"OS for all your stuff" idea to ideas.md, deliberately
grounded in Home Assistant's existing real traction rather than pure speculation —
the differentiator is the agent layer, not reinventing device-protocol integration
Home Assistant already does well.

**Closing discussion, still just talk, nothing filed anywhere yet but worth
remembering for the next research pass.** Physical actuation deliberately
deprioritized rather than researched deeper — Clarke isn't building driverless-car-
grade actuation himself, that's a real bolt-on for someone else later, not core
research; the existing "Reserved: Physical actuation" stub in standing-orders.md
stays exactly as light as it already is. Separately, confirmed the real target for
"avoid Unix's problems" is the seL4/microkernel lineage specifically — the two actual
problems being avoided: monolithic trust (a bug in any driver/service in a monolithic
kernel can compromise the whole system, since everything shares one fully-privileged
blob) and the total absence of formal correctness guarantees in Unix's own security
model (decades of convention and patches, never mathematically proven). seL4 is the
sharp example — a kernel with a complete, machine-checked proof that its
implementation matches its spec, plus proven security properties. Directly relevant
to the permission-ceiling/uid-gid/trust-boundary work already underway, not a
tangent — worth real research time later, unlike actuation. Implementation language
flagged as likely mattering here too: Rust for memory safety came up as a candidate,
since it eliminates whole classes of memory-safety bugs at compile time without
needing a full seL4-style formal proof — a lighter-weight way to get similar-in-
spirit rigor.

Also discussed and worth keeping: Mojo is honestly closer to a **Distributed
Operating System** than a variant of a single-machine one — Plan 9 and Amoeba are
the closest real historical attempts at "one coherent system spread across
machines," and Single System Image (SSI) is already a named research thread once
touched in this project's own history (ideas.md references an "SSI research pass").
But none of that prior art had a genuine persistent AI agent as the primary
interface — that combination (real distributed-OS coherence *and* a real agent
layer) is the actually novel part. On why no massive company has built this
already: real structural reasons, not just a gap nobody noticed — sovereignty/
local-first design is often directly opposed to cloud-dependent revenue models;
the enabling technology (local models good enough to be a First mate) is only a
couple of years old, so even big companies haven't fully reoriented around it yet;
and large orgs are structurally biased toward extending existing product lines over
first-principles rethinks, which is why this kind of thing has historically come
from individuals or small groups instead (Unix's own reaction against Multics,
Linux, Home Assistant already cited above). Real adjacent research does exist
(distributed systems academia, edge computing, personal-agent startups) — it's the
specific combination, not the individual pieces, that's actually rare.

**One last thing worth closing the session on: a real one-line definition of Mojo,
Google-snippet style ("Unix is a foundational, multi-user, and multitasking computer
operating system").** Took a couple of wrong passes — "sovereign, single-owner" was
right for a bare Fleet but wrong once Collectives are in view, since the actual
invariant isn't "always exactly one owner," it's that composing upward doesn't
absorb what's underneath it (though worth remembering Clarke's own caveat: top-level
overrides in a real Collective might get designed in later — this isn't claimed as
an absolute, unbreakable law, just today's default shape). Landed on:

**"Mojo is a distributed, agent-native, recursively sovereign operating system."**

Matches Unix's own three-adjectives-plus-noun cadence on purpose. Clarke's own
caveat on "agent-native," worth remembering precisely: First mate is the *primary*
interface, not the *only* one — Mojo still needs to work as a completely normal OS
underneath it (files, a terminal, ordinary apps), just with the agent as a real,
deeply-integrated first-class citizen rather than a mandatory bottleneck for
everything. "Agent-native" was confirmed as still the right word for that, just
worth being precise it doesn't mean agent-only.

Floated, not yet decided or applied: using this line for the actual GitHub org
description and a `.github` profile README. Checked tonight — the org
(`mojo-labs-circus`) and the `mojo` repo both already exist for real, with a current
live description ("Private self-hosted AI framework — OS and intelligence designed
together. Your hardware, your data, your AI."); no `.github` profile repo exists
yet. Nothing changed there tonight — a real public-facing edit, held for an explicit
decision rather than made in passing.

## 2026-07-07 (later) — naming-conventions.md written, vision.md gets a pointer to standing-orders.md, then the real work: designing the actual service architecture the whole ecosystem runs on

**Status:** [naming-conventions.md](naming-conventions.md) now exists — the three
registers (nautical/real-word, plain functional English, product branding), plus the
three loose ends flagged earlier today (Muster-ping's hybrid register, "billet" never
formally defined, the mesh still unnamed) carried over as open items rather than
resolved. vision.md's fleet paragraph now links to standing-orders.md, framed as the
POSIX-style formal contract behind the narrative, crediting the real design work
across both of yesterday's sessions plus today's, not just today. philosophy.md left
untouched on purpose — confirmed with Clarke that the "why" hasn't changed, all of
this has been "how." roadmap.md deliberately left alone too, on Clarke's call — it's
what gets worked out next, not pre-edited from tonight's conceptual pass.

**The actual subject of the second half of tonight: what services/repos the whole
ecosystem is made of, long-term, ignoring what gets built this summer.** Started from
Clarke's own framing — "mojo is two things mainly: the fleet and the first mate" —
and a real test for what earns its own service rather than just being a named
concept: a genuinely different **trust boundary** or **lifecycle** than what's already
been separated, not mere conceptual distinctness (which would justify splitting
forever). Landed on this after several wrong turns, corrected in-session rather than
carried forward wrong:

- **mojo-agent** — pure judgment. Task reasoning, memory curation, conversation,
  voice/personality. Owns no state that has to outlive it. Never makes a network call
  for anything fleet-internal, and never talks to another mojo-agent directly — only
  ever queries its own local vessel service.
- **The vessel service** — a real, separate thing, different lifecycle than
  mojo-agent (has to survive it being reinstalled or replaced entirely, same as Keel
  surviving OS reinstalls). Owns identity, the billet table, the roster, permission
  ceilings, the communication protocol (Hail/Sync/Flare/Muster-ping/Report), and — on
  whichever vessel holds Flagship — the routing policy. Only vessel-service-to-
  vessel-service traffic ever crosses the mesh; mojo-agent is always a local client of
  its own.
- **The brain (the log, #25) — corrected mid-session, this is *not* part of
  vessel-service, and probably isn't a running service at all.** First instinct was to
  fold it into vessel-service since Sync/Hail/Flare already move it around — wrong,
  caught when asked to justify it. The brain's lifecycle is scoped to the First
  Mate/Captain's continuity (has to survive even hardware being replaced), not to any
  one vessel's Keel (which explicitly retires rather than transfers when a vessel
  changes hands) — a longer-lived, differently-scoped lifecycle than anything
  vessel-service owns, and content-heavy in a way topology data isn't. Given the brain
  is already planned as plain git-backed markdown (ideas.md), it doesn't need an API
  or a query — it's a shared data store mojo-agent reads and writes directly.
  Vessel-service only touches it to replicate it across vessels, most likely just
  using git's own fetch/push as the transport, not reimplementing sync.
- **A "boundary gatekeeper" and an "action-safety-gate" were both proposed as
  separate services, then both collapsed back into existing pieces once actually
  justified against the trust-boundary/lifecycle test:**
  - The gatekeeper (enforcing the sensitivity judgment, #47, before data crosses to a
    mercenary or charter) folds into vessel-service — Permission ceiling (#10) and
    Permission grant (#24) are already fleet-layer facts in standing-orders.md, not AI
    judgment, so vessel-service already holds what's needed to enforce this; it's an
    extension of an existing responsibility, not a new one. Sensitivity gets tagged at
    write time, when mojo-agent is already doing the semantic work of curating memory
    — enforcement at send time is then mechanical, not a fresh judgment call.
    Reasoned twice over, independently: (1) data locality — only vessel-service holds
    the trust-tier facts the rule needs; (2) separation of duties — the entity
    generating/using content can't be the sole judge of what's safe to release, or a
    reasoning mistake or jailbreak just *is* the leak.
  - The action-safety-gate (confirmation/rollback before consequential actions,
    rationale #50's still-unsolved problem) doesn't become a fourth repo either — its
    mechanism is inherently posture-specific (NixOS generations vs. Foreign having no
    equivalent at all), so it's not one artifact deployed identically everywhere the
    way mojo-agent and vessel-service are. It's a contract each OS-integration
    profile — MojOS, the bring-your-own-NixOS module, the Foreign wrapper —
    implements differently, using whatever that posture actually provides.
- **Net structure:** two portable logic repos (mojo-agent, vessel service) + the brain
  (shared data, not a service) + the MojOS/bring-your-own-NixOS/Foreign platform
  family (bundles the first two onto a host, and is where the action-safety contract
  actually gets implemented per posture).
- **A separate, already-existing third substrate surfaced late: shared project files
  across vessels.** Not vessel-service's job (different mechanism) and not the
  brain's job (that's curated memory, not work files) — this is the "Shared `~` across
  the fleet" idea already sitting in ideas.md, Syncthing-based, already flagged there
  as the right existing tool rather than something to build. Three different
  already-adopted substrates doing three different jobs: Tailscale under vessel
  service, git under the brain, Syncthing for anything flagged shared.

**Two concrete end-to-end traces run to pressure-test the model, both worth keeping as
reference scenarios:**

1. **Phone (Tender, ~zero local capability) asks Jarvis to summarize a doc worked on
   earlier at the desk.** Confirmed the core rule: mojo-agent never makes a network
   call and never talks to another mojo-agent directly — every cross-vessel hop is
   vessel-service to vessel-service; mojo-agent only ever queries its own local
   vessel-service, and reads the brain directly rather than through a vessel-service
   query once that correction landed.
2. **Laptop (a normal, capable Consort — not capability-starved) writing code
   interactively, task needs Flagship's bigger model, Clarke wants continuous
   collaboration, not repeated cold requests.** Surfaced three refinements the phone
   case didn't: (a) offloading to Flagship is a *per-task* routing decision
   (Flagship-directed resourcing, #41) — a Consort doesn't get demoted to a Tender
   just because one job needs more than it has; (b) if the project folder is a shared
   directory, Flagship may already hold its own Syncthing-synced copy, so per-turn
   payloads carry only the conversational turn, not file content; (c) the whole
   multi-turn session should be one open **Hail (#34)** channel for the task's
   duration, not a fresh request-response per message — matching ideas.md's own
   existing note that hailing means "an ongoing live connection back home for the
   whole task, not a one-time payload." The extra attack-surface caution ideas.md
   attaches to hailing is specific to Charters (rented, less-trusted compute) and
   doesn't apply here — Consort and Flagship are both owned, both already fully
   trusted with the whole brain by design.

**Open for next time, nothing here has touched a spec file yet:** none of tonight's
service-architecture conclusions are written into standing-orders.md — this was all
discussion. Whenever Clarke picks this back up: decide whether/how "the vessel
service" folds into standing-orders.md's Fleet section and Deployable artifacts
(#49–53), and it still needs an actual name — register 3 (product branding, alongside
MojOS/mojo-agent/mojo-package), not resolved tonight, flagged rather than guessed at.
roadmap.md is still untouched — "how we go from here" stayed at the conceptual-
architecture level tonight rather than converging on an actual roadmap rewrite.

**Also queued for tomorrow — the vessel type-axis question isn't actually closed,
caught right at the end of the session.** Yesterday's conclusion ("no type axis
needed, vessel varies only by Capability") held for the cases actually tested — the
door lock (never becomes a Vessel at all) and the phone (scales down to Tender via
Capability). But that phone case quietly assumed mojo-agent is *installable* on it at
all — root access, a background daemon, a real Nix target. That's not guaranteed:
a stock iPhone, a locked-down smart TV, a games console can have plenty of hardware
Capability and still be structurally unable to install arbitrary software. That's a
genuinely different axis than Capability (#9) — installability/platform-openness, not
"how much can it do" — and it's not resolved. Whether a closed platform needs an
entirely different kind of client (not mojo-agent-the-Nix-package at all) is the open
question, and it could reopen something type-axis-shaped. Start here next time.

**Honest close, Clarke's own read and it's fair:** decent discussion, not much
concrete out of it. Real artifacts from tonight: naming-conventions.md (new file),
one cross-reference added to vision.md. Everything else — the whole vessel
service/mojo-agent/brain/sanitizer architecture, both end-to-end traces, the
installability gap above — is reasoned-through discussion, not yet written into
standing-orders.md or anywhere durable beyond this devlog entry. Nothing built,
no repo started, roadmap.md untouched. That's fine for a thinking session, just
worth being honest that "concrete" and "good discussion" aren't the same thing, and
tonight was mostly the second one.

## 2026-07-07 — Standing Orders walkthrough begins: vessel type axis resolved (turns out we don't need one), physical actuation and the door-lock case settled, then a long practical pass on how mojo-agent actually adapts to hardware and OS

**Status:** standing-orders.md itself wasn't edited this session — this was discussion,
Clarke wants to walk the document himself first and bring specific things back. Two
concrete corrections are now queued for whenever that edit pass happens (see bottom).

**Naming conventions, first pass.** Before Clarke started his own walkthrough, a quick
read of the existing 53 definitions surfaced three separate registers already in use,
none written down anywhere: real nautical/naval words for fleet-model entities and
actions (Captain, Keel, Flagship, Hail, Flare, Watch bill — always a real attested
term, never an invented portmanteau or forced backronym); plain functional English for
computed properties and the six Fleet section labels (Permission ceiling, the
sensitivity judgment, the routing policy); and product branding for Deployable
artifacts (MojOS, mojo-agent), a separate namespace entirely. Flagged three loose ends
in the current draft worth resolving whenever the naming-conventions file actually gets
written: **Muster-ping (#36)** welds a real nautical word to tech jargon, the only place
two registers got mashed into one word; **"billet"** is used constantly but never gets
its own numbered definition, despite being load-bearing vocabulary; **the mesh (#31)**
is explicitly still unnamed in the document itself. The theme/naming-conventions file
itself is deferred — Clarke wants to walk the doc and bring things first.

**The vessel type axis — walked through concrete future fleet members (phone, TV, car,
fridge, a JARVIS-on-everything-Stark-owns framing) and landed on not needing one at
all, which turns out to validate the original draft rather than contradict it.** The
reasoning chain: considered giving Vessel a `type` field (mojo-agent-capable vs.
peripheral) to cover devices like a smart door lock or fridge sensor that can't run
mojo-agent. Then resolved the door-lock case directly — it fails the existing
definition of Vessel (#11, requires running mojo-agent *and* holding a billet), so it
was never a vessel-type-variant to begin with, it's a tool/skill a real vessel calls,
same category as "chefs and recipes" already in ideas.md. The existing sentence in the
document — "a device isn't a Vessel until Fleet billets it, raw hardware with no role
assigned is outside the model" — already covers this; a peripheral is just the
permanent case of that same rule, not a new concept. Then resolved the phone case the
same way: a phone genuinely could run mojo-agent with near-zero Capability (#9),
computed from thin Hardware inventory (#7), and net out as a Tender (#15) — "no
independent capability, a pure access surface" — which is already the exact definition
of a phone-as-pure-proxy, and rationale #15 already cites the real precedent (thin
client / dumb terminal / remote-desktop client). Net conclusion: the original
homogeneity claim in the preamble was right — vessels vary only in degree (Capability),
not in kind — and the specific line predicting a future need for "a genuine type axis"
for sensors/peripherals is actually wrong as written, since peripherals never become
vessels at all. **Correction queued for standing-orders.md:** fix that preamble
sentence so it doesn't tell future-Clarke to build a type axis that isn't needed.

**Fleet and Collective confirmed as the same recursive container shape (directory),
differing only in one real invariant.** Both organize members without owning their
contents (Fleet holds vessels, Collective holds Fleets/other Collectives), same
recursion at any depth, no special-casing per level — this was already implicit from
the earlier session's "directory" framing but Clarke's question made it explicit one
level down. The one thing that still genuinely separates them, not just naming: a
Fleet is bounded by exactly one Captain + one First mate (the accountability
boundary); a Collective has no single owner over the whole — it's a federation of
already-sovereign Fleets. Same container mechanics, different ownership semantics
underneath, like a directory one user owns versus a mount point aggregating several
separately-owned filesystems.

**Physical actuation (door locks, eventually the car/fridge/house JARVIS-style) —
deliberately deferred, not solved, and for real reasons rather than just running out
of time to discuss it.** Unlike every other axis in the document, it has no clean
existing precedent to borrow (data-permission mistakes are recoverable — a bad read
gets logged and revoked; actuation mistakes can be actual safety failures, a much
harder category closer to industrial fail-safe/dead-man's-switch design than RBAC),
and there's no real device integration yet to design the rules against. **Correction
queued:** add a **Reserved: Physical actuation** section to standing-orders.md, same
shape as the existing Reserved: Collective / Reserved: Shell & Utilities stubs — names
the concern, explicitly out of scope until there's a real device and an actual safety
model, not designed today. Not written yet, just agreed.

**Long practical pass on how mojo-agent itself (not the Standing Orders contract, the
actual implementation) adapts to hardware and OS — this is mojo-agent-repo territory,
logged here because the reasoning happened in this session:**

- **One artifact, not per-tier builds.** The harness should talk to inference through
  one endpoint interface that doesn't care whether it's local or remote — probes
  hardware at startup, computes Capability, either spins up a local
  llama.cpp/Ollama-style server and points the client at localhost, or points the same
  client at Flagship over the mesh. Same Nix derivation everywhere; which path runs is
  a runtime branch, not a build variant. Same real precedent already in ideas.md
  (Ollama/llama.cpp already auto-detect hardware and pick backend/quantization,
  including "run nothing locally").
- **Harness scaling is the same mechanism, one layer up** — most of the harness
  (task-routing, Delegate/Report, holding a log copy) simply never activates on a
  Tender because it's never assigned that kind of work. Config/capability-gated, not a
  second build.
- **Phone should still run full mojo-agent, not a separate thin-client app** — identity
  (Keel), mesh join, and the comms protocol all have to live somewhere; forking a
  second lightweight client just to save phone footprint means maintaining two
  systems instead of gating one. The actual cost (binary size, idle resource use on a
  phone) is a packaging problem, not an architecture fork.
- **Package-size optimization: probe-then-fetch, scoped narrowly.** Real precedent
  (pip/CUDA wheels, Ollama's installer: detect hardware, pull only the matching
  backend) — but apply it to the one genuinely heavy dependency (the local-inference
  backend/model runtime), not the whole harness. On MojOS this is nearly free already
  via Nix's own declarative conditional builds (`lib.optionals` gated on a config flag
  known at eval time); a live runtime probe binary is only needed on postures where
  hardware isn't known ahead of build time (Foreign, a phone app).
- **Foreign posture, concretely:** Nix installs on Linux and macOS (+ WSL2, not native
  Windows — decided that's fine, "fuck windows"). Install Nix → build mojo-agent via
  its flake (identical reproducible derivation regardless of host) → wrap it as a
  systemd unit or launchd plist, since a non-NixOS host has no declarative service
  management of its own. `nix-darwin`/`home-manager` are optional extra layers, not
  required for the minimal version.
- **OS-level protection (generations/snapshots/impermanence) is a gradient, not a
  MojOS-or-nothing binary.** bring-your-own-NixOS keeps generations for free (still
  NixOS underneath); snapshots and impermanence are opt-in, disk-layout-dependent, and
  probably absent unless the user specifically set them up. Foreign has none of it at
  the OS level. mojo-agent needs to actually detect which protections exist on the
  vessel it's running on, not assume a blanket level from the OS-posture label —
  connects directly to rationale #50's already-flagged unsolved problem: irreversible
  actions with external side effects need harness-level judgment regardless of what
  the filesystem protects.
- **Environment awareness must be baked into flake config/flags, never left to system
  prompt or model memory** — a load-bearing fact like "do I have snapshots here"
  can't be something the model is trusted to recall reliably; it has to be a
  deterministic value read at startup.
- **v0.1 scope decided: any NixOS, not MojOS-only.** bring-your-own-NixOS is close to
  free *if* mojo-agent's module is kept properly decoupled from MojOS's
  Btrfs/snapper/impermanence-specific bundle from day one — that discipline costs
  nothing now and is expensive to retrofit later, so build it decoupled regardless of
  MojOS. Foreign genuinely requires separate host-integration work (the
  systemd/launchd wrapper) and stays deferred until after v0.1 ships and there's some
  traction — MojOS becomes "any NixOS, plus extra good," not a separate track.
- **Rebuild-to-match, split into two real cases.** Most environment awareness (current
  hardware capability, which protections exist) should be runtime-checked with no
  rebuild ever required — cheap syscalls/procfs reads, microseconds, checked once at
  startup and cached until a real trigger (rebuild completing, reconnect, a
  hardware-change signal) invalidates it, same pattern Ollama/llama.cpp already use.
  Only package-inclusion-level decisions (whether the heavy inference dependency is
  even present) require an actual Nix rebuild — which is itself one of the few
  genuinely safe self-modifying actions mojo-agent could trigger autonomously, since a
  generation is atomic and reversible.
- **mojo-agent improving the user's own setup, two different trust levels.** Nudging
  toward impermanence or other protections on an existing bring-your-own-NixOS box is
  already covered for free — OS posture (#6) is Queried, drifts without a roster event.
  Recommending a full OS switch is a categorically bigger action (real data migration,
  feels irreversible even if technically recoverable) and belongs in the same
  high-confirmation category as other consequential actions, not treated as a casual
  config nudge.

**Corrections queued for the next standing-orders.md edit pass (not yet made):**

1. Fix the preamble line predicting a future vessel `type` axis — not needed, wrong as
   written.
2. Add a **Reserved: Physical actuation** stub, same shape as the other two Reserved
   sections.
3. Naming-conventions file still pending Clarke's own walkthrough — Muster-ping, the
   undefined "billet" term, and the unnamed mesh (#31) are the three known loose ends
   to fold in whenever that happens.

## 2026-07-06 — HANDOFF: first draft of the document actually written (53 definitions), then renamed system-model.md → standing-orders.md; Clarke stepping away, here's what's next

**Status:** [standing-orders.md](standing-orders.md) exists as a real first draft —
Identity, Vessel (Definitions + General concepts), Fleet (Roster/Permissions/
Records/Communication/Accounting/Policy), Deployable artifacts, Reserved stubs for
Collective and Shell & Utilities, and a full Rationale section, 53 numbered items
total. This is the file the previous entry called "still unwritten" — it got
written in this same session, right after that entry, once Clarke confirmed he
wanted the actual draft rather than more discussion first.

**The rename, and why it's not just cosmetic:** the document was first called
"Mojo Policy" while being written (see previous entry) — functional but not a real
name, the way "Portable Operating System Interface" isn't what anyone actually
calls POSIX. Considered and rejected two real nautical alternatives before
landing: pirate Ship's Articles (rejected — historically per-crew, negotiated by
each ship's own crew, the wrong shape for a document that's *general policy for
how any fleet is constituted*); Articles of War (structurally the right shape —
real, standardized, fleet-wide, not per-crew — but carries real court-martial/
capital-discipline baggage that reads wrong for what this is). Landed on
**Standing Orders** — the real naval term for permanent, general instructions
governing how a fleet operates, as opposed to a one-off specific order. Lighter
than Articles of War, still genuinely fleet-wide rather than per-crew, still real
precedent rather than an invented term — matches how every other name in this
document works (Keel, Billet, Flare, Muster roll, Watch bill are all real words,
not acronyms or backronyms, and Standing Orders keeps that pattern rather than
forcing a POSIX-style acronym we don't actually need).

File renamed `system-model.md` → `standing-orders.md`; internal title and preamble
updated to introduce it as "Mojo's own POSIX." Links fixed in this file and in
ideas.md (two stale cross-references there also had old pre-rewrite numbering —
watch bill was `system-model.md #12`, now `standing-orders.md #38`; chartered
instance was `#7/#8`, now `#16/#17` — the full renumbering from the old 33-item
document to the new 53-item one means **any other file that cites a Standing
Orders number by memory should be treated as suspect until checked** — roadmap.md,
vision.md, philosophy.md haven't been audited for this yet).

**Checked whether MPI or Kubernetes' CRI/CNI/CSI could implement fleet control
directly, rather than designing it from scratch — same spirit as the earlier
Slurm/Petals/exo/9P pass.** Both are real, mature, and both solve a different,
lower layer than what Standing Orders addresses. MPI assumes membership is fixed
at job-launch time, no trust tiers, no tolerance for a node disappearing
mid-job — it's for fast messaging between processes that are *already* running
on nodes that are *already* trusted. CRI/CNI/CSI are plugin interfaces *within*
Kubernetes' own control plane, assuming a node has already been admitted and
scheduled by something else. Neither has any concept of "some of these machines
are mine, some are rented, one's a mercenary I don't trust at all, and there's
exactly one accountable human" — which is the actual problem Standing Orders is
for, and part of why nobody's built this already (also checked last entry via
Urbit — the sovereign-personal-AI ambition is real and pursued, the specific
billet/trust-tier mechanism isn't). Worth reusing narrowly later, as
implementation detail rather than fleet infrastructure: MPI as the wire protocol
under Delegate (#32) specifically for Combined-owned resourcing (#42), once
billeting's already resolved and a job is splitting across already-trusted,
already-connected Consorts; CRI only if mojo-agent itself needs to run sandboxed
workloads on one vessel internally.

**Next steps, for whoever picks this up (Clarke, back from work):**

1. Read [standing-orders.md](standing-orders.md) fresh and mark what's wrong or
   missing — it's a first draft Clarke asked to be checked, not a finished one.
2. Known, already-flagged gaps, not forgotten: the mesh (#31) still has no name;
   Permissions (#24) states the principle but isn't a full per-billet ×
   per-operation matrix; Collective and Shell & Utilities are reserved but empty;
   `type` heterogeneity for vessels is a named future seam, not designed.
3. Audit roadmap.md/vision.md/philosophy.md for any reference to the old
   system-model.md numbering (1–33) or the old "Mojo Policy" name — none checked
   yet, and the renumbering makes any such reference wrong until verified.
4. Once the draft is dialed in, Collective is the next real thing to build out
   (deliberately deferred until Fleet is solid) — the recursive shape and the
   Fleet-vs-Collective definition are already settled, nothing inside it is.

## 2026-07-06 — Continuing the tier work: Keel, a real permissions split, generational roster, Collective replacing a fixed Armada tier, and checking whether any of this already exists (short answer: the ambition does, Urbit; the mechanism doesn't)

**Status:** system-model.md is still unwritten as of this entry — every session so far has stayed in discussion because Clarke explicitly wants to build the actual rewrite together, piece by piece, not receive a batch-drafted file. Next real step is starting that write, framed as "Mojo Policy" (see below) — not yet begun.

**Keel** — the tier-1 gap surfaced by asking "what makes this the same vessel across every other change." A persistent per-vessel identity, established once (at first join), surviving OS reinstalls and billet reassignment, retired rather than transferred if the vessel is ever Removed. Named for a ship's keel — traditionally its true birth, separate from the christening/renaming that can happen later and doesn't change what ship it is. Real mechanism, not just metaphor: this is what Tailscale's own node identity already has to be (a keypair, not a hostname/IP) for the mesh to survive a reinstall or rename at all — Keel doesn't invent anything, it names what already has to exist. Technical parallel: an inode number, stable across directory renames, distinct from the path currently pointing at it.

**Permissions turned out to split across both tiers, not sit in one.** Two different questions were being asked as if they were one: *what could this vessel ever be trusted with* (fixed by Ownership + Attestation alone, no fleet context needed — a rented-open box can never legitimately hold genuinely sensitive data regardless of what role you'd assign it) versus *what is it actually allowed to do right now* (a consequence of its current billet, moves automatically if the billet changes). First is tier 1 — a new **Permission ceiling** entry, Derived alongside Capability, computed from Ownership/Attestation rather than hardware. Second is tier 2 — promoted to its own visible **Permissions** subsection under the fleet, not buried in Roster's prose, since Clarke was right that this is close to the whole ecosystem's trust design, not a footnote. Worth getting the technical parallel right too: this isn't classic Unix rwx bits (those are *stored*, static data) — it's closer to a capability-based or MAC system (SELinux-context-style), since the ceiling is computed from context rather than a flag you chmod.

**Roster becomes generational, Nix-style, not just logged.** Connects to reversibility as a value Clarke wants generally: rather than the billet table being one current-state snapshot, it's a sequence of generations — same mechanism #20 already established for one machine's system config (atomic, reversible), just applied one level up to fleet membership instead of packages. A bad Switch-Flagship becomes something you can roll back from, not something you're stuck with.

**Mount points sharpened: the test is identity + fleet participation, not ownership.** Charter isn't a mount despite being rented — it gets a temporary Keel, gets billeted, joins the mesh, participates in signal flags. Mercenary is the only real mount: no Keel, no billet, no mesh membership, a bare external call referenced only by the routing policy. Ownership (tier 1) and mount-vs-member (tier 2) are separate axes, not the same fact viewed twice.

**Content genuinely splits into two different kinds, and one of them was filed at the wrong tier.** *Reflected* content (abridged log, manifest, decoy cargo, full hold) is never standalone — it only means anything as a view onto #10, correctly tier 2. *Originated* content (task history, uptime/connectivity history) is authored by the vessel itself, about itself — genuinely standalone, which means it was wrong to file the watch bill under tier-2 Accounting. Consolidated into one tier-1 concept, **the local ledger** — real `wtmp` precedent, which already mixes boot events and session events in one log format. Then Clarke connected it further: the ledger is also where #14's "records locally instead of writing to #10 directly" behavior actually lives — pending-reconciliation state is a third kind of entry in the same ledger, not a separate mechanism. Real parallel sharpens to a write-ahead log / local git commits not yet pushed — which is #5's already-chosen parallel, just now clearly the same substrate as the ledger rather than a second idea living next to it. Tier-2 Accounting stops being a storage location at all — purely the collation mechanism, a fleet-wide view built by querying every vessel's own ledger.

**Live-state migration (mid-task, moving a running process between vessels) — deliberately not solved, checkpoint/resume instead.** Same honest tradeoff reasoning as Cancel: no execution engine exists yet to migrate anything within, and the mature prior art for true live migration (Sprite, LOCUS, MOSIX) is exactly why that lineage stayed research-grade rather than production-standard — fragile even when it works. Checkpoint/resume (save state periodically, pick up from the last checkpoint if interrupted) is the cheap, well-precedented version Clarke wants instead, and it's not a new mechanism either — same shape as #14, applied to in-flight task state instead of log writes.

**Heterogeneous vessel types — real, confirmed by two independent cases, still deferred.** Sensors/peripherals (vision.md's "connects to everything: your devices, your work, your life" implies something that can't run mojo-agent and holds none of #10 — closer to a Unix device file than a regular file) and, separately, Collective-level role-filling AI agents that aren't anyone's First Mate (already in ideas.md under Collectives, just not connected to this until now). Both real, neither in scope for v0.1 — noted so the eventual `type` axis has a real seam to slot into rather than needing a retrofit.

**Link count isn't always 1 — corrected mid-conversation.** True within one fleet (a vessel holds exactly one current billet in its own roster). Not true across a Collective boundary: Armada's own existing language ("references fleets without owning or absorbing their contents, same as a directory entry pointing at an inode rather than duplicating it") was already describing a hard link and nobody had named it as one until now.

**Armada isn't a fixed Tier 3 — it's "Collective," genuinely recursive.** Real Unix directories nest to arbitrary depth using the same rules at every level, no different name per depth. Clarke confirmed sub-collectives within collectives should be possible, which means a fixed "Tier 3 = Armada" was the wrong shape — there's one recursive concept, a Collective, whose members can be Fleets, other Collectives, or standalone task-scoped agents, to whatever depth is actually needed. Armada is probably just the nautical/colloquial name for a Collective, not architecturally distinct from one. Also sharpened the base case precisely: a Fleet is exactly one Captain + one First Mate (+ vessels); a Collective is multiple Fleets, each still fully sovereign, plus possibly those standalone agents.

**Named what system-model.md itself actually is: our POSIX, not a metaphor for one.** The real Unix precedent for "an abstract behavioral contract with multiple concrete implementations, rationale kept separate from spec" is the VFS layer (inode/file/dentry/superblock operations any filesystem driver implements — ext4 and 9P both satisfy the same interface without knowing about each other) plus POSIX itself (specifies behavior, not implementation; keeps rationale in a non-normative annex, exactly the shape "Technical parallels" already had before this session). Decided: system-model.md *is* our POSIX-equivalent — the contract any vessel/fleet must satisfy, independent of implementation (MojOS / bring-your-own-NixOS / Foreign are three drivers against one interface, which is the actual mechanical reason the OS-posture axis works the way it does). Landed on calling the contract itself **Mojo Policy** and anything satisfying it **Mojo-compliant** — explicit that this is an original spec borrowing Unix's proven *method*, not a claim of literal Unix/POSIX compliance or lineage.

**Checked whether any of this already exists rather than assuming it doesn't.** The broad ambition — personal, sovereign AI, own your own server — is real and actively pursued, most visibly by Urbit (currently marketing itself explicitly as "Sovereign Intelligence," self-hosted personal server/OS/identity system). Worth Clarke actually reading Urbit's Azimuth identity spec directly at some point, as the closest real system that's had years of actual users to find its cracks — not as a template to copy. But the actual mechanism is different: Urbit's ship hierarchy (galaxy/star/planet/moon/comet) is an address-space/sponsorship system, nothing like billeting or a trust ladder. The one real overlap is identity — Azimuth is genuinely close to Keel (a portable identity independent of the physical hardware running it). No existing named system turned up combining role/billet assignment + an owned/rented/mercenary trust ladder + a Unix-VFS-shaped contract the way this one does. Conclusion: the mission has prior art, the specific synthesis doesn't — worth being precise about which claim that actually is.

## 2026-07-06 — The owned-charter question resolves by finding the model was missing a layer: vessels are files, fleets are folders, roles are directory entries not inode properties. Full session, system-model.md rewrite starting next

**Status:** system-model.md is still the pre-session version as this is written — the
rewrite (renumbering from 1, restructured around the tiers below) starts right after
this entry. Everything here is settled and ready to write in, not still open, unless
marked otherwise.

**The actual unlock:** picking up last session's stuck "owned charter" question (a
headless consort that behaves like a charter) by building a real attribute table
across every kind of vessel in the network — ownership, OS, head/headless, state
held, independence, cardinality — and finding that the "owned charter" row was
attribute-for-attribute identical to an ordinary headless consort. The only thing
that had felt different was *how* it gets used (directed via task-routing rather than
sat at), which was never an architectural property to begin with.

That dissolved the immediate question but exposed a bigger one: the model had been
putting "what a vessel intrinsically is" and "what only exists once vessels are
combined" in the same flat list. Explicitly splitting into tiers, Unix-style:

- **Tier 1 — the vessel (a file).** Properties true of one machine alone, no fleet in
  view.
- **Tier 2 — the fleet (a folder).** Properties/mechanisms that only exist because
  multiple vessels are related to each other.
- **Tier 3 — the armada (a folder of folders).** Multiple sovereign fleets. Not built
  out — matches the existing ideas.md framing ("a fleet is the file, an armada is the
  folder") one level up, confirming the recursion is real rather than forced.

**The sharpest consequence of the split: role is not a tier-1 property.** Flagship /
Consort / Tender / Charter were being treated as things a vessel *is*, when they're
really things a vessel gets *assigned* — a directory entry (name → inode) is a
different thing from the inode itself. Named the assignment mechanism **Billet**
(real naval term: an assigned post, reassignable without the thing holding it
changing). Two genuinely different mechanics under one name, not one:

- **Flagship** is a true pointer over the owned pool — reassignment is exactly
  primary-replica promotion (already #4's chosen technical parallel, just not
  previously connected to "here's literally how you change who's flagship"): pick a
  consort already holding a full write-through copy of the log, flip which one
  accepts writes, demote the old one.
- **Consort/Tender** billeting is mostly capability-derived from tier-1 hardware
  eligibility, with a real choice only at the capable end (a laptop *could* be run
  thin).
- **Charter** billeting isn't a repoint at all — a fresh temporary allocation
  (`mktemp`, not `ln -sf`), created and destroyed with the rental. Still occupies a
  billet slot for its lifespan though (a tmp file still has a directory entry while it
  exists) — corrected an earlier draft of this reasoning that had wrongly concluded
  charters sidestep billeting entirely.
- **Mercenary** gets no billet at all, ephemeral or otherwise — not provisioned by the
  user, no lifecycle to track, an external mount referenced only by the routing
  policy per call. The one real "is this a vessel" boundary case, and it isn't one.

Consequence: a device only counts as a vessel once it holds a billet. Raw owned
hardware with no role assigned yet is outside the model, not a vessel-in-waiting.

**Fleet formation is incremental, one vessel at a time — not a bulk declaration.**
Walked into, then partly out of, an over-collapse here: first pass concluded
Commissioning was just "Enlist with nothing to sync from," but that's wrong —
Commissioning bootstraps the log/roster/watch-bill from genuinely nothing (no
mojo-package exists yet to sync), where Enlisting always has an existing flagship's
log to pull from. Real enough difference to keep as two named operations sharing one
step (billet the vessel), not collapse into one. Full flow, flowchart written during
the session (not yet transcribed into system-model.md):

Commission (once, bootstraps everything from empty, first vessel becomes flagship
near-by-necessity) → Enlist (repeatable, syncs from existing flagship, billeted as
Consort/Tender per eligibility + choice) → Remove (flagship case must Switch-Flagship
first — no valid state has zero flagship, not even momentarily) → Switch-Flagship
(requires a caught-up replica candidate, near-zero-downtime cutover) → Charter/
teardown (ephemeral side-loop, doesn't touch the persistent roster).

**Tier 1's actual property set** — went through several wrong drafts before landing
here, each correction from Clarke catching a real category error:

- Attestation/TEE isn't a property every vessel has a value for — it only answers "can
  I verify a third party hasn't tampered with this," a question that doesn't exist for
  owned hardware (you already have root). Not conditionally-null, structurally
  undefined on the owned branch. And even on the rented branch it's not a cached fact
  — real remote attestation is a live per-session cryptographic challenge-response,
  never stored.
- "Hardware capability" isn't a stored property either — the stored fact is the raw
  inventory (CPU/RAM/GPU/storage), and "capability" (flagship-eligible? full-log-
  eligible?) is inference computed from that inventory at decision time, by whichever
  tier-2 policy is asking. Never stored as its own value.
- OS posture isn't fixed (distro-hopping is real) — only Ownership and Head/headless
  survive as genuinely declared, roster-level facts, changed only by a deliberate
  operation (Commission/Enlist/Remove-family, or an explicit reconfigure). Everything
  else drifts under you without any roster event occurring, so it has to be read
  fresh, not cached.

Landed on treating the live-queried stuff as **Plan 9-style — every resource,
including ones on other machines, is a file you mount and read** — which turned out
not to be a new idea to reach for but a confirmation of groundwork already laid: the
project's own SSI research pass (below, same day) already picked 9P/`v9fs` as the
concrete real infrastructure for "unify the fleet's shared records as one namespace,"
rejecting Slurm/Petals/exo for good specific reasons. The vessel-as-file framing
converges on exactly the same answer from a completely different angle, which is a
real consistency check, not a coincidence to wave off.

Final tier-1 shape:

- **Fixed** (roster-level operation required to change): Ownership (owned /
  rented-sealed / rented-open), Head or headless.
- **Live-queried, `/proc`-style** (read fresh, matches #11's already-existing
  on-demand-ping precedent rather than inventing a new mechanism): OS posture,
  hardware inventory, connectivity/uptime.
- **Derived, never stored**: capability, computed from the above by tier 2 at decision
  time.

**A vessel is authoritative for its own history; others read it, they don't track
it.** Real `wtmp` precedent (a machine logs its own boot/connect events locally,
`last` just reads that local record) — extended past uptime to the watch bill itself:
decentralized into one append-only log per vessel, with "the watch bill" as a
fleet-wide view being a runtime collation over those, not a centrally-written ledger.
Safe to decentralize in a way the log (#10) isn't — task-history entries are pure
append, one writer per source, no possible conflict, an actual grow-only CRDT unlike
the log (which explicitly isn't one, per #14's own technical parallel, because edits/
deletions are a deliberate feature). Should carry the same human-review/redaction
capability as the log for the same reason — task history can itself be sensitive
(usage patterns, timing) — append-only by default, deletable only by deliberate human
action. Collation is necessarily partial when some vessels are currently unreachable
— same accepted tradeoff #14/Coda already cover for the log, not a new gap.

**Sync is pull, not push — matches the git parallel already chosen for #5, doesn't
need a new one.** Nobody pushes into a git clone; a consort periodically pulls while
connected, pulls immediately on reconnect. Avoids the flagship tracking subscriber
state or buffering updates for a laptop that's been asleep for three days. But pure
periodic pull is too stale for vessels actively collaborating at the same time — fixed
with a second, much lighter signal-flags operation: **Flare**, a payload-less
pub-sub invalidation broadcast ("something changed, go pull now"), distinct in kind
from the sustained back-and-forth of full signal flags. Real precedent: WebSub-style
cache invalidation, notify and fetch as deliberately separate mechanisms. Scales flat
because the publisher never tracks subscriber count or state — same reason it should
also generalize cleanly to Armada later (an invalidation broadcast + scoped pull), once
Armada has its own answer for what's actually shared across the sovereignty boundary,
which it doesn't yet and isn't being solved now. Correctness never depends on the
flare arriving — periodic pull is the actual floor, flare is a latency optimization
only. The "working at the same time" concurrent-edit case is already the git-merge/
human-review case #14 established, not a new mechanism.

**Open, not yet resolved:**

- The mesh itself still has no name. "Halyards" and "home waters" were both floated
  and both rejected (waters framed it as a place rather than fleet equipment;
  Halyards implied equipment each vessel individually carries rather than a shared
  namespace vessels join — closer, but Clarke didn't want it). Leave unnamed
  (`the mesh`, no proper noun) in the rewrite rather than force a name that isn't
  right yet.
- Cancel (aborting a task mid-flight) — floated twice, parked both times as
  premature, nothing in the model currently promises it as a feature.
- Armada's own shared record (what Flare would actually be invalidating one level up)
  — genuinely later work, roadmap already defers Circus/armada-scale work past the
  current build phase.

## 2026-07-06 — HANDOFF: install-list collapsed from 8 named types to 3 parameterized kinds; new "owned charter" question opened, unsolved, stopping here for the night

**Status for a fresh session picking this up:** [standing-orders.md](standing-orders.md) is
**unchanged** this session — nothing below was settled enough to write in yet.
This entry supersedes the install-list part of the previous handoff (below);
the rest of that entry (naming pass through #21, resolved principles) still
stands, don't re-litigate those.

**What happened:** started working the draft 8-item install list one at a
time as planned, but the real move turned out to be simplifying the list
itself before naming any of it. The 8 items were enumerating every
role × OS × head/headless combination by hand instead of separating "what
kind of install is this" from "what flags does it take." Collapsed to three
install *kinds*, each parameterized rather than hand-listed per combination:

1. **Commissioning** — stands up a brand-new flagship. Always NixOS, always
   headless (forced by role). Deploys the full #21 bundle. Happens once ever
   per fleet, no flags — a singleton event.
2. **Enlisting** *(tentative name)* — a consort joining the fleet. Deploys
   #19, wrapped per OS (`NixOS → MojOS` / `foreign-OS → Jury-rig`). One free
   flag: desktop layer on/off (head/headless) — the only install where this
   is actually a user choice rather than forced by role.
3. **Chartering** *(reusing the existing #7/#8 term as the verb for the
   install event itself)* — a temporary rented instance spinning up. Deploys
   #19 wrapped per whatever the provider supports. Always headless, never
   joins the muster roll (#11), torn down after. Data mode (#15/#17/#18)
   decided separately by the sensitivity judgment (#32), not part of the
   install.

**Foreign-OS wrapper artifact (#22, not yet written into the file):**
tentatively named **Jury-rig** — the real nautical term for keeping a ship
running with an improvised, non-purpose-built repair using whatever's on
hand, as opposed to a proper dockyard-built hull (#20). Liked but not
confirmed as final.

**The actual unsolved problem, worth real thought before resuming:** three
threads opened in the same breath, tangled together, none resolved:

- Is head/headless for Enlisting really "just a flag" on top of one
  identical install, or does it deserve its own build/profile (a genuinely
  different NixOS configuration for a no-desktop server-style box vs. one
  meant to be sat at)? Old #23 (pre-restructure) had modeled it as a
  different profile, not a flag — that tension never actually got resolved,
  just deferred by the collapse-to-3 move.
- Same question on the Commissioning and Chartering-on-NixOS sides:
  floated the idea of a *customized* NixOS build specifically for each of
  those, rather than treating them as MojOS-with-different-flags. Not
  explored at all yet — just named as worth exploring.
- **The big one:** a possible new class, "owned charter" — a headless
  consort that behaves functionally like a charter (task-scoped manifest
  #15 per job, no persistent #13 sync, no independent-offline-operation
  guarantee) rather than a full consort. Surfaced from Clarke's own framing
  ("utilise their entire compute for tasks just like you would with a
  charter") — if this is real, it's not just a flag on Enlisting, it may be
  a fourth install kind or a re-carving of the #5/#6 role boundary itself
  (#5 consort's definition currently requires "independent capability even
  while disconnected" — a headless owned-charter-style consort might not
  need that guarantee at all). This is the thread most likely to actually
  reshape the Roles section, not just the install list — dig into it first
  next session, the other two threads probably fall out of whatever this
  one resolves to.

Nothing named here is final. Pick up by actually sitting with the owned-
charter question before writing anything into system-model.md.

---

## 2026-07-06 — HANDOFF: naming pass through #21 done, "The deployable package" mid-restructure, unsolved

**Status for a fresh session picking this up:** [standing-orders.md](standing-orders.md)'s
one-at-a-time naming pass has #1–21 fully done (name + technical parallel,
written into the file). #22–24 are mid-restructure and NOT yet written — this
is the open problem to keep pulling on, not a finished thing to just transcribe.

**Workflow to keep using:** one definition/item at a time. For each: state the
plain definition, propose a fleet/pirate name, propose a technical parallel
(real prior art), discuss until settled, then write both into system-model.md
immediately — name in the main body next to its definition, technical parallel
in the "Technical parallels" section at the end, same number. Don't batch
multiple items ahead of confirmation.

**The specific unsolved problem:** "The deployable package" section was
conflating two different things — *what artifact exists* vs. *when/how it gets
installed onto a specific target*. Splitting into two: the package section
keeps pure artifacts only (#19 mojo-agent, #20 MojOS, #21 mojo-package —
narrowing #21 to just the bundle itself, no longer describing the flagship-
bootstrap event); a new "Installs" section gets added for the deployment events
themselves. One new artifact still needs adding: a foreign-OS wrapper (#19
wrapped as a non-NixOS host's own native service — systemd unit, launchd plist)
— not written yet.

**Key resolved principles to carry forward, don't re-litigate these:**

- "Headless" (no one sits at it) is fixed *by role* for #4 (flagship, always
  headless) and #7/#8 (charters, always headless) — but for #5 (consort) it's
  NOT fixed by role, it varies by how a given consort is actually used. A
  spare PC nobody sits at is still a consort, just a headless one. This gives
  consorts a real 2×2: NixOS×head, NixOS×headless, foreign-OS×head,
  foreign-OS×headless — all four legitimate.
- The flagship stays NixOS-only, no foreign-OS flagship option — it's the one
  role where real atomic rollback matters most, so that guarantee shouldn't be
  optional there.
- No separate builds per host OS — one build (#19) everywhere, matching the
  microkernel principle already established. Only the *wrapper* differs
  (NixOS module vs. systemd unit vs. launchd plist) and the agent's *runtime
  behavior* differs (via the capability-tier detection principle from earlier
  — full NixOS rollback / partial nix-darwin / no native rollback — the agent
  scales its own caution accordingly). Not a build-time fork.

**Current draft install list (8, not yet in the file, still open to change):**

1. New flagship → mojo-package (#21)
2. New consort, NixOS+head → full MojOS
3. New consort, NixOS+headless → background/pooled compute
4. New consort, foreign-OS+head → existing Mac/Ubuntu, used directly
5. New consort, foreign-OS+headless → existing box, background-only
6. Charter, NixOS-headless
7. Charter, foreign-OS-headless (provider doesn't support NixOS)
8. Charter, full-hold (the #18 fine-tuning case)

Explicitly not solved: none of these 8 have names yet, the foreign-OS wrapper
artifact isn't written, and #21/#22's actual text in system-model.md hasn't
been edited to reflect any of this yet. Pick up right here.

---

## 2026-07-06 — Naming pass: through #16; caught a real gap on hailing-permission along the way

Continuing the one-at-a-time naming pass on [standing-orders.md](standing-orders.md).
Through #16 so far: Vessel, Flagship, Consort, Tender, Sealed charter, Open
charter, Mercenary, The log, The muster roll, The watch bill, The abridged log,
Beyond hailing range, The manifest, Hailing — plus a real technical correction
along the way (#14's reconciliation isn't a conflict-free CRDT, since real
edits/deletions are a deliberate feature via the memory-review/veto UX, not
just append-only growth — it's genuinely git-merge-shaped, conflicts included,
which routes naturally to human review anyway).

One design gap surfaced that's worth its own note: #16 (hailing) as currently
written lets any #7/#8 query the flagship for more context mid-task
unconditionally, but that shouldn't be a blanket yes. Hailing means the charter
holds an open, live connection back to the flagship for the whole task, not a
one-time sealed payload — meaningfully more attack surface than a manifest-only
charter, since a compromised charter with an open channel can keep pulling more
data, where a manifest-only one can only ever leak what it was already given.
Should be a per-charter permission decided at charter time, same kind of call
as the tier-routing policy itself. Landed on the bigger idea this is really an
instance of: mojo-agent deployments (#19) should support real deployment-time
config/flags controlling what a given instance is allowed to do, not just this
one case — captured in [ideas.md](ideas.md), general shape still open.

Going through [standing-orders.md](standing-orders.md) one definition at a time, settling
a name and a technical parallel for each before moving to the next. Captain,
First mate, and Vessel settled so far (Captain: root of trust, not an account —
single-tenant by design, no login concept needed, and deliberately singular per
fleet for the same reason #4 is singular, an accountability invariant that's
also the real boundary where Collectives picks up rather than a gap in this
model. First mate: process migration / Erlang-actor-style location-transparent
identity. Vessel: generic compute host abstraction — had to fix its definition
mid-pass, since "capable of running or hosting the AI" wrongly excluded tenders,
which only ever access the AI running elsewhere).

Once the full naming pass is done, vision.md needs a real rework to actually use
this settled terminology and reflect what system-model.md nailed down more
precisely than vision.md's original prose did (the mandatory-flagship reasoning,
the sensitivity-based data-sharing rule, mojo-package's actual bootstrap-only
composition, MojOS's corrected defining property). Not doing that yet — flagged
here so it doesn't get lost, not attempting it mid-naming-pass.

---

## 2026-07-06 — Checked whether real SSI-family software could implement the model directly: one clear yes (9P), one "not yet" (exo), two no's (Slurm, Petals)

Follow-on to the SSI/microkernel/scheduler parallel below: had it actually
researched whether Slurm, exo, petals, and Plan 9/9P could be adopted as real
infrastructure rather than just kept as conceptual reference, against the
specific pieces of [standing-orders.md](standing-orders.md) each was proposed for.

**Slurm** (proposed for #33, the tier-routing policy) — mature, GPLv2, genuinely
light enough to run on a handful of machines. But it's daemon-only, not
embeddable — you'd run its actual services and submit jobs through its own
CLI. Its whole interaction model is multi-tenant batch scheduling for a shared
research cluster; nothing in it maps onto "charter an ephemeral cloud box" or
"call a mercenary API." HTCondor is the same story. Verdict: keep the
scheduling *concepts* (resource matching, backfill scheduling) as reference —
this is still worth digging into for #33's actual design — but don't adopt the
software itself.

**Petals** (proposed for #28, pooling compute across owned devices) — MIT
licensed, but no release in ~3 years with open issues sitting unaddressed, a
real stagnation signal. Even alive, it was built for internet-scale volunteer
swarms (BitTorrent-style, public contributor pool) — backwards architecture for
"a private handful of my own devices" regardless of maintenance state. Skip.

**exo** (also proposed for #28) — Apache-2.0, genuinely thriving (46k+ stars,
shipping into 2026, a real company behind it), and architecturally almost
exactly right: peer-to-peer auto-discovery, no master/worker, splits a model
across devices by real-time topology/bandwidth. The blocker: GPU acceleration
currently only works on Apple Silicon via MLX; Linux is CPU-only, GPU support
still "under development." On the actual fleet (PCs + a laptop), exo today
would mean CPU-only sharding — probably slower than just running a smaller
model on one GPU alone. Right shape, wrong moment — watch it, don't build on it
yet.

**Plan 9 / 9P** (proposed for #2/#10, the unified-identity/shared-record
property) — this is the one worth actually prototyping. Not "install Plan 9" —
9front is the maintained fork, but it's a whole alternative OS, not something
to bolt onto NixOS. The useful piece is narrower and far more mundane: the 9P
protocol is already in the mainline Linux kernel as `v9fs`, used in production
today for QEMU/VM folder-sharing and WSL. Unifying the fleet's shared records
(#10 the log, #11 the roster) as one namespace over 9P is buildable today with
infrastructure that already ships in the kernel — no museum-piece OS, minimal
new dependency surface. Strongest lead of the four, by a clear margin.

---

## 2026-07-06 — Built the full system model (33 definitions, system-model.md); it turns out to be a Single System Image OS in disguise

Long session working out, in plain language before any metaphor got attached,
every entity/record/rule/deployment-shape/execution-tier the mojo ecosystem
actually needs. Landed the whole thing in a new file, [standing-orders.md](standing-orders.md)
— 33 definitions across six sections (Roles, Records, Context sharing, The
deployable package, Execution tiers, Routing). The naming pass (attaching the
fleet/pirate vocabulary to each number) hasn't happened yet — this entry is the
reasoning that got the model itself locked, not the naming.

Threads that resolved along the way, roughly in order:

**The flagship is mandatory, not optional, and here's why.** CAP theorem forces
the issue: if more than one device can accept writes to the shared record, you
either need consensus (Raft/Paxos-style) or you accept permanent risk of
disagreement. This design sidesteps that entirely by having exactly one canonical
writer — the flagship (#4) — with everything else either a full independent
device (a consort, #5) or a pure access point (a tender, #6). Disconnected
devices append locally and reconcile back in on reconnect (#13/#14), which only
works because there's one authority to reconcile *into*. Consensus algorithms
would only become relevant for a different problem — flagship high-availability/
failover across two physical machines — parked as a future idea in
[ideas.md](ideas.md) under Circus, specifically framed as an accessibility
feature for people without a dedicated always-on machine.

**Two axes were getting tangled and needed separating.** Fleet composition
(what you physically own — flagship/consort/tender, any number, any mix) is not
the same axis as voyage/job sizing (how much resourcing a given task gets —
solo, flagship-routed, pooled, fan-out, chartered, mercenary). Conflating them
was making early naming attempts (e.g. calling a physical laptop a "sloop," the
same word vision.md already uses for job size) collide with themselves.

**Data-sharing rules got more honest.** Originally: attested rental (#7) gets
real data, plain rental (#8) gets placeholder-scrubbed data, fixed by tier. Cor-
rected to: what a task actually needs, genuine (#15) or scrubbed (#17), should be
decided by how sensitive that specific data is (#32, "the sensitivity judgment"
— a new, currently-unsolved piece of judgment, same category of hard problem as
the tier-routing policy itself), not by tier alone. Non-sensitive genuine data
can go anywhere, including mercenaries (#9) — though #7/#8 stay preferred over #9
regardless, mercenaries remain the rare exception. Sensitive data may only ever
pair with #7. Whole-record transfers (#18, e.g. fine-tuning) are the one
exception that isn't sensitivity-gated — always #7, never #8, no matter what.

**mojo-package's actual composition and moment of use got nailed down.** It's

# 19 (the harness) + #10 (the log) + #11 (the fleet roster) together — and

crucially, it's a *bootstrap* artifact, not something in continuous use. A new
flagship needs the whole bundle (there's no other live flagship to sync from
afterward). A new consort just needs the bare harness + MojOS, then syncs its
own partial copy from the existing flagship afterward. A chartered instance
normally just needs the harness plus whatever scoped context the task calls
for — except in the fine-tuning case, which needs the full bundle same as a new
flagship does.

**MojOS's own definition got corrected.** Its headline property isn't "looks
good, nice daily driver" — any OS can claim that. It's that system changes the
agent makes are staged, validated, and atomically reversible, because the whole
system is declared in Nix. Aesthetic/desktop is secondary to that, not the
point of it. Confirmed separately that mojo-agent has to be fully self-
sufficient without MojOS (interface, memory, reasoning all intact on any host,
even macOS) — the one real gap off-MojOS is system-mutation safety, which is a
spectrum (full NixOS rollback > partial nix-darwin rollback capped by Apple's
SIP > no native rollback at all) rather than a binary.

**The last thing that fell out, unprompted, is maybe the most useful:** the
whole model turns out to be a recognized, well-studied shape — a **Single
System Image** distributed OS. Plan 9 from Bell Labs is the closest spiritual
ancestor (every resource accessed the same transparent way regardless of which
physical machine holds it); Kubernetes is the closest modern concrete example
(schedules across heterogeneous nodes, presents one coherent cluster). #19 next
to its host-specific wrappings (#20/#23/#24) is a microkernel-and-drivers
shape, which is exactly why the harness has to be self-sufficient — a
microkernel that depended on one specific driver wouldn't be a microkernel.
And #32/#33 (the sensitivity judgment, the tier-routing policy) are structurally
a mandatory-access-control engine and a scheduler, respectively — both
well-studied problems with decades of prior art to draw on rather than invent
from scratch. Worth treating as a real research lead, not just a nice analogy.

---

## 2026-07-06 — mojo-agent runs on any Unix system via Nix (not NixOS); MojOS's real job is safe system mutation, not looks

Landed on a principle worth following deliberately from here on: mojo-agent isn't
a MojOS-exclusive thing, because Nix (the package manager) and NixOS (the whole
operating system) aren't the same thing. Nix runs standalone on any Linux distro
and on macOS via nix-darwin — a machine doesn't have to *be* NixOS to run a
Nix-packaged agent on it. So mojo-agent has to be self-sufficient by design: it
brings its own interface, memory, and reasoning regardless of host OS, and doesn't
depend on MojOS for anything functionally required (already implied by the
multi-distro idea sitting in [ideas.md](ideas.md)).

That forced a correction to what MojOS's own defining feature actually is. "Looks
good, nice daily driver" isn't it — any OS can claim that. The real headline
property: system changes the agent makes are staged, validated, and *atomically
reversible*, because the whole system is declared in Nix. That's the thing nothing
else gives you by default, and it should lead MojOS's definition, not trail it.

Follow-on: the agent needs to know, at runtime, which safety tier it's actually
running under, and behave differently depending on it — not a blanket "MojOS: can
mutate the system, anywhere else: can't." Three real tiers, each with a genuinely
different rollback guarantee: full NixOS (MojOS) gives real atomic rollback for
anything it touches; nix-darwin on macOS gives partial declarative rollback,
capped by whatever Apple's System Integrity Protection still allows to be managed
that way; plain Nix on Ubuntu/Fedora, or bare macOS without nix-darwin, gives the
agent's own package reproducibility but no native rollback for anything it touches
outside that package's footprint. Rather than disabling system mutation outright
on tiers 2–3 (which would gut the "capability gap" pitch for anyone not running
MojOS), the right shape is probably: confirmation-gate strictness and the agent's
own precautions (e.g. taking its own defensive snapshot before something genuinely
hard to undo) scale up as the safety tier gets thinner. Not built yet, but the
principle — never claim a safety guarantee the current host can't actually back
up, and degrade gracefully instead of turning a capability off entirely — is
worth carrying into every future capability decision, not just this one.

Small correction to the entry directly below: it talks about hardware scaling as
"swapping in a better model," at one point loosely as "a frontier-tier model on a
chartered instance." That conflates two things that have to stay separate —
mercenaries (frontier models) are never part of the hardware-scaling axis at all;
they stay walled off to scoped questions only, full stop. The scaling axis the
entry below is actually reaching for runs through local models and Ephemeral
Commons' chartered *open-source* models only.

---

## 2026-07-06 — Nix as the take-it-with-you layer; scaling should be a model swap, not a harness change

Two things landed talking through sops-nix. First, the mojo-package idea got its
real framing: it's not primarily about Ephemeral Commons, it's about
[vision.md](vision.md)'s "every house has a server" implying you eventually *move*
house and the server underneath you changes — Nix flakes are how the whole AI
(config, secrets via sops-nix, the works) travels to the new box instead of getting
rebuilt from scratch. Ephemeral Commons chartering is just a bonus of the same
packaging: beef up the resource spec in the same flake and it stands up mojo-agent
on rented compute too. Decided sops-nix should encrypt secrets everywhere, own
hardware included, not just the rented case — no downside to treating local as
equally untrusted-by-default.

Second, and the bigger open question: mojo scaling to hardware (a bigger server, or
a chartered rented instance) should ideally only ever mean swapping in a better
model for reasoning — the harness and the deterministic aspects (recipes, the
confirmation-gate pattern, whatever mojo-agent's structural layer ends up being)
shouldn't need to change with it. That's the same escalation-policy problem already
sitting open in [roadmap.md](roadmap.md)'s chartering section (deciding when a job's
worth chartering up), reframed as: get that router right and hardware scaling
becomes free — more compute just means better answers, not a different system. Not
fully sure the harness genuinely never has to change as the model gets bigger
(bigger models can support different kinds of harness, e.g. longer tool-use chains,
more autonomy) — worth stress-testing once there's a real harness to test it
against, not resolved now.

---

## 2026-07-05 — Locked: Hermes wholesale, no deadline, agent-first bootstrap

Talked through yesterday's Pi/OpenClaw/Hermes landing point again and it moved:
building mojo-agent's own core loop on Pi is the wrong first step. The fastest way
to an actually-working Jarvis is to run something that already exists —
[Hermes Agent](https://github.com/NousResearch/Hermes) (self-hosted, MIT, persistent
memory, a learning loop that writes and improves its own skills) — wholesale,
wrappers and all, rather than building a loop from scratch and reimplementing what
Hermes already does. mojo-agent proper doesn't get designed yet; it gets designed
once living with Hermes shows what it actually gets wrong for me. Pi and OpenClaw
stay as reference material for that later design, not the v0.1 substrate.

Two things fell out of that that are v0.1 requirements no longer: the confirmation
gate before the agent touches the system, and delegating to a mercenary (Claude
Code or another frontier model). Not dropped — deferred to whenever mojo-agent
proper gets built, since that's exactly the kind of functionality it should have
natively instead of as a wrapper bolted onto someone else's harness. Also decided:
don't pre-design the memory architecture. Point Hermes at whatever it defaults to
locally and let using it teach me what "editable brain" needs to mean — could end
up plain files, could end up Knowledge-object-style structures, could end up a
vector store with a translation layer for viewing/editing. Not deciding that ahead
of using it.

Also dropped the Sept 15 deadline framing, though not the date itself — I'm away
from my PC until then regardless, laptop is the summer machine, PC becomes the true
always-on host when I'm back. That's a fact about the hardware, not a target to hit
features by. The reason a deadline got written down in the first place (the 2026-07-03
reset entry, below) was a real pattern: months of planning, no runnable code. Keeping
that countdown pressure risked becoming its own kind of over-planning — a checklist
substituting for actually building. Progress now is "is Jarvis running and getting
used," continuously, not seven checkboxes due by a calendar.

Last piece: flipped the build order inside MojOS itself. Original plan was full OS
(modes, rice, daily-driven) before the agent goes in. Better: smallest possible NixOS
flake that boots and runs a resident service, Hermes installed on top of that
immediately, then use Jarvis itself to help build out the rest of MojOS — modes,
theming, daily-driving it — instead of hand-building all of that first and only
getting agent help at the end. Bootstrapping with the thing being built, as early as
the thing can bear weight.

Filed into [roadmap.md](roadmap.md)'s Now section. Circus/chartering/mercenaries
(the pirate-ship compute model) untouched — still Horizon, nothing to wire in until
there's a single working agent worth routing from.
[ephemeral-commons.md](ephemeral-commons.md)'s content is already reflected in
roadmap's Horizon section; keeping the file around for now rather than deleting it.

---

## 2026-07-04 — Candidate harness substrate: Pi, under OpenClaw and Hermes

Closing thread for the day. Went looking for what to actually build mojo-agent's
core loop on, now that Anthropic's own SDK is correctly off the table — building
the first mate's harness on a frontier vendor's proprietary tooling would recreate
exactly the dependency the project exists to avoid, sovereignty claim or not.

Two real, current open-source personal-agent products exist already: **OpenClaw**
(MIT, fast-growing, connects to messaging apps, orgs "own the full harness" per
Nvidia's own writeup) and **Hermes Agent** (Nous Research, MIT, self-hosted, a
built-in learning loop that writes and improves its own skills from experience,
persistent memory). Worth studying both, neither obviously the final answer.

The better find: **Pi** (`earendil-works/pi`) is the substrate *under* OpenClaw, not
a competitor to it — a genuinely minimal agent loop (one write-up: 418 lines,
reportedly outperforms thousand-line frameworks), a unified LLM layer, tools in,
results back, nothing else. OpenClaw adds the product layer — channels, queueing,
persistence — on top of Pi's bare loop. That's a better shape to build mojo-agent's
own core on than either full product: small enough to actually audit (matters for
"provable, not pledged"), unopinionated enough to bend around mojo's own
memory-as-plain-files and fleet/routing model instead of fighting someone else's
assumptions baked in.

Landing point, not a decision: mojo-agent's core probably gets built on something
Pi-shaped, borrowing specific ideas from OpenClaw and Hermes Agent (Hermes's
self-improving skill loop especially) rather than adopting either wholesale. Not
resolved — wants a real side-by-side before committing. The build target underneath
all of it stays simple regardless of which substrate wins: one agent, running on
MojOS, holding the second brain, doing fleet/mercenary routing when a job actually
needs it.

---

## 2026-07-04 — The fleet, chartering, and mercenaries (supersedes "Scallywags" below)

Kept pulling on the pirate metaphor from earlier today and it resolved into
something better than the tier ladder I wrote a few hours ago. Recording the shape,
not the whole back-and-forth.

**Captain and First Mate never fully merge, on purpose.** The long-term goal is that
it feels like one person — the first mate develops me, takes the boring stuff off my
plate, teaches so I stay accountable for what's actually happening rather than just
supervising it. But the seam stays real underneath the feeling: the first mate
defers to me on anything that matters and never substitutes its judgment for mine.
That's not a limitation to smooth away later — it's what keeps "who's accountable
here" answerable even once daily use makes the line invisible. Same instinct as
Collectives' "always traces back to a human," just one layer down.

**"Scallywags" was wrong — it's not a hired outsider, it's a bigger boat.** If it's
literally the first mate's own weights and harness running on rented hardware for a
few hours, that's not a separate trust tier, it's the same first mate wearing a
bigger hull. Collapsed the model to: one fleet, many ship classes — sloop for
something small and local, bigger hulls pooled from owned machines (the Circus), up
to a chartered frigate or galleon when a job needs more than anything owned gives.
Chartering is the verb for renting the bigger hull, same nautical sense as it always
had. Fleet command — the memory, the orchestration, the judgment — always lives at
home, on the ringmaster (or, before there's a dedicated one, whatever single machine
is doing the job) — only the hull doing the actual thinking changes underneath it.

**Mercenaries are the one real outsider**, and the distinction that actually matters:
is this me, temporarily bigger (fleet, any class), or is this someone else's model on
someone else's infrastructure that doesn't get torn down when I'm done with it
(mercenary)? Mercenaries get a fragment of the charter — the minimum context, real
values swapped for stand-ins — report back, never board anything. The defence isn't
"they'll forget," it's "there's less worth remembering" — a frontier vendor keeps
whatever they're handed regardless, so the only real lever is shrinking the fragment.

**Keeping it feeling like the same first mate across ship classes isn't free** — a
bigger model is a genuinely different mind, not just a stronger one, so "same first
mate, bigger hull" is a design target, not something the metaphor grants automatically.
Landed on: a personality LoRA per model family, carrying voice and judgment style —
never memory, which stays in the plain-file brain and gets retrieved fresh regardless
of hull. Conflating the two would mean correcting a fact requires retraining instead
of editing a file, which breaks the whole "brain as plain files, always correctable"
premise. Training and maintaining the LoRA library is MojOS's job — and training one
is itself a chartering-sized job (rare, occasional, worth the escalation), a decent
sign the pieces actually fit rather than being bolted on separately.

Same open question as before, just renamed: the escalation policy (when chartering
up or hiring a mercenary is actually worth it) is still hand-decided for now, still
the thing worth training a router on later. Filed the updated shape into
[vision.md](vision.md) and [roadmap.md](roadmap.md); [ephemeral-commons.md](ephemeral-commons.md)
is now scoped specifically to chartering.

**Addendum, same day:** three corrections after talking it through further. First,
every ship class gets the whole brain, chartered or not — that's the actual
definition of fleet vs. mercenary, not a detail; nothing about renting a bigger hull
should ever mean less trust. Second, the LoRA is the long-term fix, not the plan for
now — short term is just system prompts carrying voice and judgment, cheap and no
training required, upgraded to a LoRA per model family later if prompting alone stops
being enough. Third, the escalation ceiling is mine to set, not just the first mate's
to judge — "never hire a mercenary, full stop" has to be a real, respected setting.
Whatever tools exist, how much they get used is always my call, not a default the
system talks me out of.

---

## 2026-07-04 — Naming the ladder, and folding it into vision.md

Picked this idea back up and pushed it from "a doc that exists" into the actual
vision/roadmap. Landed on a cleaner shape than the original four-tier draft:

Local and Circus were never separate trust tiers — Circus is just more owned
hardware, so it folds entirely into **the first mate**: everything I own outright,
zero exposure. Beyond that, two tiers, named properly instead of left as loose
descriptions: **Scallywags** (rented, ephemeral, optionally attested — hired
dockside, let go after the job) and **Mercenaries**, not "crew," for the frontier
models — a crew boards the ship and has standing access; these get one mission, the
minimum context it needs, and never come aboard. That distinction matters: the whole
point is they can't access anything beyond what's handed to them for that one job.

Checked the pricing on Scallywags for real: confidential/attested GPU rental
(confidential.ai, TEE via Intel TDX + AMD SEV-SNP + Nvidia CC) runs about 30–50% over
plain rental for comparable hardware — H100 $2.00/hr attested vs $1.33–1.55/hr plain.
Expected an order-of-magnitude privacy tax; it isn't one. Changes this from "someday,
if confidential computing gets cheap" to "buildable now, cost is a rounding error
against the value."

The real open question, both times it came up: *when* is escalating tiers actually
worth it — the cost, the exposure, against how much better the answer gets. No
ground truth ahead of time, since you often don't know a higher tier would've done
better until you've run it there. Answer for now: decide by hand. Real answer,
later: train a router on accumulated first-mate decisions (task → tier → was it
worth it) once there's enough real use to learn from — same shape as "the first mate
learns how I think," not a bolt-on. Not a Sept 15 problem. Filed as the open research
question in [roadmap.md](roadmap.md)'s horizon section.

**Addendum, same day:** the first mate owns the budget too, not just the per-job
tier call — which mercenary subscriptions are actually worth keeping, what Scallywag
rental costs over time, tracked with the same discipline as what got seen. And the
expected shape of usage across the ladder should be lopsided on purpose: nearly
everything local, Scallywags for genuine load, mercenaries rare — an exception path,
not a habit.

---

## 2026-07-04 — Scallywags, and the ephemeral compute idea

Two things surfaced today, from watching Claude Code's own subagent tooling and
riffing on it.

First, a language match, not a new idea: the fork-vs-fresh-agent distinction in
Claude Code's Agent tool — a fork inherits full context so you hand it a directive,
a fresh agent gets briefed cold with everything it needs and nothing it doesn't,
and the rule "never delegate understanding" (do the thinking yourself, then dispatch
a scoped task) — is exactly the first-mate/crew relationship already in
[vision.md](vision.md). Good confirmation the metaphor holds up mechanically, not
just poetically.

Second, an actual new idea, chasing "what does the crew look like before I trust
any frontier vendor with my data." Landed on: an ephemeral compute tier — rent
open-source-model hosting anonymously (alias, crypto, VPN/Tor) and tear it down
after, so the coordinator can borrow datacenter-class compute without a company
ever holding the data *or* knowing it was mine. Checked it against reality: the
individual pieces already exist — DePIN GPU marketplaces (Akash, io.net, Nosana,
Clore — a $19B+ category) do anonymous rental, and TEE-based confidential
computing (Phala on OpenRouter, NEAR AI Cloud, Spheron, Nvidia's own Hopper/
Blackwell CC mode) does attested "not even the host can see it" inference. Nobody
combines both, aimed at an individual's privacy rather than enterprise compliance
— that's the actual gap, and it's a positioning gap, not a technology one.

Caught myself almost getting the emphasis backwards: the ephemeral tier is
plumbing, not the differentiator — it's explicitly stateless (rent, use, forget),
while "remembers you" requires the opposite, durable local memory. The first
mate's judgment (what's routine enough for local, what needs the tier, what to
strip before anything leaves) is still the hard, unbuilt, actually-mine part.
But it's also the most novel and most personally interesting thing I've got right
now, and there's a real business shape on top later — not owning datacenters
(wrong shape, capital sink) but the broker/attestation layer over hardware that
already exists, same move every DePIN player already makes.

Real risk, flagged and not resolved: anonymity-as-a-feature inherits Tor's
problem — legitimate, valuable, and permanently associated with whatever the
worst users do with it, which makes payment processors and banks skittish
regardless of the real user base. Would hate to build something that mainly
enables the wrong things. Needs a real answer before this becomes a product, not
just a vibe.

Landed somewhere, in the end. The key unlock: don't build the marketplace. Full
decentralization of datacenter-level compute is never happening — power, cooling,
capital don't bend that way — so stop trying to be Akash. Just be a privacy-
obsessed *client* of whatever GPU supply already exists, centralized or
decentralized, anonymized and attested on the way in. That drops the "entering a
$19B funded category" problem entirely — you're renting from it, not competing in
it.

And the long-term shape holds up: everyone runs their own local server for memory
and coordination, forever, and reaches for the ephemeral tier whenever a job needs
more than home hardware gives. That gap between "what my server can do" and "what
the frontier can do" never closes — it just moves — so this isn't a stopgap until
local hardware catches up, it's the permanent architecture. It's also not a novel
bet: it's cloud bursting, a proven pattern, with a new trust layer bolted on. The
first mate's judgment is still the unbuilt, actually-hard, actually-mine part —
this is the answer to "how do I get the compute without trusting a company," not
to "what makes Mojo worth having." Both still need to exist.

This is the one. Chased some version of this for months without landing it —
this time it's scoped small enough to actually build and big enough to matter.
Where it sits against the frozen 2026-07-03 core (ahead of it, alongside it, or
after) is still open — picking back up when the summer goals get replanned.

**Addendum, same day:** dropped anonymity as a goal entirely. Fine if a provider
knows it was me who rented the box — the only thing that has to stay unseen is
the logs, the actual content. That kills the "Tor for AI compute" framing (Tor
hides *who*; this only ever needed to hide *what*) and the doc's one-liner and
phasing got fixed to match. A Tor-style layer is still a real idea, just not
this project's — someone can bolt it on later if they want it.

---

## 2026-07-04 — Sovereignty, not abstractly

Talked through an ADHD/cannabis pattern with Claude tonight — the kind of thing worth
reasoning through carefully and tracking over time before a prescriber conversation.
Mid-conversation, hit the obvious problem: that reasoning now sits on Anthropic's
servers, and I don't get to decide who else ever sees it. Philosophy.md has said "data
ownership is decision-making power" for a while now; this is what that looks like when
it stops being abstract. Not a reason to stop using frontier models today — mojo-agent
doesn't exist yet, and Claude is still the best contractor available for this — but a
real, personal instance of the exact problem the project exists to fix, not a thought
experiment. Tracking doc lives outside this repo: `~/documents/adhd/`.

## 2026-07-03 — The reset

Stepped back and admitted the pattern: months of framework sessions, docs
restructures, and meta-work arranging thinking — and no runnable code anywhere in the
org. The vision was never the problem. The sequencing was. A first-year with a
manifesto is invisible; a first-year with a manifesto *and a machine that actually
runs it* is a different thing entirely. Proof, not promise — my own rule, finally
applied to myself.

So: reset to build-first. This repo becomes exactly what it should have been — the
thinking repo. Seven flat files (vision, philosophy, roadmap, ideas, devlog, README,
AGENTS), written in my voice, public, kept current. Everything else — the old `raw/`
corpus, the 9-chunk restructure tracker, the framework session briefs — distilled
into these files and deleted; git history keeps every byte if we ever need to dig.

The new goal, frozen: by 15 September — back at my PC for second year — the first
version of an always-on Jarvis exists. MojOS (NixOS-based) as my daily driver on the
laptop I have with me all summer, mojo-agent v0.1 — my first mate — resident inside
it, with phone access and voice as the surfaces once the core holds. The day I'm
back, it all deploys to the PC (dual-boot with Windows), which becomes the true
always-on host. Finish line in [roadmap.md](roadmap.md): July is Nix and the OS in a
VM, August is the laptop for real plus the agent core, September is surfaces and
hardening. The Circus and the collectives stay on the horizon, alive in
[vision.md](vision.md), untouched until the foundation is real.

Also settled today: the essays (craft problem first) come later and get written from
these files; the dotfiles repo's contents dissolve into the mojos flake when the rice
gets rebuilt; and the old Hyprland setup is not being ported — it's being replaced.
