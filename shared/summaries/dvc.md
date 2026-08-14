# Summary: DVC (Data Version Control)

Source: https://dvc.org/ (docs: https://dvc.org/doc; start guide markdown: https://raw.githubusercontent.com/treeverse/dvc.org/main/content/docs/start/index.md)

## What it is
Tool for versioning large data and models next to code without storing the
blobs in Git. Often described as “Git for data.”

## How it works (core)
- Initialize in a Git repo with `dvc init`.
- `dvc add` tracks files/dirs: real content goes to a content-addressed
  **cache**; Git tracks small `.dvc` metadata (hashes/paths) and usually
  gitignores the data itself.
- Checkout/switch versions of large artifacts via DVC + Git without stuffing
  Git objects with the blobs.
- **Remotes** (`dvc remote add`, `dvc push` / `dvc pull`) store/share cache
  content on many backends (local path, S3, GCS, SSH, HTTP, etc.)—not tied to
  the Git host’s LFS endpoint.
- Cache can be **external/shared** across projects or machines (`dvc cache dir`,
  shared permissions/link types) to avoid duplicate downloads and copies.

## Why this repo prefers it for new projects
More flexible than git-lfs: storage backend choice, shared caches, and
decoupling blob storage from the Git remote. Respect existing project
conventions when another tool is already in use.

## Agent notes
- Prefer project-documented DVC workflows over inventing new blob tooling.
- Small `.dvc` / config files are remote context; large cache/remote content is
  not for casual full-text loading.
- Host installs of `dvc` still follow host-install confirmation rules.
