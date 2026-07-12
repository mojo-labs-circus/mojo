# Naming Conventions

*How things in this project get named. Rewritten 2026-07-10, when the
plain-naming policy landed alongside [anatomy.md](anatomy.md). The old
three-register scheme (nautical vocabulary as first-class terminology) is
superseded; it lives in git history if the reasoning is ever needed.*

---

## The policy: plain names in everything public-facing

Anything a stranger is meant to read and build against uses plain functional
English: the anatomy, the future msi.md, vision, README, and the seam and piece
names themselves. Owner, client, router, harness, model endpoint, kernel,
memory provider, the identity, federation, machine admission.

The precedent is POSIX itself: process, file, pipe, signal. No real standard
asks its readers to learn an invented vocabulary first. Themed naming lives in
products (Kubernetes is literally Greek for "helmsman"), never in the standards
they implement. Urbit is the cautionary tale on the other side: it invented its
own words for everything and paid for it in adoption. The MSI's job is to be
legible to system architects who have never heard of Mojo, and every bespoke
noun is a tax on that.

## The nautical register: an analogy we think with, not terminology we ship

The fleet analogy (a captain, a first mate, vessels, hailing between ships) is
genuinely good for thinking and talking about the system, and it stays
available for exactly that: conversation, ideation, internal notes. It just
doesn't get baked into documents as terminology anyone has to learn. If
nautical terms earn a public place later, they can be added back deliberately.
Adding flavor later is cheap; removing load-bearing jargon later is not.

One distinction the old scheme blurred, worth keeping precise even in casual
use: the identity is the data; the first mate (the persona, the character, the
"Jarvis") is what's built on that data. The data is the standard's concern. The
character is built on top of it.

## Product branding

A separate namespace entirely: Mojo (the project), the Mojo System Interface /
MSI (the standard), MojOS (the NixOS-based host), mojo-agent, mojo-package.
These name things that get shipped, not concepts in the standard.

## Standing naming hazard

**Mojo** itself collides with Modular's existing Mojo programming language. Not
urgent to resolve, but carried here since it's the one name in the whole
project that isn't ours alone. Worth settling before anything goes wide
publicly.
