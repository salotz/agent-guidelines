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
- [ ] ADRs drafted under `design/decisions/` (Phase 5 ok)

## Phase 1 — Hygiene

- [x] Root `.gitignore` (`.local/`, `.mise.local.toml`, `.agent-shell/`, `*~`, …)
- [x] Delete `.editorconfig~`
- [x] Confirm `.editorconfig` defaults

## Phase 2 — `content/` migration

- [ ] Create `content/`
- [ ] `git mv` four trees → `content/`
- [ ] Rewrite links (root, contributing, intra-tree verify, plan tree)
- [ ] Hubs: `README.md`, `AGENTS.md`
- [ ] Stubs per Q3
- [ ] Link existence check green

## Phase 3 — PRJX

- [ ] `.prjx-root`
- [ ] `.config/_project-meta.toml` (Q1)
- [ ] `.local/` gitignored

## Phase 4 — Bootstrap + mise + hk + preload

- [ ] `.bootstrap/host-tools.conf` (Q7)
- [ ] `.bootstrap/host-tool-check` executable (Q7)
- [ ] `mise.toml` (Q4): tools + `host-tool-check` / `preload` / `check` tasks
- [ ] `hk.pkl` (Q5)
- [ ] `.tasks/preload.py` only if Q8 needs more than one-liner
- [ ] `./.bootstrap/host-tool-check` passes (or reported)
- [ ] `mise install`
- [ ] `mise run preload` if Q9 yes
- [ ] `mise run check` green

## Phase 5 — RFC 22 + docs

- [ ] `contributing/README.md`
- [ ] `contributing/development.md` (bootstrap → install → preload → check)
- [ ] Path fixes in editing/collation
- [ ] `.agents/README.md` (+ optional repo-map)
- [ ] `design/{README,goals,decisions}`
- [ ] Refresh root `AGENTS.md` / `README.md`

## Phase 6 — Blobs

- [ ] ADR: no DVC yet
- [ ] Mention in goals / development

## Phase 7 — Validate

- [ ] Full host-tool-check + mise check path
- [ ] Optional link-check task
- [ ] Ignores verified
- [ ] Plan index / checklist marked done

## Phase 8 — Commits (operator)

- [ ] Slices per Q6
