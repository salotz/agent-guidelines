# Summary: Git LFS (Large File Storage)

Source: https://git-lfs.com/ (README: https://raw.githubusercontent.com/git-lfs/git-lfs/main/README.md)

## What it is
Git extension (client + transfer protocol) for storing large files outside
normal Git objects while keeping pointers in the repository.

## How it works (core)
- Install client; run `git lfs install` once per machine for hooks/config.
- `git lfs track "<pattern>"` records patterns in `.gitattributes` (commit that
  file).
- Tracked files are stored as LFS objects; Git holds small pointer files.
- Normal `git add` / `commit` / `push` / `pull` upload/download LFS objects to
  the configured LFS endpoint (typically the Git host’s LFS storage).
- `git lfs ls-files` lists managed paths.
- Migrating existing history into or out of LFS rewrites history
  (`git lfs migrate import` / `export`).

## Characteristics relevant here
- Tightly coupled to Git workflows and usually to the Git remote’s LFS support.
- Less flexible storage/backend choice than tools like DVC; shared
  cross-project caches are not the primary model.
- Widely deployed on major Git hosts; many existing projects already use it.

## Agent notes
- If a project already uses git-lfs, follow its patterns; do not switch tools
  unprompted.
- For **new** projects in these guidelines, prefer DVC unless the operator or
  project requires git-lfs (see [Blob Management](../blob-management.md)).
- LFS binaries/pointers are poor full-text context; do not dump large LFS
  payloads into agent context.
