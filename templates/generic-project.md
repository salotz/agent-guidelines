# Generic project template

This document describes a very opinionated approach to starting **projects**.

These guidelines are not specific to any particular software language or type of project.

## Version control

Use [git](https://git-scm.com/) for version control.

## Tooling package manager

Use [mise](https://mise.jdx.dev/) ([summary](../shared/summaries/mise.md)) for project-specific tooling package management.

## Task runner

Use [mise](https://mise.jdx.dev/) ([summary](../shared/summaries/mise.md)) for defining tasks to run.

Follow the guidelines on writing tasks longer than one-liners in [Project management and tooling](../shared/project-management-and-tooling.md).

## Task authoring

Use [Python](https://www.python.org/) for writing anything more complicated than a one-liner, as opposed to bash, to enable testing and code quality.

## Git hooks manager

Use [hk](https://hk.jdx.dev/) ([summary](../shared/summaries/hk.md)) for managing and running git hooks.

## EditorConfig

Use [EditorConfig](https://editorconfig.org/) ([summary](../shared/summaries/editorconfig.md)) `.editorconfig` files for specifying details like line endings and tab meaning.

Default to Unix-style newlines and a final newline on every file:

```ini
[*]
end_of_line = lf
insert_final_newline = true
```

## PRJX standard

Use the [salotz RFC 28: PRJX](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.028_prjx) ([summary](../shared/summaries/salotz-rfc-028-prjx.md)) standard for meta-project information and organization.

Projects should contain a root sentinel file (`.prjx-root`) as well as a `.config` folder with `_project-meta.toml`.

## AI agent enablement

Use the [salotz RFC 22: AI Coding Repository Structure](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md)) standard for organizing agent context and integration with host context.

## Large file management

Large file management needs can vary based on project requirements.

By default choose a system that is not dependent on features of the git server.
In order:

1. [DVC: Data Version Control](https://dvc.org/) ([summary](../shared/summaries/dvc.md))
1. [git-lfs](https://git-lfs.com/) ([summary](../shared/summaries/git-lfs.md))
1. *ad hoc* system

See also [Blob management](../shared/blob-management.md).

## Guidelines context

Projects should follow the guidelines in the [shared guidelines](../shared/) section.

Projects should pull in the relevant context for both humans (`contributing/`) and agents (for example `.agents/`) so that the project is self-documenting.
See the [getting started](../shared/getting-started.md) section.
