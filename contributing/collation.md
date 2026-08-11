# Collation

## Compacted Summaries

Throughout the documents many external references are made to good standards. E.g. in the form of blogs written for humans or verbose standards meant for specificity.

The job of the collator is to go through these referenced documents, compact and summarize them into the [shared/summaries](../shared/summaries) directory, and create links to them at the location of original reference.

References should look like this (the summary path is relative to the document containing the reference, typically a file under `shared/`):

```markdown
Use this git commit message advice in this [post](https://chris.beams.io/git-commit) ([summary](./summaries/git-commit-messages-chris-beams.md))
```

Summaries live in [shared/summaries](../shared/summaries).

## Drop-In Templates

- Project bootloader: [shared/agents_md_template.md](../shared/agents_md_template.md) — drop into a project's `AGENTS.md`.
- Host bootloader: [personal/agents_md_template.md](../personal/agents_md_template.md) — drop into host-local context (`~/.agents/AGENTS.md`); configure harness pointers as in [personal/getting-started.md](../personal/getting-started.md).

Keep both up to date when guidelines change.
