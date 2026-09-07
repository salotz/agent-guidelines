# Background: `content/` migration notes

Research/notes only.
Operator answers belong in `../decisions.md`.

## Motivation

- Root today mixes **control plane** with **four guideline products**.
- Generic project template adds root machinery (PRJX, `.bootstrap/`, mise, hk, design).
- A single `content/` parent keeps guideline substance grouped and root scannable.

## What does *not* move

- `contributing/` — process for maintaining **this** repo
- `.agents/` — plans/skills for this repo
- Future `design/` — goals/ADRs for this repo
- `.bootstrap/`, tooling, PRJX, EditorConfig, git metadata at root

## Path vocabulary

| Term in prose | In-repo path (after migration) |
|---------------|--------------------------------|
| shared guidelines | `content/shared/` |
| personal guidelines | `content/personal/` |
| operator docs | `content/operator/` |
| templates | `content/templates/` |

Consumers who vendor files may copy into their own layout; drop-in templates need not force a `content/` prefix on *consumer* projects.

## Link classes

1. **Intra-content sibling** — usually stable after joint move
2. **Root/contributing → content** — add `content/` segment
3. **Summaries ↔ templates** — verify `../` depth under `content/shared/summaries/`

## Stub tradeoff (Q3)

- Stubs: kinder to external bookmarks; duplicate-path risk for agents
- No stubs (default): one canonical path

## Bootstrap vs preload (related)

See shared guide; examol ops reference implementation under `~/tree/examol/devel/ops` (`.bootstrap/`, `mise run preload`).
This guidelines repo likely hooks-only preload (Q8), not `uv sync`.
