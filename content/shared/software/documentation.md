# Software Documentation

How to structure and author documentation **for software projects**.

Editorial style for non-personal technical prose lives in [Technical Writing](../technical-writing.md).
Contributor-facing docs (for example under `contributing/`) also follow [Writing Contributor Documentation](../writing-contributing.md).
This file is about **what kinds of docs to write** and how to keep them separate—not Markdown formatting or chat tone.

## Diátaxis

Use the [Diátaxis](https://diataxis.fr/) framework ([summary](../summaries/diataxis.md)) as the default map for software documentation.

Diátaxis is a systematic approach to technical documentation.
It identifies **four distinct user needs** and **four corresponding forms** of documentation, and organizes content around those needs rather than around the product’s internal structure or the author’s convenience.

The four types:

### Tutorials

**Learning-oriented.**

Take a beginner through a **guided lesson**.
The goal is confidence and a first successful experience, not coverage of every feature.

- Prefer a single happy path with working steps the reader can follow.
- Do not digress into alternatives, edge cases, or exhaustive options.
- Teach by doing; explanation belongs elsewhere (or only as much as the lesson needs).

### How-to guides

**Task-oriented.**

Show how to solve a **concrete problem** or achieve a real goal the reader already has.

- Assume the reader has basic familiarity (from tutorials or prior use).
- Steps should be goal-directed and adaptable; name the problem in the title when possible.
- Do not turn a how-to into a tutorial (over-scaffolding) or into reference (dumping every flag).

### Reference

**Information-oriented.**

Describe the system **accurately and completely** so readers can look things up (APIs, CLIs, config keys, schemas, error codes).

- Neutral, consistent structure; bare facts over narrative.
- Match the product’s structure where that aids lookup (same names, same hierarchy).
- Do not bury norms and contracts inside tutorials or prose essays.

### Explanation

**Understanding-oriented.**

Provide background, context, and discussion so readers can understand **why** the system is the way it is.

- Concepts, design rationale, trade-offs, history of decisions.
- May be discursive; still stay clear of step-by-step procedure and dry API catalogs.
- Link to tutorials, how-tos, and reference instead of duplicating them.

## How to apply

### Keep the four types distinct

Do not mix types in one page when that would blur the reader’s job.

A page that tries to teach, instruct for a task, list every option, and argue design at once is hard to use and hard to maintain.
Split by need; cross-link freely.

Small projects may keep fewer files, but still **label sections by type** (Tutorial / How-to / Reference / Explanation) so later splits are obvious.

### Prefer user need over code layout

Default navigation and IA to Diátaxis (or an equivalent need-based split), not only to internal package or module trees.

Code-shaped trees are fine **inside** reference.
They are a poor sole top-level map for people learning or completing a task.

### Choose the type from the reader’s job

When adding or revising a doc, decide which need it serves first:

- “I am new and need to learn by doing” → tutorial
- “I need to accomplish X” → how-to
- “I need the exact contract / options / shape” → reference
- “I need to understand why / how it fits together” → explanation

If a draft serves two jobs, split it or demote the secondary material to a short link.

### Do not recapitulate upstream

Same rule as contributor docs: do not restate upstream documentation at length.
Project docs should cover **this** project’s behavior, choices, and gaps.
Link out; keep local prose terse.
See [Writing Contributor Documentation](../writing-contributing.md#dont-recapitulate-upstream-documentation).

### Style and format

- Prose and structure: [Technical Writing](../technical-writing.md)
  (including [placeholders](../technical-writing.md#placeholders))
- Markdown mechanics: [Markdown style guide](../markdown-style-guide.md)
- Code in docs: comment and snippet habits aligned with [Software Guidelines — Source Comments](./guidelines.md#source-comments) and the code-snippet notes in technical writing

### Agent habits

When asked to “add docs,” “write a README,” or “document this API”:

1. Identify which Diátaxis type(s) the request actually needs (often more than one deliverable).
2. Propose or place content in the matching type; do not default everything to a single long README section.
3. For APIs, CLIs, and config, prefer **reference** that mirrors the real interface; add how-tos only for real tasks.
4. Do not invent tutorials that only restate reference tables.
5. Link existing types instead of copying.

For CLI product layout (`docs/` vs `design/` vs `contributing/`, help vs repo reference, examples vs host state), also follow [CLI and application patterns](./cli-applications.md).

## Related writing guides

- Software docs structure (this file):
  Diátaxis types and application
- Non-personal technical prose:
  [Technical Writing](../technical-writing.md)
- Contributor-oriented project docs:
  [Writing Contributor Documentation](../writing-contributing.md)
- Agent ↔ operator chat and plans:
  [Writing for humans](../writing-for-humans.md)
- Research notes (personal use):
  [Research](../research.md)

## Further reading

- [Diátaxis](https://diataxis.fr/) — canonical site
- [summary](../summaries/diataxis.md) — compacted external summary
- Historical presentation as the Divio documentation system:
  [docs.divio.com/documentation-system](https://docs.divio.com/documentation-system/)
