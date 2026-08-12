# Summary: salotz RFC 26 - Domain Local Configuration

Source: https://github.com/salotz/rfcs/tree/master/rfcs/salotz.026_domain-local-configuration

## Purpose
Standard mechanism for host-local, domain- or directory-scoped configuration shared across projects (and git worktrees) without committing host-specific files to remote repos. Builds on domain concepts from RFC 25 but does not strictly depend on it.

## Problem
Per-repo local config (e.g. `.envrc.local` with direnv) often duplicates host- or directory-wide settings. Git worktrees multiply the duplication.

## Specification
Beside a project collection directory (e.g. `~/projects` or `~/tree/<domain>/devel`), place a `.local/` directory:

- `~/projects/.local/` — config scoped to that collection
- `~/projects/.local/<project>/` — config shared by a primary project name and its worktrees

Worktrees named like `widgets__feat-a` share `.../.local/widgets/` with the primary `widgets` checkout.

## direnv pattern (example)
Remote `.envrc` loads shared non-host settings, then:

```sh
source_env_if_exists .envrc.local
```

Host `.envrc.local` in the repo points at the shared local file:

```sh
source ../.local/widgets/.envrc.local
```

## Agent relevance
Software projects under a domain `devel/` tree often share host-local configuration via a sibling `.local/` directory (RFC 26). Prefer extending that shared config over copying host paths into each clone or worktree.
