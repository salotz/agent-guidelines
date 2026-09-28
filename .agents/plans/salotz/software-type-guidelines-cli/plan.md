# Plan: software-type guidelines + CLI design (clig.dev)

## Context

Repository: **agent-guidelines** (reflective guidelines repo).

Operator found [Command Line Interface Guidelines](https://clig.dev/)
([GitHub](https://github.com/cli-guidelines/cli-guidelines); machine-readable
[`/llms.txt`](https://clig.dev/llms.txt)) and wants that advice folded into
**agent-facing** shared guidelines.

Today `content/shared/software-guidelines.md` is **process/authorship** focused
(git hygiene, codetags, comments, commits, TDD, coverage).
There is **no** place for guidelines keyed by **kind of software** being designed
(CLIs, libraries, services, etc.).

This plan adds that structure and seeds it with CLI design guidance distilled
from clig.dev (plus related further reading where useful).

---

## Goals

1. Introduce a durable home for **software-type** (product-shape) guidelines under
   `content/shared/`, separate from generic software authorship rules.
2. Add **CLI design** guidelines that agents can apply when designing or reviewing
   command-line programs.
3. Collate clig.dev into a local **summary** (same pattern as other external
   standards under `content/shared/summaries/`) and point guidelines at that
   summary + the canonical URL—not a full mirror of the site.
4. Wire hubs (`content/shared/README.md`, and any thin links from
   `software-guidelines.md` / glossary if terms warrant it) so the new material
   is discoverable.

Non-goals:

- Rewriting clig.dev verbatim into the repo.
- Language- or framework-specific CLI tutorials (Click, Cobra, clap, …) beyond
  brief “use a real parser” pointers already in clig.dev.
- Full-screen TUI / curses app design (explicitly out of scope in clig.dev).
- Implementing example CLIs in this repo.

---

## Design questions (resolve before or during drafting)

| ID | Question | Lean |
|----|----------|------|
| Q1 | **Layout:** one file `software-types.md` with per-type sections, vs a directory `software-types/cli.md` (and later siblings)? | Prefer **directory + index** if more than one type is expected soon; otherwise a single file with `## CLI` is enough to start. Default proposal: **`content/shared/software-types.md`** hub + optional split later, **or** `content/shared/software-types/README.md` + `cli.md` from day one. **Recommend:** `software-types/README.md` + `cli.md` so CLI can grow without bloating a monolith. |
| Q2 | **Relationship to `software-guidelines.md`:** merge types into that file vs sibling topic? | **Sibling.** `software-guidelines.md` stays authorship/process; software-types stays design-of-artifact. Cross-link both ways in hubs. Matches [contributing/editing.md](../../../../contributing/editing.md): new cross-cutting shared topics get their own file(s) + thin hub links. |
| Q3 | **Depth of CLI doc:** principles-only checklist vs worked agent checklist (do / don’t when generating a CLI)? | **Agent-oriented checklist** with short rationale, grouped like clig.dev (basics, help, output, errors, flags, …), each item one tight rule. Link summary for depth. |
| Q4 | **Normative voice:** “MUST/SHOULD” vs soft preference? | Match existing shared tone (imperative operator preference, not RFC 2119 unless the rest of shared adopts it). |
| Q5 | **Further reading in-repo:** only clig.dev summary, or also thin notes on 12 Factor CLI / POSIX utility conventions? | **clig.dev summary required.** Optional one-liners + URLs for POSIX, GNU Program Behavior, 12 Factor CLI, Heroku CLI style—no extra full summaries unless they pull weight later. |

---

## Proposed tree (recommended)

```text
content/shared/
├── software-guidelines.md          # unchanged role; add “see also” link
├── software-types/
│   ├── README.md                   # what “software type” means; index
│   └── cli.md                      # CLI design guidelines for agents
└── summaries/
    └── clig-dev.md                 # compacted external summary
```

Hub updates:

- `content/shared/README.md` — list Software types + CLI.
- Root `README.md` only if it enumerates shared topics the same way (keep consistent with current hub style).
- `content/shared/software-guidelines.md` — short pointer under Generic or a new “See also” at top/bottom.
- `.agents/context/repo-map.md` — only if we routinely list shared files there (today it does not enumerate them; skip unless desired).

---

## Content outline

### `software-types/README.md`

- Purpose: guidelines that apply when the **artifact** has a known shape
  (CLI, library API, HTTP service, …)—not general coding process.
- How agents should load them: when the task is designing, reviewing, or
  implementing that shape, read the matching type doc **in addition to**
  `software-guidelines.md`.
- Index table: CLI → `cli.md`; placeholders or “not yet” for future types
  (library, service, library+CLI, etc.) without empty stub files.

### `software-types/cli.md`

Structure (align with clig.dev, compress for agents):

1. **Scope** — human-first CLIs; not full-screen TUIs; language-agnostic.
2. **Philosophy (short)** — human-first; composable parts; consistency;
   say enough; discoverability; conversation; robustness; empathy;
   break rules deliberately.
3. **Hard basics**
   - Real argument parser
   - Exit 0 success / non-zero failure; map important failures
   - Primary machine/human result → stdout; diagnostics → stderr
4. **Help & docs**
   - `-h` / `--help` (and git-like `help` if subcommands); don’t overload `-h`
   - Concise help on missing args; full help on request; examples first
   - Suggest corrections (DWIM carefully); support/feedback URL
   - Web docs + terminal docs; man pages optional but `help` should work offline
5. **Output**
   - Humans first (TTY detection); `--json` / `--plain` where structure matters
   - Brief success output; tell user when state changes; easy “status”
   - Suggest next commands; explicit crossing of program boundary (net, surprise files)
   - Color with intention; honor `NO_COLOR`, non-TTY, `TERM=dumb`, `--no-color`
   - No animations when stdout isn’t a TTY; pager only when appropriate
6. **Errors**
   - Rewrite for humans; high signal; important info last; red sparingly
   - Unexpected errors: how to debug / file bugs without dumping noise by default
7. **Arguments, flags, subcommands**
   - Consistency with UNIX/git-ish norms; subcommand help; dangerous ops confirm/dry-run
8. **Interactivity & robustness**
   - Don’t hang on unexpected TTY stdin when pipe input was expected—show help
   - Signals; timeouts/progress for long work; idempotency where natural
9. **Config & environment**
   - Precedence clarity; don’t surprise with hidden phone-home
   - Env vars: documented, namespaced; no secret defaults in process listings when avoidable
10. **Naming, distribution, analytics** (brief)
    - Easy install/uninstall; prefer single binary when realistic
    - No telemetry without consent; prefer opt-in
11. **Agent checklist** — bullet “before you ship a CLI” do/don’t
12. **References** — clig.dev + local summary; optional further reading links

### `summaries/clig-dev.md`

Follow existing summary pattern (e.g. `llms-txt-standard.md`,
`google-developer-documentation-style-guide.md`):

- Source URL(s): https://clig.dev/ , https://clig.dev/llms.txt , upstream GitHub
- What it is / who for
- Philosophy bullets
- Guideline areas with the **highest-leverage rules** (not every example)
- Explicit out-of-scope (full-screen programs)
- Further reading list (names + URLs only)
- License/attribution note if needed (check upstream LICENSE when collating)

---

## Work phases

### Phase 0 — Decide layout (Q1–Q2)

- Lock directory-vs-single-file and relationship to `software-guidelines.md`.
- Record in this plan’s [decisions.md](./decisions.md) (create when answered).

### Phase 1 — Collate clig.dev

- Fetch canonical text (`llms.txt` / markdown source preferred over HTML).
- Write `content/shared/summaries/clig-dev.md`.
- Skim for anything that conflicts with existing shared advice (e.g. commit
  messages, project tooling)—CLI doc should not fork those topics.

### Phase 2 — Author software-types + CLI guidelines

- Add `content/shared/software-types/README.md` and `cli.md` (or chosen layout).
- Keep prose consistent with [markdown-style-guide.md](../../../../content/shared/markdown-style-guide.md)
  and human-writing prefs.
- Add glossary terms only if a term is reused and unstable (e.g. “software type”
  if it appears in hubs)—optional, RFC 29 format.

### Phase 3 — Hub wiring + consistency

- Update `content/shared/README.md`.
- Cross-link from `software-guidelines.md`.
- Update root README shared list if applicable.
- Link check; spelling pass per [contributing/editing.md](../../../../contributing/editing.md).

### Phase 4 — Close out

- Mark checklist done; move this plan to **Done** in [../todo.md](../todo.md)
  with date/notes.
- Do not stage/commit unless operator asks (software-guidelines git hygiene).

---

## Success criteria

- [ ] Agents designing a CLI have an obvious shared doc to load.
- [ ] clig.dev is represented as a summary + cited source, not an uncredited paste.
- [ ] Authorship guidelines and type-design guidelines are clearly separated.
- [ ] Hubs link the new pages; internal links resolve.
- [ ] Future types (library, service, …) have a defined place to land.

---

## Open follow-ons (out of this plan)

- Library API design guidelines.
- Service / HTTP API design guidelines.
- “Library + thin CLI” dual-surface patterns.
- Optional summaries: 12 Factor CLI Apps, POSIX utility conventions.
