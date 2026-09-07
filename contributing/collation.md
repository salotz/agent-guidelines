# Collation

## Compacted Summaries

Throughout the documents many external references are made to good standards.
E.g. in the form of blogs written for humans or verbose standards meant for specificity.

The job of the collator is to go through these referenced documents,
compact and summarize them into the [content/shared/summaries](../content/shared/summaries) directory,
and create links to them at the location of original reference.

References should look like this (the summary path is relative to the document containing the reference, typically a file under `content/shared/`):

```markdown
Use this git commit message advice in this [post](https://chris.beams.io/git-commit) ([summary](./summaries/git-commit-messages-chris-beams.md))
```

Summaries live in [content/shared/summaries](../content/shared/summaries).

## Drop-In Templates

- Project bootloader:
  [content/shared/agents_md_template.md](../content/shared/agents_md_template.md) —
  drop into a project's `AGENTS.md`.
- Host bootloader:
  [content/personal/agents_md_template.md](../content/personal/agents_md_template.md) —
  drop into host-local context (`~/.agents/AGENTS.md`);
  configure harness pointers as in [content/personal/getting-started.md](../content/personal/getting-started.md).

Keep both up to date when guidelines change.

## Opinionated project templates

Specification-style stacks (not generator engines) live under [content/templates/](../content/templates/).
When those docs gain or change external references, collate summaries the same way as for shared prose and link them at the reference site.
