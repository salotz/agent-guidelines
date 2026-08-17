# Host Agent Context

This host follows the operator guidelines at
https://github.com/salotz/agent-guidelines
(`personal/` plus the portable `shared/` baseline).

This file is host-local context (salotz RFC 23), intended for
`~/.agents/AGENTS.md`. It applies on this machine in addition to any
project `AGENTS.md`.

Agent harnesses that do not load `~/.agents/AGENTS.md` automatically
need a pointer from their own config (for example goose:
`~/.config/goose/AGENTS.md` → this file). See personal getting-started.

## Load order

1. **Shared (portable baseline)** — always apply when available:
   - `shared/generic-agent-guidelines.md`
   - task-specific shared docs as needed (`software-guidelines.md`,
     `research.md`, `technical-writing.md`, `writing-contributing.md`)
   - `shared/glossary.md` for terms
   - `shared/summaries/` before fetching full external standards
2. **Personal (this host)** — apply because this host context is loaded:
   - `personal/work-process.md` — plan-execute + multi-session decision Q&A
   - `personal/shell-and-bimker.md`
   - `personal/host-layout.md`
   - `personal/skills/` as relevant to the task

If the guidelines repo is not already in context, locate or cache a
checkout of `salotz/agent-guidelines` and read from there rather than
relying only on this summary.

## Personal host rules (summary)

- **Shell configuration** is managed with
  [bimhaw](https://github.com/salotz/bimhaw). Do not edit generated
  `~/.bashrc` / `~/.profile` style files as the source of truth.
- **Bimker** (`~/.bimker`) is the git-tracked personal host configuration
  repo. Shell modules live at `~/.bimker/bimhaw`.
- **Before changing Bimker**: propose exact changes and rationale; get
  confirmation. Do not refactor Bimker unless that repo is the active
  project and the operator asked for it.
- **Host installs**: confirm with the operator first; outline install and
  integration steps (binaries, shell modules, cache/data dirs). Prefer
  project-local tooling over host-wide installs when possible.
- **Host / agent paths**: obey salotz RFC 23 (local agent context) and
  RFC 24 (extended XDG; agent-generated data under `xagents/` namespaces).
- **Host layout**: remote work under `~/tree/<domain>/` (domains
  `personal`, `examol`); see `personal/host-layout.md` and RFCs 25/26.
- **Remote git repos as reference**: prefer a local cache via the personal
  **cacher** skill (`personal/skills/cacher`) instead of repeated clones.
- **Tools**: prefer task-specific tools over a generic shell when available.

## Personal skills

- `personal/skills/cacher` — cache/refresh remote git repositories for
  reuse as reference context.

## Shared habits still in force

- Prefer task-specific tools over shell.
- Follow salotz RFC 22 (project layout), RFC 23/24 (host context and paths),
  and RFC 25/26 when placing or resolving host domain paths.
- Cache remote resources locally when possible.
- Use compacted inlining; check `shared/summaries/` first.
- Store multi-session plans under `.agents/plans` in the project when
  applicable.
- Multi-session decision back-and-forth: only that plan’s `decisions.md`
  (see `personal/work-process.md`); no all-caps `PROCESS.md` or dated
  `review-*.md` questionnaires.

## Conflicts

On conflict between project remote context and this host context, prefer
this file for host paths, shell, installs, and personal skills; prefer
project context for project-specific policy. If still ambiguous, ask the
operator before acting.
