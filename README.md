# Coding Guidelines

This repo is intended to be a guide primarily read by agents to understand preferences and guidelines for coding, research, or any other task.

This repo should be accessible for reference over the internet or placed into context accessible by agents for work on a project.

Guideline substance lives under **`content/`**:

- [content/shared/](./content/shared/):
  Portable guidelines that can be imported into projects and shared with others (primarily **agent-readable**).
- [content/personal/](./content/personal/):
  Operator- and host-specific guidelines (shell config, host paths, personal skills).
  Stays in this repo;
  load when that context applies (still shaped for agents on those hosts).
- [content/operator/](./content/operator/):
  **Human operator** guidance for designing and running host controls around agents (sandboxing, PATH layout, loud bypasses).
  Not default agent session context.
- [content/templates/](./content/templates/):
  Opinionated stacks (specifications, not generators).

See the [shared glossary](./content/shared/glossary.md) for definition of specific terms and concepts as they are used throughout these documents.

## Shared guideline groups

- [Generic Agent Guidelines](./content/shared/generic-agent-guidelines.md):
  Generic advice for any agent-assisted work.
- [Project Management and Tooling](./content/shared/project-management-and-tooling.md):
  Config-file style, layered tooling, bootstrap/preload, and how automation invokes tools.
- [Software](./content/shared/software-guidelines.md):
  Guidelines specific to the authorship and maintenance of software projects.
- [Blob Management](./content/shared/blob-management.md):
  Large or opaque files managed alongside repository history (prefer DVC over git-lfs for new projects).
- [Markdown style guide](./content/shared/markdown-style-guide.md):
  GFM dialect, wrapping, fences, tables, and related formatting rules.
- [Technical Writing](./content/shared/technical-writing.md):
  Guidelines for technical writing meant for a non-personal audience.
- [Writing Contributor Documentation](./content/shared/writing-contributing.md):
  Contributor-facing docs, style-guide exceptions, and avoiding upstream restatement.
- [Research](./content/shared/research.md):
  Guidelines for research.

## Operator guideline groups

For humans who install and supervise agents on a host (not agent bootloaders):

- [Operator hub](./content/operator/README.md):
  audience split and how to use this tree.
- [Sandboxing agents](./content/operator/sandboxing.md):
  why default-on sandboxing, principles, threat model, escape hatches.
- [Implementing a sandboxed agent CLI](./content/operator/implementing-sandbox.md):
  reproduce the landrun + opt + PATH wrapper pattern on your own machine.

## Templates

Opinionated, concrete stacks (not a cookiecutter/copier generator):

- [Templates hub](./content/templates/README.md)
- [Generic project template](./content/templates/generic-project.md):
  git, mise, hk, EditorConfig, PRJX, RFC 22 agent layout, blob defaults

## Getting Started

- Projects:
  [shared/getting-started.md](./content/shared/getting-started.md) (drop-in project `AGENTS.md`).
- Hosts (personal):
  [personal/getting-started.md](./content/personal/getting-started.md) (drop-in `~/.agents/AGENTS.md`, plus harness pointers such as `~/.config/goose/AGENTS.md` for goose).
- Host hardening (operators):
  [operator/README.md](./content/operator/README.md) (sandboxing patterns distilled from bimker design decisions).

## Maintaining this repo

See [contributing/](./contributing/) for roles and workflows used to maintain this repository (not part of the portable shared set).

**Developing this checkout** (bootstrap, mise, hooks, checks):
[contributing/development.md](./contributing/development.md).

**Design / ADRs**:
[design/](./design/).
