# Org

The **org** project is this operator's personal notes silo: thoughts, priorities, capture, and the digital work board.

It is a standard object on this host, not a one-off folder name.
Agents should recognize "org", "the org repo", and "org notes" as this project.

Work processes that collect or route todos start here: [Work processes](./work-processes/).

## Identity

- Yerk project: `yerk://personal/salotz-org` (catalog name `salotz-org`).
- Typical replica: `yerk://personal/salotz-org/master`.
- Fully qualified project name in the tree: `salotz.org`.
- Do not guess the on-disk path.
  Resolve with yerk (`yerk path`, `yerk get`, `yerk lookup`).
  On this operator's usual layout the bound path is under `~/tree/personal/admin/` with name-tags placement (often `…/admin/org`).

Org is an **admin** notes project ([Host layout](./host-layout.md)), not a software product.
Do not implement target-repo features from org notes.
The note records intent; the work happens in the target project replica.

## What it holds

Product notes live under `notes/` (Denote naming).
Project meta (design, contributing, agents, tooling) stays at the repository root beside `notes/`.

### Digital work board

Context-heavy computer work uses one file per item under `notes/todo/`:

- `notes/todo/current/`: urgent digital issues (keep small; type keyword `todo`)
- `notes/todo/backlog/`: committed, capacity-groomed work (keyword `backlog`)
- `notes/todo/ideas/`: uncommitted ideas (keyword `idea`)
- `notes/todo/finished/`: completed work kept for history (keyword `finished`)
- `notes/todo/graveyard/`: cancelled / will not do (keyword `graveyard`)

`notes/incoming/` is general capture for any note kind.
It is not a work-board column.

Physical / errand plate-clearing stays in Bullet Journal or phone apps.
Do not revive a GSD-style org TODO database.

### Other buckets

- `notes/journal/`: one day note per calendar day (replaces a former monolithic `notebook.org`)
- `notes/threads/`: research conversation artifacts
- `notes/ref/`: evergreen reference (schema, maps, profile)
- `notes/logs/`: chore logs (mostly historical)
- `notes/archive/`: cold history that is not a live note

Prefer one concern per note file.
Route agents at specific bucket paths or filename keywords, not at a revived monolith.

## How notes relate to other projects

A work-board note often names a **target** (Bimker, yerk, Emacs config, a devel project).
Bimker in particular hosts many local topics, so one tree may have several independent change sets.

Typical pointers inside a note:

- Target project or yerk id
- Plan path in that project's `.agents/plans/` (or `.agents/plan/`)
- Denote links to related research or idea notes
- Locked constraints and non-goals

The note is the board card.
Plans, replicas, and commits live in the target project.

For churning many independent, implementation-ready cards, see [Ready work batch](./work-processes/ready-work-batch.md).

## Agent habits

- Read the note and the target it names.
  Do not treat org as the implementation checkout.
- Do not mix control-plane paths into `notes/`, or invent a second product root.
- When syncing progress, update the board note to match the target (plan state, replica, leftover decisions).
  Do not duplicate the plan into org.
- Bimker care still applies when the target is `~/.bimker` ([Shell and Bimker](./shell-and-bimker.md)).
