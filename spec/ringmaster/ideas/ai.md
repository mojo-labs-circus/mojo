# Ringmaster — AI Model Ideas

Future work on model quality, self-improvement, and the external model boundary.

---

## Last-mile fine-tuning

Fine-tune an existing open source model (e.g. a 7B–34B Llama or Qwen variant) on personal data — writing samples, coding patterns, conversation history, notes — to produce a model that genuinely behaves like a personal assistant rather than a generic one shaped by prompting. The home server A6000 (48GB VRAM) is capable of LoRA/QLoRA fine-tuning at this scale. Tools like Unsloth and LLaMA-Factory have made this accessible to individual developers.

The result: a base model that already knows your style, your domain, your preferences, and how you think — before any system prompt is applied. Layered on top of the existing Ringmaster memory and skills system, this is the endgame for a truly personal AI. No platform code changes needed — swap the model name in `config.yaml` and the rest of the system works as-is.

Once fine-tuning is working well, the constitutional check shifts from active enforcement to audit layer — the principles are baked into the weights, so CONSTITUTIONAL rarely fires. Its firing history also becomes a useful training signal for the next fine-tuning pass.

Revisit once the home server is live and the platform has accumulated enough personal data to make a fine-tuning dataset meaningful.

---

## Autonomous self-improvement loop

The Ringmaster runs overnight as an autonomous agent to improve its own graph behaviour — refining intent descriptions, validating that generated DAGs are correct, and modifying planner prompts where it detects systematic failures. Rather than waiting for a user to notice that routing went wrong, The Ringmaster observes its own outputs, identifies patterns (e.g. `tasks` intent consistently dropped on conditional requests), proposes and applies fixes, and logs what changed and why.

Key open problems to solve before this is viable:
- **Correctness definition** — what does a "correct DAG" mean formally? The loop needs an evaluator, not just a generator.
- **Scope guardrails** — define exactly which files and prompts the Ringmaster is allowed to touch unsupervised. Changes outside that scope require user approval.
- **Audit trail** — every self-modification logged with before/after and the reasoning, so the user can review and roll back.

The groundwork is the clean intent description contract being built now — the self-improvement loop automates what is currently manual prompt tuning.

---

## Post-stream correctness watcher

A second concurrent task alongside CONSTITUTIONAL that checks factual correctness of RESPONDER's output after the stream completes — not ethics violations, but factual errors, hallucinations, or claims that contradict the user's known context. Would run post-stream only (retract path) since correctness can only be evaluated on the complete response. Lower priority than ethics checking; only worth adding once the platform is stable and CONSTITUTIONAL is well-validated. Could log to the improve log as a new event type for fine-tuning signal.

---

## External AI escape hatch (highly constrained)

The Ringmaster is fully local — Ollama is the AI backbone, no data leaves the home network. However, in rare cases (a task genuinely requires a capability the local model can't handle, or a major Claude update makes it temporarily worth it), there should be a tightly contained, opt-in escape hatch to route a single request to an external model.

Hard constraints:
- Explicit user confirmation required before any data leaves the network — no silent cloud calls ever
- Only the minimum necessary data for that specific request is sent — no memory, no history, no profile data unless the user explicitly includes it
- The user is told exactly what is being sent and to where before it goes
- Disabled by default, behind a config flag
- Scoped to a single request only — no persistent external session

This is an emergency valve, not a feature. The north star is always fully local.
