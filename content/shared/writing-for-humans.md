# Writing for humans

Guidelines on how agents should write when communicating with human [operators](./glossary.md#operator).

This covers chat and prompt output,
plans written for operator review,
and other agent-to-human prose that is usually read as plain text rather than as a polished web page.

It shares some preferences with [technical writing](./technical-writing.md),
but the audience and medium differ:
technical writing targets a non-personal external or intra-org audience;
this document targets the operator in the loop.

For contributor-facing project docs, see [Writing Contributor Documentation](./writing-contributing.md).
For Markdown dialect and formatting mechanics, see the [Markdown style guide](./markdown-style-guide.md).

## Default assumptions

- Prefer prose that is easy to scan in a terminal, chat buffer, or plain-text editor.
- Assume the reader sees raw Markdown (or lightly rendered Markdown), not a designed HTML page.
- Keep output terse.
  Operators read much more slowly than agents;
  avoid [too much information (TMI)](./glossary.md#too-much-information-tmi).
- Favor structure that works without rich rendering:
  short sections, lists, and plain headings over dense emphasis and wide tables.

Table guidance that applies especially to human-to-agent and plan prose lives in [Technical writing: When to use tables](./technical-writing.md#when-to-use-tables).

## Patterns to avoid

### Overuse of bold and emphasis

Do not overuse bold and emphasis markers.

Reserve strong emphasis for rare cases where the medium is known to be rendered as a web page and the highlight carries real weight (for example a single warning label).

In default agent output, extra `*` / `**` markup is noise:
it is harder to parse in raw text and rarely improves comprehension.

Prefer plain wording and clear headings over decorating every key phrase.
