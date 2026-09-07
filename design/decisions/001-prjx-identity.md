# ADR 001: PRJX project identity

## Status

accepted

## Context

The generic project template and [salotz RFC 28](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.028_prjx) require a project root sentinel and portable metadata so tools can discover the project and a stable fully qualified name.

## Decision

- Empty `.prjx-root` at the **git repository root**.
- `.config/_project-meta.toml` with:

  ```toml
  [project]
  name = "agent-guidelines"
  namespace = "salotz"
  ```

- Fully qualified project name: `salotz.agent-guidelines`.
- Host/replica overrides only under gitignored `.local/`.

## Consequences

- PRJX-aware tools can locate this project without hard-coded paths.
- Operators must not commit `.local/`.
- `content/` is ordinary project material, not the PRJX root.
