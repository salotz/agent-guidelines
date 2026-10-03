# CLI UX guidelines

Human-facing **interaction design** for command-line programs.

Distilled primarily from [Command Line Interface Guidelines](https://clig.dev/)
([summary](../summaries/clig-dev.md); [llms.txt](https://clig.dev/llms.txt)).
Language-agnostic.

For **product architecture** (layers, config/catalog, env registry, identity, resources, versioning, release), see [CLI and application patterns](./cli-applications.md).
Load **both** when building a shippable CLI.

## Scope

**In:** human-first CLIs and multi-tool (`git`-like) surfaces—help, output, errors, flags, interactivity, composability.

**Out:** full-screen TUI / immersive apps; framework tutorials (Cobra, clap, Click, …)—use a real parser; details are framework docs.

## Philosophy

1. Design for **humans first**; keep UNIX composability (pipes, exit codes, text/JSON).
2. Prefer **small clear parts** over kitchen-sink defaults.
3. Stay **consistent** with common UNIX/git norms and within your own subcommands.
4. **Say just enough**—no silent multi-minute hangs; no default debug floods.
5. Optimize for **discovery**: examples, suggestions, next steps.
6. Treat usage as a **conversation** (trial/error, multi-step, dry-run, corrections).
7. Be and *feel* **robust**; show empathy; break rules only with intention.

## Hard basics

- Use a **real argument parser**.
- Exit **0** on success; **non-zero** on failure; map important failures to distinct codes when scripts care.
- Primary machine/human **result → stdout**.
- **Messaging, logs, errors → stderr** so pipes stay clean.

## Help and documentation

- Support **`-h` and `--help`**; never overload `-h`.
- If args are required and the command is not interactive-by-default: bare invoke → **concise** help (what it does, 1–2 examples, important flags, “see `--help`”).
- Full help on request; for git-like tools also `help` / `help <subcommand>`.
- **Lead with examples**; park huge example sets in a cheat sheet or web docs.
- Put the most common commands/flags **first**.
- Include a **support/feedback URL** in top-level help.
- Link web docs from help when a deeper page exists; keep terminal help usable **offline**.
- Man pages are optional; if you ship them, also expose them via the tool (`help`).
- On likely typos, **suggest** a fix; don’t force-run dangerous reinterpretations; document accepted alternate syntax you commit to.

Repo docs follow [Diátaxis](./documentation.md).
CLI help remains authoritative for flags; see [CLI and application patterns — Documentation layout](./cli-applications.md#documentation-layout).

## Output

- **Humans first**; detect TTY per stream when choosing color/animation/pager behavior.
- Offer structured output (`--json`) and/or `--plain` when human layout would break scripting.
- Prefer salotz shared **`--output json|yaml|table`** when the product uses model-driven resources ([application patterns](./cli-applications.md#output-formats)); still honor clig expectations (JSON available, plain/table for humans).
- Keep success output **brief**; provide `-q` / `--quiet` to suppress non-essential output in scripts.
- **Announce state changes**; provide an easy **status** (or get) path for complex state.
- **Suggest next commands** in multi-step workflows.
- Actions that cross the program boundary (unexpected files, network) should usually be **explicit**.
- Color with intention; disable when not a TTY, `NO_COLOR` is set and non-empty, `TERM=dumb`, or `--no-color` (optional product-specific `APP_NO_COLOR`).
- No spinners/progress animations when stdout isn’t a TTY.
- Don’t print developer-only traces by default; no `ERR`/`WARN` log cosmetics on stderr unless verbose.
- Pagers only when appropriate and stdin/stdout are interactive; prefer robust library wrappers over naive pipes.

## Errors

- Catch expected failures; **rewrite messages for humans** (problem + actionable fix).
- Maximize **signal-to-noise**; group repeated similar errors.
- Put the most important line where eyes land (**end** of the block); use red sparingly.
- Unexpected failures: path to debug/bug report without dumping a novel by default (file log OK).

## Arguments, flags, subcommands

- Prefer **named flags** over ambiguous multi-position args (except well-known pairs like source/dest or multi-file globs).
- Every flag gets a **long** form; short forms only for common flags.
- Reuse **standard names** where they fit: `-h/--help`, `--version`, `-n/--dry-run`, `-f/--force`, `-q/--quiet`, `--json`, `-o/--output`, `-v`/`-d` for verbose carefully (avoid `-v` meaning both verbose and version).
- Make the **default** correct for most humans; don’t rely on everyone remembering a magic flag.
- Prompt only when stdin is a TTY; **never require** a prompt—always offer flags/args; honor **`--no-input`** (fail with how to pass data).
- Confirm dangerous operations; raise friction with severity; offer **dry-run** for moderate/complex mutates.
- Support **`-`** for stdin/stdout where a file path is accepted.
- Prefer order-independent flags vs subcommands when the parser allows.
- **Never put secrets on argv** (`--password`); use `--password-file` or stdin so values don’t leak via `ps`/history.
- Subcommands: consistent flag names and output; stable noun/verb patterns when the domain is object-heavy.

## Interactivity and robustness

- No interactive UI when stdin isn’t a TTY.
- Don’t hang waiting for a pipe that never comes—show help or a clear error.
- Password prompts: no echo.
- Make escape obvious; keep Ctrl-C meaningful; document escape sequences for connection wrappers.
- Long operations: progress or heartbeat; timeouts where unbounded wait is hostile.
- Prefer **idempotent** mutates when natural.

## Config, environment, and trust

- Document config/env **precedence** clearly (flags > env > file > default is common—state the product’s order).
- Namespace product env vars; register and help-surface them ([application patterns — env registry](./cli-applications.md#environment-variable-registry)).
- No surprise phone-home; **no telemetry without consent** (prefer opt-in).
- Don’t encourage secrets in environment variables when files or OS secret stores are viable.

## Naming, install, distribution

- Obvious command name; uninstall as easy as install.
- Prefer a **standalone binary** when the audience shouldn’t need a language runtime ([application patterns — release](./cli-applications.md#versioning-and-release)).
- Version via `--version` / `version` subcommand; keep product version scheme explicit (Growth Versioning vs SemVer meanings).

## Agent checklist

**Do**

1. Real parser; correct exit codes; stdout/stderr split.
2. `-h`/`--help` with examples first; concise help on missing args.
3. TTY-aware color/animation; `NO_COLOR` / `--no-color`.
4. Human errors with fixes; quiet and JSON/plain (or `--output`) paths for scripts/agents.
5. Long flags; dry-run/force/confirm matched to danger level.
6. Document env and config precedence; register env in SSOT + appinfo when applicable.
7. Tell users when state changes; provide status/get.

**Don’t**

1. Hand-roll argv or overload `-h`.
2. Print primary results on stderr or diagnostics on stdout.
3. Hang on empty TTY stdin when a pipe was expected.
4. Default to stack traces or log-level spam.
5. Require interactive prompts in scripts.
6. Pass secrets on the command line.
7. Ship stub subcommands that only say “not implemented.”
8. Surprise network or homedir writes without saying so.

## Related

- [clig.dev summary](../summaries/clig-dev.md) — compacted external source
- [CLI and application patterns](./cli-applications.md) — architecture and product spine
- [Software Documentation](./documentation.md) — Diátaxis for repo docs
- [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46) (further reading)
- [no-color.org](https://no-color.org/)
