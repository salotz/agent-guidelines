# Goals

## Goals

1. **Portable agent guidelines** under `content/shared/` that other projects can import or reference.
2. **Personal / host guidelines** under `content/personal/` for this operator without polluting the portable set.
3. **Operator (human) docs** under `content/operator/` for sandboxing and host hardening — not default agent session rules.
4. **Opinionated templates** under `content/templates/` as specifications (not generators).
5. **Reflective dogfooding**: this repo follows the generic project stack (git, mise, hk, EditorConfig, PRJX, bootstrap/preload, RFC 22 surfaces).
6. **Thin control plane at repo root**: tooling, contributing, design, and agent plans stay outside `content/`.

## Non-goals

1. Shipping a copier/cookiecutter (or similar) generator for templates.
2. Host-global hook or tool installs as the default story.
3. Blob/VCS-sidecar tooling (DVC, git-lfs) until real large assets exist.
4. Duplicating portable prose into `.agents/` or `contributing/`.
5. Auto-reflowing Markdown in hooks (semantic line breaks must survive checks).

## Principles

- Config files stay content-focused; how-to lives in `contributing/`.
- Bootstrap (host check) → project install → preload → check.
- One canonical path for guideline trees (`content/…`); no old-path stubs.
- Promote lasting choices to ADRs under `design/decisions/`; keep plan Q&A ephemeral under `.agents/plans/`.
