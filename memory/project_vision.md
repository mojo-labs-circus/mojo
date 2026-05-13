---
name: project-vision
description: Core mission, north star concepts, and key framing decisions for MojOS Circus
metadata:
  type: project
---

## North Star

Jarvis from Iron Man. The personal agent is the first mate — the constant link between Clarke and all his projects and tools. Clarke is the captain. Enterprise AIs (Claude Code, Codex, Cursor, etc.) are the scallywags — best in class at their jobs, managed by the first mate, never needing the full picture.

The personal agent orchestrates everything, acts as the privacy layer (gives enterprise AIs only the bare minimum for each task), and compiles outputs back into something relevant to Clarke. Enterprise-grade capability without enterprise-grade exposure.

## Two Audiences, One Platform

**Everyone:** A private personal agent that knows you, runs on your hardware, works across your machines, and never hands your data to someone else.

**Developers:** The platform for building what comes next. Anti-vibe-coding. The developer operates as CTO with an AI engineering org — conceptual ownership of everything, line-level ownership of nothing. Boris Cherny with a full team. Tony Stark with the suit.

## The .mojo Language (future, not Mk1)

A block-based DSL where a `.mojo` file is a machine-readable spec. Running it spins up a full AI company (Architect, Coders, Reviewer, QA, Ethics, Watcher). The Architect can generate subsidiary `.mojo` files for complex components — recursive hierarchy, each independently watchable. Same file runs on any machine, scales to available hardware. Versioned artifacts in the repo.

Captured in `spec/ringmaster/ideas/mojo-language.md`. Not a Mk1 priority — foundation first.

## Key Principles

- Anti-vibe-coding is the overarching principle for the developer layer
- Nothing personal ever touches enterprise cloud products
- Private compute is the real moat, not the .mojo language concept
- The river keeps flowing — the developer layer is for people who build the next foundation

## Mk1 Goal

ringbaker running as Ringmaster serving the family via web client. pearlybaker and nomadbaker running MojOS with a local agent connected to ringbaker.

**Why:** Keep this in mind when scoping work — don't let future ideas pull focus from Mk1.
**How to apply:** Flag when conversations drift toward .mojo or advanced features that aren't Mk1. Keep implementation sessions anchored to the Mk1 goal.
