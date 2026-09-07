# Sandboxing agents

Guidance for operators who want agent tooling (goose and similar) to run
with **filesystem isolation by default**, without chasing every caller that
resolves the agent binary by name.

This document is the **why** and the **policy shape**. For install steps and
wrapper design, see [Implementing a sandboxed agent CLI](./implementing-sandbox.md).

## Table of contents

1. [Problem](#problem)
2. [Goals and non-goals](#goals-and-non-goals)
3. [Threat model (practical)](#threat-model-practical)
4. [Design principles](#design-principles)
5. [Default topology](#default-topology)
6. [Filesystem policy posture](#filesystem-policy-posture)
7. [Escape hatches](#escape-hatches)
8. [Shell and editor integration](#shell-and-editor-integration)
9. [GUI and secondary packages](#gui-and-secondary-packages)
10. [What sandboxing does *not* give you](#what-sandboxing-does-not-give-you)
11. [Operator checklist](#operator-checklist)
12. [Glossary touchpoints](#glossary-touchpoints)
13. [Further reading](#further-reading)

## Problem

Modern agent CLIs can:

- Execute arbitrary tools and shell commands as **your user**
- Read and write large parts of the filesystem
- Reach the network (model APIs, package registries, your intranet)

If the upstream installer drops a bare executable named `goose` (or similar)
onto `PATH`, every integration that “just runs `goose`” inherits **full user
privilege** with no extra friction. That includes:

- Interactive terminals
- Shell startup hooks (`completion`, `term init`, prompt helpers)
- Editors and IDE tasks that resolve the binary by name
- Scripts and CI helpers that assume the default command name

Ad-hoc `bash` functions fail this test: anything that
does not go through the alias remains unsandboxed, and muscle memory favors
the short name.

## Goals and non-goals

### Goals

- **Default safe**: the command name operators type daily is sandboxed.
- **Hard to do by accident**: the real upstream binary is **not** on `PATH`
  under the default name.
- **Easy to deliberate**: full-privilege runs remain possible, but **loud**
  (distinct name and/or banner).
- **Caller-agnostic**: `PATH` resolution is enough; you should not need to
  re-teach every editor, completion script, and alias.
- **Single policy source**: mount lists (and related flags) are declared once
  and drive both enforcement and human-visible banners.
- **Layout hygiene**: ad-hoc user software lives under extended XDG **opt**
  ([salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory)
  ([summary](../shared/summaries/salotz-rfc-024-extended-xdg-base-directory.md)));
  only wrappers/links live on `~/.local/bin`. **opt** itself is **not** on `PATH`.

### Non-goals

- Replacing application-level permission systems inside the agent product
- Perfect isolation equivalent to a VM or multi-user server hardening
- Sandboxing every GUI Electron shell that vendors its own CLI (see
  [GUI and secondary packages](#gui-and-secondary-packages))
- Network micro-segmentation (many setups still allow unrestricted network
  inside the sandbox so model APIs work; tighten later if you need it)
- Making agents “safe” against a malicious operator — this is **operator
  self-protection** and blast-radius reduction for agent mistakes and prompt
  injection side effects

## Threat model (practical)

Assume:

- You trust the **operator** (yourself) more than the **agent session**.
- Prompt injection, tool overreach, and buggy agent plans can cause unwanted
  reads/writes under your UID.
- The host still has secrets in `$HOME` (SSH keys, password stores, browser
  profiles, cloud creds, mail, etc.) that a coding agent rarely needs.

Sandboxing aims to make the **common path** unable to touch those secrets or
system-critical trees unless you deliberately widen policy or unsandbox.

It does **not** assume:

- A hostile local user with the same UID
- A compromised kernel
- That Landlock / landrun / seccomp setups are identical on every kernel

Treat enforcement as **best-effort on the kernels you actually run**. Verify
on each host class (laptop, remote workstation, container).

## Design principles

### 1. Sandbox the name, not the ritual

Operators and tools will keep typing `goose` (or your agent’s default name).
Put the sandbox on **that name**. Do not rely on “remember to prefix with
landrun.”

### 2. Real binary off PATH

Store the upstream executable under something like:

```text
${XDGX_OPT_HOME:-~/.local/opt}/<product>/bin/<binary>
```

Never link that executable onto `PATH` as the default command. PATH entries
should be **wrappers** (or symlinks to wrappers) you control.

### 3. Explicit entrypoints

| Entrypoint                           | Behavior                            |
|--------------------------------------|-------------------------------------|
| Default name (`goose`)               | Sandboxed                           |
| Alias of default (`goose-sandboxed`) | Same sandbox (explicit intent)      |
| Bypass name (`goose-unsandboxed`)    | Full host access + **loud warning** |

Optional env bypass (`GOOSE_UNSANDBOXED=1`) should still route through the
**warning** wrapper so quiet env-only skips cannot silence the social signal
unless a separate quiet flag is set for deliberate scripting.

### 4. One list for policy and honesty

Declare mounts once (`MODE:path[,path…]`). Generate:

- landrun (or equivalent) flags
- The startup banner operators see

If the banner and the flags can drift, operators will trust a lie.

### 5. Stderr for humans, stdout for machines

Banners and warnings go to **stderr**. Subcommands used in `eval` contexts
(`completion`, `term`, and similar) must stay quiet on stdout and should
suppress banners so shell startup does not break.

Provide quiet env vars for scripting (`*_QUIET=1`) without making quiet the
interactive default.

### 6. Prefer kernel-backed FS limits you can reason about

Landlock (via helpers such as
[landrun](https://github.com/zouuup/landrun)) is a good fit for “this process
tree may only see these paths.” Containers and VMs are stronger but heavier
for interactive coding agents. Pick the lightest control that matches your
risk; this guide documents the **PATH + Landlock** pattern used in practice
for goose CLI.

### 7. Supported surface is deliberate

If you only secure the CLI PATH entry, **say so**. Do not imply that a
distro GUI package, browser extension, or second binary tree is covered.
Document secondary surfaces as out of scope or remove them.

## Default topology

Conceptual layout (names illustrative):

```text
~/.local/opt/goose/bin/goose          # real executable — NOT on PATH as "goose"
        │
        ├─────────────────────────────┐
        ▼                             ▼
 wrappers/goose.sh              wrappers/goose-unsandboxed.sh
   landrun + policy               loud stderr warning
        │                             │
        ▼                             ▼
 ~/.local/bin/goose                 ~/.local/bin/goose-unsandboxed
 ~/.local/bin/goose-sandboxed
 ~/.local/bin/landrun               # sandbox runner on PATH
```

Properties:

- Default `command -v goose` → sandbox wrapper
- Upgrades replace the executable under opt without clobbering the PATH name
- Host config repo (e.g. bimker) owns wrapper **source**; `~/.local/bin`
  holds symlinks

## Filesystem policy posture

Start **narrow**, widen when real workflows fail — not the reverse.

### Typical starter classes

**Execute + read system tools (rox)**

- `/usr`, `/bin`, `/lib`, `/lib64` (adjust for your distro merge layout)

**Read-only system state (ro)**

- `/etc`, `/proc` (as required), CA trust stores

**Scratch and devices (rw)**

- `/tmp`, `/var/tmp`, `/dev` (as required by the runtime)

**Agent product state (rw)**

- XDG config/data/state/cache directories for that product
  (e.g. `~/.config/goose`, `~/.local/share/goose`, …)

**Operator work trees (rw)**

- Primary content roots only — e.g. `~/tree`, maybe `~/local`, `~/scratch`
- Common drop folders if you truly need them (`Downloads`, etc.) — each is a
  conscious expansion of blast radius

**Dotfiles and host config (rw, carefully)**

- Only if agents are expected to edit them (shell bunker, editor config).
  Prefer not mounting secret-bearing trees (`.ssh`, password stores, browsers)
  unless a dedicated workflow requires it.

### Network

Many interactive agent setups leave network **unrestricted** so providers and
package tools work. That is a conscious tradeoff: FS blast radius shrinks;
exfiltration and supply-chain fetches remain possible. Tighten later with
proxy allowlists or landrun network options if your threat model demands it.

By restricting the filesystem we drastically reduce the number of very
sensitive secrets that could be exfiltrated. Thus the risk of an unrestricted
network is much reduced.

### PATH inside the sandbox

Pass a **reduced** `PATH` (system bins + `~/.local/bin`) so nested tools the
agent spawns do not accidentally pick up surprise wrappers from random
project dirs — unless you intentionally want that.

### Env allowlist

Pass only what the agent needs (`HOME`, `USER`, `TERM`, `LANG`, XDG_*). Avoid
dumping the entire operator environment (cloud keys in env, `SSH_AUTH_SOCK`
if not required, etc.) unless a workflow needs them — and document those
exceptions.

## Escape hatches

| Mechanism | Intent |
|-----------|--------|
| `goose-unsandboxed …` | Explicit full access; prints warning |
| `GOOSE_UNSANDBOXED=1 goose …` | Same, but still via warning wrapper |
| `GOOSE_UNSANDBOXED_QUIET=1` | Scripting; suppresses warning (use sparingly) |
| Widen `MOUNTS` | Keep sandbox; grant specific paths |
| Profiles (optional later) | e.g. strict agent vs looser “serve” |

**Prefer widening mounts over unsandboxing** when the failure is “needed file
outside the jail.” Unsandbox when the failure is “needs ambient host
identity” (odd devices, full keyring, package manager that must see all of
`/`, etc.) and you accept the risk for that session.

## Shell and editor integration

- Point completion and terminal integration at the **PATH** name (`goose`),
  not the opt executable.
- Suppress sandbox banners for plumbing subcommands so `eval "$(goose …)"`
  stays valid.
- Editors that hardcode an absolute path to the executable bypass your design —
  configure them to call the PATH command or the wrapper path explicitly.

## GUI and secondary packages

Distro or vendor **Desktop** apps often:

- Bundle a **second** CLI under `/usr/lib/…`
- Ignore `PATH` and reject override env vars when packaged
- Spawn `serve` with full user rights

Forcing those through your sandbox usually means RPM diversions, external
backend lifecycles, or wrapping all of Electron — high maintenance relative
to benefit if you do not need the GUI.

**Operator stance that scales:**

1. Treat the **sandboxed CLI** as the supported agent surface.
2. Leave GUI sandboxing **out of scope** until a new explicit decision.
3. Optionally uninstall the GUI package to reduce confusion and attack
   surface.
4. If you launch the GUI anyway, assume **unsandboxed** bundled backend —
   ADR 0001-style CLI controls do not cover it.

## What sandboxing does *not* give you

- Protection if you run `goose-unsandboxed` out of habit
- Identical enforcement on old kernels when using `--best-effort`
- Coverage of other agents (Claude Code, Codex, Cursor helper binaries,
  etc.) until you apply the same pattern to **their** PATH names
- Safety for secrets the agent must use (API keys you inject, files you
  mounted rw)
- A substitute for not pasting production credentials into prompts

## Operator checklist

Use this when standing up or reviewing a host:

1. Real binary lives under opt (or equivalent), **not** as `PATH` default.
2. `command -v goose` (or your agent name) resolves to a **wrapper**.
3. Wrapper invokes landrun (or equivalent) with a declared mount list.
4. Banner on stderr matches the live mount list.
5. `…-unsandboxed` exists, warns loudly, and is the only blessed full-access
   path.
6. Env bypass cannot silently skip the warning wrapper.
7. Completion / term init still work (quiet plumbing).
8. Work trees you care about are rw; secret-heavy homes are **not**, unless
   justified.
9. GUI/other packages documented as covered or explicitly **not** covered.
10. Host ADRs record the decision; architecture notes track as-built paths.

## Glossary touchpoints

- [operator](../shared/glossary.md#operator) — you; these docs are for you
- [agent sandbox](../shared/glossary.md#agent-sandbox) — host-level confinement
  this guide designs
- [sandboxed entrypoint](../shared/glossary.md#sandboxed-entrypoint) /
  [unsandboxed entrypoint](../shared/glossary.md#unsandboxed-entrypoint)
- [host system](../shared/glossary.md#host-system) / [host home](../shared/glossary.md#host-home)
- [host local context](../shared/glossary.md#host-local-context) — where
  wrappers and opt installs live
- [remote context](../shared/glossary.md#remote-context) — project repos the
  agent should work in (often under `~/tree`)

## Further reading

- [Implementing a sandboxed agent CLI](./implementing-sandbox.md) — do it on
  your host
- [Operator guidelines hub](./README.md)
- [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure)
  ([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md)) —
  `design/architecture` vs `design/decisions` (ADRs)
- [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory)
  ([summary](../shared/summaries/salotz-rfc-024-extended-xdg-base-directory.md)) —
  `XDGX_OPT_HOME` and bin vs opt
- [landrun](https://github.com/zouuup/landrun) — Landlock helper used in the
  reference implementation
- Personal host notes (this author’s machines):
  [Shell and Bimker](../personal/shell-and-bimker.md),
  [Host layout](../personal/host-layout.md)
