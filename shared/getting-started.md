# Getting Started

To get a project to use these guidelines you should:

## 1. Copy the bootloader section

There is a file [agents_md_template.md](./agents_md_template.md) which provides some context you can drop into your `AGENTS.md` file to have it reference these guidelines.

## Shared vs personal

- **Shared** (`shared/`): portable guidelines for any project. Use these by default.
- **Personal** (`personal/`): operator host and configuration rules. Load only when that context applies.

Projects that only need portable rules should reference `shared/` (or an equivalent checkout of it).
