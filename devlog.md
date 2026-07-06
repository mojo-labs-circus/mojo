# Devlog

*Dated thinking, newest first. Messy is fine — this is where the reasoning trail
lives.*

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
