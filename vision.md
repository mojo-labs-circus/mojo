# Vision

*What I think the future should look like, and what I'm building toward. The beliefs
underneath it are in [philosophy.md](philosophy.md); what's actually happening right
now is [roadmap.md](roadmap.md); the full interoperability argument this vision now
rests on is [interoperability.md](interoperability.md). The destination here is firm —
the specific mechanisms are hypotheses that building will test.*

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
are has power over you. The only real answer is ownership — and ownership has a second
half that's easy to miss. A fully open-source, self-hosted assistant still traps you
if there's no shared shape underneath it, because the lock-in that matters was never
"can I read the code" — it's "can I leave with what I've built up." A year of
accumulated memory inside any product's private format is a year of switching cost,
whatever the license says. Sovereignty is ownership plus portability. Either one
alone is branding.

**The context gap.** Every AI conversation starts from zero. You re-explain who you
are, what you're working on, what you've already figured out — every single time.
There's no persistent picture of you. A real personal AI carries your context
permanently, across every device, and uses it to actually understand you rather than
asking you to repeat yourself.

## What Mojo is

Every AI product that exists today welds together things that don't need to be
welded: which model does the thinking, what loop drives it, where memory and
identity live, what tools it can reach, how it reaches you. Pick a product and you
get all of it as one inseparable block — and the piece that actually matters, the
accumulated picture of you, is trapped inside. Mojo exists to pull that piece out,
permanently.

This has happened before, and it always breaks the same way: along an interface.
Unix broke the proprietary OS monolith, and its descendants now run nearly
everything. The PC clones broke IBM's hardware monopoly across a published
hardware boundary. Carterfone broke Bell's grip on what was allowed to touch the
phone network. The open web broke the walled gardens that wanted to be the whole
internet. And AI has already started the same pattern one layer down: harnesses
decoupled the model from the product, and nobody now would weld an agent to a
single model. Mojo is the next decoupling in the same lineage — identity from
everything else. The thing that should outlive every product gets pulled out of
all of them.

So Mojo, precisely, is a standard. The **Mojo System Interface** defines the
contract at every seam a complete personal AI system is made of — and the scope is
deliberately the whole system, not just an agent's anatomy. The field itself is
moving there: every serious product I've surveyed now pitches persistence as the
product, not the loop. A Jarvis-class system — one identity, reachable from every
machine you own, growing for years — needs seams an agent alone never would:
reachability, proactive time-driven behavior, coherence across a fleet of devices,
routing between the pieces. The standard is built the way POSIX is built:
primitives that hold, schemas describing the shape of what's built from the
primitives, contracts describing the seams between parts.

The standard doesn't invent what already exists. Where the industry has a real,
working answer, the MSI adopts it — MCP for tools, SKILL.md for skills, the
already-agnostic model APIs, A2A for agent-to-agent traffic. Where the market
should compete, it deliberately leaves the inside open — how a loop plans, how it
manages its context — the same way POSIX never standardized a scheduler's
internals, only what a process must expose. It defines only what nothing existing
covers: the shape of memory and identity, and the enforcement boundary that
protects it. Even there, most of the substance is already precedented — agents
today mostly keep their memory in plain files, which is exactly why this is
standardizable at all. What's genuinely new is thin and specific: the permissions
travelling *with* the data, provenance, and the rule that anything a session
learns gets written back in the shared shape instead of staying trapped in one
product's private buffer.

Mojo builds the first implementation of exactly those two pieces — the kernel and
the memory — because they're the two seams where there's nothing to adopt, and
nobody with the resources to build them well has any incentive to: a neutral
standard removes the moat, and nobody who profits from the moat funds its removal.
The kernel is singular *per Fleet* — one instance holds authority over what any
plugged-in agent, tool, or model may touch — but it is not singular in the world:
the enforcement contract is part of the standard, Mojo's kernel is only the first
reference, and someone smarter than me can and should build a better one. The same
goes for the memory implementation. Every piece Mojo builds is designed to be
outcompeted through the same contracts it publishes.

The first Mojo system, then, is a distro in the honest Linux sense: assembled from
existing parts that already speak the adopted standards — a harness someone else
built, models someone else trained, tools someone else wrote — around the kernel
and the memory that make them one system. That's not a shortcut; it's the proof.
Every seam in the standard gets tested by whether something real, built by someone
who has never heard of Mojo, actually plugs into it. The MSI earns its authority
from the running system, never from being declared.

The system the standard describes is built from a small vocabulary. A **Captain**
is a person. A **First mate** is a persistent AI identity: what it remembers, what
it's allowed to do — living on the Captain's own hardware, owing nothing to any
product, handed fresh to whatever is actually doing the thinking. A **Vessel** is
any machine that participates — owned, borrowed, or rented. A **Fleet** is the
simplest whole system: one Captain, one First mate, every Vessel they work with.
Which machine runs a task, which agent drives it, and which model does the
thinking are three independent choices, made per job and changed without cost —
different agents are good at different work, exactly as different models are —
because nothing that matters ever lives inside any of them. The first mate is what
stays constant across every swap.

Mine is called Jarvis, and the name is the spec: Tony Stark's JARVIS — one
continuous counterpart across every machine, every interface, every scale of
problem. That's also why the first mate can't just be a database with permissions
attached: it's a character, with a voice and judgment that stay recognizable no
matter what's underneath. The memory and the permissions are the foundation and
the claim I can already stand behind; the character layer is built on top of them,
and it earns a bigger place here as it proves out.

Sovereignty in this design is structural, not behavioral. The boundary around a
first mate is enforced from outside whatever agent is plugged in — an agent that
has never heard of Mojo still cannot exceed what it was handed, because what it
wasn't handed simply isn't there. And the thinking itself stays genuinely private:
models chosen on verifiable local privacy, which today means open weights on
hardware I control — not because open source is the aesthetic, but because it's
currently the only way "this never left my machines" is checkable rather than
promised.

Not every mind runs on hardware that's yours, and the model doesn't pretend
otherwise. A frontier model behind someone else's API — Claude, GPT, whatever is
sharpest in the world — is a **mercenary**: a hired gun, taken on for one mission
at a time, handed the minimum fragment of context the job needs with real values
swapped for stand-ins, and never given Fleet trust. Not because the model is
worse — often it's better — but because its weights answer to someone else. The
sharpest tools in the world, used without handing them your life. A *rented
machine* is different: attested so even its owner can't see what ran, it's a real
Vessel for the length of the rental. The fleet grows and shrinks with need;
ownership was never what gated existence, only trust.

And the structure recurses. A Fleet is the atomic case — one Captain, one First
mate. The same structure, unchanged, holds several Captains and several First
mates sharing a system: a **Collective** — a team, a family, a community, each
member still sovereign, each first mate still its owner's alone, permissions never
surrendered, participating in something larger without giving up what's theirs. AI
as a real member of the group, accountability always tracing back to a human.
People already work in groups where everyone runs a separate AI tool and
coordination happens despite the fragmentation; Collectives makes the group
itself — people and their AIs together — one system. The academic case that the
group is the right unit is real and converging
([collective-intelligence-research.md](collective-intelligence-research.md));
nobody has built the system that treats it as one. It's aimed at deliberately
second: an extension of the same primitives, reached only once the single-Captain
system is real — if the primitives are right, Collectives is what they already
support, not a new system.

## MojOS

Mojo runs on any Unix system — that reach is the point, and it's what makes the
sovereignty claim credible to people who won't bet their machines on an unproven
project. MojOS is the deepest host: NixOS plus Mojo's own opinionated defaults, the
whole machine declarative and always safe to undo, not just Mojo's own footprint.
It's the host I'll run myself. Best home, not a requirement.

## Who's building this, and who it's for

Right now: me, alone, and being upfront about that is part of the design, not a
disclaimer. I've just finished the first year of a CS degree. This is a personal,
hands-on learning experiment, done in the open — the register Linux was announced
in ("just a hobby, won't be big and professional"), and I mean that comparison as
discipline, not destiny: build the small real thing first, stay honest about
everything past it. The seed is solo-sized on purpose — Mk1 of the standard, the
first kernel, the first memory implementation, the first system stitched from
existing parts — because the adopt-everything posture keeps it that way. The thing
it should become is not solo-sized, definitionally: a standard is a social object,
and nobody delivers one alone — not even POSIX's committee did. Better kernels,
better memory implementations, systems assembled by people I'll never meet. The
project is designed so that gap is an invitation, not a rescue.

I'm also the first user — dogfooding from day one, sample size of one, finding out
what's real. Beyond me: technically capable people motivated by autonomy, who
understand the sovereignty argument, run their own hardware, and tolerate rough
edges in exchange for something real. Correct sequencing, not a limitation — the
PC wasn't for everyone in 1977 either. The early technical users are the
bootstrapping strategy, not the target state. The agent itself is what lowers the
barrier: over time it abstracts away the expertise sovereignty currently demands,
until owning your AI is the path of least resistance rather than the hard path.

## When it's right

The measure is the name: how much of JARVIS actually exists yet.

1. **It's everywhere you are.** One first mate across every Vessel — the server,
   the laptop, the phone in your hand. Every device is a window into the same
   one, never a separate instance per machine.
2. **It never starts from zero.** You never re-explain yourself. Memory
   accumulates for years, and next year it knows you better than this year does.
3. **Swap the agent, swap the model — it's still Jarvis.** Nothing moves: the
   memory, the permissions, the relationship. The falsifiable core; if this
   fails, the central claim fails.
4. **It's yours alone.** "Who owns this data?" always has one answer, and
   context leaves your hardware only by explicit consent — anything hired from
   outside sees a mission fragment, never your life.
5. **It survives Mojo.** Someone replaces a piece Mojo itself built — the
   kernel, the memory implementation — with a better one that speaks the same
   contracts, and nothing is lost in the move. The standard outgrowing its own
   reference implementation counts as winning, not losing.

Five tests, each one a property the finished thing visibly has: omnipresence,
continuity, one identity regardless of substrate, absolute loyalty to the data,
and a foundation nobody — including me — gets to own. How the first mate
*behaves* — the sensei, pushing back, developing its user — is
[philosophy.md](philosophy.md)'s jurisdiction, unchanged by any of this.

## An argument, not just software

Mojo is an existence proof. The dark path doesn't look dramatic — it looks like
convenience: a better chatbot, a smarter phone, an AI that just handles things for
you. An entire generation will take it because it's easy and nobody told them what
they were trading away. The counter isn't a manifesto or a warning. It's something
real enough that people can see it, touch it, and understand: *this is what it could
have been instead.*

The success condition is written into the design rather than hoped for: every
piece Mojo builds is replaceable through the contracts it publishes, so the idea
can win even where my implementation loses. Mojo is the proof that another path
was possible. And if it's good enough — if the *standard* is good enough — it
becomes the path.
