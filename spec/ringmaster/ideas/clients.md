# JARVIS — Client Ideas

Future work and improvements for all client surfaces (TUI, web, voice, hardware).

---

## TUI — evolve into a Claude Code-style power interface

Long-term the TUI becomes a full development and operations environment, not just a chat panel. Two axes:
- **UX/visual** — inline diffs, file tree, split panes, rich output rendering (tables, code blocks, task lists)
- **Agent capability** — Jarvis can read/write files, run shell commands, operate on a codebase directly from within the conversation

Effectively: Claude Code, but it's Jarvis, running against your server, with your memory and task context baked in.

---

## TUI interrupt channel (`/btw`)

Secondary input channel for power TUI users — inject a message mid-invocation without cancelling the current one. Opt-in, TUI-only.

---

## Inline editing for skill confirmation gates

When Jarvis proposes content before sending (email, message, calendar event), the client renders an editable form so the user can tweak the draft directly before confirming. `confirm` frame carries optional `data` with the edited content. Action-type confirmations (shell commands, plans) stay as plain confirm/cancel — no inline editing. Mk1 handles this conversationally (cancel → "I liked that but include X" → Jarvis redrafts → new confirm gate). Inline editing is a client UX improvement for Mk2.

---

## Voice mode in all clients

TUI gets STT/TTS first. Extending to desktop and iOS clients follows once that architecture is proven.

---

## Custom watch client (microcontroller)

A hardware client built on a microcontroller worn on the wrist — microphone + speaker, speech-only interface. Long-term goal. Use cases are read-only/conversational: "what's on my calendar today", "what tasks do I have", "remind me about X". No code output, no text editing. Connects back to the Jarvis server over Tailscale like any other client. This is the most constrained point in the multi-client family — proves that the API is client-agnostic.

---

## Client auto-update

Desktop clients have no auto-update mechanism. Lightweight solution: ping a version endpoint at startup, prompt to re-download if behind. iOS (TestFlight) and web handle updates automatically.

---

## Multi-client philosophy ("dispatch")

Jarvis is accessible from anywhere via whatever client fits the context — web, TUI, iOS, iPad, watch. There's no separate "dispatch mode"; the multi-client architecture *is* the dispatch layer. Each client exposes a subset of functionality appropriate to its form factor.

---

## Offline mode (very long term)

Once everything is stable and the family is using Jarvis daily, explore a lightweight offline mode — a self-contained local build that works without the home server (e.g. on a plane). Data syncs back to the server when reconnected. Not worth thinking about until everything else is solid.
