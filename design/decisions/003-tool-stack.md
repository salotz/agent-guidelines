# ADR 003: Tool stack (mise, hk, EditorConfig, bootstrap/preload)

## Status

accepted

## Context

The generic project template and shared tooling guide define a layered stack:
host bootstrap checks, project tool manager, task runner, git hooks, EditorConfig.
Live reference shape: examol `ops` (without copying its Python/uv app preload).

## Decision

- **EditorConfig**: root `.editorconfig` with LF + final newline defaults.
- **mise**: root `mise.toml` pins project CLIs (starting with `hk`) and thin tasks.
- **hk**: `hk.pkl` for hooks and `check` / `fix` entrypoints.
- **Bootstrap**: `.bootstrap/host-tool-check` + `host-tools.conf` (required: `git`, `mise`; check-only).
- **Preload**: mise task `preload` → `hk install` (hooks only for this docs repo).
- **Format**: mise task `format` → `hk fix --all`.
- **Check**: mise task `check` → `hk check --all`.
- No network installers under `.bootstrap/` in the initial dogfood.

## Consequences

- Happy path: `host-tool-check` → `mise install` → `mise run preload` → `mise run check`.
- Docs in `contributing/development.md`; config stays content-focused.
- Agents must not treat preload as host bootstrap.
