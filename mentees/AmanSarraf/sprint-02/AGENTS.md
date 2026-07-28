# AGENTS.md — Sprint 2 · System Architecture

Sprint: 30 June – 14 July 2026 | Review: 5th Mentee Sync, 7 July 2026

## Context for agents

This sprint produces **architecture artifacts only** — no running code. The three deliverables are:
1. Agent architecture diagram (Locator / Connector / Pathfinder with subagents, skills, tools)
2. Stack decision document (graph DB, backend, LLM provider, orchestration framework)
3. End-to-end query flow diagram

If present locally, read `progress.md` first (gitignored tracker) — full work plan,
task checklist, and open decisions. Deliverables for review: `ONE-PAGER.md` and
`ARCHITECTURE.md`.

## Folder conventions

| Folder | Purpose |
|--------|---------|
| `scratch/` | PDFs and private drafts — never commit |
| `decisions/` | ADRs (Architecture Decision Records) — one `.md` per decision |
| `diagrams/` | draw.io or Mermaid source + exported PNGs |
| `specs/` | Architecture spec documents |
| `progress.md` | Sprint tracker — update as tasks complete |

## Agent hierarchy (canonical for this sprint)

| Level | Role | Key property |
|-------|------|--------------|
| Agent | Top-level reasoner | Owns objective, controls flow |
| Subagent | Scoped worker | Returns distilled result |
| Skill | Passive procedure text | Describes *how*; cannot execute |
| Tool | Deterministic action | Touches external world; knows nothing else |

## Rules

- Do not write application code — design artifacts only.
- Diagrams: prefer draw.io (`.drawio`) or Mermaid inside Markdown.
- ADRs follow the format: Context → Decision → Consequences.
- DCO required on every commit: `git commit -s`
- Never commit `scratch/` contents.
