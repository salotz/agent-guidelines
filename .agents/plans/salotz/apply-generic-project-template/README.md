# Apply generic project template (this repo)

Dogfood [templates/generic-project.md](../../../../content/templates/generic-project.md) on **agent-guidelines**, migrate guideline trees under **`content/`**, and add **bootstrap → install → preload** per [project-management-and-tooling](../../../../content/shared/project-management-and-tooling.md#bootstrapping-a-project).

## Status

- Planning:
  **Q1–Q9 locked** (operator accepted 2026-09-07).
- Execution:
  **Phase 1–6 done** (hygiene, `content/`, PRJX, tooling files, RFC 22 docs, blob ADR).
  **Operator still needs** `mise trust` → `mise install` → `mise run preload` → `mise run format`/`check` (sandbox blocked installs).
  Next = Phase 7 validate (operator-side tooling + optional link crawl).

## Goals

1. Satisfy generic-project template + bootstrap/preload guide (or ADR for blobs).
2. Move `operator/`, `personal/`, `shared/`, `templates/` → `content/<name>/`.
3. Add PRJX, `.bootstrap/`, mise, hk, `preload`, RFC 22 surfaces.

## Documents

| File | Role |
|------|------|
| [README.md](./README.md) | This index |
| [plan.md](./plan.md) | Full plan |
| [checklist.md](./checklist.md) | Execution checklist |
| [decisions.md](./decisions.md) | **Only** operator Q&A (`Q*`) |
| [background/](./background/) | Long notes (no answers) |

## Phase order (summary)

0. Lock **Q1–Q9**
1. Hygiene
2. **`content/` migration**
3. PRJX
4. **`.bootstrap/` + mise + hk + preload**
5. RFC 22 docs / contributing development flow
6. Blob ADR
7. Validate + commit slices

Decision ids use **`Q*`** per [work-process](../../../../content/personal/work-process.md).

## After each execution step

Draft commit message, operator test commands, truncated agent test output.
No commits unless asked.
