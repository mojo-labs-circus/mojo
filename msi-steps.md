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
3. Before phase 2 asks for any decision, first **teach**: read the seam's row
   in research-plan.md's seam tracker, then the actual sources it names (the
   real spec documents, real code, real files on disk, not summaries of
   them), and explain them to Clarke well enough that he could restate the
   real trade-off himself. Only once that's landed, lay out the candidate
   answers and their consequences and let Clarke pick, or push back and argue
   for one — his call either way, argued out loud, not decided for him.
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

For each piece, work through it with Clarke: (1) read its prose in anatomy.md
out loud together and fix what's unclear or wrong, mirroring any change in
anatomy.html; (2) confirm its seam-sides in research-plan.md's Pieces table
are right and complete; (3) hunt gaps by asking Clarke "what would a builder
of this piece still not know," recording each gap as a question in the right
seam's tracker row; (4) flip its Pieces-table Status once Clarke agrees it's
covered. A piece pass that finds nothing new is a pass, not a failure.

- [ ] 1.1 Window (Client)
- [ ] 1.2 Peripheral
- [ ] 1.3 Agent runtime (Harness)
- [ ] 1.4 Model (Model endpoint)
- [ ] 1.5 Tools (consumed format)
- [ ] 1.6 Memory provider
- [ ] 1.7 Sandbox
- [ ] 1.8 Router
- [ ] 1.9 Kernel
- [ ] 1.10 Credential broker
- [ ] 1.11 Provenance
- [ ] 1.12 Fleet manager
- [ ] 1.13 Close phase 1: read anatomy.md end to end once for coherence,
      confirm every seam row holds the questions phase 1 raised, update the
      "Currently on" line to phase 2.

## Phase 2 — seam walk

For each step: brief Clarke on the seam's prior art (its research-plan.md row
names it) until he actually understands the trade-off, work out the answer
together (his call, argued not assumed), draft the msi.md section the same
session, set the row's Status once he's confirmed it. msi.md gets created at
the repo root by step 2.1 with the §0–§4 skeleton from research-plan.md's
target section, sections filled in as their seams land.

- [ ] 2.1 Seam e (model wire contract) → §4. Read the OpenAI-compatible API
      surface as actually implemented by llama.cpp/Ollama/vLLM, and the
      Anthropic Messages API for what the envelope must declare. Creates
      msi.md.
- [ ] 2.2 Seam f (tools) → §4. Read the MCP spec itself: pin a version, list
      the surface a compliant piece assumes.
- [ ] 2.3 Seam h (skills) → §4. Read the SKILL.md/agentskills.io format.
- [ ] 2.4 Seam l (agent-to-agent) → §4. Read the A2A spec; flag the
      composition-with-k question for step 2.18.
- [ ] 2.5 Seam a (owner verification) → §3. Root-of-trust question included.
- [ ] 2.6 Seam m (data shape) → §1 + §2. The long one; expect multiple
      sessions. Look at real agent memory on disk (Claude Code's own files,
      OpenClaw's), not just the papers. The primitive question (what the
      unit of data is, on top of Unix files) is this step's first question.
- [ ] 2.7 Seam s (memory access) → §3. First check the collapse-into-f
      candidate against what 2.2 pinned.
- [ ] 2.8 Seam j (enforcement) → §3. Hardest seam; expect multiple sessions.
- [ ] 2.9 Seam i (run contract) → §3.
- [ ] 2.10 Seam d (run assembly) → §3.
- [ ] 2.11 Seam g (triggers) → §3.
- [ ] 2.12 Seam p (secret custody) → §3. Prior art still to gather; that
      gathering is part of the step.
- [ ] 2.13 Seam q (sandbox) → §3.
- [ ] 2.14 Seam r (content trust) → §3.
- [ ] 2.15 Seam o (peripherals) → §3. Prior art still to gather.
- [ ] 2.16 Seam c (window contract) → §3.
- [ ] 2.17 Seam b (proactive delivery) → §3.
- [ ] 2.18 Seam k (federation) → §3. Hardest distributed seam; expect
      multiple sessions. Revisit 2.4's A2A-composition flag.
- [ ] 2.19 Seam n (machine admission) → §3.

## Phase 3 — assembly

- [ ] 3.1 Write §0: the eleven conformance profiles, each as the list of
      seam-side obligations that actually landed in §1–§4.
- [ ] 3.2 Cross-seam contradiction check: walk every pair of seams that
      share a piece, confirm their obligations don't conflict.
- [ ] 3.3 Coherence read: msi.md end to end in one sitting, fixing prose
      only, no new decisions. Anything that wants a new decision gets a flag
      for MSI-2, not an edit.
- [ ] 3.4 Declare MSI-1 draft done; update roadmap.md (the Next milestone,
      the first runnable system, starts here).
