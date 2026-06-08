# Session 08 — Dev Team Instance

> Applies the complete framework to a software development team.
> The first concrete instance. Do not design anything new here —
> map the framework onto the domain.

---

## Purpose

Take everything established in sessions 01-07 and apply it to a software dev team.
This is where the abstract framework becomes the specific operating model for
building Mojo itself.

Every decision in this session must trace back to the framework. If something
cannot be derived from the framework, it either belongs in the framework (go back
and add it) or it is domain-specific glue that lives only in this instance.

---

## Context

The framework was designed to be general. A dev team is one instance of it.
The goal here is not to redesign the framework for software — it is to answer:
given this framework, what does a software dev team built on it actually look like?

**What we already know about the dev team instance:**

- Leader is Clarke. No agent holds equivalent authority.
- Contribution lifecycle: Inbox → Backlog → Ready → In Progress → In Review → Done
- Clarke explicitly advances at human gates. Mechanical transitions are automated
  by the tooling layer.
- Agents communicate via async channels throughout execution. Clarke gets
  notifications at key moments.
- Everything that can be tooled must be tooled: CI, CD, pre-commit hooks, branch
  rules, board automation, PR templates, notifications.
- Repos: ringmaster, performer, mojos, client-web, client-tui, mojo-sdk
- Ticket system: GitHub Issues. Board: GitHub Projects. One org-level board.

---

## Questions to Resolve

**Mapping the collective structure**

- How does the framework's node taxonomy map to a dev team?
  What are the specific roles — and which are human, which are agents?
- What sub-groups exist? How are they defined — by repo, by domain, by something else?
- What does ownership mean for a software codebase — who owns what, and how is
  that enforced?

**Mapping the agent model**

- What does a dev agent look like in this instance?
  How is it instantiated, what context does it receive, how does it terminate?
- Are dev agents ephemeral (per contribution) or persistent (per domain)?
- What is in the agent's context at the start of a contribution?
  (Ticket brief, repo CLAUDE.md, system architecture, relevant spec sections)
- What does an agent hand off when it completes a contribution?

**Mapping communication**

- What channels does a dev team use?
  GitHub Issue comments, PR descriptions, PR reviews — map each to the
  communication model's channel taxonomy.
- What are the required communication touchpoints for a dev agent?
  (Start, key decisions, blockers, PR ready — define exactly)
- How does Clarke stay informed without being pulled into every detail?

**Mapping the tooling layer**

- What goes in the tooling layer for a dev team?
  CI (linting, type checking, tests), CD (deployment pipeline), pre-commit hooks,
  branch naming enforcement, board automation, PR templates, notifications.
- How is each tool configured — per repo, globally, or both?
- What events trigger what automations?

**Mapping the leadership interface**

- What are Clarke's specific decision gates in the dev team instance?
- What does Clarke's view of the collective look like day-to-day?
- What surfaces to Clarke automatically vs what requires him to ask?

**Ticket as contribution brief**

- Map the framework's contribution unit to a GitHub Issue.
- What must a ticket contain before it can be picked up by an agent?
- Who writes tickets — Clarke, agents, or both?
- What is the template?

**Branch and PR conventions**

- Branch naming, PR structure, Definition of Done — define concretely.
- How does a PR map to the framework's contribution handoff model?

---

## Output Required

1. The dev team collective structure — node map, sub-groups, ownership boundaries
2. The dev agent definition — context, lifecycle, handoff
3. Communication conventions — channels, required touchpoints, tone
4. Tooling layer specification — every tool, what it does, how it's configured
5. Clarke's leadership interface — decision gates, situational awareness, escalation path
6. Ticket template — complete, ready to use
7. Branch and PR conventions — concrete rules

---

## Dependencies

All previous sessions (01-07) must be complete before this session runs.
