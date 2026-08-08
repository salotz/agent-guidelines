# Software Guidelines

These are guidelines specific to the authorship and maintenance of software projects.

## Generic

Requirements for generic coding tasks.

### Code Tags

Follow [salotz RFC 6](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](./summaries/salotz-rfc-023-local-agent-context.md)) when writing comments in source code.

### Git Commit Messages

Use the advice in this [blog post](https://chris.beams.io/git-commit) ([summary](./summaries/git-commit-messages-chris-beams.md))

### Git Branching Strategies

Unless otherwise specified in a project you should always assume that the repository follows the [trunk based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development) ([summary](./summaries/trunk-based-development-atlassian.md)) pattern with short lived feature branches.

There should be no merges and instead use rebasing heavily.

Treat writing git commits like "layers" rather than simply a stream of consciousness.

While not required it is nice to rebase commits into logical chunks of understanding.

This aids in code review for multi-step changes.


