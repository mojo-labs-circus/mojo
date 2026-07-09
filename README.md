# mojo

Mojo is a standard first: the **Mojo System Interface** — effectively Mojo's own
POSIX — defining the contract at every seam a complete personal AI system is
made of, so that a persistent AI identity (a Jarvis: your memory and your
permissions, yours alone) can be carried into whatever agent or model is
actually doing the thinking. Swap any piece and nothing that matters moves.
Mojo builds its own system as the proof — stitched from existing parts around
the two pieces nobody else has an incentive to build, the kernel and the
memory — and every piece it builds is designed to be outcompeted through the
same contracts it publishes. Scales from one machine, to your fleet, to
Collectives of people and AIs working together.

This repo holds no code. It's the project's brain: the vision, the philosophy, the
plan, and every idea worth keeping — written for me and for the AI sessions I build
with, and public because the thinking is part of the work.

| File | What it is |
|---|---|
| [vision.md](vision.md) | The future this builds toward |
| [philosophy.md](philosophy.md) | What I believe and why this exists |
| [interoperability.md](interoperability.md) | The full argument behind the mission — shape and seam, why the standard has to exist |
| [roadmap.md](roadmap.md) | Now / next / horizon |
| [research-plan.md](research-plan.md) | The current phase's live tracker, piece by piece |
| [collective-intelligence-research.md](collective-intelligence-research.md) | The academic case for Collectives as a real, converging field |
| [ideas.md](ideas.md) | The net — every live idea, one line each |
| [devlog.md](devlog.md) | Dated thinking, newest first |

**Currently defining:** the Mojo System Interface, Mk1 — structured the POSIX
way: primitives that hold, schemas built on them, contracts at the seams. A
first-pass, complete-coverage answer for every piece of the system, checked
against real precedent (Unix, seL4, Plan 9, Erlang/OTP).
[research-plan.md](research-plan.md) is the live tracker; [roadmap.md](roadmap.md)
has the full phase breakdown.

The first Mojo system, assembled once the standard covers every piece: a distro
in the honest sense — existing compliant parts (a harness someone else built,
models someone else trained, MCP tools, SKILL.md skills) stitched around Mojo's
own kernel and memory, doubling as the standard's test suite. Mojo is
agent-agnostic by construction — anything that speaks the standard can plug in.
[mojos](https://github.com/mojo-labs-circus/mojos) is the deepest host, MojOS.

---

Docs: [CC BY-SA 4.0](LICENSE)
