# Getting Started (Personal / Host Context)

Use this when you want agents on **your machines** to load personal host
guidelines automatically—shell config, Bimker, personal skills—without
pasting the same preferences into every prompt.

This is distinct from [shared getting started](../shared/getting-started.md),
which bootstraps **projects** via `AGENTS.md`. Personal context is
**host-local** (salotz RFC 23).

## Canonical host context file

Install the personal bootloader at:

```text
~/.agents/AGENTS.md
```

That path is the operator-owned source of host guidelines (salotz RFC 23
also allows `$XDG_CONFIG_HOME/agents`, typically `~/.config/agents`). Prefer
a single `AGENTS.md` entry file under `~/.agents/` unless your tooling
requires the XDG config location.

Avoid a bare `~/AGENTS.md`.

## 1. Copy the host bootloader

There is a file [agents_md_template.md](./agents_md_template.md) with
context you can drop into `~/.agents/AGENTS.md`.

```bash
mkdir -p ~/.agents
cp path/to/agent-guidelines/personal/agents_md_template.md ~/.agents/AGENTS.md
```

Edit the copy only if you need host-specific path hints (for example,
where this `agent-guidelines` repo is checked out on that machine). Keep
behavioral rules in the git-tracked `personal/` tree so they stay shared
across hosts via this repo.

Optional per-machine line:

```markdown
Guidelines checkout on this host: `~/tree/personal/devel/agent-guidelines`
```

## 2. Configure your agent system to load it

Copying `~/.agents/AGENTS.md` is not always enough. Many agent harnesses
only auto-load project-local `AGENTS.md` (or their own config paths). You
must point each harness at the host file (or at a small pointer file it
already reads).

### Pattern

1. Keep the full host bootloader at `~/.agents/AGENTS.md`.
2. For each agent product, add a **pointer** in whatever path that product
   loads on every session, telling it to read `~/.agents/AGENTS.md` and
   the guidelines checkout.

Do not fork a second full copy of the guidelines into every product
config. Prefer one host file plus thin pointers.

### goose

goose reads agent guidance from its own config tree. On this operator's
setup, put a pointer in:

```text
~/.config/goose/AGENTS.md
```

Example contents:

```markdown
# goose host context pointer

Load host-local operator guidelines from:

- `~/.agents/AGENTS.md`

That file bootstraps personal and shared guidelines from the
`agent-guidelines` repo (see checkout path noted there if present).

When working on this operator's machine, apply those host rules in
addition to any project `AGENTS.md`.
```

If goose is configured to load additional instruction files, you may
instead register `~/.agents/AGENTS.md` directly; the important part is
that sessions actually include it.

### Verify

Start a session and confirm the agent can see host rules (for example,
that Bimker/`~/.bimker` care rules apply without restating them). If not,
the harness pointer is missing or not on the product's load path.

## 3. Make the full guidelines reachable

The bootloader is a summary plus pointers. Agents still need the real
files under `personal/` and `shared/` for detail.

Options (pick what fits the host):

- Keep a git checkout of this repo somewhere stable and ensure agents can
  read it (path in `~/.agents/AGENTS.md`, or harness config).
- Rely on remote fetch/cache (e.g. personal **cacher** skill) of
  `https://github.com/salotz/agent-guidelines` when the checkout is not
  present.

## 4. What agents should do once this is loaded

When host context from step 1 is in session:

1. Apply **shared** guidelines as the portable baseline.
2. Apply **personal** guidelines for host paths, shell/Bimker, installs,
   and personal skills.
3. Read full docs under `personal/` and `shared/` as the task requires
   instead of asking the operator to restate standing preferences.

## 5. Project vs host

| Context | Location | Role |
|---|---|---|
| Shared project bootloader | Project `AGENTS.md` from [shared/agents_md_template.md](../shared/agents_md_template.md) | Portable project rules |
| Personal host bootloader | `~/.agents/AGENTS.md` from [agents_md_template.md](./agents_md_template.md) | This operator’s machines |
| Harness pointer | e.g. `~/.config/goose/AGENTS.md` | Tells a specific product to load the host bootloader |
| Optional project-local host overrides | `.agents.local.md` / `.agents.local/` (RFC 23) | Per-clone host tweaks |

You usually want **both** project rules and personal host context, plus a
pointer per agent product that does not read `~/.agents/` automatically.

## Maintaining the template

When personal guidelines change, update
[agents_md_template.md](./agents_md_template.md) so new hosts get the
right summary. Refresh `~/.agents/AGENTS.md` on each machine when the
bootloader drifts. Harness pointer files can stay stable if they only
reference `~/.agents/AGENTS.md`.
