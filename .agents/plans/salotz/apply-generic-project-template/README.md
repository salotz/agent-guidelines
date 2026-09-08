# Apply generic project template (this repo)

Dogfood [content/templates/generic-project.md](../../../../content/templates/generic-project.md) on **agent-guidelines**, migrate guideline trees under **`content/`**, and add **bootstrap → install → preload** per [project-management-and-tooling](../../../../content/shared/project-management-and-tooling.md#bootstrapping-a-project).

## Status

**Done** (2026-09-08).

- Q1–Q9 locked; phases 1–8 complete.
- Operator ran mise on the host (`install` / `preload` / `format` / `check`).
- Preload confirmed in-repo: `.git/config` registers `hook.hk-pre-commit` → `mise x -- hk run pre-commit --from-hook`.
- Final agent re-check: host-tool-check green; 0 broken relative links outside fences; layout/tooling/PRJX/RFC22 files present.
- Agent sandbox still cannot invoke host mise (expected); not a project defect.

Optional leftover: commit these plan-meta files if not already staged.

## Goals (achieved)

1. Generic-project template + bootstrap/preload guide dogfooded (blobs = ADR 004 defer).
2. Guideline trees only under `content/`.
3. PRJX, `.bootstrap/`, mise, hk, `preload`/`format`/`check`, RFC 22 surfaces.

## Documents

| File | Role |
|------|------|
| [README.md](./README.md) | This index |
| [plan.md](./plan.md) | Full plan |
| [checklist.md](./checklist.md) | Execution checklist |
| [decisions.md](./decisions.md) | Operator Q&A (`Q*`) |
| [background/](./background/) | Long notes |

## Happy path (this repo, going forward)

```sh
sh .bootstrap/host-tool-check
mise install
mise run preload
mise run format   # optional
mise run check
```

See [contributing/development.md](../../../../contributing/development.md).
