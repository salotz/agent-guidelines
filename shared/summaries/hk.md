# Summary: hk

Source:
https://hk.jdx.dev/ (README:
https://raw.githubusercontent.com/jdx/hk/main/README.md)

## What it is
Git hooks and project checks that run linters and formatters **in parallel**, with file locking so independent steps run concurrently and writers on the same files take turns.
Same steps are reusable in hooks, the terminal, and CI.

## How it works (core)
- Config is Pkl (`hk.pkl`), often generated with `hk init` from detected project tools.
- Typical bootstrap (often via mise):
  `hk init`, `hk install`, then `hk check --all`.
- Everyday commands:
  `hk check` / `hk fix` (modified files),
  `hk check --all` (full tree / CI),
  plan and explain flags (`--plan`, `--why`).
- Hooks can stash unstaged work around fixes on staged files, then restore.
- hk configures **how** to run tools;
  it does not install those linters/formatters for you (pair with mise or another tool manager).
- Optional mise integration and bundled agent skills (`hk-configure`, `hk-debug`) in releases.

## Characteristics relevant here
- Fits the “git hooks manager” slot in opinionated project templates.
- Keeps check/fix configuration shareable and typed via Pkl rather than ad-hoc shell hook scripts.
- Aligns with thin task-runner recipes that call `hk check --all` / similar (see [Project management and tooling](../project-management-and-tooling.md)).

## Agent notes
- Prefer project `hk.pkl` and documented recipes over inventing parallel hook frameworks.
- After `hk fix`, review `git diff` / staged changes before commit.
- Ensure linter binaries the config expects are on `PATH` (usually via mise) before debugging “missing tool” hook failures.
