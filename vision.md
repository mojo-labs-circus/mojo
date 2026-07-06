# Vision

*What I think the future should look like, and what I'm building toward. The beliefs
underneath it are in [philosophy.md](philosophy.md); what's actually happening right
now is [roadmap.md](roadmap.md). The destination here is firm — the specific
mechanisms are hypotheses that building will test.*

---

## The future

Every house has a server in it. Not a novelty — infrastructure. As normal as a router:
a square box, sits in a cupboard, you forget it's there. That's where your intelligence
lives. Always on, always yours, never leaving your home.

The router analogy is deliberate. A router connects you to the internet — you don't
think about it, it just works. The home server connects your intelligence to
everything: your devices, your work, your life. It will probably be marketed and sold
the same way. It comes with the house. It's in the listing. One bed, one bath, a
server.

Your personal AI lives on that server and grows with you. It learns how you think, how
you work, what you value, how you make decisions. Over years it becomes a genuine
extension of you — not a tool you use but a counterpart that knows you. The longer you
have it the more valuable it becomes, and it's yours in the same way your memories are
yours. No company owns it. No subscription can take it away.

Take that far enough and it changes what a computer even is. Personalisation gets deep
enough that the system stops having a UX and starts having *your* UX — the machine
conforms to the person instead of the person conforming to the machine. A kid who
grows up with their AI from age five has, by adulthood, a counterpart that has watched
them learn for fifteen years — not something they use, something they think with. The
relationship is the asset, not the model running underneath it.

And from that server, eventually, you can be part of any group that matters to you —
your family, your team, your community — each getting exactly the version of you
you've consented to share, your intelligence the constant thread through all of it.
Intelligence as personal infrastructure: as unremarkable, and as essential, as running
water.

## The three gaps

**The capability gap.** The tools that make computers genuinely powerful — Linux,
version control, system configuration, scripting, automation — are only accessible to
people who spent years learning them. Technical users have an enormous advantage over
everyone else, and AI is widening it: the people who already know these tools use AI
to go faster; everyone else still clicks through menus. The agent closes that gap —
full system capability, through conversation, no expertise required.

**The sovereignty gap.** Compute has centralised twice. Mainframes sat in institutions
until the PC decentralised it. Then the cloud came, with better UX as the bribe, and
people handed back their data, their compute, their sovereignty without noticing.
Personal AI is the same inflection point as the PC — and right now it's heading toward
capture. Every major assistant routes through someone's cloud, learns from your data,
and answers to a company whose incentives are not yours. When you talk to an AI you're
not browsing the internet — you're thinking out loud. Those logs are thought logs, and
they should belong to you alone. Data is power: whoever holds the picture of who you
are has power over you. The only real answer is ownership.

**The context gap.** Every AI conversation starts from zero. You re-explain who you
are, what you're working on, what you've already figured out — every single time.
There's no persistent picture of you. A real personal AI carries your context
permanently, across every device, and uses it to actually understand you rather than
asking you to repeat yourself.

## What I'm building

One system, in pieces that compose.

**mojo-agent — the first mate.** My personal AI, running on my machines. It's embedded
in the system rather than sitting in a tab: it knows my machines, my files, my
workflow, my preferences, and it keeps a brain — real, accumulating memory of me that
lives on my hardware and nowhere else. It isn't a coding tool. I'm the captain; it's
the first mate — and it works for me, not itself: it defers on anything that matters,
never substitutes its judgment for mine, and its actual job is making me better at
being captain, not more dependent on it. The seam between us never fully disappears —
that's deliberate, it's what keeps "who's accountable here" answerable even once the
day-to-day feels like one continuous person.

Command of everything I own outright: this machine, and every other machine of mine
once the Circus exists — ringmaster and performers, still one fleet. Fleet command,
the memory and the judgment, always lives at home, on the ringmaster (or, before I
have one, whatever single machine I've got); only the hull doing the actual thinking
changes. The fleet has classes, the same first mate at the helm of all of them, sized
to the job — a sloop for something small and local, up through bigger hulls pooled
from my own machines, up to a chartered frigate or galleon when a job needs more than
anything I own gives. Every class gets the whole brain, chartered or owned — that's
what makes it the fleet and not a mercenary: what changes between hulls is how much
compute is under the hood, never how much it's trusted with. Chartering means renting
compute for the length of one voyage, sometimes attested so even the dock owner can't
see what ran, then letting it go — and it's cheap enough today to actually use:
checked the pricing, confidential rental runs maybe 30–50% over plain for comparable
hardware, not the order-of-magnitude tax expected, and whatever's rentable only keeps
growing. What keeps it feeling like the same first mate across every hull rather than
a different mind at a different size, short term: system prompts carrying voice and
judgment, injected fresh regardless of hull — cheap, no training required. Long term,
once prompting alone stops being enough: a personality adapter (a LoRA) per model
family, same job, more durable. Either way memory itself never lives in the hull — it
stays in the plain-file brain and gets retrieved fresh every time. Building and
keeping whichever version current is MojOS's upkeep, same category as staging changes
safely.

Beyond the fleet, mercenaries — the frontier models, Claude, GPT, whatever is
sharpest in the world. Never part of the fleet, never trusted with it: hired for one
mission at a time, handed a fragment of the charter — the minimum context the job
needs, real values swapped for stand-ins first — and dismissed the moment they report
back. They keep whatever they're handed regardless of what's asked of them; the only
real defence is shrinking the fragment, not trusting their side to forget.

The first mate only charters a bigger hull, or hires a mercenary, when it's actually
worth what it costs — in money, in what has to be revealed — never by default. In
practice this should skew hard toward the bottom: nearly everything on my own fleet,
chartering for real but occasional load, mercenaries the rare exception, not a
default channel. Working out *when* that's worth it well enough to trust
automatically is still open; deciding it by hand for now, training a router on the
decision once there's enough real use to learn from. And the ceiling itself is mine
to set, not just the first mate's to judge — a hard line like "never hire a
mercenary, full stop" is a real, respected setting, not a suggestion the system talks
me out of. Whatever tools exist, how much they get used is my call.

The budget is the first mate's to manage, too — not just which hull a job goes to,
but the standing decisions: which mercenary subscriptions are worth keeping, what
chartering is costing over time, what the spend actually bought. "What did this cost
me" gets the same discipline as "what did it see."

**MojOS — the workshop.** The operating system built around the agent, on NixOS. The
agent isn't a feature bolted on — it's the primary interface, and the OS is designed
for it: the whole system declared in one configuration, changes staged and validated
before they commit, always safe to undo. And the workshop has moods — switchable modes
that change how the system looks, behaves, and how much of it shows: a dense hacker
mode for deep work, a Grateful Dead mode when that's the energy, a plain-looking mode
for when mates are in the room. The machine matches the person I am that day.

**The Circus — later.** Today each machine is an island. The Circus connects them so
the agent follows you across devices as one coherent intelligence, not a separate
instance per machine — the PC that becomes the home server, the laptop that joins it,
the phone that's a window into it. Still entirely the first mate's own fleet: bigger
ship classes to draw on, not a new tier of trust.

**The Commons — the far horizon.** Chartering today means renting from whatever GPU
supply already exists, one voyage at a time. The Commons is what that grows into: a
network of datacentres run on attestation instead of trust — the operator can prove,
cryptographically, that they never saw what ran on their hardware — that any personal
AI, mine or anyone else's, can borrow from *and give back to*, not just charter from.
How much to trust any given provider is the owner's call, not a default baked in —
attested-but-known, or attested-and-anonymous, by choice. This is what keeps "everyone
has their own server" from quietly recentralising the moment something needs real
scale — the sovereignty promise holds even at the edge of what your own hardware can
do.

**Collectives — the far horizon.** Groups of people, each with their own sovereign
agent, forming formal collectives: shared goals, explicit structure, AI participating
in the group as a first-class member, accountability that always traces back to a
human. The deepest and least-tested part of the vision. It stays on the horizon until
the foundation under it is real — but it's where this ends up: if everyone has their
own AI, the collectives they form are how individuals get institutional depth without
institutional capture.

## Who it's for

Right now: me. That's the point — dogfooding from day one, sample size of one, finding
out what's real. Beyond that: technically capable people motivated by autonomy, who
understand the sovereignty argument, run their own hardware, and tolerate rough edges
in exchange for something real. Correct sequencing, not a limitation — the PC wasn't
for everyone in 1977 either.

The early technical users are the bootstrapping strategy, not the target state. The
agent itself is what lowers the barrier: over time it abstracts away the expertise
sovereignty currently demands, until owning your AI is the path of least resistance
rather than the hard path.

## When it's right

The system is right when these hold:

1. **The agent is the primary interface.** You live in conversation with it — it
   controls the system, works in parallel with you, surfaces things before you ask.
   Not a chat tab you switch to.
2. **Ownership is always answerable.** At any point in the system, "who owns this
   data?" has one clear answer, and context leaves your hardware only by explicit
   consent.
3. **Mercenaries see questions, not your life.** The first mate hires the sharpest
   frontier model for a single mission at a time, hands it a fragment of the charter
   — the minimum it needs — and holds the rest locally.
4. **It makes you more capable, not more dependent.** It pushes back, explains its
   reasoning, and develops its user — measured by what you can do, not just by what
   it can do.
5. **Your machines stop being islands.** One intelligence across every device you own.
6. **Groups can constitute themselves.** A family or a team can form a collective
   with sovereign members and accountable AI participation.

## An argument, not just software

Mojo is an existence proof. The dark path doesn't look dramatic — it looks like
convenience: a better chatbot, a smarter phone, an AI that just handles things for
you. An entire generation will take it because it's easy and nobody told them what
they were trading away. The counter isn't a manifesto or a warning. It's something
real enough that people can see it, touch it, and understand: *this is what it could
have been instead.*

Mojo is the proof that another path was possible. And if it's good enough, it becomes
the path.
