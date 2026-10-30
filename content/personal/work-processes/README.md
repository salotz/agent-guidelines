# Work processes

Preferred methods of doing work with AI agents on this operator's hosts.

This covers work on and in [projects](../../shared/glossary.md#project).
In other contexts (check the weather, one-off questions, and similar) disregard this advice.

When this operator's [host local context](../../shared/glossary.md#host-local-context) is loaded, treat this directory as standing procedure.
Read this hub, then the matching process document, instead of inventing a new loop.

Host bootloader: `~/.agents/AGENTS.md` (see [Getting started](../getting-started.md)).
That file should point here.

## New work vs predefined

### New work

Undefined processes: no written procedure yet.

Prefer a plan-execute strategy.
Details: [New work](./new-work.md).

### Predefined work processes

Multi-step tasks that already have precedent in this tree.

The one-step-at-a-time default in [New work](./new-work.md) does **not** apply when the operator asked for a named process here.
Follow that document's cadence instead.

These may later become skills.
Until there is a skill install story, the documents in this directory are the context.

## Processes

- [New work](./new-work.md):
  plan-execute chunks, unstaged git, tests in chat, multi-session plan decision Q&A, owner plan index.
- [Ready work batch](./ready-work-batch.md):
  collect implementation-ready items from the org work board, initialize yerk replicas and plans, hand a checklist back.
  The operator starts implementation sessions.

## Related objects

- [Org](../org.md):
  the notes project that holds the digital work board.
- [Host layout](../host-layout.md):
  domain trees under `~/tree`.
- [Shell and Bimker](../shell-and-bimker.md):
  host configuration repo; a frequent target of board items.
- [Yerk](https://github.com/salotz/yerk):
  project catalog and replica materialization on this host (see host bootloader, `yerk://…` identifiers).
