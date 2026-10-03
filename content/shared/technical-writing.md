# Technical Writing Guidelines

Guidelines for technical writing meant for a non-personal audience.

This is distinguished from "research" in that research is meant for an operator's personal use rather than external communication.

Audiences can be intra-company audiences or the public.

For agent-to-operator chat, plans for review, and other plain-text-first output, see [Writing for humans](./writing-for-humans.md).

For documentation aimed at project contributors (for example under `contributing/`), see [Writing Contributor Documentation](./writing-contributing.md).

For structuring software project documentation with Diátaxis (tutorials, how-to guides, reference, explanation), see [Software Documentation](./software/documentation.md).

## Formats

The default format for technical writing is Markdown.
Other systems are acceptable when presentation needs go beyond what Markdown can provide;
those choices are project-specific.

Examples include [Markdoc](https://markdoc.dev/), [Skribilo](https://www.nongnu.org/skribilo/), and [Pollen](https://docs.racket-lang.org/pollen/).

Guidance here is generic and usually uses Markdown examples,
though other formats may appear as well.

See the format-specific style guides —
for Markdown, [Markdown style guide](./markdown-style-guide.md).

## Structure and section breaks

Do not insert superfluous thematic breaks (`---` / `***` / `___` in
Markdown) between sections in technical writing.

Headings already separate topics. Extra horizontal rules add visual noise,
travel poorly across renderers, and encourage decorative structure instead
of clear outline hierarchy.

Use a thematic break only when you need a true break in thought that is
*not* a new heading (rare in specs, guidelines, and project docs). Prefer
another heading, a short transitional sentence, or simply the next
section.

## When to use tables

Do not use tables for verbose prose even if a tabular format makes sense abstractly.

Humans have a hard time reading and editing tables with lots of content in plain-text formats like Markdown.
Even formatted tables in web pages with lots of prose can be difficult to read.

Instead, favor sub-sections with headers for complex multi-column information.

For example, instead of:

```markdown
| Rule                           | Detail                                                                         |
|--------------------------------|--------------------------------------------------------------------------------|
| Do this when that says to do it | We want to do this because it is the right thing to do and my human said so. |
| Another rule to follow         | This is an additional rule we have to follow                                   |
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

Follow the same guidelines for writing [software](./software/guidelines.md).
However, by the nature of code snippets they will differ in some ways (for example modularity).
Those differences will be documented specifically in time.

For **what kinds** of software docs to write and how to separate them, see [Software Documentation](./software/documentation.md) (Diátaxis).

### Comments

For comments in snippets, follow the [source comment](./software/guidelines.md#source-comments) style used in real code—comments on their own lines:

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

## Placeholders

Mark **replaceable** names, paths, and CLI tokens so readers (and agents)
can tell documentation slots from literal text.

Prefer forms that stay readable in **raw** plain text
(Markdown, Org, man-like SYNOPSIS),
not only after HTML export or rich typesetting.

This section is the house default for docs written under these guidelines.
It is not a template language.
It does not require fill tooling in the source.

Compacted external sources and the decision background:
[Technical doc placeholders (summary)](./summaries/technical-doc-placeholders.md).

### Keep four concerns separate

Do not collapse these into one ad-hoc markup choice:

1. **Replaceable token (slot)**:
   the name that means "put your value here"
   (for example `<project>`).
2. **Metasyntax**:
   optional / repeated / exclusive *structure*
   (`[]`, `|`, `...`, and in some CLI guides `{A|B}`).
3. **Presentation**:
   italic, HTML `<var>`, color chips in a doc platform.
   Presentation must not be the *only* signal in plain-text source.
4. **Binding / fill** (optional):
   one filled value rewriting many examples
   (sample-variable UIs, envsubst-like tools, front matter maps).
   Keep this out of band.
   Do not sprinkle Jinja or similar through visible example lines.

Metasyntax characters are **not** part of the slot token.
Do not put `[]`, `{}`, `|`, or `...` inside the placeholder name wrapper.

### Primary profile: angle brackets

Default for general technical prose, path templates, and CLI samples:

```text
projects/<project>/<replica>
tool <arg1> <arg2>
```

Why angle brackets:

- Familiar from POSIX alternate parameter form, man/docopt usage lines,
  Kubernetes and Red Hat docs, Microsoft code placeholders,
  and BNF-style nonterminals.
- Strong "documentation slot" prior in monospace.
  Forms like `{name}` and `${name}` more often look like real shell,
  format strings, or template engines.
- Visible without italics or HTML `<var>`.

Spelling *inside* the brackets (case, snake vs kebab vs loud snake)
is **not** prescribed here.
Different hosts and products disagree; delimiter mechanism stays stable.

Inside one document or example set:

- Prefer descriptive names over bare `x` / `foo` / `xxx`
  (unless the domain standard is a bare form, for example HTTP `2xx`).
- Stay consistent.
- Explain tokens on first use.
  For several tokens, use a short list under "Replace the following:".

### Fallback profiles

When `<>` is a bad host citizen, switch the **whole** example set
to one fallback profile.
Do not mix marker families inside one block or one tightly coupled example group.

- **XML / HTML host**:
  use a non-tag-like form such as `${value_name}` (Red Hat rule),
  never bare `<value>` inside markup samples where it reads as an element.
- **URI / OpenAPI host**:
  use `{param}` when the example *is* a URI Template / OpenAPI path
  (RFC 6570 level 1).
  Say that the line is a URI template.
- **Env-key / sample-variable host**:
  `UPPER_SNAKE` (for example `PROJECT_ID`) when slots are credential-shaped
  env values or the doc targets Google-style sample-variable UIs.
  Still explain on first use in plain text.
  In fenced blocks, loud snake alone is often the only available signal.
- **Sigil named hole** (non-default):
  `?name` or `_name` only when a document deliberately wants typed-hole flavor
  and unpaired markers.
  Not for general audience docs.
- **Provisional / non-empty hole** (rare):
  prefer a realistic example value without hole marks,
  or a pure `<slot>`.
  Paired draft forms such as `{! draft !}` are inspiration from proof assistants,
  not the default doc profile.

Optional short legend only when the profile is non-default
or the audience may not know the convention.

### Paths vs command SYNOPSIS

The same slot style can serve both, but metasyntax differs:

- **Paths**:
  usually only replaceable segments:
  `projects/<project>/<replica>`
- **CLI SYNOPSIS / grammar lines**:
  POSIX-shaped diagram marks are allowed:
  `tool <project> <branch> [flags...]`

Dominant metasyntax (POSIX Ch. 12 family; also man, docopt, Click):

- `[x]`: optional (reader must not type the brackets)
- `x|y` or `{x|y}`: mutually exclusive
  (editorial guides differ on brace-vs-bare pipe; pick one style per doc set)
- `x...` or `[x...]`: repeated
- `(x)`: grouping (common in docopt; less universal in classic man SYNOPSIS)

**Runnable copy-paste samples** are a different genre from **grammar / SYNOPSIS** lines.

- Grammar lines may use full metasyntax.
- Click-to-copy / "run this" blocks should avoid live `[]{}|...` litter when practical
  (those characters break the command if left in).
  Use a concrete sample, strip optionals, or warn explicitly.

Do not assume path-template practice alone documents CLI synopsis rules.

### Quoting placeholders in shell samples

Prefer making the slot obvious without implying shell expansion:

```sh
cp source '/path/to/<project>/file'
tool --name '<display-name>'
```

Unquoted `<name>` is fine in SYNOPSIS-style lines that are not claimed to be runnable as written.

Do not write `${name}` in general samples unless the host profile is shell-env or the XML/HTML fallback above.

### Binder keys vs display form

Display can stay `<project>` while an out-of-band binder uses a stable key
such as `project` or `PROJECT_ID`.
Normalization and extract/fill tool policy belong to tooling,
not to every example line.

Until a project actually builds fill tooling,
"Replace the following:" prose lists are enough.

### What not to do by default

- Do not use `{name}` or `${name}` as the general-prose or path default.
- Do not rely on italics-only (or bold-only) as the sole replaceability signal
  in Markdown/Org source; font is lost in too many plain-text paths.
- Do not mix angle brackets, braces, loud snake, and shell `$` markers
  in one example block without a stated profile switch.
- Do not put a template engine in the visible source of examples
  just to support later fill-in.
- Do not treat real env vars and doc slots as the same thing
  without saying which is which
  (`$HOME` may be literal shell; `<project>` is a slot).

### Minimal examples

Path template:

```text
$XDG_CONFIG_HOME/<app>/config.toml
```

SYNOPSIS-style (grammar):

```text
yerk project get <name> [--output json|yaml|table]
```

Runnable-leaning sample (no optional-metasyntax litter):

```sh
yerk project get widgets --output json
```

Replace list when several slots appear:

```markdown
Replace the following:

- `<name>`: project name in the catalog
- `<app>`: application id used in XDG paths
```
