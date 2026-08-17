# Personal Guidelines

Operator- and host-specific guidelines for this repository's author.

These specialize the [shared](../shared/) guidelines with host paths,
shell/config layout, and skills that assume a particular machine setup.

Do not treat this tree as portable project defaults. Load it when working
on this operator's hosts or when personal context is explicitly in scope.

## Contents

- [Getting Started](./getting-started.md): install personal context on a
  host (`~/.agents/AGENTS.md`) and point agent harnesses at it.
- [agents_md_template.md](./agents_md_template.md): drop-in bootloader for
  host-local agent context (`~/.agents/AGENTS.md`).
- [Work process](./work-process.md): plan-execute chunks, commits, tests in
  chat, and **multi-session plan decision Q&A** (single `decisions.md` inbox).
- [Shell Configuration and Bimker](./shell-and-bimker.md): bimhaw-managed
  shell config and the `~/.bimker` host configuration repo.
- [Host Layout](./host-layout.md): domain trees under `~/tree` and related
  path conventions (extends salotz RFCs 22–26).
- [skills/](./skills/): personal agent skills (e.g. remote repo cacher).

## Relationship to shared

Always follow [shared](../shared/) first. Files here only add host-specific
paths and procedures. Where both apply, shared standards win on portable
behavior; personal wins on concrete locations and install/config mechanics
for this operator.
