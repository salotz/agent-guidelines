# Markdown style guide

## Dialect

All Markdown documents should default to using the [GitHub Flavored Markdown (GFM)](https://github.github.com/gfm/) dialect.
This means the documented spec and not the current renderer features on [github.com](https://github.com) that are not in the spec should not be used for these documents.

## Style guide

All Markdown documents should follow the [style guide from Ciro Santilli](https://cirosantilli.com/markdown-style-guide/).
This guide provides several options that must have a stance taken on.
The following choices should be made accordingly:

| option           | choice           |
|------------------|------------------|
| `space-sentence` | `1`              |
| `wrap`           | `inner-sentence` |
| `list-marker`    | `hyphen`         |
| `code`           | `fenced`         |
| `header`         | `atx`            |
| `list-space`     | `mixed`          |

## Additional style rules

Some additional clarifications or adjustments to the style guide.

We usually defer to specifics in the [Google Markdown style guide](https://google.github.io/styleguide/docguide/style.html),
but this is not adopted wholesale as it differs significantly in philosophy.

### Line wrapping

For the `space-sentence` / `inner-sentence` choice specifically, follow the [Semantic Line Breaks](https://sembr.org/) guide for line breaks.
Also see the [sembr reformat skill](https://github.com/sembr/skills/blob/5f973aaa75b1165b03dd45dca4cd1dc0437deba3/skills/sembr-reformat/SKILL.md).

**Semantic breaks are the rule; column width is not.**

Apply breaks only where meaning already divides the prose:

1. After every sentence ending in `.`, `!`, or `?`.
2. Prefer a break after independent clauses ending in `;`, `:`, or `—`.
3. Optionally break after a comma when it separates true independent clauses (both sides substantial), not after every list comma or short phrase.
4. Optionally break after a dependent clause when that clarifies structure.
5. Never break inside a hyphenated word.
6. Never break at an arbitrary space only to satisfy a column budget.

A line may exceed 80 columns when the sentence or clause has no semantic break.
That is expected and preferred over mid-phrase wraps.

About **80 columns** is a soft *goal* for how long a single thought may run when you *could* break at a real boundary, not a hard wrap width.
Do not soft-wrap long sentences at spaces to “fit” 80 characters.
If you did that, you would lose the point of semantic line breaks (one thought per line, stable diffs at phrase boundaries).

**Incorrect** (break only for width):

```markdown
This user uses the [bimhaw](https://github.com/salotz/bimhaw) tool for managing
shell configuration.
```

**Correct** (one sentence, one line, even if long):

```markdown
This user uses the [bimhaw](https://github.com/salotz/bimhaw) tool for managing shell configuration.
```

URLs, links, and code spans may make lines longer still; leave them intact.

<!-- TODO: add document suggesting settings for tooling and editors to support this in git hooks and editing. -->

### Headers

Follow the guidance in the [Google developer documentation style guide on capitalization](https://developers.google.com/style/capitalization) for headers.

In short: use sentence case and do not add end punctuation.

### Inline HTML

Never use inline HTML (except for comments) unless the document is specifically written for web display.
For example a GitHub README can use inline HTML for badges for display on the website,
but an internal planning document should never use HTML.

### Synonym headers

Prefer never to use synonym headers.
These confuse writing.
Instead prefer using ID markers.

### Links

Follow the guidance in the [Google Markdown style guide](https://google.github.io/styleguide/docguide/style.html) on links.

Specifically, note that paths from the root treat root as the **project root**.
See guidance on project organization for how to handle this.

Local viewers and code editors may need special support for handling link paths in this manner and not treating them as the actual host root.

<!-- TODO: Create an accessory guide with tips on configuring code editors on how to reconfigure themselves for this practice. Use the PRJX standard (shared/summaries/salotz-rfc-028-prjx.md) for identifying project roots. -->

### Reference links

Prefer reference links (footnotes) for reused URLs or for long URLs.
There is no hard requirement because line lengths are not fixed.

Otherwise follow the [Google Markdown style guide](https://google.github.io/styleguide/docguide/style.html).

Reference link definitions should be placed in their own header section or after a final thematic break if document-wide.
If they are placed in the local header section they should also be placed after a thematic break but not under a header.

### Code fences

Always set a language tag on fenced code blocks.
Never open a fence with only three backticks and no language info string unless there really is no better option.

For **shell commands** (including CLI invocations the reader would run in a terminal), use one of:

| Tag       | Use                                                           |
|-----------|---------------------------------------------------------------|
| `sh`      | Portable POSIX-oriented examples (default for this repo)      |
| `bash`    | Bash-specific syntax (`[[ ]]`, arrays, bashisms)              |
| `console` | Mixed prompt + command + sample output sessions               |

Do **not** mark runnable shell with `text`.
Reserve `text` for diagrams, trees, non-executable snippets, and plain prose samples.
Use `yaml`, `json`, `python`, etc. when the fence is that language.

### Tables

Follow the guidance in the [Google style guide on tables](https://google.github.io/styleguide/docguide/style.html#tables).

Additionally, see the guidance in the [Technical writing guidelines](./technical-writing.md) on what to use instead of tables.

Almost always prefer tables with fixed plaintext columns so that they are readable in plain text.

For example prefer this style:

```markdown
| App | Python      |
|-----|-------------|
| 0.1 | 3.11 - 3.14 |
| 0.2 | 3.13 - 3.15 |
```

over:

```markdown
| App | Python |
| --- | --- |
| 0.1 | 3.11 - 3.14 |
| 0.2 | 3.13 - 3.15 |
```

Reading plaintext of tables takes higher priority than HTML presentation.

If you are writing tables for which having formatted columns makes the table too wide (more than 80-100 columns) or unreadable then it is not suitable for a Markdown table.
Rework to headings style, consider moving to a real web page,
or moving the data to a configuration file like YAML or a CSV.
