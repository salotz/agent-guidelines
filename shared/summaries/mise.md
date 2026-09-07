# Summary: mise (mise-en-place)

Source:
https://mise.jdx.dev/ (README:
https://raw.githubusercontent.com/jdx/mise/main/README.md)

## What it is
Dev-tools, environment variables, and project tasks in one CLI.
Declare them in `mise.toml`, commit the file, and use the same setup in shell, editor, and CI.

## How it works (core)
- **Tools**:
  install and pin versions (Node, Python, Go, and many others) per project;
  switch automatically with directory context when activated.
- **Environments**:
  project env vars from `mise.toml`, `.env` files, secrets, and commands.
- **Tasks**:
  named recipes (`mise run …`) that run with the project’s tools and env, with dependencies and parallel runs.
- **Bootstrap** (optional):
  declare broader machine setup (OS packages, dotfiles, services).
- Invocation without shell activation:
  `mise exec -- …` and `mise run …` apply pins and env for a single command (preferred for in-repo automation and agents).

## Characteristics relevant here
- Can cover both **tool/version manager** and **task runner** layers (see [Project management and tooling](../project-management-and-tooling.md)).
- Compatible with asdf-style `.tool-versions` and optional idiomatic version files.
- Local overrides and secrets belong in gitignored files (for example `.mise.local.toml`), not committed config.

## Agent notes
- Prefer project-documented `mise` entrypoints (`mise exec -- …`, documented tasks) over assuming interactive shell activation.
- Do not bury non-trivial logic in multi-line task bodies when a script under something like `.tasks/` would be clearer.
- Host installs of `mise` itself still need operator confirmation when not already present.
