# Checklist — apply generic project template + `content/` migration

Track execution here.
Do not put operator answers here (use [decisions.md](./decisions.md)).

## Phase 0 — Decisions

- [x] Q1 PRJX identity locked/accepted
- [x] Q2 `content/` layout locked/accepted
- [x] Q3 stubs locked/accepted
- [x] Q4 mise filename locked/accepted
- [x] Q5 hk strictness locked/accepted
- [x] Q6 commit slicing locked/accepted
- [x] Q7 `.bootstrap/` scope locked/accepted
- [x] Q8 `preload` scope locked/accepted
- [x] Q9 run preload in-session locked/accepted
- [x] ADRs drafted under `design/decisions/` (001–005)

## Phase 1 — Hygiene

- [x] Root `.gitignore`
- [x] Delete editor backup junk
- [x] Confirm `.editorconfig` defaults

## Phase 2 — `content/` migration

- [x] Create `content/`
- [x] `git mv` four trees → `content/`
- [x] Rewrite links
- [x] Hubs: `README.md`, `AGENTS.md`
- [x] Stubs per Q3 (none)
- [x] Link existence check green

## Phase 3 — PRJX

- [x] `.prjx-root`
- [x] `.config/_project-meta.toml` (Q1)
- [x] `.local/` gitignored

## Phase 4 — Bootstrap + mise + hk + preload

- [x] `.bootstrap/host-tools.conf` (Q7)
- [x] `.bootstrap/host-tool-check` (Q7)
- [x] `mise.toml` (Q4): host-tool-check / preload / check / format / validate
- [x] `hk.pkl` (Q5)
- [x] `.tasks/preload.py` skipped (Q8 one-liner)
- [x] `sh .bootstrap/host-tool-check` passes
- [x] `mise install` — operator on host (2026-09-08)
- [x] `mise run preload` — operator; verified via `.git/config` `hook.hk-pre-commit` → `mise x -- hk run pre-commit --from-hook`
- [x] `mise run format` / `check` — operator on host (2026-09-08); agent sandbox cannot re-exec mise

## Phase 5 — RFC 22 + docs

- [x] `contributing/README.md`
- [x] `contributing/development.md`
- [x] Path fixes in editing/collation
- [x] `.agents/README.md` + `context/repo-map.md`
- [x] `design/{README,goals,decisions}` (ADRs 001–005)
- [x] Refresh root `AGENTS.md` / `README.md`

## Phase 6 — Blobs

- [x] ADR 004: no DVC yet
- [x] Mention in goals / development

## Phase 7 — Validate

- [x] `sh .bootstrap/host-tool-check` green
- [x] Full relative-link crawl: 0 broken outside fences (re-checked 2026-09-08)
- [x] Header audit clean on prior crawl
- [x] Ignores verified
- [x] Operator mise install / preload / format / check
- [x] Optional link-check task deferred
- [x] Plan index / checklist marked **done**

## Phase 8 — Commits (operator)

- [x] Product slices on `main` (hygiene → content → PRJX → tooling → RFC22)
- [ ] Optional: commit remaining plan-meta (`checklist` / plan `README` / `todo.md`) if desired
