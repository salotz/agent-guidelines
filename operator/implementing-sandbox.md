# Implementing a sandboxed agent CLI

This is the **how** companion to [Sandboxing agents](./sandboxing.md).
Follow it when you want the same control shape on your own host, whether or not you use bimker.

The worked example uses **goose** and paths from a bimker-style host bunker.
Rename product strings for other agents.

## Table of contents

1. [What you will build](#what-you-will-build)
2. [Prerequisites](#prerequisites)
3. [Reference map (worked example)](#reference-map-worked-example)
4. [Step A — Install the sandbox runner](#step-a--install-the-sandbox-runner)
5. [Step B — Install the real CLI off PATH](#step-b--install-the-real-cli-off-path)
6. [Step C — Write the sandboxed wrapper](#step-c--write-the-sandboxed-wrapper)
7. [Step D — Write the unsandboxed warning wrapper](#step-d--write-the-unsandboxed-warning-wrapper)
8. [Step E — Link PATH entrypoints](#step-e--link-path-entrypoints)
9. [Step F — Shell integration](#step-f--shell-integration)
10. [Step G — Verify](#step-g--verify)
11. [Step H — Evolve policy safely](#step-h--evolve-policy-safely)
12. [Step I — Record the decision](#step-i--record-the-decision)
13. [Desktop / GUI packages](#desktop--gui-packages)
14. [Adapting to other agents](#adapting-to-other-agents)
15. [Failure modes](#failure-modes)
16. [Minimal policy starter](#minimal-policy-starter)
17. [Maintenance](#maintenance)

## What you will build

| Piece                         | Role                                            |
|-------------------------------|-------------------------------------------------|
| Sandbox runner on `PATH`      | e.g. `landrun`                                  |
| Real agent executable under opt | e.g. `~/.local/opt/goose/bin/goose`           |
| Sandbox wrapper source        | owned by your host config repo                  |
| Warning wrapper source        | same                                            |
| PATH symlinks                 | `goose`, `goose-sandboxed`, `goose-unsandboxed` |
| Optional installers           | idempotent scripts to download + link           |

End state:
typing `goose` always enters Landlock policy;
typing `goose-unsandboxed` is an explicit, noisy exception.

## Prerequisites

- Linux with Landlock support recent enough for your landrun version (confirm with landrun docs;
  use `--best-effort` only if you accept degraded enforcement on older ABIs)
- Write access to your [host home](../shared/glossary.md#host-home)
- A place for host config source (git-tracked bunker recommended)
- For landrun via Go:
  a Go toolchain on `PATH` **at install time**
- For goose:
  ability to fetch upstream release archives

Optional but recommended:

- [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](../shared/summaries/salotz-rfc-024-extended-xdg-base-directory.md)) opt layout
- [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md)) ADRs for durable decisions

## Reference map (worked example)

On a bimker-managed host the durable pieces look like:

```text
Host config repo (e.g. ~/.bimker)
├── lib/installers/landrun.sh
├── lib/installers/goose-cli.sh
├── lib/wrappers/goose.sh              # single source of truth for MOUNTS + landrun argv
├── lib/wrappers/goose-unsandboxed.sh
└── design/
    ├── architecture/goose-cli-sandbox.md
    └── decisions/
        ├── 0001_goose-cli-landrun-sandbox-default.md
        └── 0002_goose-desktop-gui-sandboxing-deferred.md

Host runtime (not necessarily all in git)
├── ~/.local/opt/goose/bin/goose       # real executable
├── ~/.local/bin/landrun               # link to go install bin
├── ~/.local/bin/goose                 # -> goose.sh
├── ~/.local/bin/goose-sandboxed       # -> goose.sh
└── ~/.local/bin/goose-unsandboxed     # -> goose-unsandboxed.sh
```

If you do not use bimker, keep the **same roles**:
installers,
wrappers as source of truth, opt binary, PATH links, design ADRs.

## Step A — Install the sandbox runner

1. Install [landrun](https://github.com/zouuup/landrun) (or pin a known-good version).
2. Put `landrun` on `PATH` via `~/.local/bin`, not by adding `~/go/bin` to global PATH unless you already manage Go that way.
3. Confirm:
   `landrun --version` (or the project’s equivalent).

Bimker-style installer responsibilities:

- `go install …/landrun@…`
- `ln -sfn` from GOPATH/GOBIN into `~/.local/bin/landrun`

## Step B — Install the real CLI off PATH

1. Create opt prefix:

   ```sh
   mkdir -p "${XDGX_OPT_HOME:-$HOME/.local/opt}/goose/bin"
   ```

2. Download or migrate the upstream executable to:

   ```text
   ${XDGX_OPT_HOME:-$HOME/.local/opt}/goose/bin/goose
   ```

3. **Do not** leave a raw executable at `~/.local/bin/goose`.
   If an upstream installer already put one there:

   - Move it into opt
   - Replace `~/.local/bin/goose` with your wrapper link (Step E)

4. Make the installer **idempotent**:
   `SKIP_DOWNLOAD=1` (or similar) should only refresh links.

5. Prefer not adding `~/.local/opt/goose/bin` to `PATH`.

## Step C — Write the sandboxed wrapper

Create a bash wrapper (landrun’s flag style is easiest from bash arrays).
Store it in git;
install via symlink.

### Required behaviors

1. Resolve `GOOSE_REAL` defaulting to the opt path;
   optional legacy fallback during migrations.
2. Fail clearly if the real binary or landrun is missing (point at installers).
3. If `GOOSE_UNSANDBOXED=1`, **exec the unsandboxed wrapper**, never the raw executable.
4. Declare `MOUNTS=( "mode:path[,path…]" … )` once.
5. Build landrun argv from that list + `LANDRUN_OPTS` + `PASS_ENVS`.
6. Print a banner on **stderr** listing binary,
   network posture,
   PATH,
   each mount,
   and bypass instructions —
   unless quiet or plumbing subcommand.
7. `exec` landrun with a reduced `PATH`, then the real binary and `"$@"`.

### Suggested landrun options (starting point)

- `--best-effort` —
  only if you accept weaker hosts;
  document the risk
- `--ignore-missing` —
  so optional paths do not break startup
- `--add-exec` / `--ldd` —
  help the dynamic loader see needed libs
- `--unrestricted-network` —
  until you intentionally restrict network

### Plumbing quiet rules

Suppress the banner when:

- First argument is `completion` or `term` (or your agent’s equivalent)
- `GOOSE_SANDBOX_QUIET=1`

### Single source of truth

Do **not** maintain a separate human string list of mounts.
Loop `MOUNTS` for both flags and banner lines.

See [Minimal policy starter](#minimal-policy-starter) for a first `MOUNTS` array.

## Step D — Write the unsandboxed warning wrapper

Separate script on PATH only as `goose-unsandboxed` (name should not be the default).

### Required behaviors

1. Resolve the same `GOOSE_REAL` as the sandbox wrapper.
2. Print a **loud** stderr warning (full host FS as user;
   no landrun).
3. Quiet only for plumbing subcommands or `GOOSE_UNSANDBOXED_QUIET=1`.
4. `exec` the real binary with `"$@"`.

Never symlink the raw executable to `goose-unsandboxed` without the warning layer if you want the social/technical signal to hold.

## Step E — Link PATH entrypoints

From your installer (or a one-shot setup):

```sh
BIN_HOME="${BIN_HOME:-$HOME/.local/bin}"
# WRAPPER_* paths point at your git-tracked wrapper sources
ln -sfn "$WRAPPER_SANDBOXED"   "$BIN_HOME/goose"
ln -sfn "$WRAPPER_SANDBOXED"   "$BIN_HOME/goose-sandboxed"
ln -sfn "$WRAPPER_UNSANDBOXED" "$BIN_HOME/goose-unsandboxed"
```

Ensure `~/.local/bin` is early enough on interactive `PATH` to beat system-wide goose packages if any exist.

## Step F — Shell integration

- Keep calling `goose completion …` / `goose term init` via the PATH name.
- No separate `landgoose` function once the default name is sandboxed.
- If a shell module hardcodes an absolute path to the executable, change it to `goose` or the wrapper path.

## Step G — Verify

Run on each host class you care about:

```sh
command -v goose goose-sandboxed goose-unsandboxed landrun
ls -la "${XDGX_OPT_HOME:-$HOME/.local/opt}/goose/bin/goose"
file "$(command -v goose)"   # should be symlink/script, not the only executable story
goose --version              # stderr banner + version on stdout/normal flow
goose-sandboxed --version    # same sandbox
goose-unsandboxed --version  # loud warning + version
```

Behavioral checks:

1. From a directory **outside** allowed rw roots,
   try to write a file via the agent or a landrun-wrapped `touch` equivalent —
   expect failure.
2. From `~/tree` (or your rw root), expect success.
3. Confirm `goose completion bash` (or similar) produces **no** banner on stdout when evaluated.
4. Confirm `GOOSE_UNSANDBOXED=1 goose --version` still shows the unsandboxed warning (unless quiet).

## Step H — Evolve policy safely

When a workflow fails with EACCES / landrun denial:

1. Identify the path actually needed.
2. Add the **narrowest** mount (prefer specific project roots over `$HOME`).
3. Edit only the `MOUNTS` single source of truth.
4. Re-run and confirm the banner shows the new path.
5. Note *why* in a short comment above that mount entry or in the host ADR follow-up section.

Avoid:

- Granting all of `$HOME` rw “to make it work”
- Copy-pasting mount paths into a second documentation list that can rot
- Turning on quiet flags globally in your shell rc

Optional later:
named profiles (strict vs serve) selected by env or symlink, still sharing one mount vocabulary.

## Step I — Record the decision

In the host config repo (RFC 22):

1. **ADR** —
   sandboxed by default;
   opt layout;
   loud bypass;
   landrun dependency.
2. **ADR** (if applicable) —
   GUI/Desktop explicitly deferred or rejected, with alternatives considered.
3. **Architecture note** —
   as-built path diagram and verify commands.
4. Operator how-to may live under that repo’s `docs/`;
   keep **why** in ADRs.

These agent-guidelines operator docs stay portable;
host ADRs stay instance- specific.

## Desktop / GUI packages

If a Desktop build:

- Embeds its own CLI under `/usr/lib/…`
- Ignores `PATH` / rejects `GOOSE_BINARY` when packaged

…then PATH wrappers **do not** protect GUI sessions.

Operator options (pick deliberately;
record in an ADR):

1. **Do not use the GUI** —
   CLI only (simplest;
   matches reference decision).
2. Uninstall the GUI package.
3. External backend + UI config (you own `serve` lifecycle and secrets;
   watch version skew).
4. Package diversion of bundled CLI → wrapper (rootful, upgrade-fragile).
5. Landrun around the whole GUI (usually poor ROI).

Do not half-implement (1)–(5) without documenting the security story.
False confidence is worse than an honest “GUI unsandboxed.”

## Adapting to other agents

For each agent binary `NAME`:

1. Opt install:
   `~/.local/opt/NAME/bin/NAME` (or vendor layout under opt).
2. Wrappers:
   `NAME`, `NAME-sandboxed`, `NAME-unsandboxed`.
3. Product-specific XDG dirs in `MOUNTS`.
4. Quiet subcommands appropriate to that CLI’s shell plumbing.
5. Separate ADR if trust boundaries differ (e.g. cloud-only vs local tools).

Shared landrun install can serve multiple wrappers.
Do **not** assume one mount profile fits every product.

## Failure modes

### “goose: landrun not found”

Install landrun;
ensure `~/.local/bin` is on PATH for non-interactive contexts too if editors launch the agent.

### “Permission denied” on a project path

Path not in `MOUNTS`, or parent path only mounted ro.
Widen carefully.

### Banner breaks `eval "$(goose completion …)"`

Banner leaked to stdout or completion not quieted. Fix quiet rules;
keep banners on stderr only.

### Still running unsandboxed executable

- Stale absolute path in scripts/editors
- Opt dir accidentally on PATH
- Distro package earlier on PATH than `~/.local/bin`
- GUI bundled binary

Debug with `type -a goose`, `readlink -f "$(command -v goose)"`, and process trees from the GUI.

### Kernel / landrun mismatch

`--best-effort` may silently weaken policy.
Test denials on that host;
do not assume laptop policy equals CI container policy.

## Minimal policy starter

Tighten or widen for your layout.
This matches the *spirit* of the reference implementation, not every path on every machine:

```bash
MOUNTS=(
  "rox:/usr,/bin,/lib,/lib64"
  "ro:/etc,/proc,/var/lib/ca-certificates"
  "rw:/tmp,/var/tmp,/dev"
  # product XDG (adjust names)
  "rw:${CONFIG_HOME}/goose"
  "rw:${DATA_HOME}/goose"
  "rw:${STATE_HOME}/goose"
  "rw:${CACHE_HOME}/goose"
  # primary work roots only
  "rw:${HOME}/tree"
)
```

Add host config, editor, or media directories only when you have a concrete agent workflow that needs them.

Env allowlist starter:

```bash
PASS_ENVS=(
  HOME PATH TERM USER LANG
  XDG_CONFIG_HOME XDG_DATA_HOME XDG_STATE_HOME XDG_CACHE_HOME
)
```

Reduced PATH starter:

```bash
SANDBOX_PATH="/usr/bin:/bin:/usr/sbin:/sbin:${HOME}/.local/bin"
```

## Maintenance

- Upgrade goose:
  replace opt executable;
  keep wrappers.
- Upgrade landrun:
  reinstall runner;
  re-verify denials.
- Policy changes:
  edit `MOUNTS` single source of truth;
  commit in host config repo.
- Re-read [sandboxing.md](./sandboxing.md) when changing goals (e.g. adding network restriction).
- Refresh architecture notes when paths change;
  add ADRs when decisions change.

## See also

- [Sandboxing agents](./sandboxing.md) —
  principles and threat model
- [Operator hub](./README.md)
- [Shell and Bimker](../personal/shell-and-bimker.md) —
  where this author’s host config lives
- [Host layout](../personal/host-layout.md) —
  `~/tree` domains
