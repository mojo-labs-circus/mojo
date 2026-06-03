# How to Run a Planning Session

> Read this before opening any session brief. It is the same every time.

---

## Session Status

| # | Session | Status |
|---|---------|--------|
| 01 | First Principles | pending |
| 02 | Agent Model | pending |
| 03 | Frontier Interface | pending |
| 04 | Org Structure | pending |
| 05 | Communication Model | pending |
| 06 | Leadership Interface | pending |
| 07 | Layers and Contracts | pending |
| 08 | Dev Team Instance | pending |

Update the status to **in progress** when a session opens, **done** when its section in `framework.md` is complete and signed off.

---

## What These Sessions Are

Each session is a focused research and design sprint. You are not just thinking —
you are learning from what already exists, finding the patterns that matter, and
producing output that the next session builds on.

Passion is the fuel. Output is the proof. Both matter.

---

## The Five Phases

### 0. Orient

Before anything else, the planning session reads the brief and gives Clarke a plain-English
overview:

- What this session is trying to understand (not design — understand)
- What the core questions are, and why they matter for the framework
- What the output will look like when we're done

Then we talk. Clarke asks questions, pushes back, flags what he already knows or
doesn't know. We go back and forth until Clarke is genuinely ready — not just
told what's happening. Only then do we move to Phase 1.

This phase has no time limit. It exists so the research has direction and Clarke
isn't just watching videos without knowing what he's looking for.

### 1. Find Resources First

Before reading the session brief in depth, ask your planning session to surface
learning material for that session's topic:

> "Before we begin, what are the best videos, talks, papers, and books on [topic]?
> Prioritise accessible material — YouTube talks, short papers, real case studies."

Watch at least one or two things before forming opinions. The goal is to let the
material change your thinking before you start designing. If nothing surprised you,
you haven't gone deep enough.

Good sources to look for:

- TED / TEDx talks on the topic
- Academic talks on YouTube (Santa Fe Institute is excellent for emergence and
  collective intelligence)
- Documentaries on natural systems (ant colonies, murmuration, the brain)
- Interviews with practitioners (military commanders, coaches, team leads)
- Foundational papers (often readable in an evening)

### 2. Research and Explore

Go deep. Follow threads that interest you. This is supposed to feel like learning,
not work. The passion is the point — it is what makes the output good.

Keep a running note of:

- **What surprised you** — these are usually where the real principles are hiding
- **What appeared in multiple domains** — these are the universal patterns
- **What contradicted your assumptions** — these need to be in the output

Don't rush this phase. But also don't let it become infinite. When you find yourself
revisiting the same ideas, you are done researching.

### 3. Synthesise

Now read the session brief's questions carefully. For each question:

- What did your research tell you?
- What is the most defensible answer?
- What is still uncertain — and does that uncertainty matter for the design?

The session brief tells you what the output must look like. Produce that output.
Write it clearly enough that a future session can take it as given without
re-deriving it.

### 4. Write Output to framework.md

All session output goes into `spec/framework/framework.md` — one section per session.
This is the rolling document that becomes the finished Mojo Framework when session 08
is complete. Write your section directly into it. Do not create separate output files.

Write clearly and concisely. Future sessions will read your section as established
fact and build on it. The whole document must read coherently end to end.

### 5. Check Before Moving On

Before closing the session, ask:

- Does the output in framework.md actually answer all the questions in the brief?
- Could someone read that section and build the next session on it confidently?
- Is anything stated as decided that is actually still uncertain?

If yes to all three, you are done. Move to the next session.

---

## The Balance

Research feeds output. It does not replace it.

A session with beautiful research and no clear output is not done. A session with
quick output and no research behind it is not trustworthy. The right session ends
with output you believe in because you earned it.

When you hit the output required section and can write it clearly and confidently —
that is the signal to stop.

---

## On Depth vs Speed

These sessions are not meant to be fast. They are meant to be correct. The whole
point of doing this before writing code is to get the foundation right. Rushing
the foundation is the same mistake as not having one.

That said: done is better than perfect. When you have principles you believe in
and can defend, move on. You can always come back.

---

## Session Order

Sessions must run sequentially. Each one takes the previous as given.

```
00  ← this guide (read before every session)
01  first-principles       ← start here
02  agent-model
03  frontier-interface
04  org-structure
05  communication-model
06  leadership-interface
07  layers-and-contracts
08  dev-team-instance      ← finish here, then dev resumes
```

Each session follows the same five phases: Orient → Resources → Research → Synthesise → Write → Check.
