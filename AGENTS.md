# Agent guidelines

This is a repository meant to be read by agents and provide guidelines for agentic aided work.

## Guideline substance (`content/`)

- [shared/](./content/shared/):
  portable guidelines for import into projects
- [personal/](./content/personal/):
  operator host, configuration, and personal skills
- [operator/](./content/operator/):
  human-operator docs (sandboxing agents, host hardening patterns).
  Load when implementing or explaining host sandbox controls;
  do not treat as default session rules.
- [templates/](./content/templates/):
  opinionated project stacks (specifications, not generators)

Not all of it is meant to be used for all projects and it is organized such that it can be referenced independently.

For most project work, load shared guidelines (`content/shared/`).
Also load personal guidelines when interacting with this operator's host,
shell configuration, or personal skills.
When the task is designing or reproducing agent CLI sandboxing on a host, read `content/operator/`.

## This repository (control plane)

- [contributing/](./contributing/):
  maintainer roles — start with [development.md](./contributing/development.md) (bootstrap → preload → check)
- [design/](./design/):
  goals and ADRs for this repo
- [`.agents/`](./.agents/):
  plans, [repo map](./.agents/context/repo-map.md), optional skills
- Tooling:
  `.bootstrap/`, `mise.toml`, `hk.pkl`, `.editorconfig`, PRJX (`.prjx-root`, `.config/`)

## Drop-in bootloaders (for other projects / hosts)

- Project: [shared/agents_md_template.md](./content/shared/agents_md_template.md)
- Host: [personal/agents_md_template.md](./content/personal/agents_md_template.md)
  (see [personal/getting-started.md](./content/personal/getting-started.md); harness pointer e.g. `~/.config/goose/AGENTS.md`)

## Reference aids

- Compacted external summaries: [content/shared/summaries](./content/shared/summaries)
- Glossary: [content/shared/glossary.md](./content/shared/glossary.md)
- Topic examples: [project-management-and-tooling.md](./content/shared/project-management-and-tooling.md), [blob-management.md](./content/shared/blob-management.md)
- Generic stack: [content/templates/generic-project.md](./content/templates/generic-project.md)
- Skills: [content/personal/skills/](./content/personal/skills/), [content/shared/skills/](./content/shared/skills/)

This project is "reflective" in the sense that it should follow its own guidelines.
