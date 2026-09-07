# ADR 002: Guideline trees under `content/`

## Status

accepted

## Context

Root mixed control-plane files (LICENSE, AGENTS, contributing) with four guideline products (`shared`, `personal`, `operator`, `templates`).
Dogfooding the generic template adds more root machinery (PRJX, bootstrap, mise, hk, design).

## Decision

- Move all guideline substance to:

  ```text
  content/shared/
  content/personal/
  content/operator/
  content/templates/
  ```

- Keep vocabulary (“shared guidelines”, etc.) in prose; in-repo paths use the `content/` prefix.
- **No** compatibility stubs or symlinks at the old root paths.

## Consequences

- Root stays scannable as the project control plane.
- External deep links to former root paths break (accepted).
- Hubs and relative links must use `content/…`.
- Consumers vendoring files may place them anywhere; this layout is canonical **for this repo**.
