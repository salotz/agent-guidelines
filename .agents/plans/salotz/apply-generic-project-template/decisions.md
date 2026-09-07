# Decisions

Single inbox for operator↔agent prompts on this plan.
Plan-local ids only (`Q1`, `Q2`, …) — do not cite these in durable repo docs; promote lasting choices to ADRs at close-out.

Process: [personal/work-process.md](../../../../personal/work-process.md) (Q-record shape).

## Open queue

| ID | Status | Need |
|----|--------|------|
| Q1 | proposed | Confirm PRJX namespace + name |
| Q2 | proposed | Confirm `content/` layout |
| Q3 | proposed | Old-path stubs after migration? |
| Q4 | proposed | `mise.toml` filename |
| Q5 | proposed | Initial hk check strictness |
| Q6 | proposed | Commit slicing preference |
| Q7 | proposed | Scope of **this repo’s** `.bootstrap/` + `host-tools.conf` |
| Q8 | proposed | Scope of **this repo’s** `preload` (hooks only vs more) |
| Q9 | proposed | When to run preload / install hooks in this work |

Statuses: `open` (needs Answer) | `proposed` (default drafted; accept or edit) | `locked` (implement against this).

**Superseded:** earlier “OK to `hk install`?” standalone item — hooks are installed via **`mise run preload`** after bootstrap + `mise install` ([project-management-and-tooling](../../../../shared/project-management-and-tooling.md#bootstrapping-a-project)). Covered by Q8–Q9.

---

## Q1 — PRJX project identity

Status: proposed

### Prompt

Confirm `[project]` metadata for `.config/_project-meta.toml`.

### Context

PRJX portable metadata at repo root (plan Phase 3).

### Answer

<!-- operator: accept proposal or edit -->

`namespace = "salotz"`, `name = "agent-guidelines"` (FQ name `salotz.agent-guidelines`).

### Notes

---

## Q2 — `content/` directory layout

Status: proposed

### Prompt

Confirm target layout for migrating guideline trees.

### Answer

```text
content/
  shared/
  personal/
  operator/
  templates/
```

Root keeps control plane: `AGENTS.md`, `README.md`, `LICENSE`, `contributing/`, `.agents/`, `design/` (added), `.bootstrap/`, tooling/PRJX files.
No root-level stub symlinks unless Q3 says otherwise.

### Notes

---

## Q3 — Compatibility stubs at old paths

Status: proposed

### Prompt

After moving trees under `content/`, should old paths `shared/`, `personal/`, `operator/`, `templates/` remain as symlinks (or tiny pointer READMEs) for external bookmarks / old clones?

### Answer

**No stubs** — one canonical tree under `content/`; fix all in-repo links; accept that external deep links break.

### Notes

---

## Q4 — mise config filename

Status: proposed

### Prompt

Prefer `mise.toml` or `.mise.toml` at repo root?

### Answer

`mise.toml` (visible; matches examol ops and common docs).

### Notes

---

## Q5 — Initial hk check strictness

Status: proposed

### Prompt

How aggressive should the first `hk.pkl` be for **this** Markdown-first repo?

### Answer

Defer this. Just put a stub hook in for testing. This issue will be determined separately.

### Notes

---

## Q6 — Commit slicing

Status: proposed

### Prompt

Prefer many small commits (per phase) or fewer larger slices?

### Answer

I'll guide the commit frequency. You just follow each step of the plan. Agent never commits.

### Notes

---

## Q7 — Bootstrap materials for this repo

Status: proposed

### Prompt

What should `.bootstrap/` contain when dogfooding the generic template **here**?

### Context

Normative guide: [Bootstrapping a project](../../../../shared/project-management-and-tooling.md#bootstrapping-a-project).
Live shape: examol `ops` (`.bootstrap/host-tool-check`, `host-tools.conf`).
This repo is docs-first (no uv/python app stack required to edit Markdown).

### Answer

- Ship **`.bootstrap/host-tool-check`** (POSIX `sh`, check-only) + **`.bootstrap/host-tools.conf`**.
- **Required** in conf: `git` (min version TBD, e.g. `2.30.0`), `mise` (min version TBD, e.g. `2024.1.0` or “current major you already run”).
- Thin mise task `host-tool-check` → `sh .bootstrap/host-tool-check` after mise exists.
- **Do not** add a network installer script in `.bootstrap/` in this pass (check-only; operator installs mise/git themselves if missing).
- Use PRJX root detection. Don't use `ops` as a template for that as it is not up to date with that standard.

### Notes

---

## Q8 — Preload scope for this repo

Status: proposed

### Prompt

What should `mise run preload` do in **agent-guidelines**?

### Context

Generic template: hooks via preload; language env only if the project has that stack.
examol ops: `uv sync` + `hk install` in `.tasks/preload.py`.
This repo: no `pyproject`/`uv` app env required for day-to-day doc work.

### Answer

Just `hk install` for now.

### Notes

---

## Q9 — Run preload during this plan’s execution?

Status: proposed

### Prompt

When executing the plan, should the agent run `mise install` and `mise run preload` (thus installing hooks in this clone) once those files exist?

### Answer

The agent can do this sure.

### Notes

---

## Changelog

- 2026-09-07: Initial draft used incorrect `D*` ids; renamed to `Q*`.
- 2026-09-07: Rebuilt queue after bootstrap/preload guide landed — dropped standalone “hk install OK?”; added Q7–Q9 for `.bootstrap/`, preload scope, and whether to run preload in-session; renumbered layout/mise/hk/commits to Q2–Q6.
