# Personal Guidelines

Operator- and host-specific guidelines for this repository's author.

These specialize the [shared](../shared/) guidelines with host paths,
shell/config layout, and skills that assume a particular machine setup.

Do not treat this tree as portable project defaults. Load it when working
on this operator's hosts or when personal context is explicitly in scope.

## Contents

- [Shell Configuration and Bimker](./shell-and-bimker.md): bimhaw-managed
  shell config and the `~/.bimker` host configuration repo.
- [skills/](./skills/): personal agent skills (e.g. remote repo cacher).

## Relationship to shared

Always follow [shared](../shared/) first. Files here only add host-specific
paths and procedures. Where both apply, shared standards win on portable
behavior; personal wins on concrete locations and install/config mechanics
for this operator.
