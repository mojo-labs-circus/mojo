# Docs Maintenance Automation — Draft Thinking

## Purpose and scope

Distinct problem from `docs-convention-proposal.md`, which covers how docs get
**created** to start with. This covers how they stay **current and worked on** once
they exist — enforcement and reminder mechanisms via Claude Code hooks and skills.

**Status:** Thinking only. Not yet built, not yet tested against a real project.
Mechanism-level claims below are cross-checked against this repo's own prior research
(`raw/research/claude-code-findings.md`), which is sufficient for Claude-Code-specific
plumbing — this is not a "real professional convention" claim the way ADRs or Diátaxis
were, so it doesn't need the same external verification pass. What it does still need
is building and testing against an actual repo before it's trusted.

**Sequencing:** downstream of the docs-convention skill actually existing and having
scaffolded at least one real project. Building enforcement for a structure that isn't
in use yet is premature — there's no real signal yet on what actually goes stale in
practice, or which of these hooks turn out to be noise. Do not start building this
until the docs-convention skill is built and verified.

---

## Governing principle

Automate the reminder, never the judgment. A hook can flag "this looks worth
documenting" — whether to write it, and what it says, stays a deliberate act. Consistent
with the rest of the design: draft-status ADRs are a deliberate promotion, `ideas.md`
triage is a deliberate review, not something a hook should resolve on its own.

## Per-doc-type mapping

| Doc | Failure mode | Mechanism | Hook type |
|---|---|---|---|
| `AGENTS.md` | Commands drift from reality as the project changes | `PostToolUse` on edits to build config (`package.json`, `pyproject.toml`, etc.) — semantic check: does this change what's documented in AGENTS.md's build/test commands | Prompt hook (semantic) |
| `docs/decisions/` | A real architectural decision gets made in passing, no ADR written | `PostToolUse` on source edits, classifying "is this architecturally significant" — surfaces a note, does not block | Prompt hook (semantic) — scope the matcher narrowly, don't run on every routine edit |
| `docs/devlog.md` | Forgotten — "I'll write it later" | `Stop` hook: reminder if the session touched a meaningful number of files and no devlog append happened. Start with a reminder only — full auto-summarization from the transcript would lose the author's actual voice, which is the entire point of a devlog | `Stop` hook, reminder not auto-write |
| `docs/ideas.md` | Grows forever, never triaged | Deliberately not automated — GTD is explicit that the Someday/Maybe review is the judgment call, automating it defeats the point. At most a soft nudge ("hasn't been touched in 3 weeks") | Command hook (mechanical nudge) + manual skill entry point for the actual triage |
| `docs/roadmap.md` | Goes stale as milestones ship silently | Manual — tied to the author's own sense of finishing a chunk of work, not automation | None |
| Diátaxis docs (`reference/`, `how-to/`, etc.) | Fall behind the code they describe | `PostToolUse` scoped to interface-surface changes (CLI args, API routes, config schema) | Prompt hook (semantic), narrowly scoped |
| Session cold-start | Agent starts fresh every session with no picture of doc state | One hook injecting a compact status block: open draft ADRs, `ideas.md` line count, days since last devlog entry, current roadmap milestone | Command hook (mechanical — pure counting/grepping, no LLM call needed) |

## Corrections and refinements from mechanism review

Checked directly against `raw/research/claude-code-findings.md:249-328, 495-544`:

- No `invocation: auto` field exists on skill frontmatter (this was in the original
  draft of this thinking and is wrong). Auto-invocation is driven by the `description`
  field — Claude matches the task at hand against it. Opt-outs are
  `disable-model-invocation: true` and `user-invocable: false`.
- Prefer command hooks over prompt hooks wherever the check is mechanical (counting,
  grepping, date math) rather than a genuine semantic judgment. Command hooks are
  deterministic and free; prompt hooks cost a real model call (~30s timeout) each time
  they fire. Reserve prompt hooks for things that can't be expressed as a grep — "is
  this architecturally significant" being the clearest case.
- Hook event mechanics confirmed real and used correctly in the mapping above:
  `PreToolUse` can block (exit 2, even bypasses `bypassPermissions` mode); `PostToolUse`
  cannot block but can add stdout to context; `Stop` can block up to 8 consecutive
  times before an override cap; `SessionStart` cannot block but injects content — this
  is the documented pattern for loading session context before work starts.

## Open questions

- Whether prompt-hook-based "architectural significance" classification is worth its
  per-edit cost even narrowly scoped, or whether a periodic (not per-edit) check fits a
  solo project better.
- Exact matcher scoping for the build-config and interface-surface hooks — needs a real
  project structure to test against, can't be finalized in the abstract.
- Whether this file needs its own dedicated review pass before building, given it's
  infrastructure claims about Claude Code itself rather than external professional
  convention — current read is: cross-checking against our own prior research is
  enough, actual testing against a real repo is what validates it, not another
  literature check.
