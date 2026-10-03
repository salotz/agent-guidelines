# Software

Guidelines for authorship, maintenance, and documentation of software projects.

Load these when the work is software (implementation, review, tests, project docs)—in addition to [Generic Agent Guidelines](../generic-agent-guidelines.md).

## Contents

- [Guidelines](./guidelines.md):
  process and authorship (git hygiene, codetags, comments, commits, branching, tests, coverage)
- [Documentation](./documentation.md):
  structure software docs with [Diátaxis](https://diataxis.fr/) (tutorials, how-to guides, reference, explanation)
- [CLI UX](./cli-ux.md):
  interaction design (help, output, errors, flags; clig.dev)
- [CLI and application patterns](./cli-applications.md):
  product shape for shippable CLIs (layers, config/catalog, env, identity, resources, versioning, release)

### When the artifact is a CLI

Load **both** [CLI UX](./cli-ux.md) and [CLI and application patterns](./cli-applications.md), plus process/docs above.
UX is how the tool feels in a terminal; application patterns are how it is structured, configured, versioned, and released.
