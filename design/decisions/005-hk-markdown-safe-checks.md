# ADR 005: hk checks do not reflow Markdown

## Status

accepted

## Context

Guideline Markdown uses semantic line breaks.
Aggressive formatters or Markdown prettier steps would fight that style and create noisy diffs.

## Decision

- Initial `hk.pkl` steps are only:
  - `Builtins.trailing_whitespace`
  - `Builtins.newlines` (final newline / end-of-file fixer)
- Do **not** enable Markdown reflow, prettier-markdown, or similar in hooks without a new ADR.
- `mise run format` may apply those fixes; it must not reflow prose.

## Consequences

- `mise run check` / pre-commit stay safe for sembr documents.
- Style beyond whitespace remains a human/editor concern ([markdown style guide](../../content/shared/markdown-style-guide.md)).
