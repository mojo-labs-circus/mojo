# mojo

Mojo is a standard first: the **Mojo System Interface (MSI)**, effectively a
POSIX for personal AI. It defines every piece a complete personal AI system is
made of and every seam between them, so that the system's identity (a digital
counterpart: your memory, your permissions, your assistant's character, yours
alone, as plain portable data) survives any change of the pieces underneath it.
Swap the
agent runtime, the model, the memory provider, even the kernel, and nothing
that matters moves. Anyone can build a better implementation of any piece; the
owner runs whichever they want. Mojo builds its own system as the proof,
assembled from existing parts around the two pieces nobody else has an
incentive to build (the kernel and the memory layer), and every piece it builds
is designed to be outcompeted through the same contracts it publishes.

The whole system, drawn piece by piece and seam by seam, is
**[anatomy.md](anatomy.md)**: the most concrete statement of what Mojo is, and
the document everything else here is written against.

This repo holds no code. It's the project's brain: the vision, the philosophy,
the plan, and every idea worth keeping. Written for me and for the AI sessions
I build with, and public because the thinking is part of the work.

| File | What it is |
|---|---|
| [anatomy.md](anatomy.md) | What a Mojo system is: every piece, every seam, and the one thing that never swaps |
| [vision.md](vision.md) | The future this builds toward |
| [philosophy.md](philosophy.md) | What I believe and why this exists |
| [interoperability.md](interoperability.md) | The full argument behind the mission: why the standard has to exist |
| [roadmap.md](roadmap.md) | Now / next / horizon |
| [research-plan.md](research-plan.md) | The research tracker for the standard's first version |
| [collective-intelligence-research.md](collective-intelligence-research.md) | The academic case for Collectives as a real, converging field |
| [ideas.md](ideas.md) | The net: every live idea, one line each |
| [devlog.md](devlog.md) | Dated thinking, newest first |

**Currently:** the anatomy is the map ([anatomy.md](anatomy.md), landed
2026-07-10), and the research plan that turns each of its seams into the MSI's
first version is being re-derived from it. The standard is built the POSIX way:
adopt what exists (MCP, SKILL.md, A2A, the model wire format), define only what
nothing covers (the identity's data shape, the enforcement contract), and leave
the market's problems deliberately open.

The first Mojo system gets assembled once the standard covers every piece. A
distro in the honest sense: existing compliant parts (an agent runtime someone
else built, models someone else trained, MCP tools, SKILL.md skills) stitched
around Mojo's own kernel and memory layer, doubling as the standard's test
suite. Anything that speaks the standard can plug in.
[mojos](https://github.com/mojo-labs-circus/mojos) is the deepest host, MojOS.

---

Docs: [CC BY-SA 4.0](LICENSE)
