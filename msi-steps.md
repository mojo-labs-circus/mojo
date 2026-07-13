# MSI-1 Steps

*The procedural tracker for getting MSI-1 drafted. This file says what to work
through next; [research-plan.md](research-plan.md) says why and holds the real
status (its Status columns are the truth, checkboxes here just mean "Clarke
and I worked this step"). This is not a queue for an agent to run
unsupervised — every step is a session Clarke and I work together, precisely
so a cheaper model can drive it: the model's job is to bring the prior art and
lay out the real options, Clarke's job is to actually decide, and nothing gets
marked landed on the model's say-so alone.*

## How to run a session against this file

1. Read the last devlog entry, then find the first unchecked step below.
2. Work only that step with Clarke. One step per session (a step spanning
   several sessions is fine; two steps in one session is not).
3. Before phase 3 asks for any decision, first **teach**: read the seam's row
   in research-plan.md's seam tracker, then the actual sources it names (the
   real spec documents, real code, real files on disk, not summaries of
   them), and explain them to Clarke well enough that he could restate the
   real trade-off himself. Only once that's landed, lay out the candidate
   answers and their consequences and let Clarke pick, or push back and argue
   for one — his call either way, argued out loud, not decided for him.
   Phase 2 is lighter than this (confirming attribution, not drafting
   contract text), but nothing there gets marked confirmed on the model's
   say-so either.
4. Record outcomes where they belong: prose fixes in anatomy.md (+ mirror in
   anatomy.html), gaps and decisions in research-plan.md's tracker rows,
   drafted contract text in msi.md, reasoning in devlog.md.
5. Before ending: tick the step here, update research-plan.md's Status
   column and its "Currently on" line, write the devlog entry.

**Guardrails, non-negotiable:**

- Nothing gets marked Decided without Clarke explicitly confirming it in the
  session. A model working alone, or a Clarke message that only implies
  agreement, isn't enough — get the actual call.
- If a decision would change the piece list, add or remove a seam, or sit
  oddly against vision.md/philosophy.md, stop and put that to Clarke directly
  rather than folding it quietly into the step's normal decision.
- A seam that resists landing after one real session gets a provisional
  answer plus an explicit open-question flag in its row, then move on
  together (the timebox rule). Never leave a row silently half-done.
- Only mark a seam Decided if a stranger could build a compliant piece from
  the drafted text without asking questions. Otherwise it stays Deciding,
  with the gap named.
- Never mark anything Decided just because a prior note leaned that way.
  Inputs are not outcomes, and Clarke re-deciding something the tracker
  leaned on before is expected, not friction.
- Public-doc conventions hold everywhere: plain names, no em dashes, nothing
  described as existing unless it exists.

## Phase 1 — piece pass

For each piece, work through it with Clarke, in this order: (1) go find and
check real prior art relevant to this specific piece — how existing
digital-counterpart systems and adjacent standards actually shape it and the
seams around it. Same discipline POSIX used against real Unix systems:
nothing here is being invented from nothing, it's being standardized. (2)
Only then define or fix the piece's prose in anatomy.md, grounded in what
that prior art showed but written as the abstract contract any compliant
implementation must satisfy, never as a description of how one particular
existing system happens to do it. Piece prose says what a piece IS, on its
own terms: never define it by what it isn't or by contrast with a sibling
piece (that content belongs in the sibling's own prose instead), and never
restate the system-wide invariant that every piece is a swappable
implementation, that's said once, at the top of the document, not per
piece. (3) Confirm its seam-sides in research-plan.md's Pieces table are
right and complete against the same prior art. (4) Hunt gaps by asking
Clarke "what would a builder of this piece still not know," recording each
gap as a question in the right seam's tracker row. (5) Flip its
Pieces-table Status once Clarke agrees it's covered. A piece pass that
finds nothing new is a pass, not a failure. anatomy.html is brought back in
sync once, at phase 1's close (step 1.13), not per piece.

- [x] 1.1 Client
- [x] 1.2 Peripheral (retired: collapsed into Tools, no piece, no profile,
      no seam; see research-plan.md's Pieces table)
- [x] 1.3 Harness
- [x] 1.4 Model endpoint
- [x] 1.5 Tools (no profile)
- [x] 1.6 Memory provider
- [x] 1.7 Sandbox
- [x] 1.8 Router
- [x] 1.9 Kernel
- [x] 1.10 Credential broker
- [x] 1.11 Provenance
- [x] 1.12 Fleet manager
- [x] 1.13 Close phase 1: read anatomy.md end to end once for coherence,
      confirm every seam row holds the questions phase 1 raised, sync
      anatomy.html to match anatomy.md, update the "Currently on" line to
      phase 2.

## Phase 2 — seam pass

**Suspended 2026-07-13.** See devlog.

For each seam, work through it with Clarke, prior art first, same
discipline as phase 1, adapted for seams: (1) For the four adopted seams
(e, f, h, l), the prior art is the real spec itself (the OpenAI-compatible
wire format plus the Anthropic Messages API, MCP, SKILL.md/agentskills.io,
A2A), read for which component on the implementer's side actually makes
and receives that call. That's the real evidence for piece attribution,
not analogy. (2) For the MSI-posture seams there's no interchangeable protocol
to check, that's the whole point of MSI posture, but the seam still exists
in every real digital-counterpart system, just bolted in permanently
rather than swappable; prior art there means reading how OpenClaw, Hermes,
Letta, and Khoj actually wire that specific boundary internally, to find
the real shape of the relationship, not to find a spec. (3) Only once
prior art is checked does the seam's anatomy.md ledger prose get
tightened: what it actually does, and which piece(s) actually touch it,
fixing whichever is wrong, missing, or silently assumed. (4) A question
that seems like it belongs to this seam but is actually already answered
by another seam's existing mechanism gets routed there and cross-
referenced, not re-decided or duplicated. (5) A
seam with no piece attribution anywhere and no blanket clause covering it
is a real gap: record it as an open question in the seam's research-plan.md
tracker row, don't invent an answer just to make a diagram render. A seam
pass that finds nothing wrong is a pass, not a failure. anatomy.html is
brought back in sync once, at this phase's close (step 2.20), not per seam.

- [ ] 2.1 Seam a (owner verification)
- [ ] 2.2 Seam b (proactive delivery)
- [ ] 2.3 Seam c (client contract)
- [ ] 2.4 Seam d (run assembly)
- [ ] 2.5 Seam e (model wire contract)
- [ ] 2.6 Seam f (tools)
- [ ] 2.7 Seam g (triggered runs)
- [ ] 2.8 Seam h (skills)
- [ ] 2.9 Seam i (run contract)
- [ ] 2.10 Seam j (enforcement contract)
- [ ] 2.11 Seam k (federation)
- [ ] 2.12 Seam l (agent-to-agent). Confirmed 2026-07-13 to have zero piece
      attribution anywhere in research-plan.md; this step actually decides
      it rather than re-confirming the gap.
- [ ] 2.13 Seam m (data shape)
- [ ] 2.14 Seam n (machine admission)
- [ ] 2.15 Seam p (secret custody)
- [ ] 2.16 Seam q (sandbox contract)
- [ ] 2.17 Seam r (content trust)
- [ ] 2.18 Seam s (memory access)
- [ ] 2.19 Missing-seam hunt: walk every pair of pieces that plausibly
      interact and confirm each real relationship has a lettered seam.
      Anything real that doesn't gets a new letter, added to anatomy.md's
      map, prose, and ledger, and to research-plan.md's seam tracker.
- [ ] 2.20 Resync anatomy.html to match whatever this phase changed in
      anatomy.md, once, at the close, the same pattern as 1.13.
- [ ] 2.21 Stress-test the whole map against real prior art: read how
      OpenClaw, Hermes, Letta, and Khoj are actually architected as whole
      systems, not seam by seam, and confirm anatomy.md's shape genuinely
      fits, the way POSIX had to fit the real Unix systems it standardized.
      Fix anything that doesn't.
- [ ] 2.22 Close phase 2: update research-plan.md's Status columns and
      "Currently on" line to phase 3 (the seam walk), write the devlog
      entry.

## Phase 3 — seam walk

For each step: brief Clarke on the seam's prior art (its research-plan.md row
names it) until he actually understands the trade-off, work out the answer
together (his call, argued not assumed), draft the msi.md section the same
session, set the row's Status once he's confirmed it. msi.md gets created at
the repo root by step 3.1 with the §0–§4 skeleton from research-plan.md's
target section, sections filled in as their seams land.

- [ ] 3.1 Seam e (model wire contract) → §4. Read the OpenAI-compatible API
      surface as actually implemented by llama.cpp/Ollama/vLLM, and the
      Anthropic Messages API for what the envelope must declare. Creates
      msi.md.
- [ ] 3.2 Seam f (tools) → §4. Read the MCP spec itself: pin a version, list
      the surface a compliant piece assumes.
- [ ] 3.3 Seam h (skills) → §4. Read the SKILL.md/agentskills.io format.
- [ ] 3.4 Seam l (agent-to-agent) → §4. Read the A2A spec; flag the
      composition-with-k question for step 3.17.
- [ ] 3.5 Seam a (owner verification) → §3. Root-of-trust question included.
- [ ] 3.6 Seam m (data shape) → §1 + §2. The long one; expect multiple
      sessions. Look at real agent memory on disk (Claude Code's own files,
      OpenClaw's), not just the papers. The primitive question (what the
      unit of data is, on top of Unix files) is this step's first question.
- [ ] 3.7 Seam s (memory access) → §3. First check the collapse-into-f
      candidate against what 3.2 pinned.
- [ ] 3.8 Seam j (enforcement) → §3. Hardest seam; expect multiple sessions.
- [ ] 3.9 Seam i (run contract) → §3.
- [ ] 3.10 Seam d (run assembly) → §3.
- [ ] 3.11 Seam g (triggers) → §3.
- [ ] 3.12 Seam p (secret custody) → §3. Prior art still to gather; that
      gathering is part of the step.
- [ ] 3.13 Seam q (sandbox) → §3.
- [ ] 3.14 Seam r (content trust) → §3.
- [ ] 3.15 Seam c (client contract) → §3.
- [ ] 3.16 Seam b (proactive delivery) → §3.
- [ ] 3.17 Seam k (federation) → §3. Hardest distributed seam; expect
      multiple sessions. Revisit 3.4's A2A-composition flag.
- [ ] 3.18 Seam n (machine admission) → §3.

## Phase 4 — assembly

- [ ] 4.1 Write §0: the ten conformance profiles, each as the list of
      seam-side obligations that actually landed in §1–§4.
- [ ] 4.2 Cross-seam contradiction check: walk every pair of seams that
      share a piece, confirm their obligations don't conflict.
- [ ] 4.3 Coherence read: msi.md end to end in one sitting, fixing prose
      only, no new decisions. Anything that wants a new decision gets a flag
      for MSI-2, not an edit.
- [ ] 4.4 Declare MSI-1 draft done; update roadmap.md (the Next milestone,
      the first runnable system, starts here).
