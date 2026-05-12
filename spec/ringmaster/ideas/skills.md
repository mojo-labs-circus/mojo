# JARVIS — Skills Backlog

Ideas for skills Jarvis can invoke on request. These are tools, not automatic behaviours — triggered by the user asking, not by Jarvis deciding.

---

## Collaborative development partner (full lifecycle)

Jarvis walks through development projects from start to finish as a genuine partner — deep interactive planning, architecture decisions, implementation, review, deployment. Not a code generator; a collaborator who ensures the user understands and drives every decision. Applies to personal projects and uni work alike.

**Uni mode** — optional pacing layer for academic projects. Jarvis ensures work progresses at a realistic human pace (not suspiciously fast) and that the user can genuinely explain everything produced. Nothing about hiding AI involvement — just making sure the collaboration is honest and appropriately paced. Uni only; personal projects don't need it.

---

## Dynamic project planning

Distinct from the tasks system (which is a to-do list) — this is higher-level phase and milestone tracking for ongoing projects. Jarvis understands where a project is, what phase comes next, and adjusts the roadmap as things change. Works for any type of project: a coding project, my mum planning a room redesign, a home renovation, etc. Jarvis acts as a proactive project manager who always knows the current state and what should happen next.

---

## Feature rollout convention

New features that interact with real-world systems (devices, messages, finances, etc.) should default to admin-only on release. Once stable and tested, promote to power and standard tiers. This gives a safe testing window without exposing unproven features to all users. Apply this pattern consistently across the platform.

---

## Device control (SSH + power management)

Jarvis can SSH into any device on the network and run commands. Can also power devices off, and power them on via Wake-on-LAN (works over Tailscale with a subnet router — so remote power-on is doable for supported devices). Long-term available to all family members ("Jarvis, turn off the TV"), but admin-only until stable. Confirmation gate required for any destructive or irreversible actions.

---

## Voice/personality presets (fun mode)

Per-user switchable personality presets for how Jarvis speaks — respond in the style of a specific character or person (Yoda, Jarvis from Iron Man, a comedian, etc.). Purely for fun, late stage development. Part of the voice mode system, not the humaniser (which is about making Jarvis's text output sound like the user — this is about Jarvis's own personality in speech). Requires enough voice/text data on the character to build a convincing style.

---

## Photos tool

Jarvis can search and retrieve photos from the user's image libraries — iCloud and self-hosted options (Jellyfin, Immich, etc.). Used by other tools: messaging tools can attach a photo to send, document tools can pull an image in, etc. Requires read access to the library; any action beyond retrieval (deleting, sharing) needs a confirmation gate.

---

## Finance integration

Broad future capability — Jarvis has access to financial data and can reason over it. Specific use cases to be defined as the platform matures (tracking spending, billing people, splitting costs, generating invoices, etc.). Confirmation gate required before any financial action is taken. Research open banking APIs and account aggregation options when the time comes.

---

## Screen and UI control layer

Open research question — find an open source Cursor-style tool that lets Jarvis interact with whatever is on screen: click, scroll, fill forms, control apps that have no API. This fills the gaps in the life operating layer where API-based tools can't reach. To be researched and implemented once the core platform is stable.

---

## Confirmation gate pattern

Many of Jarvis's actions have real-world consequences and need user confirmation before proceeding — sending messages, making purchases, deleting things, running commands, etc. Build a single reusable confirmation gate pattern (like Claude Code's approval flow) rather than implementing it ad-hoc per tool. Every tool that touches the outside world should use it.

---

## Email and messaging tools

Tools for sending emails and texts on the user's behalf. Output goes through the humaniser node so it sounds like the user wrote it. Requires confirmation gate before anything is sent — no silent outbound messages ever. Integration paths: SMTP/IMAP for email, iMessage via Mac or SMS gateway for texts, WhatsApp (via API or open source client library).

---

## OS and device awareness tool

A tool Jarvis calls when needed — not queried at session start. When someone asks for help with a tech problem, Jarvis calls the tool to get the user's current OS, version, and relevant hardware info, then gives precise device-specific troubleshooting. Long-term: once the screen/shell control layer exists, Jarvis can apply the fix directly rather than walking the user through it. Particularly valuable for less technical family members.

---

## Style database + humaniser node

Two connected pieces:

1. **Style database** — user uploads writing samples (emails, messages, documents) and Jarvis builds a per-user style profile: vocabulary, sentence structure, tone, cadence. Jarvis actively maintains and refines the profile over time as it sees more of the user's writing — primarily through file uploads rather than prompts (prompts are often full of typos for speed, not representative of real writing style).
2. **Humaniser node** — transforms Jarvis's output into the user's voice using that profile. Runs automatically whenever document-style output is being generated (emails, reports, formal messages), not on regular chat responses. Can also be explicitly requested.

The goal: anything Jarvis writes on the user's behalf sounds like they wrote it themselves.

---

## Per-user behavioural config file (CLAUDE.md equivalent)

Each user gets a personalised instruction file named after their assistant (e.g. `JARVIS.md`). Jarvis builds and maintains it automatically over time — the user never writes it manually. Captures structured behavioural patterns: "when this user starts a new project, always do X, Y, Z" and "at the end of every session, always do A, B, C". Distinct from personality/style (which is about tone and voice) — this is about workflow preferences and session structure. Gets loaded into context at the start of every session, same way CLAUDE.md works here.

---

## Session recap

Summarise what happened in a conversation session — decisions made, code written, problems hit, outcomes. Particularly useful for coding sessions. Pulls from conversation history, structures into a clean writeup. If a project is active, optionally saves to vault under `03-projects/<project>/`. Node: SYSTEM or dedicated MEMORY skill.
