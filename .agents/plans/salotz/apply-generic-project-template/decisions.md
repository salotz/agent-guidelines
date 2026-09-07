# Decisions

Single inbox for operator↔agent prompts on this plan.
Plan-local ids only (`Q1`, `Q2`, …) — do not cite these in durable repo docs; promote lasting choices to ADRs at close-out.

Process: [personal/work-process.md](../../../../personal/work-process.md) (Q-record shape).

## Open queue

| ID | Status | Need |
|----|--------|------|
| Q1 | locked | PRJX namespace + name |
| Q2 | locked | `content/` layout |
| Q3 | locked | Old-path stubs after migration |
| Q4 | locked | `mise.toml` filename |
| Q5 | locked | Initial hk check strictness |
| Q6 | locked | Commit slicing preference |
| Q7 | locked | Scope of **this repo’s** `.bootstrap/` + `host-tools.conf` |
| Q8 | locked | Scope of **this repo’s** `preload` |
| Q9 | locked | When to run preload / install hooks in this work |

Statuses: `open` | `proposed` | `locked`.

**Superseded:** earlier “OK to `hk install`?” standalone item — hooks via **`mise run preload`** (Q8–Q9).

---

## Q1 — PRJX project identity

Status: locked

### Prompt

Confirm `[project]` metadata for `.config/_project-meta.toml`.

### Answer

`namespace = "salotz"`, `name = "agent-guidelines"` (FQ name `salotz.agent-guidelines`).

### Notes

Operator accepted proposal 2026-09-07.

---

## Q2 — `content/` directory layout

Status: locked

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
No root-level stub symlinks (see Q3).

### Notes

Operator accepted proposal 2026-09-07.

---

## Q3 — Compatibility stubs at old paths

Status: locked

### Prompt

After moving trees under `content/`, should old paths remain as stubs?

### Answer

**No stubs** — one canonical tree under `content/`; fix all in-repo links.

### Notes

Operator accepted proposal 2026-09-07.

---

## Q4 — mise config filename

Status: locked

### Prompt

Prefer `mise.toml` or `.mise.toml` at repo root?

### Answer

`mise.toml` (non-hidden).

### Notes

Operator accepted proposal 2026-09-07.

---

## Q5 — Initial hk check strictness

Status: locked

### Prompt

How aggressive should the first `hk.pkl` be for this Markdown-first repo?

### Answer

Whitespace + final newline (EditorConfig-aligned) only.
Do **not** auto-reflow Markdown.
Link-check may be a later separate task — not required for first green check.

### Notes

Operator accepted proposal 2026-09-07.

---

## Q6 — Commit slicing

Status: locked

### Prompt

Prefer many small commits or fewer larger slices?

### Answer

Small slices (hygiene → content move → PRJX → bootstrap/mise/hk → RFC22 docs → blob ADR).
Operator commits via their git UI; agent does not commit unless asked.

### Notes

Operator accepted proposal 2026-09-07.

---

## Q7 — Bootstrap materials for this repo

Status: locked

### Prompt

What should `.bootstrap/` contain when dogfooding here?

### Answer

- `.bootstrap/host-tool-check` (POSIX `sh`, check-only) + `.bootstrap/host-tools.conf`.
- **Required:** `git` (min e.g. `2.30.0`), `mise` (min e.g. `2024.1.0`).
- **Optional rows:** none by default (minimal conf).
- Thin mise task `host-tool-check` → `sh .bootstrap/host-tool-check`.
- No network installer script in this pass.
- Adapt/simplify from examol ops; detect repo root via `.prjx-root` and/or `mise.toml`.

### Notes

Operator accepted proposal 2026-09-07.

---

## Q8 — Preload scope for this repo

Status: locked

### Prompt

What should `mise run preload` do in agent-guidelines?

### Answer

- Task name **`preload`** only.
- **Default:** install hk git hooks (`hk install` / `hk install --mise` as appropriate).
- Inline one-liner if hooks-only; `.tasks/preload.py` only if best-effort logic needed.
- No `uv sync` / language env in first slice.
- Optional `clean-preload` → `hk uninstall || true`.

### Notes

Operator accepted proposal 2026-09-07.

---

## Q9 — Run preload during this plan’s execution?

Status: locked

### Prompt

When executing, should the agent run `mise install` and `mise run preload` once files exist?

### Answer

**Yes.** After bootstrap + `mise.toml`/`hk.pkl` land:

1. `./.bootstrap/host-tool-check`
2. `mise install`
3. `mise run preload`
4. `mise run check`

Local hooks only (not global).
If host-tool-check fails, stop and report; no forced host package installs.

### Notes

Operator accepted proposal 2026-09-07.

---

## Changelog

- 2026-09-07: Initial Q1–Q7 as `proposed` (later renumbered).
- 2026-09-07: Rebuilt for bootstrap/preload; Q1–Q9 `proposed`.
- 2026-09-07: Operator accepted all proposals; Q1–Q9 → `locked`. Phase 1 execution started.
