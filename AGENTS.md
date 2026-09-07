# Agent guidelines

This is a repository meant to be read by agents and provide guidelines for agentic aided work.

Guideline substance lives under **`content/`**:

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

See the [contributing](./contributing/) directory for roles and workflows to run when maintaining this repository.

The file [shared/agents_md_template.md](./content/shared/agents_md_template.md) is a pre-generated context to put into your projects.

The file [personal/agents_md_template.md](./content/personal/agents_md_template.md) is a pre-generated context to put into host-local agent config (`~/.agents/AGENTS.md`).
Agent harnesses may need a separate pointer (e.g. goose:
`~/.config/goose/AGENTS.md`).
See [personal/getting-started.md](./content/personal/getting-started.md).

The [shared/summaries](./content/shared/summaries) folder contains compacted summaries of externally referenced resources.
Check here before reading the referenced resource.

Shared topic docs under [content/shared/](./content/shared/) are independently referenceable (for example [project-management-and-tooling.md](./content/shared/project-management-and-tooling.md) and [blob-management.md](./content/shared/blob-management.md)).

Concrete opinionated stacks live under [content/templates/](./content/templates/) (start with [generic-project.md](./content/templates/generic-project.md)).

Personal skills live under [content/personal/skills/](./content/personal/skills/).
Shared skills live under [content/shared/skills/](./content/shared/skills/).

This project is "reflective" in the sense that it should follow its own guidelines.
