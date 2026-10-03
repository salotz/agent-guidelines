# Summary: Command Line Interface Guidelines (clig.dev)

Source:
- https://clig.dev/
- Machine-readable: https://clig.dev/llms.txt
- Upstream: https://github.com/cli-guidelines/cli-guidelines

License:
[CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/) (attribute when quoting substantially).

## What it is

Open guide for designing **human-first** command-line programs.
Updates classic UNIX practice for modern multi-tool CLIs (`git`-like surfaces).
Language- and framework-agnostic.

**Out of scope:** full-screen TUI programs (emacs, vim, immersive apps).

## Philosophy

1. **Human-first** — people first; machine composability still matters.
2. **Simple parts that work together** — stdout/stderr, exit codes, signals, plain text and JSON.
3. **Consistency** across programs and within a multi-command tool.
4. **Say just enough** — neither silent hangs nor debug floods by default.
5. **Discoverability** — help, examples, suggestions, next-step hints.
6. **Conversation** — multi-step workflows, dry-runs, corrections, intermediate state.
7. **Robustness** (be and *feel* solid) and **empathy**.
8. **Chaos** — break conventions only with intention when they harm usability.

## Highest-leverage rules

### Basics

- Real argument parser (Cobra, clap, Click, …)—not hand-rolled `argv`.
- Exit **0** on success, **non-zero** on failure; map important failure modes.
- Primary result → **stdout**; diagnostics/logs/errors → **stderr**.

### Help

- `-h` / `--help` show help; do **not** overload `-h`.
- Missing required args → **concise** help (description, 1–2 examples, common flags, pointer to full help)—unless interactive by default.
- Full help on request; git-like tools also support `help` / `help <sub>`.
- **Lead with examples**; exhaustive sets live elsewhere if huge.
- Common flags/commands first; support URL in top-level help.
- Suggest corrections on typos (DWIM carefully; don’t silently rewrite dangerous input).
- If stdin is a TTY and the command expected a pipe, show help or a clear error—don’t hang like bare `cat`.

### Output

- Humans first; detect TTY. Offer `--json` and/or `--plain` when structure or scripting matters.
- Brief success output; `-q` / `--quiet` when scripts need silence.
- **Tell the user when state changes**; easy **status** for complex state.
- Suggest next commands in multi-step workflows.
- Crossing program boundaries (unexpected files, network) should usually be **explicit**.
- Color with intention; disable when not a TTY, `NO_COLOR` set, `TERM=dumb`, or `--no-color`.
- No animations when stdout isn’t a TTY.
- Don’t dump developer-only noise by default; avoid log-level spam on stderr unless verbose.

### Errors

- Catch expected failures and **rewrite for humans** (what happened + how to fix).
- High signal; important info last; red sparingly.
- Unexpected errors: debug path + how to file a bug without drowning the default UI.

### Args, flags, subcommands

- Prefer **flags** to ambiguous multi-args; long names for every flag; short flags only for common ones.
- Prefer standard names (`-h/--help`, `--version`, `-n/--dry-run`, `-f/--force`, `-q/--quiet`, `--json`, …).
- Default should be right for most users.
- Never require a prompt; always allow flags/args; skip prompts when stdin isn’t a TTY; honor `--no-input`.
- Confirm dangerous ops; escalate friction with severity; offer dry-run.
- Support `-` for stdin/stdout file slots where natural.
- **Do not** pass secrets on argv (`--password`); prefer `--password-file` or stdin.
- Subcommands: consistent verbs/flag names; noun/verb nesting when the domain is object-heavy.

### Robustness / distribution

- Responsive Ctrl-C; clear escape for wrappers that trap signals.
- Timeouts/progress for long work; idempotency where natural.
- Easy install/uninstall; single binary when realistic.
- No telemetry without consent (prefer opt-in).

## Further reading (names only)

- 12 Factor CLI Apps (jdx)
- GNU Coding Standards — Command-Line Interfaces
- POSIX utility conventions
- no-color.org
- Google / NN/g error-message guidance (linked from clig)

## Use with this repo

Agent checklist and salotz product-shape rules:
[CLI UX guidelines](../software/cli-ux.md) and
[CLI and application patterns](../software/cli-applications.md).
