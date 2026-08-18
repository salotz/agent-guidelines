## Project Management and Tooling

Generic rules for how agents (and humans) structure **project tooling**
and **machine-oriented config** versus **human/operator usage docs**.

### Relationship to other standards

| Concern | Where it lives |
|---------|----------------|
| Project folder layout, `contributing/`, `AGENTS.md` | [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](./summaries/salotz-rfc-022-ai-coding-structure.md)) |
| Host operator prefs, install confirmation, shell integration | [salotz RFC 23](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](./summaries/salotz-rfc-023-local-agent-context.md)); see also [Host System Interaction](./generic-agent-guidelines.md#host-system-interaction) |
| Where binaries and agent-generated data live on disk | [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](./summaries/salotz-rfc-024-extended-xdg-base-directory.md)) |
| **This document** | In-repo tooling contracts, config-file style, and how automation invokes tools |

### Config and tooling files stay content-focused

Applies to machine-oriented manifests such as: `.mise.toml`, `justfile`,
`pyproject.toml`, `package.json`, `Makefile`, CI workflow env blocks, and
similar.

- **Do**: short comments on *why a specific choice or value* exists.
- **Do not**: long usage, bootstrap, install tutorials, or copy-paste
  operator manuals inside those files.
- **Do**: put how-to, bootstrap, and day-to-day workflow in the **project**
  `contributing/` tree (RFC 22), for example `contributing/development.md`.

### Layered tooling (do not collapse roles)

Keep responsibilities separate. Concrete tools are examples; projects choose stacks.

| Layer | Role | Examples |
|-------|------|----------|
| Tool / version manager | Pin and install **host CLIs**; optional project env vars | mise, asdf, devbox |
| Language package manager | Project dependencies, lockfiles, build/publish | uv, npm, cargo |
| Task runner | Thin recipes only — not a second package manager | just, make |
| Build backend | Packaging metadata and build — not env management | hatchling, setuptools |

### Invocation: project automation vs operator shell

- **In-repo** commands (task recipes, scripts, CI, agent-run docs happy path):
  call the tool manager explicitly (for example `mise exec -- …`) so pinned
  tools and env apply **without** requiring shell activation.
- **Operator** interactive shell: activate, shims, or bare PATH are **optional**
  personal choice. Never make them a prerequisite for project recipes to work.

Document the project’s entrypoint in `contributing/` (for example
`mise exec -- just <recipe>`), not only in config file comments.

### Secrets and local overrides

- Committed config: non-secret defaults only.
- Secrets and machine-local overrides: gitignored local files (pattern depends
  on stack; for example `.mise.local.toml`). Never commit tokens. For secrets prefer just-in-time fetching from tools if possible (e.g. `bitwarden` or `op` (OnePass)).

### Host-installed tools

Prefer project-local tooling over host-wide installs when possible.

Host installs need **operator confirmation** first, plus a short plan for
integration (binaries, shell modules, cache/data dirs). See
[Host System Interaction](./generic-agent-guidelines.md#host-system-interaction)
and RFC 23. Typical host-level cases: bootstrapping a tool manager, tools that
must be host-wide, or projects with no project-local tooling story.

### Task runner recipes vs external scripts

Task runners (mise tasks, Make, just, …) are the **menu and wiring**: name the
work, pass flags, compose dependencies. They are not a place to bury
non-trivial shell or application logic inside TOML/Make by default.

#### Inline in the task runner

Keep the recipe **in the task definition** only when it is a **series of
simple one-liner commands** (or a meta-task), for example:

- A single CLI call: `uv lock`, `hk check --all`, `hk uninstall || true`
- A short list of independent one-liners: `rm -rf .venv .nox` then
  `hk uninstall || true`
- Composition the runner already owns: `depends = ["a", "b"]`, aliases,
  `run = [{ task = "other" }]`

```toml
[tasks.validate]
description = "Full-tree format check + flake8"
run = "hk check --all"

[tasks.clean-preload]
description = "Undo preload env + hooks"
run = [
  "rm -rf .venv .nox",
  "hk uninstall || true",
]
```

No variables, pipelines, `find`, loops, or multi-branch control flow in the
task body.

#### Put it in a script (e.g. `.tasks/`)

Extract a file when **any** of these hold:

- The work **needs shell features** — variables, defaults, pipelines,
  `find`/`xargs`, loops, conditionals, non-trivial quoting
- **Shared behavior** across tasks or flags (preload `--clean`/`--check`,
  DevPod id slug used by up/stop/delete/status, Pulumi project+stack helper)
- Logic is hard to read, test, or review as inline one-liners
- Real reuse or tests outside the task runner

Then the task stays a **one-line call** to that script:

```toml
[tasks.clean-caches]
run = "python .tasks/clean.py caches"

[tasks.devpod-up]
run = "python .tasks/devpod.py up"
```

Language of the script is whatever fits (Python, bash, …). Prefer readability
over matching the task runner’s config language.

#### Do not

- Embed multi-line bash programs inside `mise.toml` / Make when a `.tasks/`
  file would be clearer
- Add a script that only wraps **one** simple CLI with no shared logic
  (`subprocess` → `hk check --all` and nothing else)
- Grow a private framework under `.tasks/` when the runner can compose tasks
  (`depends`, `mise run a ::: b`)

#### How to choose

1. Can it be **only** simple one-liners (or depends/alias)? → **inline**.
2. Needs shell features, sharing, or non-trivial logic? → **`.tasks/` script**,
   task is a one-line call.
3. Document operator entrypoints in `contributing/`, not long novels in config.

Bootstrap scripts that run **before** the tool manager exists (POSIX host
checks) are a separate layer from task-runner recipes; do not force them into
the same language or folder conventions as post-mise tasks.

### What agents should do

1. When adding or editing tooling config: keep files **content-focused**.
2. When teaching usage: edit or create project `contributing/` docs — not
   long preambles in config.
3. When running project tools: prefer the project’s documented tooling
   entrypoint over assuming global PATH or a pre-activated manager in the
   agent shell.
4. Do not install host tooling without operator confirmation.
5. Do not collapse tool-manager, package-manager, and task-runner roles into
   one ad-hoc script stack without project agreement.
6. When adding tasks: **inline only simple one-liners**; if shell features or
   shared logic are needed, use a `.tasks/` (or equivalent) script (see above).
