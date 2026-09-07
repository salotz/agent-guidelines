# Development (this repository)

How to bootstrap and run tooling for **agent-guidelines**.

Normative patterns: [Project management and tooling](../content/shared/project-management-and-tooling.md)
([bootstrap](../content/shared/project-management-and-tooling.md#bootstrapping-a-project),
[preload](../content/shared/project-management-and-tooling.md#preload-post-bootstrap)).
Stack defaults: [generic project template](../content/templates/generic-project.md).

## Layout (short)

| Path | Role |
|------|------|
| `content/shared/` | Portable guidelines |
| `content/personal/` | This operator’s host guidelines |
| `content/operator/` | Human sandboxing / host-hardening docs |
| `content/templates/` | Opinionated project specs |
| `contributing/` | Maintainer process for **this** repo |
| `design/` | Goals and ADRs for this repo |
| `.agents/` | Agent plans, repo map, optional skills |
| `.bootstrap/` | Host tool **check-only** scripts (POSIX) |
| `.prjx-root`, `.config/` | PRJX project root + portable metadata |
| `mise.toml`, `hk.pkl`, `.editorconfig` | Tool pins, hooks/checks, editor defaults |

Git/project root is the **repo root**, not `content/`.

## Happy path

```sh
# 1. Host prerequisites (check-only; no installs)
sh .bootstrap/host-tool-check
# or, once mise is on PATH and this repo is trusted:
mise run host-tool-check

# 2. Project pins
mise trust .          # once per clone if mise prompts
mise install          # installs hk (and future pins)

# 3. Checkout init (git hooks)
mise run preload      # hk install

# 4. Verify / fix whitespace + final newlines only
mise run check        # hk check --all (read-only)
mise run format       # hk fix --all (writes)
```

Shell **activation** of mise is optional.
Prefer `mise run …` / `mise exec -- …` so pins apply without a pre-activated shell.

### Tasks (menu)

| Task | What it does |
|------|----------------|
| `host-tool-check` | `sh .bootstrap/host-tool-check` |
| `preload` | `hk install` (hooks for this clone) |
| `clean-preload` | `hk uninstall` (best-effort) |
| `check` | `hk check --all` |
| `format` | `hk fix --all` (trailing whitespace + final newlines) |
| `validate` | depends on `check` |

`hk.pkl` does **not** reflow Markdown (semantic line breaks stay intact).

## Host tools vs project pins

- **`.bootstrap/host-tools.conf`**: required **host** tools (`git`, `mise`). Optional rows are unused here by default.
- **`mise.toml` `[tools]`**: project-managed CLIs (`hk`, …). Do not duplicate those into the host conf.

## PRJX

- Sentinel: empty [`.prjx-root`](../.prjx-root)
- Metadata: [`.config/_project-meta.toml`](../.config/_project-meta.toml) — FQ name `salotz.agent-guidelines`
- Host overrides only under **`.local/`** (gitignored), e.g. `.local/_config.toml`

## Do not commit

- `.local/`
- `.mise.local.toml` / `mise.local.toml`
- `.agent-shell/`
- editor backups (`*~`, swap files)

See root [`.gitignore`](../.gitignore).

## Maintaining guideline content

- **Edit** prose under `content/` using [editing.md](./editing.md).
- **Collate** external references into `content/shared/summaries/` per [collation.md](./collation.md).
- Prefer thin hubs; put substance in topic files.

## Blobs

This repo has **no** DVC/git-lfs setup yet.
If large binary assets appear, prefer DVC (see [blob management](../content/shared/blob-management.md) and `design/decisions/`).
