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

## What Mojo is

Every AI someone uses today forgets them the moment the session ends, and is stuck
wherever they opened it — a phone assistant that doesn't know what the laptop's
knows, neither carrying anything forward, both owned by someone else's product.
Mojo pulls identity out of any single app entirely.

The precedent is already proven one level down: harnesses made models
hot-swappable, and nobody now would weld a harness to one model. Mojo is the same
move one level up. Agents themselves — the harnesses, the products, the whole
crowded fast-moving layer of them — become the swappable part, and Mojo is the
layer underneath that survives the churn. An AI is stateless between calls; the
same frozen weights run for everyone. What makes my AI *mine* was never in there.
It's the context: the memory of my life and the permissions I've granted. Some days
I think slower too — I'm still me. Identity is context, not compute.

So the system is built from that layer down, in a small vocabulary. A **Captain**
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
nobody has built the system that treats it as one.

## MojOS

Mojo runs on any Unix system — that reach is the point, and it's what makes the
sovereignty claim credible to people who won't bet their machines on an unproven
project. MojOS is the deepest host: NixOS plus Mojo's own opinionated defaults, the
whole machine declarative and always safe to undo, not just Mojo's own footprint.
It's the host I'll run myself. Best home, not a requirement.

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

Four tests, each one a property JARVIS visibly has: omnipresence, continuity, one
identity regardless of substrate, absolute loyalty to the data. How the first mate
*behaves* — the sensei, pushing back, developing its user — is
[philosophy.md](philosophy.md)'s jurisdiction, unchanged by any of this.

## An argument, not just software

Mojo is an existence proof. The dark path doesn't look dramatic — it looks like
convenience: a better chatbot, a smarter phone, an AI that just handles things for
you. An entire generation will take it because it's easy and nobody told them what
they were trading away. The counter isn't a manifesto or a warning. It's something
real enough that people can see it, touch it, and understand: *this is what it could
have been instead.*

Mojo is the proof that another path was possible. And if it's good enough, it becomes
the path.
