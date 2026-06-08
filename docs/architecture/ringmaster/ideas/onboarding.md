# Ringmaster — Onboarding Ideas

Ideas for getting new users (especially non-technical ones) set up inside the Circus.

---

## File system scan and reorganise

A lot of people who'd benefit from an agent assistant have no idea how their own file system is organised. They can't map it manually and they can't describe it. Before an agent can be useful to them, their chaos needs a shape.

The idea: on first setup, the Ringmaster (or the local agent on a MojOS machine) scans the user's machine, categorises what it finds, and proposes a clean reorganisation into a sensible layout. The user sees a diff of what moves where, approves it, and the files get moved — with a rollback manifest kept until they confirm they're happy.

**Privacy is the point.** This is explicitly the anti-Google, anti-Microsoft pitch. Every major platform that offers to "organise your stuff" does it by uploading your stuff to their servers, training on it, selling insight into it. This tool runs entirely locally — no file content, filename, or directory structure ever leaves the machine. The scan happens on-device, the categorisation model runs on-device, the proposed layout is generated on-device. Nothing phones home. For the kind of senior people this targets — executives, professionals, anyone with sensitive documents — that's not a nice-to-have, it's the reason they'd trust it at all.

**Rough shape:**

- Scan home directory (or user-specified paths) — on-device only
- Categorise: documents, projects, media, downloads, config, etc.
- Propose a target layout — ideally something the agent can also navigate later
- Show a "what moves where" preview before touching anything
- Move on approval; write a rollback manifest
- After a grace period, offer to delete the manifest

**Constraints:**

- Never move anything without an explicit approval step — this is irreversible territory
- Don't touch hidden directories (`.ssh`, `.config`, etc.) by default
- The proposed layout should be one the Ringmaster's memory and file tools can navigate predictably — this isn't just tidying, it's making the user's machine legible to the agent
- All processing stays local — no cloud calls, no telemetry, no "sync to improve the model"

**Why this matters:** The agent's usefulness is proportional to how much of the user's world it can see and reference. A chaotic file system is a blind spot. This turns setup into the first meaningful thing the agent does for the user — and does it without asking them to hand over their data to anyone.

---

## Email scan and reorganise

Same idea applied to email. Most people's inboxes are a disaster — years of unread newsletters, important threads buried under noise, no folder structure, no labels. The agent can fix this.

Connect to the user's email account (IMAP or OAuth — their choice, runs locally), scan the inbox, and propose an organisation scheme: folders or labels, rules for what goes where going forward, and a bulk-archive pass on the backlog. User reviews the proposed structure before anything is touched.

**Privacy framing:** The scanning and categorisation runs locally against the mail data — email content never gets sent to a third party. This is the explicit contrast with Gmail's "Smart Labels" or Outlook's "Focused Inbox", both of which process your email on Microsoft/Google servers. Here the model running the categorisation is sitting on the user's own hardware.

**Rough shape:**

- Connect via IMAP or OAuth (user provides credentials, stored locally in the Circus credential store — never in the Ringmaster)
- Scan inbox and existing folder/label structure
- Propose: folder hierarchy, labelling rules, bulk-archive candidates
- Show preview — "these 847 emails would be archived", "these senders would get a newsletter label"
- Apply on approval; don't delete anything, only move/label
- Set up ongoing rules so future mail lands in the right place automatically

**Constraints:**

- Never delete email — only move, label, or archive
- Credentials stay on the user's machine, not on the Ringmaster
- The agent can read email to answer questions later, but only with explicit per-session consent — not ambient background access
- All categorisation runs locally

---
