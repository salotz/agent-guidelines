# Summary: salotz RFC 28 - PRJX (Project Layout and Specification)

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.028_prjx

## Purpose
**PRJX** (“Project Spec Extended”) defines portable project-root discovery, in-repo config vs host-local trees, naming/IDs for projects and replicas, XDG/XDGX namespacing under `prjx/`, and `PRJX__`-prefixed project env vars.
Builds on ideas from the PRJ Base Directory Spec and patterns from [RFC 24](./salotz-rfc-024-extended-xdg-base-directory.md);
this RFC does **not** require PRJ compatibility.

## Core mechanisms

### Project root discovery (precedence)
1. `PRJX_ROOT` env var (absolute path).
2. Walk upward for sentinel file `.prjx-root` (empty;
   contents ignored).
Nested multiple sentinels are not a supported multi-root model;
closest upward match (or `PRJX_ROOT`) wins.

### In-repo directories
- **Config** (default `${PRJX_ROOT}/.config`, override `PRJX_CONFIG_HOME`):
  portable, tracked;
  reserved metadata file `_project-meta.toml`.
- **Local** (default `${PRJX_ROOT}/.local`, override `PRJX_LOCAL_CONFIG_HOME`):
  host/replica-specific, gitignored;
  reserved `_config.toml` deep-merged over portable metadata.

### Naming and identity
- Project **name** + **namespace** in `_project-meta.toml` form a fully qualified project name (for example `acme.wumpus`).
- **Replicas** (for example git worktrees) add a distinguisher → FQ replica name.
- **Project ID** (`PRJX_ID` or derived) uniquely identifies a replica on a host for global resources (constrained charset/length).

### Host XDG / XDGX homes under `prjx/`
Defaults such as `~/.config/prjx`, `~/.cache/prjx`, `~/.local/share/prjx`, plus XDGX-style tmp/scratch/var under `~/.local/…/prjx`, with env overrides.
Leaves are typically project- or replica-scoped using encoded names / `PRJX_ID`.

### Project-local env vars
Prefix `PRJX__…`.
Declare names and **groups** under `[project.env-vars]` in `_project-meta.toml` so tools know which vars a workflow needs.

## Agent notes
- New opinionated projects in these guidelines should include `.prjx-root` and `.config/_project-meta.toml` (see [Generic project template](../../templates/generic-project.md)).
- Do not commit `.local/`;
  put host overrides only there.
- Prefer `PRJX__` for project-specific env names to avoid collisions and ease discovery.
- For full field reference and examples, read the RFC tree (including `reference.md` and the `wumpus` example).
