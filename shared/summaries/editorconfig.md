# Summary: EditorConfig

Source:
https://editorconfig.org/ (format overview and examples on that site;
formal spec:
https://spec.editorconfig.org/)

## What it is
A **file format** plus editor/IDE plugins for consistent basic coding styles across tools:
indent style/size, charset, end-of-line, final newline, and related properties.
Files are plain text, VCS-friendly, and named `.editorconfig`.

## How it works (core)
- Place `.editorconfig` in the project (often at the root with `root = true`).
- When a file is opened, plugins walk from the file’s directory upward until `root = true` or the filesystem root.
- Sections are filepath globs (gitignore-like);
  later matching properties win;
  closer files override farther ones.
- Common properties:
  `indent_style`, `indent_size`, `tab_width`, `end_of_line`, `charset`, `insert_final_newline`, `trim_trailing_whitespace`.
- Many editors ship support or need a small plugin;
  not every property is implemented everywhere.

## Characteristics relevant here
- Complements (does not replace) language formatters and linters:
  EditorConfig covers editor-level whitespace and line basics shared across languages.
- Cheap remote context:
  small INI-style file at repo root.

## Agent notes
- Respect existing `.editorconfig` when editing;
  do not fight indent/newline conventions it sets.
- Prefer adding or adjusting `.editorconfig` for cross-editor defaults rather than editor-specific settings committed to the repo.
- Pair with project formatters (ruff, prettier, and so on) managed via the project’s tool stack for deeper style rules.
