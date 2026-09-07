# ADR 004: No blob tooling until needed

## Status

accepted

## Context

The generic project template prefers DVC, then git-lfs, then ad hoc systems for large/opaque files.
This repository is Markdown-first and currently has no large binary assets under versioned workflow.

## Decision

- Do **not** initialize DVC, git-lfs, or an ad hoc blob store in-tree yet.
- When large assets appear, prefer **DVC** per [blob management](../../content/shared/blob-management.md) unless an external constraint forces git-lfs.

## Consequences

- No empty `.dvc` / LFS scaffolding to maintain.
- Revisit this ADR when binaries, datasets, or media enter the project.
