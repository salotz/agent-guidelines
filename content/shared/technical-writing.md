# Technical Writing Guidelines

Guidelines for technical writing meant for a non-personal audience.

This is distinguished from "research" in that research is meant for an operator's personal use rather than external communication.

Audiences can be intra-company audiences or the public.

For agent-to-operator chat, plans for review, and other plain-text-first output, see [Writing for humans](./writing-for-humans.md).

For documentation aimed at project contributors (for example under `contributing/`), see [Writing Contributor Documentation](./writing-contributing.md).

## Formats

The default format for technical writing is Markdown.
Other systems are acceptable when presentation needs go beyond what Markdown can provide;
those choices are project-specific.

Examples include [Markdoc](https://markdoc.dev/), [Skribilo](https://www.nongnu.org/skribilo/), and [Pollen](https://docs.racket-lang.org/pollen/).

Guidance here is generic and usually uses Markdown examples,
though other formats may appear as well.

See the format-specific style guides —
for Markdown, [Markdown style guide](./markdown-style-guide.md).

## When to use tables

Do not use tables for verbose prose even if a tabular format makes sense abstractly.

Humans have a hard time reading and editing tables with lots of content in plain-text formats like Markdown.
Even formatted tables in web pages with lots of prose can be difficult to read.

Instead, favor sub-sections with headers for complex multi-column information.

For example, instead of:

```markdown
| Rule | Detail |
| --- | --- |
| Do this when that says to do it | We want to do this because it is the right thing to do and my human said so. |
| Another rule to follow | This is an additional rule we have to follow |
```

Write this:

```markdown
# Other headers...
## Rules

### Do this when that says to do it

We want to do this because it is the right thing to do and my human said so.

### Another rule to follow

This is an additional rule we have to follow
```

If there are many "rows," number or label the headers (for example `A`) so they are easier to reference.

Use tables when you want to organize structured,
succinct value-type information that is not prose —
for instance, tabulating version support:

```markdown
| App | Python |
| --- | --- |
| 0.1 | 3.11 - 3.14 |
| 0.2 | 3.13 - 3.15 |
```

All of these rules are especially true for human-to-agent communication,
such as planning,
where Markdown is the medium of communication and not a rendered presentation format like HTML.

There may be exceptions for more polished documents and documentation for which a table is favorable when rendered in HTML.

This preference will be made explicit when needed,
so default to not using tables —
or only suggest them.

## Code Snippets

Follow the same guidelines for writing [software](./software-guidelines.md).
However, by the nature of code snippets they will differ in some ways (for example modularity).
Those differences will be documented specifically in time.

### Comments

For comments in snippets, follow the [source comment](./software-guidelines.md#source-comments) style used in real code—comments on their own lines:

```python
# Data for the process
data = {
    # the first thing
    "a": 1,
    # the second thing
    "b": 2,
}
```

Comments in code snippets should not use [codetags](./glossary.md#codetag) (unless demonstrating codetags) and may break from the stricter rules that apply in actual software module code.
