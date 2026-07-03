# Handoff — Restructure Docs Across mojo-labs

## The situation

`mojo`, `mojos`, `mojo-agent`, `dotfiles`, and the archived `ringmaster` are all sibling
repos under the `mojo-labs-circus` GitHub org. Their documentation is a mess of
half-finished plans, scattered thinking, and structure that was proposed but never fully
applied — mostly sitting in `mojo/raw/`, which was set up as a dumping ground while all
of this got figured out. Nothing in `raw/` should be treated as decided just because
it's written down.

## What I want

Make every repo's documentation genuinely good — the way a real open-source project or
a professional engineering team would do it. Not a one-time cleanup: these repos are
going to keep growing for a long time, so the structure needs to actually work as a
living knowledge base, not just look tidy on day one.

I'm handing you this because I trust you to figure out the right way to do this better
than we have so far. Don't take anything already proposed in `raw/` (conventions,
directory layouts, ADR formats, whatever) as correct just because a prior session wrote
it down — research how real projects and teams actually structure and maintain their
docs, and build from that. Prior thinking in this repo is raw material you can draw on,
not a spec to implement.

## Step 1 — Understand everything before touching anything

- Read all five repos in full: `mojo`, `mojos`, `mojo-agent`, `dotfiles`, `ringmaster`
  (archived, but its thinking still needs accounting for).
- Read `mojo/raw/` in full — this is where most of the scattered thinking lives.
- Check the `mojo-labs-circus` GitHub org itself — the org profile, every repo it
  contains, and anything else there (issues, projects, other repos not cloned locally)
  that adds to the overarching picture.
- Ask me directly about anything confusing, contradictory, or missing rather than
  guessing or picking silently between conflicting prior versions.

**Checkpoint — do not proceed past this point without me confirming.** Once you've read
everything, stop and tell me what you think this project actually is: the vision, the
phases, how the repos relate, and the current real state of each one. I need to confirm
you've understood it correctly before any docs get restructured.

## Step 2 — Produce a plan

Once I've confirmed your understanding, work out (independently, grounded in how real
projects actually do this — not inventing a house style) what good documentation looks
like for a project like this: README, agent-instruction files, and a `docs/` knowledge
base per repo, structured around the project's actual phases so Phase 1 is front and
center everywhere and later-phase thinking has a home without cluttering it.

Write this up as a concrete, ordered, step-by-step plan — broken into chunks small
enough that I can hand each one to you separately and implement it without burning a
huge amount of context in one go. Give me the plan before executing any of it.

## Step 3 — Execute

Work the plan step by step. Each repo ends with a real README, real agent-instruction
files, and a real, populated `docs/` tree — nothing referenced but missing, nothing
empty and scaffolded "for later." Fully triage `mojo/raw/` in the process — everything
in there either finds a proper home or gets deliberately dropped.

## Why this matters now

We've spent a long time planning and not enough time producing anything. I want this to
be the thing that finally gets us to real output — once the docs are right, we start
building.

## Done when

Every repo has documentation good enough that a stranger — human or AI — could land on
it cold and immediately understand what it is, what phase it's in, and what to do next.
`mojo/raw/` no longer holds anything that already has a better home.
