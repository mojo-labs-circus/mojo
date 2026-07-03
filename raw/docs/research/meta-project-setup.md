# Meta: Project Setup Research

Research agenda for getting the project's operational foundation right before dev begins.
The goal is to establish conventions, tooling, and templates that a team can plug into
from day one — not retrofit later.

---

## Decisions Already Made

- **Team from day one.** Design as if contributors could join tomorrow. Full conventions,
  contributor docs, onboarding materials.
- **Tracking tool is open.** Obsidian Kanban is local-only — not viable for a team.
  Research what actually fits before committing.
- **Best practices first.** Research how mature software teams do this. Adopt what works.
  Don't invent before understanding what already exists.

---

## Research Areas

### 1. Tracking Tool

Current: Obsidian Kanban (local, not team-accessible — needs replacing).

Questions to answer:

- Linear vs GitHub Issues/Projects vs alternatives for a small team on a multi-repo project
- What does Linear give you that GitHub Issues doesn't? Is the overhead worth it?
- How do tracking tools integrate with GitHub for a multi-repo org?
- What does onboarding a new contributor into the tracking system look like?

### 2. Docs Structure

Questions to answer:

- How do mature multi-repo orgs (Vercel, Rust, Kubernetes, Prisma, etc.) lay out docs?
- What lives at the org level vs per-repo?
- Docs-as-code vs wiki vs separate docs site — what's the right call for a project at this stage?
- How do ADRs work across multiple repos — one central location or per-repo?
- What's the minimum viable docs structure for a repo that a new contributor can orient from?

### 3. Conventions

Questions to answer:

- Conventional Commits — is this the right call? What does it unlock (changelogs, semver)?
- Branch naming conventions — what's standard and what's overkill?
- PR conventions — size limits, naming, description standards
- Versioning — SemVer, how does it apply across a multi-repo project?
- Changelog format — Keep a Changelog, auto-generated, or manual?

### 4. Templates and Governance Docs

Questions to answer:

- What issue templates does a multi-repo GitHub org need?
- PR template — what fields actually get used vs ignored?
- CONTRIBUTING.md — what does a good one look like? What's the minimum viable version?
- README standards — what should every repo README contain?
- CODEOWNERS — how does this work across a multi-repo org?
- Code of conduct, security policy — when do these matter and what's standard?

---

## What We Decide After Research

1. Tracking tool — pick one, migrate kanban.md out of Obsidian
2. Docs layout — decide final structure, update directory maps in CLAUDE.md files
3. Conventions — write `docs/reference/conventions.md` (expand what's there)
4. Templates — create all templates, wire up GitHub issue/PR templates per repo

## What We Implement After Deciding

- Migrate tracking to chosen tool
- Create GitHub issue and PR templates across all repos
- Write or update CONTRIBUTING.md per repo
- Write README template and audit existing READMEs
- Finalize and lock `docs/reference/conventions.md`
- Update all CLAUDE.md files to reflect final structure
- Set up CODEOWNERS where relevant
