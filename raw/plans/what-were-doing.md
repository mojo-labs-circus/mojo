# What We're Doing

For Clarke, and for any future collaborator — human or AI — who needs to understand why the project is structured the way it is and what we're working toward.

---

## The Vision

Mojo is a collective intelligence framework. The goal is a system where human and AI members collaborate as genuine peers on a shared codebase — not AI as tool, but AI as team member, with roles, responsibilities, context, and continuity across sessions.

The end state: a development collective where humans set direction and make final calls, AI agents handle research, implementation, review, and verification, and the whole thing compounds — each session building on the last, each mistake encoded as a rule, each decision preserved and accessible to every member of the collective.

---

## Why We're Not Writing Code Yet

There are four things that need to happen before implementation starts, and the order matters.

### 1. Framework Design

What is Mojo, exactly? How does the collective work? What roles exist? How do humans and AIs collaborate? What are the governance rules? What does membership mean?

These answers shape everything downstream: the project structure, the agent configuration, the hooks, the conventions, the rules that govern all collaborators. You cannot design infrastructure for a collective intelligence framework without knowing how the collective works.

The framework sessions are already in progress under `mojo/docs/framework/`. The outputs may be spread across files in a messy state. That is fine — the goal during these sessions is to get the thinking right, not to produce clean documentation. Once the design is complete, we synthesize and build the proper structure from those findings.

**Framework sessions come first, or run as the top priority alongside everything else.**

### 2. Claude Code Setup

Configure the tooling to the highest possible standard before writing any code.

Memory in the right places. Hooks that automate the workflow and enforce rules deterministically. Agents that verify their own work. Sessions that hand off cleanly when context fills up. Slash commands for every repeated workflow. MCP servers that give Claude access to the full environment. Settings that encode deliberate decisions rather than defaults.

This is about compounding. A well-configured tool makes every future session faster, more reliable, and lower friction. A poorly configured one accumulates friction that compounds in the wrong direction — mistakes not caught, context not preserved, workflows that require manual intervention every time.

**Claude Code setup runs in parallel with framework sessions.** It is independent of Mojo specifics and should not wait.

### 3. Mojo Project Setup

Once the framework is designed and the tooling is right, structure the project correctly.

CLAUDE.md hierarchy that reflects what Mojo actually is. `.claude/` config committed to git so every future collaborator — human or AI — gets the same environment when they clone the repo. GitHub Actions that review PRs automatically. Verification agents per runnable component. Branch and PR conventions. Hooks that govern the collective, not just a solo dev.

This session cannot be done well without the framework design outputs. The collective model, the roles, the governance rules — these inform every config decision.

**Mojo project setup happens after framework sessions are complete.**

### 4. Dev Conventions Research

What do the best engineering teams actually do? How do top software companies structure code review, testing, documentation, releases, and cross-team collaboration? What is transferable to a project like this?

This research feeds back into the project conventions as implementation begins. It is not a prerequisite for setup, but it is a prerequisite for building well.

---

## The Sequencing

```
Framework sessions  ──────────────────────────────────────┐
                                                           ↓
Claude Code setup   ──────────────────┐          Mojo project setup
(parallel)                            ↓                   ↓
                             Dev conventions research
                                       ↓
                                  Implementation
```

Framework sessions and Claude Code setup run in parallel. Both feed into Mojo project setup. Then dev conventions. Then code.

---

## The Compounding Principle

Every mistake encoded as a rule. Every decision written down. Every session handing off cleanly to the next. Every repeated workflow reduced to a slash command. Every quality check automated rather than manual.

Foundations that compound produce better results on every session after they are built. Debt that accumulates does the opposite — it does not stay flat, it grows.

The setup work takes time upfront. That is the point. We are not trying to move fast now. We are building the platform that makes everything after it move fast and stay clean.

---

## What "Done" Looks Like

**Framework designed.** The collective intelligence model is fully specified: roles, responsibilities, governance, how humans and AIs collaborate, what membership means, what the boundaries are. Not perfect — but decided. Ready to build against.

**Claude Code configured.** Every session starts with full context. The tool verifies its own work. Sessions hand off cleanly. Mistakes become rules. A future collaborator can start a session and have the same environment, the same constraints, the same quality gates as anyone else who has worked on this.

**Mojo project structured.** The directory layout, the CLAUDE.md hierarchy, the committed `.claude/` config — all reflect what Mojo actually is and how the collective works. Any member of the collective, human or AI, reads the CLAUDE.md and understands the project, the conventions, and their role.

**Dev conventions established.** The engineering practices that govern implementation — code review, testing, documentation, release process — are researched, decided, and encoded. Not invented, borrowed from what works.

Then we build.

---

## Plans

- `plans/claude-code-setup.md` — 9 sessions to configure the global Claude Code installation
- `plans/mojo-project-setup.md` — 7 sessions to structure the Mojo project (update: add collective model session before GitHub integration)
- `mojo/docs/framework/` — the framework design sessions in progress
