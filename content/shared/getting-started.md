# Getting Started

To get a project to use these guidelines you should:

## 1. Copy the bootloader section

There is a file [agents_md_template.md](./agents_md_template.md) which provides some context you can drop into your `AGENTS.md` file to have it reference these guidelines.

## 2. Optional: apply an opinionated stack

For concrete tool and layout choices (git, mise, hk, EditorConfig, PRJX, RFC 22 paths, blobs), see the [generic project template](../templates/generic-project.md).

## Shared vs personal

- **Shared** (portable set; in this repo: `content/shared/`):
  portable guidelines for any project.
  Use these by default.
- **Personal** (operator/host set; in this repo: `content/personal/`):
  operator host and configuration rules.
  Load only when that context applies.

Projects that only need portable rules should reference the shared tree (or an equivalent checkout of it).
When pointing at **this** guidelines repository, use paths under `content/shared/`.
