# Summary: Google Style Guides

Source: https://google.github.io/styleguide/

## Overview
Collection of style guides for Google-originated open-source projects. Covers language-specific conventions for consistency in large codebases.

This is **not** the
[Google developer documentation style guide](https://developers.google.com/style/)
([summary](./google-developer-documentation-style-guide.md)), which covers
editorial style for technical prose.

Available guides include:
- C++ Style Guide
- Python Style Guide
- Java Style Guide
- JavaScript / TypeScript
- Go, Shell, HTML/CSS, Markdown, and others (full list on site)

Also includes google-c-style.el for Emacs.

## Usage in this project
When a particular language or format style guide is referenced, prefer the compacted version if available, or consult the specific guide linked from this page.

General principle: consistent style makes large codebases easier to understand.

## Relationship to operator comment style
Google (and other) language guides cover formatting, naming, and often
comment *mechanics*. For **what** comments should contain, **where** they
attach, file preambles vs local notes, and **codetag** discipline, also
follow [software-guidelines.md](../software-guidelines.md) (Source Comments
and Code Tags). Those operator rules win when a generic style guide is
silent or encourages noisier commentary.