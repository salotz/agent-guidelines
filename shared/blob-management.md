# Blob Management

[Blobs](./glossary.md#blob) are files in remote repositories that are too
large or opaque to store effectively under normal VCS operation (for example
git).

Additional extensions and tools must be used to manage them.

When appropriate, blobs are expected to be managed alongside the history of
the rest of the repository and kept in step.

Different projects may have different conventions; respect those when they
are documented.

New projects should favor a flexible, decoupled system like
[DVC](https://dvc.org/) ([summary](./summaries/dvc.md)) over
[git-lfs](https://git-lfs.com/) ([summary](./summaries/git-lfs.md)).

DVC allows more customization of the storage backend as well as shared
caches and similar setup.
