# Summary: salotz RFC 6 - Codetags

Source: https://github.com/salotz/rfcs/tree/master/rfcs/salotz.006_codetags

## Purpose
Standardize "codetags" — specially formatted comment tags (e.g. `TODO`, `FIXME`) added to source code to give semantic, searchable meaning beyond freeform comments. Makes comments machine-readable for tooling.

Draws from PEP 350 and similar efforts. The format is more important than the exact tag set; the provided tags are "sane defaults".

## Format
- Tags appear in comments preceding the relevant code.
- Uppercase word, optionally followed by `:` and explanation.
- Example (Python):
  ```python
  # TODO: remove hard-coding of number of iterations
  for i in range(10):
      print(i)
  ```
- Tags should generally include context; bare `TODO` is discouraged.
- Support for multi-line tags, multiple tags per comment, and parametrization:
  ```python
  # TODO(paramA, keyB=val): explanation
  ```
- Parametrization follows Python keyword semantics.

## Categories and Tags

### Tasks (should not ship to production without tracking)
- TODO — specific change item
- FIXME — broken code needing fix
- TOREV — flag for review
- TODOC — needs documentation
- REFACT — scheduled refactoring

### Warnings (may ship; indicate quality issues)
- ALERT, HACK, WKRD (workaround), SMELL, UGLY, GOTCHA

### Resolved (document intentional non-ideal resolutions)
- REVD (reviewed), WONTFIX, CANTFIX, DONTFIX

### Temporary (remove before commit)
- DEBUG, QUEST (question while exploring)

### Growth (future improvements, not defects)
- STUB, SNIPPET, OPT (optimization), IDEA, TEST (test sketch), REQ (requirement)

### Statements (neutral)
- CREDIT, NOTE (plain comments without tags are treated as NOTE)

### Review (iterative review flow)
- TOREV/REV/REVD with asker/req parameters for review handoff.

## Usage Notes
- Prefer separate tags over long conjunctions in one comment.
- Use for machine searchability and editor highlighting (e.g. hl-todo-mode in Emacs).
- Tags imply production-readiness rules per category.

See the full RFC for detailed examples, review flow, and implementation advice (editor coloring, etc.).
