# Summary: salotz RFC 002 — Growth Versioning (Semantic Changelog)

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.002_semantic-changelog

## Overview

Product versioning that keeps a **SemVer-shaped** `B.R.G` tuple but assigns **different meanings** than classic MAJOR.MINOR.PATCH.

| Part | Name | Increments when |
| --- | --- | --- |
| **B** | Breakage | User-facing breakage (stricter, removal, rename, replaced, …) |
| **R** | Regression | Noteworthy regression (deprecation, hamstring, noisier, …) |
| **G** | Growth | Noteworthy growth (feature, repair, performance, clarity, …) |

## Rules

- Three non-negative integers separated by dots.
- Incrementing **B** resets **G** to `0`.
- Incrementing **R** does **not** reset **G**.
- While **B is 0** (pre-stable), interface breakages are recorded as **regressions** (bump **R**, not **B**).
- **G**-only bumps should be safe for users who avoid breakage/regression intent.
- Release tags commonly `vB.R.G`; binary stamp often `B.R.G` without `v`.

## Changelog shape

End-user sections follow RFC 002 naming (Improvements / Breakages / Regressions) rather than stock Keep a Changelog “Added/Changed/Fixed” alone.
Commit trailers (`Change`, `Audience`, …) are preferred structured metadata when used; Conventional Commits subject prefixes for change kind are **not** required.

## Do not conflate

- Product `B.R.G` ≠ API resource `apiVersion` (e.g. `app/v1`)
- Product `B.R.G` ≠ `.appinfo/meta.toml` document `version` (RFC 030)

## See also

- [CLI and application patterns](../software/cli-applications.md) — how products apply this
