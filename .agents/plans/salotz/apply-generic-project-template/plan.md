# Plan: dogfood generic project template + `content/` migration

## Context

Repository: **agent-guidelines** (reflective guidelines repo).

Apply [templates/generic-project.md](../../../../templates/generic-project.md) (path becomes `content/templates/generic-project.md` after Phase 2).

Also migrate the four guideline trees into one parent:

| Today (root) | After |
|--------------|--------|
| `shared/` | `content/shared/` |
| `personal/` | `content/personal/` |
| `operator/` | `content/operator/` |
| `templates/` | `content/templates/` |

Root remains the **project control plane**: git, PRJX, **`.bootstrap/`**, mise/hk, EditorConfig, `AGENTS.md`, `README.md`, `LICENSE`, `contributing/`, `.agents/`, and (new) `design/`.

Normative bootstrap/preload behavior is defined in
[shared/project-management-and-tooling.md](../../../../shared/project-management-and-tooling.md#bootstrapping-a-project)
(edited/collated in this planning pass). Live shape reference: examol `ops` (`.bootstrap/`, `mise run preload` → hooks ± env).

---

## Gap analysis (template vs today)

| Template / guide requirement | Status now | Action |
|------------------------------|------------|--------|
| **git** | Yes | Keep |
| **`.bootstrap/`** host-tool-check + conf | Missing | Add POSIX check-only materials (Q7) |
| **mise** | Host only; no project config | Add `mise.toml` + tasks |
| **preload** task | Missing | Add; hooks via preload (Q8) |
| **Tasks / Python for non-trivial logic** | Almost none | `.tasks/` when needed (preload if non-trivial; link-check later) |
| **hk** | Missing | Pin via mise + `hk.pkl`; install via **preload** |
| **EditorConfig** | Present; matches defaults | Keep; drop `.editorconfig~` |
| **PRJX** | Missing | `.prjx-root` + `.config/_project-meta.toml` |
| **RFC 22** | Partial | `.agents/` map, `design/`, `contributing` hub + development/onboarding |
| **Blobs** | None | ADR: defer; no empty DVC |
| **Guidelines under `content/`** | Trees at root | Move + rewrite links |
| **`.gitignore`** | Missing | PRJX local, mise local, `.agent-shell/`, backups |

---

## Architecture after completion

```text
.
├── AGENTS.md
├── README.md
├── LICENSE
├── .editorconfig
├── .prjx-root
├── .config/_project-meta.toml
├── .bootstrap/
│   ├── host-tool-check          # POSIX sh, check-only
│   └── host-tools.conf
├── mise.toml
├── hk.pkl
├── .tasks/                      # optional; preload.py if needed
├── .gitignore
├── contributing/                # development: bootstrap → install → preload
├── design/
├── .agents/
│   └── plans/salotz/...
└── content/
    ├── shared/
    ├── personal/
    ├── operator/
    └── templates/
```

**Operator / agent happy path (this repo):**

```text
./.bootstrap/host-tool-check   # or: mise run host-tool-check (after mise on PATH)
mise install
mise run preload               # hk hooks (Q8)
mise run check                 # hk check --all
```

---

## Phase 0 — Decisions

Record answers in [decisions.md](./decisions.md) before destructive moves or preload.

| ID | Topic | Default proposal |
|----|--------|------------------|
| Q1 | PRJX name/namespace | `salotz` / `agent-guidelines` |
| Q2 | `content/` layout | Four trees under `content/` |
| Q3 | Old-path stubs | **No** |
| Q4 | mise filename | `mise.toml` |
| Q5 | hk strictness | Whitespace/EOF only; no MD reflow |
| Q6 | Commit slicing | Small per-phase commits |
| Q7 | `.bootstrap/` scope | `host-tool-check` + conf; required `git`+`mise`; minimal optionals |
| Q8 | `preload` scope | Hooks only for this repo |
| Q9 | Run preload in execution | Yes after files land (local hooks) |

Draft ADRs (land under `design/decisions/` in Phase 5):

- PRJX identity (Q1)
- Guideline trees under `content/` (Q2–Q3)
- Tool stack = mise + hk + EditorConfig + **bootstrap/preload** stages
- No blob tooling until needed
- hk does not reflow Markdown (Q5)

---

## Phase 1 — Hygiene

1. Add **`.gitignore`**: `.local/`, `.mise.local.toml`, `.agent-shell/`, `*~`, swap files, optional OS noise.
2. Delete **`.editorconfig~`**.
3. Keep **`.editorconfig`** (LF + final newline).

**Exit:** host-local and backup paths not accidentally committable.

---

## Phase 2 — Migrate trees into `content/`

Highest link-break risk. Prefer **before** PRJX/bootstrap/mise so new docs only cite `content/…`.

### 2.1 Move

```sh
mkdir -p content
git mv shared personal operator templates content/
```

### 2.2 Link rewrite

- **Sibling links** among the four trees (`../shared/…`) often remain valid after a joint move — **verify**.
- **Root + `contributing/`**: prefix `content/`.
- **Prose** in hubs/bootloaders: in-repo paths use `content/shared` etc.; portable *names* (“shared guidelines”) stay.
- **Plan tree** under `.agents/plans/`: update links to templates/shared after move.
- **Stubs (Q3):** default none.

### 2.3 Verify

Broken relative-link crawl; `rg` for stale `](./shared/` etc.; spot-check glossary/summary/template links.

### 2.4 Commit

Prefer one tight move+rewrite commit (or move-only then fix-up).

**Exit:** substance only under `content/`; hubs green.

---

## Phase 3 — PRJX

1. Empty **`.prjx-root`** at **repo** root (not under `content/`).
2. **`.config/_project-meta.toml`** from Q1.
3. Never commit `.local/`.

**Exit:** PRJX discoverable; metadata tracked.

---

## Phase 4 — Bootstrap, mise, hk, preload

Follow guide stages: **bootstrap → mise install → preload → check**.

### 4.1 `.bootstrap/` (Q7)

- `host-tools.conf` — required `git`, `mise`; optionals per Q7.
- `host-tool-check` — POSIX `sh`, check-only, parse conf; resolve repo root via `.prjx-root` and/or `mise.toml`.
- Adapt/simplify from examol ops; no network installers in this pass.

### 4.2 `mise.toml` (Q4)

- `[tools]`: pin `hk` (and nothing unnecessary — no dvc, no uv app stack unless preload needs Python beyond system `python3`).
- Tasks (thin):
  - `host-tool-check` → `sh .bootstrap/host-tool-check`
  - `preload` → hooks install (inline or `.tasks/preload.py` per Q8)
  - `check` / `validate` → `hk check --all`
  - optional `clean-preload` → `hk uninstall || true`
- No long bootstrap tutorial comments in TOML; point to `contributing/development.md`.

### 4.3 `hk.pkl` (Q5)

Whitespace / final newline only; no Markdown reflow.

### 4.4 Execute on this clone (Q9)

If Q9 locked yes:

1. `./.bootstrap/host-tool-check`
2. `mise install`
3. `mise run preload`
4. `mise run check`

Stop and report on host-tool-check failure; no unapproved host installs.

**Exit:** check-only bootstrap works; `mise run check` green; hooks present if Q9 allows.

---

## Phase 5 — RFC 22 surfaces + contributing bootstrap docs

### `contributing/`

| Path | Purpose |
|------|---------|
| `README.md` | TOC |
| `development.md` (or onboarding) | **Bootstrap → install → preload → check** for *this* repo; paths under `content/`; PRJX; ignores |
| `editing.md` / `collation.md` | Path fixes to `content/shared/…` |

### `.agents/`

- `README.md`; keep `plans/salotz/`; optional `context/repo-map.md`

### `design/`

- `README.md`, `goals.md`, `decisions/` ADRs
- Glossary pointer → `content/shared/glossary.md`

### Bootloaders

- Root `AGENTS.md` / `README.md`: `content/…`, `.bootstrap/`, mise entrypoints

**Exit:** maintainer can onboard from `contributing/development.md` alone.

---

## Phase 6 — Blobs

ADR: no DVC/git-lfs until needed; prefer DVC later.
Note in design non-goals + development.

---

## Phase 7 — Validate

1. `./.bootstrap/host-tool-check`
2. `mise run check` (+ `validate` if link-check exists)
3. Full relative-link crawl
4. Spot-check new maintainer docs
5. Confirms ignores; update plan checklist + [../todo.md](../todo.md)

---

## Phase 8 — Suggested commit slices (Q6)

1. Hygiene
2. `content/` move + link rewrites
3. PRJX
4. `.bootstrap/` + `mise.toml` + `hk.pkl` + preload/check tasks
5. `contributing/` + `.agents/` + `design/` + bootloaders
6. Blob ADR (+ optional link-check task)

Agent does not commit unless asked.

---

## Risks and mitigations

| Risk | Mitigation |
|------|------------|
| Missed links after `content/` move | Automated existence check |
| External deep links break | Q3 no-stubs; optional stubs if flipped |
| hk reflows Markdown | Q5 whitespace only |
| Preload confused with host bootstrap | Guide + contributing; agents checklist |
| Host missing mise/git | host-tool-check fails clearly; Q9 stop |
| Copying examol ops preload uv stack needlessly | Q8 hooks-only |

---

## Out of scope

- copier/cookiecutter generators
- Host-global hk
- CI hosted pipeline (optional later)
- Empty DVC
- Network bootstrap installers (unless a later Q locks them)

---

## Success criteria

1. Generic-project + bootstrap/preload guide dogfooded (or ADR-decided for blobs).
2. Trees only under `content/` (unless Q3 stubs).
3. `.bootstrap/host-tool-check` + conf present and usable without mise.
4. `mise run preload` installs hooks (Q8); `mise run check` green.
5. PRJX at repo root; `.local/` ignored.
6. `contributing/` documents bootstrap → install → preload.
7. Q1–Q9 locked or explicitly deferred; checklist complete.

---

## Execution protocol

Per [personal/work-process.md](../../../../personal/work-process.md):

- One step at a time on “go” / “execute next step”.
- No commits unless asked.
- After each step: draft commit message, test commands, truncated agent test output.
- Answers only in `decisions.md` (`Q*`).

## Immediate next step (when execution starts)

1. Lock Q1–Q9 (or accept proposals).
2. Phase 1 hygiene, then Phase 2 `content/` migration, unless operator orders otherwise.
