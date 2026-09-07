# `.agents/` (this repository)

Harness-oriented context for **agent-guidelines** ([salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](../content/shared/summaries/salotz-rfc-022-ai-coding-structure.md))).

## What lives here

| Path | Role |
|------|------|
| [plans/](./plans/) | Ephemeral operator↔agent plans (see [work process](../content/personal/work-process.md)); owner index [plans/salotz/todo.md](./plans/salotz/todo.md) |
| [context/repo-map.md](./context/repo-map.md) | Short directory map for agents |
| `skills/` (optional, later) | Project skills; prefer linking `content/*/skills` until local skills pay off |

## What does **not** live here

- Portable guideline prose → [`content/shared/`](../content/shared/)
- Host/personal rules → [`content/personal/`](../content/personal/)
- Human sandboxing how-to → [`content/operator/`](../content/operator/)
- Project goals / ADRs → [`design/`](../design/)
- Maintainer bootstrap → [`contributing/development.md`](../contributing/development.md)

Root [`AGENTS.md`](../AGENTS.md) is the bootloader TOC for this repo.
