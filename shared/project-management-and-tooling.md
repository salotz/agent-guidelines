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
