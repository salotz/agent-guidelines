# Project management and tooling

Generic rules for how agents (and humans) structure **project tooling** and **machine-oriented config** versus **human/operator usage docs**.

## Relationship to other standards

| Concern | Where it lives |
|---------|----------------|
| Project folder layout, `contributing/`, `AGENTS.md` | [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](./summaries/salotz-rfc-022-ai-coding-structure.md)) |
| Project root, `.config` / `.local`, project metadata | [salotz RFC 28 PRJX](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.028_prjx) ([summary](./summaries/salotz-rfc-028-prjx.md)) |
| Host operator prefs, install confirmation, shell integration | [salotz RFC 23](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](./summaries/salotz-rfc-023-local-agent-context.md)); see also [Host System Interaction](./generic-agent-guidelines.md#host-system-interaction) |
| Where binaries and agent-generated data live on disk | [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](./summaries/salotz-rfc-024-extended-xdg-base-directory.md)) |
| Concrete default stack for new projects | [Generic project template](../templates/generic-project.md) |
| **This document** | In-repo tooling contracts, config-file style, bootstrap/preload, and how automation invokes tools |

## Config and tooling files stay content-focused

Applies to machine-oriented manifests such as:
`mise.toml` / `.mise.toml`,
`justfile`,
`pyproject.toml`,
`package.json`,
`Makefile`, CI workflow env blocks, and similar.

- **Do**:
  short comments on *why a specific choice or value* exists.
- **Do not**:
  long usage, bootstrap, install tutorials,
  or copy-paste operator manuals inside those files.
- **Do**:
  put how-to, bootstrap, and day-to-day workflow in the **project** `contributing/` tree (RFC 22), for example `contributing/development.md`.

## Layered tooling (do not collapse roles)

Keep responsibilities separate.
Concrete tools are examples;
projects choose stacks.

| Layer | Role | Examples |
|-------|------|----------|
| Tool / version manager | Pin and install **project CLIs**; optional project env vars | [mise](https://mise.jdx.dev/) ([summary](./summaries/mise.md)), asdf, devbox |
| Language package manager | Project dependencies, lockfiles, build/publish | uv, npm, cargo |
| Task runner | Thin recipes only — not a second package manager | mise tasks, just, make |
| Git hooks / project checks | Shared lint/format steps in hooks and CI | [hk](https://hk.jdx.dev/) ([summary](./summaries/hk.md)) |
| Build backend | Packaging metadata and build — not env management | hatchling, setuptools |

## Invocation: project automation vs operator shell

- **In-repo** commands (task recipes, scripts,
  CI,
  agent-run docs happy path):
  call the tool manager explicitly (for example `mise exec -- …`) so pinned tools and env apply **without** requiring shell activation.
- **Operator** interactive shell:
  activate,
  shims,
  or bare PATH are **optional** personal choice.
  Never make them a prerequisite for project recipes to work.

Document the project’s [tooling entrypoint](./glossary.md#project-tooling-entrypoint) in `contributing/` (for example `mise exec -- …` or `mise run <task>`), not only in config file comments.

## Secrets and local overrides

- Committed config:
  non-secret defaults only.
- Secrets and machine-local overrides:
  gitignored local files (pattern depends on stack;
  for example `.mise.local.toml`).
  Never commit tokens.
  For secrets prefer just-in-time fetching from tools if possible (for example `bitwarden` or `op` (1Password)).

## Bootstrapping a project

Most tooling should be **project-local** when possible.
That still requires a minimal **host** capability to install and invoke the project tool manager.

**[Project bootstrapping](./glossary.md#project-bootstrapping)** is the stage from “operator has a project replica” until host prerequisites are satisfied so project-managed tools can run.
It typically includes:

1. Installing required **host** tools (outside the project pin set).
2. Checking that required (and reporting optional) host tools are present at usable versions.
3. Setting any required host-local settings or environment needed before the tool manager works.

Keep bootstrap materials in **`.bootstrap/`**, separate from post-bootstrap project config and tasks.
Bootstrap scripts and configs must be **highly portable**: prefer POSIX `sh` and a portable command suite (for example `awk`, `sed`, `cut`) with **no** dependency on the project tool manager, language runtimes, or package managers that bootstrap is trying to unlock.

Document the human flow in `contributing/` (for example onboarding or development), not as long tutorials inside `mise.toml`.

### Stages (happy path)

Order matters:

1. **Bootstrap (host)** —
   ensure required host tools exist (see below).
   Check-only scripts do **not** install packages or run network installers unless the project explicitly documents a separate, operator-approved installer path.
2. **Install project tools** —
   once the tool manager is on `PATH`, pin-install from project config (for example `mise install`).
3. **[Preload](./glossary.md#preload)** —
   project-local initialization via the task runner (for example `mise run preload`):
   hooks, language envs, other checkout setup that assumes pins are already installed.

Agents must not skip straight to preload when host-tool checks would fail, and must not treat preload as a substitute for host bootstrap.

### Minimal bootstrap layout

Recommend at least:

| Path | Role |
|------|------|
| `.bootstrap/host-tool-check` | Executable POSIX `sh` **check-only** script |
| `.bootstrap/host-tools.conf` | Required/optional host tools and minimum versions |

Run either:

```sh
./.bootstrap/host-tool-check
```

or, after the tool manager is available, a thin task that only wraps the same script (for example `mise run host-tool-check` → `sh .bootstrap/host-tool-check`).

### `host-tools.conf` format

Line-oriented, whitespace-separated columns, `#` comments, blank lines ignored.
Designed for easy parsing with portable POSIX tools (not YAML/TOML/JSON):

```text
# name    min_version    class
git     2.30.0    required
mise    2024.1.0  required

docker  -         optional
podman  -         optional
devpod  0.5.0     optional
gcloud  -         optional
op      -         optional
```

| Column | Meaning |
|--------|---------|
| name | Executable expected on `PATH` |
| min_version | Dotted numeric minimum (for example `2.30.0`), or `-` for “any version if present” |
| class | `required` (missing or too old → fail) or `optional` (missing or too old → warn) |

**What belongs here vs project pins:**

- **Host conf:** tools the operator (or CI image) must provide **before** or **outside** project-managed installs — typically `git`, the tool manager (`mise`), and optional host integrations (`docker`, `gcloud`, `op`, …).
- **Project tool config** (for example `mise.toml` `[tools]`): language runtimes, linters, `hk`, CLIs installed **by** the tool manager after bootstrap.

Do not duplicate mise-managed pins into `host-tools.conf`.

### Preload (post-bootstrap)

After host-tool check passes and project tools are installed, operators run a **`preload`** [task](./glossary.md#task).

Preload **initializes the checkout** using already-installed project tools.
Examples:
install git hooks (`hk install`),
sync a language virtualenv,
generate local derived config.

Conventions:

- Task name is exactly **`preload`** (for example `mise run preload`).
- Prefer a thin task body that calls a script under `.tasks/` when preload is more than one simple CLI (shared flags, best-effort hooks, clean/check modes).
- Optional companion tasks (examples only): `clean-preload`, `host-tool-check` — keep them one-liners or small scripts per the task rules below.
- Preload must **not** be the host bootstrapper:
  no “install mise from the internet” inside `preload` by default.

Reference shape (illustrative only; not a dependency of this guidelines repo):
examol `ops` uses `.bootstrap/host-tool-check` + `host-tools.conf`, then `mise install`, then `mise run preload` → `.tasks/preload.py` (for example `uv sync` + `hk install`).

## Host-installed tools

Prefer project-local tooling over host-wide installs when possible.

Host installs need **operator confirmation** first,
plus a short plan for integration (binaries, shell modules, cache/data dirs).
See [Host System Interaction](./generic-agent-guidelines.md#host-system-interaction) and RFC 23.
Typical host-level cases:
bootstrapping a tool manager,
tools that must be host-wide,
or projects with no project-local tooling story.

Projects should provide portable check (and any approved install) materials under **`.bootstrap/`** (see [Bootstrapping a project](#bootstrapping-a-project)).

## Task runner recipes vs external scripts

Task runners (mise tasks, Make, just, …) are the **menu and wiring**:
name the work, pass flags, compose dependencies.
They are not a place to bury non-trivial shell or application logic inside TOML/Make by default.

### Inline in the task runner

Keep the recipe **in the task definition** only when it is a **series of simple one-liner commands** (or a meta-task), for example:

- A single CLI call:
  `uv lock`, `hk check --all`, `hk uninstall || true`
- A short list of independent one-liners:
  `rm -rf .venv .nox` then `hk uninstall || true`
- Composition the runner already owns:
  `depends = ["a", "b"]`, aliases, `run = [{ task = "other" }]`

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

No variables, pipelines, `find`, loops, or multi-branch control flow in the task body.

### Put it in a script (e.g. `.tasks/`)

Extract a file when **any** of these hold:

- The work **needs shell features** —
  variables,
  defaults,
  pipelines,
  `find`/`xargs`,
  loops, conditionals, non-trivial quoting
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

Language of the script is whatever fits (Python, bash, …).
Prefer readability over matching the task runner’s config language.

### Do not

- Embed multi-line bash programs inside `mise.toml` / Make when a `.tasks/` file would be clearer
- Add a script that only wraps **one** simple CLI with no shared logic (`subprocess` → `hk check --all` and nothing else)
- Grow a private framework under `.tasks/` when the runner can compose tasks (`depends`, `mise run a ::: b`)

### How to choose

1. Can it be **only** simple one-liners (or depends/alias)?
   → **inline**.
2. Needs shell features, sharing, or non-trivial logic?
   → **`.tasks/` script**, task is a one-line call.
3. Document operator entrypoints in `contributing/`, not long novels in config.

Bootstrap scripts that run **before** the tool manager exists (POSIX host checks under `.bootstrap/`) are a separate layer from task-runner recipes and from `.tasks/` preload helpers;
do not force them into the same language or folder conventions as post-bootstrap tasks.

## What agents should do

1. When adding or editing tooling config:
   keep files **content-focused**.
2. When teaching usage:
   edit or create project `contributing/` docs —
   not long preambles in config.
3. When running project tools:
   prefer the project’s documented [tooling entrypoint](./glossary.md#project-tooling-entrypoint) over assuming global PATH or a pre-activated manager in the agent shell.
4. Respect bootstrap → install → preload order;
   run `.bootstrap/host-tool-check` (or the project’s equivalent) when host readiness is unclear.
5. Do not install host tooling without operator confirmation.
6. Do not collapse tool-manager, package-manager,
   and task-runner roles into one ad-hoc script stack without project agreement.
7. When adding tasks:
   **inline only simple one-liners**;
   if shell features or shared logic are needed,
   use a `.tasks/` (or equivalent) script (see above).
8. Name checkout initialization **`preload`**;
   do not invent a parallel “setup” task that duplicates it without project agreement.
