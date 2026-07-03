# Org Setup Tracker

**Goal:** Get the entire org public-facing and coherent. Every repo has the right license
and a README that tells a clear story. The mojo repo holds the org-level knowledge base
in `docs/`. Component repos hold their own `docs/` as they develop. No wikis.

**Structure:**

- Public face → READMEs (per repo) + `.github` org profile
- Org-level knowledge base → `mojo/docs/`
- Component knowledge bases → `docs/` in each component repo (as they develop)
- Raw/unprocessed ideas → `mojo/docs/inbox/`

---

## mojo repo — org hub + knowledge base

The mojo repo is the org-level knowledge base. `docs/` holds all framework design,
vision, architecture, ADRs, and research. The root README explains what mojo-labs is
building and links to the components.

- [ ] Triage `docs/` — mark live vs. dead, delete the dead
- [ ] Pull any still-relevant ringmaster architecture notes into `docs/`
- [ ] Create `docs/inbox/` for raw/unprocessed ideas
- [ ] Write root README — what mojo-labs is building, links to component repos
- [ ] Review for consistency with org profile README

## .github repo — org profile

- [x] Push updated profile README
- [x] Fix CONTRIBUTING.md — open invitation, solo project looking for collaborators
- [x] Review CODE_OF_CONDUCT.md
- [x] Review SECURITY.md
- [ ] Final pass on profile README when org is fully public

## mojo-agent

- [ ] Make public
- [x] Add AGPL license
- [ ] Write README

## mojos

- [x] License (AGPL)
- [x] README accurate
- [ ] Remove wiki (replaced by `docs/` convention)

## dotfiles

- [ ] Decide license
- [ ] Write README
- [ ] Make public

## herald

Standalone tool — not part of the mojo system, but lives under the mojo-labs org.
Capture what you understand about something once, render it into the right format for
whoever needs it (brief, presentation, etc.). Just started, design.md is the only artifact.

- [ ] Create repo under mojo-labs
- [ ] Decide license
- [ ] Turn design.md into a proper vision doc and scoped v1 plan
- [ ] Write README
- [ ] Make public

## ringmaster

- [x] Archive it
- [x] Update README — explains what it was, superseded by mojo-agent
