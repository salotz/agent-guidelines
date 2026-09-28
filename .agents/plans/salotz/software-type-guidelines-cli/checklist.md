# Checklist — software-type guidelines + CLI (clig.dev)

Track execution here.
Do not put operator answers here (use [decisions.md](./decisions.md)).

## Phase 0 — Decisions

- [ ] Q1 layout locked (single file vs `software-types/` directory)
- [ ] Q2 relationship to `software-guidelines.md` locked (sibling recommended)
- [ ] Q3 depth locked (agent checklist + pointers)
- [ ] Q4 normative voice locked (match shared tone)
- [ ] Q5 further-reading scope locked (clig summary required; others optional URLs)
- [ ] Record answers in [decisions.md](./decisions.md)

## Phase 1 — Collate clig.dev

- [ ] Prefer non-HTML source (`https://clig.dev/llms.txt` and/or upstream repo markdown)
- [ ] Note upstream license/attribution requirements
- [ ] Write `content/shared/summaries/clig-dev.md`
- [ ] Sanity-check against existing shared guidelines for conflicts/overlap

## Phase 2 — Author software-types + CLI

- [ ] Add `content/shared/software-types/README.md` (or chosen hub path)
- [ ] Add `content/shared/software-types/cli.md` (or chosen CLI path)
- [ ] Cover philosophy + hard basics + help/output/errors/flags/config/telemetry
- [ ] Include condensed agent do/don’t checklist
- [ ] Link summary + https://clig.dev/
- [ ] Optional glossary entries only if terms need stability

## Phase 3 — Hubs and consistency

- [ ] Update `content/shared/README.md`
- [ ] Cross-link from `content/shared/software-guidelines.md`
- [ ] Update root `README.md` if it lists shared topics
- [ ] Link check
- [ ] Editing pass (spelling, consistency, [editing.md](../../../../contributing/editing.md))

## Phase 4 — Close out

- [ ] Checklist complete vs [plan.md](./plan.md) success criteria
- [ ] Move plan to Done in [../todo.md](../todo.md)
- [ ] Leave staging/commit to operator unless asked
