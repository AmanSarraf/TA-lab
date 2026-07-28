# AGENTS.md — Sprint 01 (O\*NET Knowledge Graph)

Sprint-specific context. Parent rules: `../AGENTS.md`.

## Assignment

- **Sprint:** 1 — Research & build a taxonomy graph
- **Taxonomy:** O\*NET 30.3 only (do not deep-dive other mentees' taxonomies)
- **Exercise brief:** `../../../exercises/sprint-01-taxonomy-graph/README.md`
- **Due / review:** Jun 23 sync · due Jun 30, 2026

## Status (last updated: after PR #1 merge)

| Item | State |
|------|-------|
| Graph model (Model A) | Locked |
| Loader + queries | Done — `load_onet.py`, `queries.cypher`, `run_queries.py` |
| Neo4j backends | Aura **or** local Docker (`docker-compose.yaml`) |
| `NOTES.md` | Primary deliverable — complete |
| Git PR | **Merged** — [PR #1](https://github.com/LFX-Talent-Angels/TA-lab/pull/1) |
| Homologation | Mentor follow-up — review [PR #15](https://github.com/LFX-Talent-Angels/TA-lab/pull/15) |
| Presentation (PPT) | Done for Sprint 1 sync — materials in `scratch/` (not committed) |

## Deliverables (what mentors review)

| File | Role |
|------|------|
| `NOTES.md` | Main write-up: model, license, reproduce, learnings, agents |
| `load_onet.py` | O\*NET → Neo4j loader (4-occupation slice) |
| `queries.cypher` | Example Cypher Q0–Q8 |
| `run_queries.py` | Automated verification (expect 9/9 PASS) |
| `docker-compose.yaml` | Optional local Neo4j |
| `.env.example` | Aura or local credentials template |
| `requirements.txt` | `neo4j`, `python-dotenv` |

**Not deliverables:** `scratch/` (DRAFT, PDFs, presentation), `data/`, `.env`

## Locked decisions (do not change without user approval)

- **Model A:** `:Occupation`, `:Task`, `:Skill`, `:Software`
- **Relationships:** `HAS_TASK`, `REQUIRES_SKILL`, `USES_SOFTWARE`
- **Slice:** 4 SOC codes — `15-1252.00`, `15-1251.00`, `15-1243.00`, `15-1253.00`
- **Source files (6 of 45):** Occupation Data, Task Statements, Essential Skills,
  Transferable Skills, Software Skills, Content Model Reference
- **Loaded size:** 758 nodes · 1,703 relationships

## Reproduce (quick reference)

```bash
cd mentees/AmanSarraf/sprint-01
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# Aura: fill .env from .env.example  OR  local: docker compose up -d
python load_onet.py --clear
python run_queries.py
```

Full steps: `NOTES.md` → Reproduce this graph.

## Architecture (load_onet.py)

```
data/*.txt (O*NET TSVs, gitignored)
  → Python: filter rows to OCCUPATIONS frozenset (4 SOC codes)
  → Python: merge IM + LV rows → one dict per (onet_soc_code, element_id)
  → Cypher: batched UNWIND $rows … MERGE nodes + relationships
  → Neo4j: 758 nodes · 1,703 relationships
```

Key design points for future edits:
- `:Skill` nodes are **shared** across occupations (same `element_id`, different edge scores). This enables Q2/Q8 skill-overlap queries — don't make them per-occupation.
- Essential and Transferable Skills are separate files → same `REQUIRES_SKILL` edge type, distinguished by `skill_type` property. Skip either file and the graph looks empty for tech roles.
- `Software Skills.txt` → `:Software` nodes, not `:Skill`. "Python" is software; "Programming" is a skill.
- `Content Model Reference.txt` is a lookup only (enriches `:Skill.description`); not loaded as graph nodes.
- All MERGE operations are idempotent — safe to re-run with `--clear`.
- `run_queries.py` imports `neo4j_credentials()` from `load_onet.py`; both share the same `.env` resolution.

## Agents (Locator / Connector / Pathfinder)

Deep dive in `NOTES.md`. Summary for agents editing this sprint:

- **Locator:** resolve title/text → `onet_soc_code`; match `:Skill`, `:Software`,
  `:Task` text; rank via edge properties
- **Connector:** crosswalk keys `onet_soc_code`, `element_id`; shared `:Skill` nodes
  as overlap evidence (not other taxonomies yet)
- **Pathfinder:** not in v1; `Related Occupations.txt` + Job Zones are future edges

**Team next step:** each mentee exposes stable IDs; normalization layer combines
graphs — not sprint-1 scope.

## Presentation (historical)

Sprint 1 collective presentation covered:

- Knowledge graphs basics
- O\*NET taxonomy understanding (assigned taxonomy only)
- Graph model and demo
- Challenges faced
- How Locator, Connector, Pathfinder use this graph

Working materials stayed in `scratch/` (e.g. presentation script, mentor PDFs) and are
not committed. Do not create a committed `.pptx` unless the user asks.

## Working notes

- Phase log: `scratch/DRAFT.md` (reference only)
- IDE: Zed toolchain → `scratch/pyrightconfig.json` points at `.venv`