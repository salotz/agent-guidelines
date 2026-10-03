# Summary: Technical doc placeholders

House guidance (normative for this repo):
[Technical writing: Placeholders](../technical-writing.md#placeholders).

This file is a compacted background and external-source index.
Prefer the technical-writing section for day-to-day rules.

## Intent

Standardize how technical docs mark **replaceable** names, paths, and CLI tokens
in plain text (Markdown, Org, man-like SYNOPSIS)
without a heavy templating language in the visible source.

## Layers (do not mix)

1. **Replaceable token (slot)**:
   what string means "substitute your value"
   (`<project>`, `PROJECT_ID`, …).
2. **Metasyntax**:
   optional / exclusive / repeat structure
   (`[]`, `|`, `...`, sometimes `{A|B}`).
3. **Presentation**:
   italic, HTML `<var>`, UI chips.
   Not sufficient alone in plain-text source.
4. **Binding / fill** (optional platform or tool):
   one value rewrites many examples.
   Out of band; not Jinja in every sample line.

Metasyntax is not part of the slot token.

## House default (mechanism)

- **Primary profile**: angle brackets, `<name>`.
- **Fallbacks** when the host language collides
  (XML/HTML, URI Template, env-key/sample-variable UIs, rare sigil holes):
  one profile per block; no random mixing.
- **Case and word-separation** inside the delimiters:
  not fixed by the mechanism rules
  (house style may pin later; tools may normalize keys separately).
- **SYNOPSIS vs runnable samples**:
  grammar lines may use full POSIX-like metasyntax;
  click-to-copy blocks should avoid live `[]{}|...` when practical.

## Why not `{name}` / `${name}` as default

In mixed shell and config examples those forms look like real languages
(env expansion, template engines, format strings).
Angle brackets carry a stronger documentation-slot prior for experienced readers
(POSIX alternate form, Kubernetes, Red Hat, docopt positionals, BNF nonterminals).

`UPPER_SNAKE` remains useful as a binding key and in Google-like HTML/`<var>` workflows,
and as a fallback profile when slots are env-shaped.
Alone in raw Markdown paths it is a weaker "this is fictional" signal
(`projects/PROJECT/REPLICA` can read like real segments).

## Editorial sources

- Google Developer Documentation Style Guide:
  [placeholders](https://developers.google.com/style/placeholders),
  [command-line syntax](https://developers.google.com/style/code-syntax).
  Prefer descriptive `UPPER_SNAKE` token text;
  HTML `<var>`; Markdown italic+code outside fences;
  SYNOPSIS metasyntax `[]`, `{A|B}`, `...` kept *outside* the var wrapper;
  runnable samples should not rely on metasyntax characters left in the copy.
  Sample-variable *UIs* are a doc-platform feature on the same tokens, not the style grammar.
- Microsoft Writing Style Guide:
  [developer text elements](https://learn.microsoft.com/en-us/style-guide/developer-content/formatting-developer-text-elements).
  Italic for UI placeholders; angle brackets for code placeholders when `<>` is not host syntax.
- [Kubernetes docs style](https://kubernetes.io/docs/contribute/style/style-guide/):
  explicit angle brackets; often lowercase hyphenated names.
- [Red Hat supplementary style](https://redhat-documentation.github.io/supplementary-style-guide/)
  (user-replaced values):
  default `<value_name>`; `${value_name}` when XML makes `<>` lethal.
- [man-pages(7)](https://man7.org/linux/man-pages/man7/man-pages.7.html):
  bold = literal, italic = replaceable; `[]` `|` `...` in SYNOPSIS.
  Plain-text flattenings often reintroduce `<name>` or `NAME` when italic is lost.

## Spec and tool relatives (informative)

- [POSIX.1 Utility Argument Syntax (Ch. 12)](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html):
  root of most man SYNOPSIS diagrams; alternate multi-word form uses angle brackets.
- [docopt](https://github.com/docopt/docopt):
  parseable usage dialect; `<name>` positionals by convention.
- [Click documentation](https://click.palletsprojects.com/en/stable/documentation/):
  default metavar often loud snake; can override to `<name>`; cites POSIX and man-pages.
- [RFC 6570 URI Templates](https://www.rfc-editor.org/rfc/rfc6570) and OpenAPI path params:
  `{param}` is real template syntax, not general prose placeholder markup.

## Typed holes (inspiration only)

Proof assistants and typed-hole systems (Hazel/Hazelnut, GHC `_` / `_name`,
Agda `?` / `{! !}`, Idris `?name`, Lean `_` / `sorry`, Coq `?Goal`)
show useful axes: empty vs provisional fill, named identity, owner of the hole.
They are **not** the default documentation markers.
Optional non-default profiles may borrow a sigil form; general docs stay on `<name>`.

BNF-family `<nonterminal>` is the historical plain-text home of angle-bracket slots.

## Possible later RFCs (not required to write docs)

Split if formalized so mechanism does not absorb style and tooling:

1. Delimiters and fallback profiles (mechanism).
2. Naming style inside slots (optional house fashion).
3. Command SYNOPSIS metasyntax (L2), including EBNF-vs-Google brace meaning collision.
4. Binding / extract / fill tooling (only when building a tool).

Humans can author slots with (1) alone.
"Replace the following:" lists cover binding until (4) exists.

## Agent notes

- When writing or editing technical examples under these guidelines,
  default to `<slot>` and the rules in
  [technical-writing.md](../technical-writing.md#placeholders).
- Do not invent a project-local placeholder dialect without a host-language reason.
- Do not load this whole summary unless deciding fallbacks or citing externals;
  the technical-writing section is enough for ordinary edits.
