# Coding Guidelines

This repo is intended to be a guide primarily read by agents to
understand preferences and guidelines for coding, research, or any
other task.

This repo should be accessible for reference over the internet or
placed into context accessible by agents for work on a project.

Guidelines are split into two trees:

- [shared/](./shared/): Portable guidelines that can be imported into
  projects and shared with others.
- [personal/](./personal/): Operator- and host-specific guidelines
  (shell config, host paths, personal skills). Stays in this repo; load
  when that context applies.

See the [shared glossary](./shared/glossary.md) for definition of
specific terms and concepts as they are used throughout these
documents.

## Shared guideline groups

- [Generic Agent Guidelines](./shared/generic-agent-guidelines.md):
  Generic advice for any agent-assisted work.
- [Project Management and Tooling](./shared/project-management-and-tooling.md):
  Config-file style, layered tooling, and how automation invokes tools.
- [Software](./shared/software-guidelines.md): Guidelines specific to
  the authorship and maintenance of software projects.
- [Blob Management](./shared/blob-management.md): Large or opaque files
  managed alongside repository history (prefer DVC over git-lfs for new
  projects).
- [Technical Writing](./shared/technical-writing.md): Guidelines for
  technical writing meant for a non-personal audience.
- [Writing Contributor Documentation](./shared/writing-contributing.md):
  Contributor-facing docs, style-guide exceptions, and avoiding upstream
  restatement.
- [Research](./shared/research.md): Guidelines for research.

## Getting Started

- Projects: [shared/getting-started.md](./shared/getting-started.md)
  (drop-in project `AGENTS.md`).
- Hosts (personal): [personal/getting-started.md](./personal/getting-started.md)
  (drop-in `~/.agents/AGENTS.md`, plus harness pointers such as
  `~/.config/goose/AGENTS.md` for goose).

## Maintaining this repo

See [contributing/](./contributing/) for roles and workflows used to
maintain this repository (not part of the portable shared set).
