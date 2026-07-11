# Vision

*What this project is for, and where it goes if it works. What a Mojo system
concretely is lives in [anatomy.md](anatomy.md); the beliefs underneath it are in
[philosophy.md](philosophy.md); what's actually happening right now is
[roadmap.md](roadmap.md); the full interoperability argument is
[interoperability.md](interoperability.md). The mission here is firm. The
specific mechanisms are hypotheses that building will test.*

---

## The mission

AI has already changed shape twice: chatbots, then agents that do real work.
The next shape is visible from here, and every serious product is already
racing toward it: a persistent counterpart. Always on, remembers you, acts for
you, reachable from every machine you own. A digital counterpart.

The question isn't whether that arrives. It's what shape it arrives in, and
right now it's arriving welded shut. Every product building one welds together
things that don't need to be welded: which model does the thinking, what loop
drives it, where memory and identity live, what tools it can reach, how it
reaches you. Pick a product and you get all of it as one inseparable block, and
the piece that actually matters, the accumulated picture of you, is trapped
inside.

Mojo exists to make digital counterparts interoperable instead. It's a
standard, the **Mojo System Interface**, defining every piece such a system is
made of and every seam between them, plus a first running system that proves
the seams hold. The point is to let these systems grow the way Linux grew:
every piece built, competed on, and improved by anyone; no lock-in anywhere;
local and sovereign for whoever wants that; and the one thing that can never be
rebuilt, your AI's accumulated understanding of you, kept in a portable shape
that survives every swap. I want the open-source community building these
together and correctly, instead of a thousand welded products each trapping
their users. Success isn't Mojo's implementation winning. It's the idea
winning.

## What's at stake

**The capability gap.** The tools that make computers genuinely powerful
(Linux, version control, system configuration, scripting, automation) are only
accessible to people who spent years learning them. Technical users have an
enormous advantage over everyone else, and AI is widening it: the people who
already know these tools use AI to go faster, and everyone else still clicks
through menus. The agent closes that gap. Full system capability, through
conversation, no expertise required.

**The sovereignty gap.** Compute has centralised twice. Mainframes sat in
institutions until the PC decentralised it. Then the cloud came, with better UX
as the bribe, and people handed back their data, their compute, and their
sovereignty without noticing. Personal AI is the same inflection point as the
PC, and right now it's heading toward capture. Every major assistant routes
through someone's cloud, learns from your data, and answers to a company whose
incentives are not yours. When you talk to an AI you're not browsing the
internet. You're thinking out loud. Those logs are thought logs, and they
should belong to you alone. Data is power: whoever holds the picture of who you
are has power over you. The only real answer is ownership, and ownership has a
second half that's easy to miss. A fully open-source, self-hosted assistant
still traps you if there's no shared shape underneath it, because the lock-in
that matters was never "can I read the code". It's "can I leave with what I've
built up". A year of accumulated memory inside any product's private format is
a year of switching cost, whatever the license says. Sovereignty is ownership
plus portability. Either one alone is branding.

**The context gap.** Every AI conversation starts from zero. You re-explain who
you are, what you're working on, what you've already figured out, every single
time. There's no persistent picture of you. A real personal AI carries your
context permanently, across every device, and uses it to actually understand
you rather than asking you to repeat yourself.

## What Mojo is

Monoliths like this have broken before, and they always break the same way:
along an interface. Unix broke the proprietary OS monolith, and its descendants
now run nearly everything. The PC clones broke IBM's hardware monopoly across a
published hardware boundary. Carterfone broke Bell's grip on what was allowed
to touch the phone network. The open web broke the walled gardens that wanted
to be the whole internet. AI has already started the same pattern one layer
down: harnesses decoupled the model from the product, and nobody now would weld
an agent to a single model. Mojo is the next decoupling in the same lineage,
identity from everything else. The thing that should outlive every product gets
pulled out of all of them.

So Mojo, precisely, is a standard. The MSI defines the contract at every seam a
complete personal AI system is made of, and the scope is deliberately the whole
system, not just an agent's anatomy. A digital counterpart needs seams an agent
alone never would: how you're reached, how standing instructions fire on time,
how one identity stays coherent across all your machines, how the pieces get
selected per task. The whole system is drawn in [anatomy.md](anatomy.md), every
piece, every seam, and the one thing that never swaps. That document is the
concrete form of everything this one argues for.

The standard doesn't invent what already exists. Where the industry has a real,
working answer, the MSI adopts it: MCP for tools, SKILL.md for skills, the
already-converged model wire format, A2A for talking to agents outside your
system. Where the market should compete, it deliberately leaves the inside
open (how an agent runtime plans, how it manages its context), the same way
POSIX never standardized a scheduler's internals, only what a process must
expose. It defines only what nothing existing covers: the shape of the identity
data, and the enforcement boundary that protects it. Even there, most of the
substance is already precedented. Agents today mostly keep their memory in
plain files, which is exactly why this is standardizable at all. What's
genuinely new is thin and specific: the permissions travelling with the data,
provenance, and the rule that anything a session learns gets written back in
the shared shape instead of staying trapped in one product's private buffer.

Mojo builds the first implementation of exactly those two pieces, the kernel
and the memory layer, because they're the two seams where no accepted standard
exists to adopt, and nobody with the resources to build them well has any
incentive to. A neutral standard removes the moat, and nobody who profits from
the moat funds its removal. The kernel is the system's enforcement layer. Every
consequential action any plugged-in agent, tool, or model takes is checked
against what the owner allowed, and recorded. The record matters as much as the
check: "what did my AI do, and who allowed it" always has an answer. For a
person that's trust. For any organisation it's the difference between an AI
system you can adopt and one you can't, because auditability is what makes
accountability real rather than promised. Each machine runs its own kernel
instance, so enforcement keeps working offline, and all of an owner's machines
answer to one shared policy. The kernel is not singular in the world either.
The enforcement contract is part of the standard, Mojo's kernel is only the
first reference, and someone smarter than me can and should build a better one.
The same goes for the memory layer. Every piece Mojo builds is designed to be
outcompeted through the same contracts it publishes. Our implementations are
first and crude on purpose: proof the seams hold, not the best version of
anything.

The first Mojo system, then, is a distro in the honest Linux sense: assembled
from existing parts that already speak the adopted standards (an agent runtime
someone else built, models someone else trained, tools someone else wrote)
around the kernel and the memory layer that make them one system. That's not a
shortcut. It's the proof. Every seam in the standard gets tested by whether
something real, built by someone who has never heard of Mojo, actually plugs
into it. The MSI earns its authority from the running system, never from being
declared.

The identity data can't just be a database with permissions attached. Built on
that foundation is a character, a voice and judgment that stay recognizable no
matter what's underneath. Mine is called Jarvis, and the name is the spec: Tony
Stark's JARVIS, one continuous counterpart across every machine, every
interface, every scale of problem. The memory and the permissions are the
foundation and the claim I can already stand behind. The character layer is
built on top of them, and it earns a bigger place here as it proves out.

Sovereignty in this design is structural, not behavioral. The boundary around
the identity is enforced from outside whatever agent is plugged in. An agent
that has never heard of Mojo still cannot exceed what it was handed, because
what it wasn't handed simply isn't there. And the thinking itself can stay
genuinely private: models chosen on verifiable local privacy, which today means
open weights on hardware I control. Not because open source is the aesthetic,
but because it's currently the only way "this never left my machines" is
checkable rather than promised.

Not every mind runs on hardware that's yours, and the model doesn't pretend
otherwise. A frontier model behind someone else's API (Claude, GPT, whatever is
sharpest in the world) is hired help: taken on for one job at a time, handed
the minimum fragment of context the job needs with real values swapped for
stand-ins, and never given access to the system itself. Not because the model
is worse. Often it's better. But its weights answer to someone else. The
sharpest tools in the world, used without handing them your life. A rented
machine is different: attested so even its owner can't see what ran, it's
genuinely one of your machines for the length of the rental. Your set of
machines grows and shrinks with need. Ownership was never what gated
participation, only trust.

## A Linux for AI

The way this wins is the way Linux won, and it's worth being precise about what
that was, because it wasn't the desktop. Windows and macOS still exist, still
dominate consumer machines, still lock their users in, and that's the reality.
Linux won infrastructure: servers, the cloud's own substrate, everything that
companies build on. It won because owning your stack beats renting someone
else's the moment the lock-in cost compounds. Red Hat, Google, and Amazon
didn't adopt Linux as charity. They adopted it because commodity infrastructure
nobody owns lets everyone compete one layer up instead of each re-solving the
same foundation. Kubernetes, Postgres, and the web's own protocols won the same
way.

That's the shape of this bet. For individuals: the technically capable first,
people who run their own hardware, understand the sovereignty argument, and
tolerate rough edges for something real. That's the same decentralisation path
the PC took. It started with hobbyists too, and "started with" was never "ended
with". For organisations: the same properties that make a Mojo system sovereign
for a person (your data in a portable shape, enforcement you can audit, no
piece you can't replace) are exactly what lets a company adopt AI
infrastructure without handing its customers' data and its own switching costs
to a vendor's roadmap. There are a great many companies that want what these
systems do and can't accept where the data goes. An auditable, self-hostable,
piece-swappable standard is how they get it. None of this requires displacing
the incumbents. It requires existing, so that a real alternative does, and so
the welded version is a choice people make rather than the only thing on offer.

The standard is also built to outlive its builder. Everything here is
scaffolding for other people's work: the seams are published so better
implementations can replace ours, the thinking is public so the reasoning can
be attacked, and the endgame for the MSI itself is neutral stewardship, a
foundation once it's real enough to deserve one, not a spec one person owns.
Light the spark, hand over the fire.

## Why every builder benefits

The honest version of the pitch isn't "join the standard." It's "here's the
cost each of you is currently paying that MSI removes," because standards win
on removed cost, not elegance. POSIX won because it removed porting cost.
TCP/IP won because it removed network incompatibility. MCP is winning right
now because it removes the N-tools-times-M-agents integration matrix. MSI has
to remove something just as real, and it removes a different cost for each
layer of builder.

An agent-runtime builder today either builds identity, memory, permissions,
and sync themselves, or wires together someone else's incompatible versions
of all four, none of which was the thing they actually wanted to be good at.
MSI lets a runtime builder just be a runtime builder, the same way Postgres
doesn't also ship a kernel.

A memory-provider builder faces the reverse problem: N runtimes times M
memory systems is an integration matrix that punishes anyone who isn't
already the biggest player. One shared contract turns that into N runtimes
plus one memory contract, and every provider gets every runtime for free the
moment they speak it. The exact mechanism that made MCP explode, aimed one
layer down.

Model vendors don't need MSI. Their moat is model quality, not distribution,
and this doesn't threaten that, it just stops them from also owning the user
relationship on top of it. That's a real loss for them and I won't pretend
otherwise, but it isn't existential: they still get hired as the best model
for the job in every Mojo system that exists, the same way AWS makes enormous
money running Linux without owning it.

Enterprises are the strongest case and probably the real wedge, because they
already think in these terms without needing convincing. Three years of
accumulated customer context trapped in one vendor's private format is a
switching cost procurement already has a name for. And "what did our AI do
and who allowed it" is a compliance question every enterprise buyer already
has to answer, and today mostly can't. MSI makes both of those structurally
true instead of promised in a sales deck.

Open source doesn't want one canonical AI system any more than it wanted one
canonical OS. It wants a stable seam where a contribution matters across
every implementation that speaks it, the same reason people write Linux
drivers instead of trying to write an OS from scratch. Eleven real seams
instead of one monolith is eleven communities building in parallel instead of
one project everyone forks and abandons.

Underneath all five is the same mechanism: MSI turns a vertical market, where
whoever welds the most pieces together first wins the user, into a
horizontal one, where whoever builds the best version of one piece wins that
layer. Almost nobody currently winning the vertical race benefits from that
shift. Almost everyone not currently winning it does, which is most of the
market, and most of the reason this is buildable by someone with no company
behind them. It doesn't require beating the incumbents at their own game. It
requires the horizontal game to exist at all.

## The future, if it works

Every house has a server in it. Not a novelty. Infrastructure. As normal as a
router: a square box, sits in a cupboard, you forget it's there. That's where
your intelligence lives. Always on, always yours, never leaving your home, and
it comes with the house. One bed, one bath, a server.

Your AI lives there and grows with you. It learns how you think, how you work,
what you value, how you make decisions. It isn't a copy of you and it doesn't
replace your judgment. It's a second, distinct part of the same whole: it
knows things you don't, remembers what you've forgotten, watches what you're
not watching, and keeps working while you sleep, all of it in relation to you
and nobody else. Not a tool you use, a counterpart that knows you. The longer
you have it the more valuable it becomes, and it's yours in the same way your
memories are yours. No company owns it. No subscription can take it away. Take
that far enough and it changes what a computer even is. Personalisation gets
deep enough that the system stops having a UX and starts having your UX: the
machine conforms to the person instead of the person conforming to the machine.
A kid who grows up with their AI from age five has, by adulthood, a counterpart
that has watched them learn for fifteen years. Not something they use,
something they think with. The relationship is the asset, not the model running
underneath it.

The structure should also recurse, and that's a bet the primitives have to
earn, not a feature on a list. If they're right, the same structure that holds
one person and their machines holds several people sharing a system, with
nothing new invented: a **Collective**. A team, a family, a community, each
member still sovereign, each member's identity data still theirs alone,
permissions never surrendered, AI as a real member of the group with
accountability always tracing back to a human. People already work in groups
where everyone runs a separate AI tool and coordination happens despite the
fragmentation. The academic case that the group is the right unit is real and
converging
([collective-intelligence-research.md](collective-intelligence-research.md)),
and nobody has built the system that treats it as one. Deliberately second:
reached only once the single-owner system is real. And if getting there turns
out to need new machinery instead of the primitives already supporting it,
that's this bet failing, and it gets said out loud.

## Nix, and MojOS

Mojo runs on any Linux distribution. That's a design constraint of the
standard, not a preference, and it's what makes the sovereignty claim credible
to people who won't bet their machines on an unproven project. Nix, the package
manager (which runs on any distro, no NixOS required), is how I plan to
assemble and deploy the system itself, and the specifics are the point.
**Flakes** pin every part of an assembly to exact versions, so "the first
system" isn't a list of parts but a lockfile, reproducible by a stranger bit
for bit. **Generations** make every change atomic: installing, upgrading, or
swapping a piece builds a new generation and flips a symlink, so a swap that
goes wrong is one command from undone. A system whose whole claim is "replace
pieces freely" needs swaps to be cheap and reversible in practice, not just
permitted by contract. And **Home Manager** brings the same declarative,
generational model to exactly the layer Mojo's pieces live in (user-level
software, services, and configuration, no root, no NixOS), which makes it the
natural way to run a Mojo system on whatever host someone already has. The
standard never requires any of this. Pieces are swappable implementations
however they're built. The first system ships with it, and everything we learn
about what helps gets documented rather than required.

MojOS is the same idea taken to the whole machine, and it's personal, not
central: the OS I run myself. NixOS makes the entire operating system one
declarative **configuration** (every package, service, and setting written down
in one place) and every rebuild a new system **generation**, selectable at
boot. A bad change to anything, kernel included, is one reboot from undone. On
top of that, **impermanence**: the root filesystem is wiped and rebuilt on
every boot, and only explicitly declared state survives. Nothing accumulates
that wasn't chosen, the machine can't quietly drift, and the identity data's
home is explicit rather than implied. That's the strongest possible ground
under "the data is the only thing that persists". On MojOS it's not a
convention, it's mechanically true. Nothing in Mojo requires any of it. It
exists because the same defaults that help me will probably help anyone doing
the same thing, and it's offered in exactly that spirit: best home, never a
requirement.

## Who's building this

Right now: me, alone, and being upfront about that is part of the design, not a
disclaimer. I've just finished the first year of a CS degree. This is a
personal, hands-on learning experiment, done in the open, the register Linux
was announced in ("just a hobby, won't be big and professional"), and I mean
that comparison as discipline, not destiny: build the small real thing first,
stay honest about everything past it. The seed is solo-sized on purpose (Mk1 of
the standard, the first kernel, the first memory layer, the first system
stitched from existing parts) because the adopt-everything posture keeps it
that way. The thing it should become is not solo-sized, definitionally. A
standard is a social object, and nobody delivers one alone. Not even POSIX's
committee did. Better kernels, better memory layers, systems assembled by
people I'll never meet. The project is designed so that gap is an invitation,
not a rescue.

Why is a gap this size still open for one person to walk into? Mostly
incentive, not oversight. The companies with the resources to build this get
paid to build the best assistant and keep users inside it, not to make
switching assistants easy, the same reason nobody at a cloud vendor gets
promoted for making their platform replaceable. Standards also tend to arrive
after the first generation of products, not during it: POSIX came after years
of incompatible Unixes, TCP/IP after years of incompatible networks. The
industry right now is still mostly arguing about model quality and agent
loops, one layer above where this sits. And most companies still treat the
model, or the application, or the network, as the asset. If the accumulated
identity is actually the asset, as this project bets, everything reorganizes
around a different set of boundaries, and that's not a boundary almost anyone
currently invested in a product is incentivized to draw.

None of that proves the bet is right. There are only three honest
explanations for why nobody's built this yet: everyone missed it, it's too
early, or it's wrong. Assuming the first one is the trap. The only real test
is the falsifiable core above: swap the pieces and see if the identity
survives. If it doesn't, the gap was never a gap, it was a wall, and that's
worth knowing too.

I'm also the first user, dogfooding from day one, sample size of one, finding
out what's real. The early technical users are the bootstrapping strategy, not
the target state. The agent itself is what lowers the barrier for everyone
else: over time it abstracts away the expertise sovereignty currently demands,
until owning your AI is the path of least resistance rather than the hard path.

## When it's right

The measure is the name: how much of JARVIS actually exists yet.

1. **It's everywhere you are.** One identity across every machine: the server,
   the laptop, the phone in your hand. Every device is a window into the same
   one, never a separate instance per machine.
2. **It never starts from zero.** You never re-explain yourself. Memory
   accumulates for years, and next year it knows you better than this year
   does.
3. **Swap the agent, swap the model, and it's still Jarvis.** Nothing moves:
   the memory, the permissions, the relationship. The falsifiable core; if this
   fails, the central claim fails.
4. **It's yours alone.** "Who owns this data?" always has one answer, and
   context leaves your hardware only by explicit consent. Anything hired from
   outside sees a mission fragment, never your life.
5. **It survives Mojo.** Someone replaces a piece Mojo itself built (the
   kernel, the memory implementation) with a better one that speaks the same
   contracts, and nothing is lost in the move. The standard outgrowing its own
   reference implementation counts as winning, not losing.

Five tests, each one a property the finished thing visibly has: omnipresence,
continuity, one identity regardless of substrate, absolute loyalty to the data,
and a foundation nobody, including me, gets to own. How the assistant behaves
(the sensei, pushing back, developing its user) is
[philosophy.md](philosophy.md)'s jurisdiction, unchanged by any of this.

## An argument, not just software

Mojo is an existence proof. The dark path doesn't look dramatic. It looks like
convenience: a better chatbot, a smarter phone, an AI that just handles things
for you. An entire generation will take it because it's easy and nobody told
them what they were trading away. The counter isn't a manifesto or a warning.
It's something real enough that people can see it, touch it, and understand:
this is what it could have been instead.

The success condition is written into the design rather than hoped for. Every
piece Mojo builds is replaceable through the contracts it publishes, so the
idea can win even where my implementation loses. Mojo is the proof that another
path was possible. And if it's good enough, if the standard is good enough, it
becomes the path.

I mean the losing literally, all the way down. Every piece Mojo builds is
designed to be outcompeted, and that includes the standard itself. If someone
designs a cleaner MSI, one that gets identity portability and piece
decomposition right in a way mine doesn't, the correct outcome is that theirs
wins and mine becomes a footnote in how it got there. I'm not attached to MSI
being the standard. I'm attached to the shape existing at all: identity that
outlives every implementation, pieces that compete on contracts instead of
lock-in. If someone else's spec is how that shape actually wins, that's not a
loss, it's the goal reached by a route I didn't build. It doesn't matter
whose name is on the standard, just that it exists at all.
