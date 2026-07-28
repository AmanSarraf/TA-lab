# AGENTS.md — AmanSarraf (LFX'26 · Talent Angels)

Agent context for work under `mentees/AmanSarraf/`. Read this before editing
anything in this folder.

## Read first (in order)

1. `../../CLAUDE.md` and `../../AGENTS.md` — TA-lab sandbox rules
2. `../../../CLAUDE.md` — workspace policy (DCO, secrets, git)
3. `../../exercises/sprint-XX-*/README.md` — find the active sprint's exercise brief
4. Active sprint `sprint-XX/AGENTS.md` — sprint-specific context

## Who and where

- **Mentee:** Aman Kumar Sarraf (`AmanSarraf`)
- **Scope:** Only `mentees/AmanSarraf/` — never edit other mentees' folders
- **Program:** Talent Angels — Locator, Connector, Pathfinder graph agents over
  skill/occupation taxonomies

## Git and contribution

- Branch pattern: `mentee/AmanSarraf/sprint-XX`
- **DCO required:** `git commit -s`
- **Never push to `main`**
- **Do not `git add`, `commit`, or `push` unless the user explicitly asks**
- Keep PRs small; one logical change per PR

## Never commit

- `scratch/` (mentee-level and per-sprint) — drafts, PDFs, presentation WIP
- `data/`, `*.zip` — large O\*NET downloads (link in `NOTES.md`)
- `.env`, `.venv/`, `__pycache__/`, `.DS_Store`
- `CLAUDE.local.md` — personal machine-specific notes

## How to work with the user

- User owns deliverables and must understand what ships
- Prefer **phased progress**; don't skip ahead without agreement
- **No drive-by refactors** or unrelated file edits
- Link to `NOTES.md` instead of duplicating long content in chat or new docs
- **Do not compare** other mentees' taxonomies in depth — each mentee owns one;
  future work is **normalization/crosswalks** across graphs

## Folder layout

```
mentees/AmanSarraf/
├── AGENTS.md          ← this file (all agents)
├── CLAUDE.md          ← Claude Code entry (imports this file)
├── scratch/           ← private experiments (gitignored)
├── sprint-01/         ← O*NET knowledge graph (Sprint 1)
│   ├── AGENTS.md      ← sprint-specific rules
│   ├── NOTES.md       ← primary mentor deliverable
│   └── scratch/       ← DRAFT, presentation script, PDFs
└── sprint-02/         ← agent & system architecture (Sprint 2)
    ├── AGENTS.md
    ├── ONE-PAGER.md
    ├── ARCHITECTURE.md
    └── diagrams/
```

## Active sprints

| Sprint | Taxonomy / focus | Status |
|--------|------------------|--------|
| sprint-01 | O\*NET 30.3 | Merged ([PR #1](https://github.com/LFX-Talent-Angels/TA-lab/pull/1)); review homologation [PR #15](https://github.com/LFX-Talent-Angels/TA-lab/pull/15) |
| sprint-02 | Agent & system architecture | PR open ([PR #12](https://github.com/LFX-Talent-Angels/TA-lab/pull/12)) |

## Session start prompt (copy-paste)

```
Read mentees/AmanSarraf/AGENTS.md and mentees/AmanSarraf/sprint-XX/AGENTS.md.
Then help me with: [task]
```