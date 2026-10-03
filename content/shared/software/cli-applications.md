# CLI and application patterns

Product-shape patterns for **shippable CLIs** and similar host tools:
layers, config, identity, env discovery, docs layout, versioning, and release.

Distilled from product work (especially [yerk](https://github.com/salotz/yerk)) and salotz RFCs.
Not a language tutorial.
Does not replace [Software Guidelines](./guidelines.md) (process), [Documentation](./documentation.md) (Diátaxis), or [CLI UX](./cli-ux.md) (terminal interaction).

Load with [CLI UX](./cli-ux.md) when implementing or reviewing a CLI.

## Scope

**In:** single- or few-binary CLIs; host-global or multi-project tools with config, catalogs, and machine-readable output.

**Out:** web/API server product design (reuse resource ideas only where they fit); GUIs; language choice (Go is common for standalone binaries; not required).

## Product spine before code

Freeze vocabulary and the first feature slice in design prose **before** growing the CLI surface.

| Artifact | Role |
| --- | --- |
| `design/goals.md` | North star and post-MVP directions |
| `design/…` domain doc | Nouns, verbs, status model, non-goals |
| `design/decisions/` | ADRs (config layout, identity, output, adapters) |
| `.agents/plans/` | Ephemeral implementation coordination—not product truth |

1. Name **nouns** (resource kinds) and **internal verbs** separately from CLI spellings.
2. Avoid overloaded product verbs (e.g. “checkout” with three meanings). Prefer precise verbs (`materialize`, `ensure`, `get`, `lookup`).
3. **No stub commands.** Add a command only when behavior exists. Design sync/push/pull in an ADR before any CLI that pretends to do them.
4. Separate conflatable concepts in the domain doc (project ≠ directory; replica ≠ branch; tool config ≠ catalog; path ≠ identity).
5. `design/` = history and intent; `docs/` = operators; `contributing/` = bootstrap/build/test.

## Layered architecture

Keep ownership boundaries sharp so CLI frameworks do not own product types.

```text
CLI (flags, selection, print)
  → load config / catalog (storage structs)
  → map into product API resources
  → pure domain (path math, id expand, policy merge)
  → adapters (VCS, FS, network) behind interfaces
  → print resources (table | json | yaml | …)
```

| Layer | Owns | Must not own |
| --- | --- | --- |
| CLI package | Flag parse, selection, help, printers | Business rules; TOML schema as product API |
| Config load | File parse/validate, storage structs | Observed runtime state; table layout |
| API / resources | Stable product types (`apiVersion`, `kind`, fields, `uri`) | TOML tags; tabwriter layout |
| Domain | Path math, identity expand, placement/policy, collectors | Ad-hoc shell-outs; cobra `RunE` sprawl |
| Adapters | One seam per external system (e.g. `git` runner) | Product identity or config shape |

Rules:

1. CLI `RunE` stays thin: load → select → domain → print.
2. Pure layout and identity have no network and no VCS.
3. Only the adapter talks to git (or docker, etc.).
4. Prefer small internal packages over a mega-`cli` god package for domain logic.
5. Inject IO streams (`In`/`Out`/`Err`) so tests capture output without global monkey-patches.

### External tool adapters

Default for operator-host tools that need a real toolchain:

- **Subprocess to the real CLI** (`git`, etc.) behind an interface when behavior must match the operator’s tool (SSH agent, hooks, worktrees, credentials).
- Pure-library ports only with a concrete need (hosts without the binary, proven process scaling limits).

Document “requires `git` on `PATH`” (or equivalent)—do not imply a pure-language VCS if that is false.

## Config and data files

### Split by lifetime

| Concern | Typical file | Lifetime |
| --- | --- | --- |
| Tool / host behavior | `config.toml` | Per-machine: paths, styles, probe defaults |
| Portable registry / catalog | `catalog.toml` | Shareable; agents edit lists safely |
| Tool-written state | under XDG state | Tool-owned; not hand-edited as source of truth |

Missing tool config ⇒ **defaults**.
Missing catalog ⇒ **empty registry**, not a hard error (unless the command requires entries).

### Host paths vs portable identity

- Keep **host paths** out of portable catalogs.
- Resolve placement from **host config** (domain roots, overrides) and optional bound state—not “the catalog row is a path.”
- Identity fields (`name`, `domain`, …) stay portable; placement is host policy.

### XDG and prefixes

- Default config under `$XDG_CONFIG_HOME/<app>/`; state under `$XDG_STATE_HOME/<app>/` when the tool writes bindings.
- Overrides: explicit file path env, config **directory** env, targeted overlays—not a zoo of one-off paths without a directory root.
- Tool env prefix is product-owned (RFC 027, e.g. `YERK__…`).
- Do **not** register tool knobs under PRJX `[project.env-vars]` (wrong prefix and document). PRJX is project-local; the CLI product is app-local.
- See [RFC 24](../summaries/salotz-rfc-024-extended-xdg-base-directory.md) (extended dirs) and [RFC 28](../summaries/salotz-rfc-028-prjx.md) (project metadata boundaries).

### Examples vs operator state

| Allowed in-repo | Forbidden in shared trees |
| --- | --- |
| `examples/config.toml`, `examples/catalog.toml` (generic placeholders) | Root-level `*.example.toml` sprawl as the only pattern |
| Small portable demos under `examples/` | Real home paths, private remotes, one operator’s catalog |
| Tests using **temp dirs** and synthetic data | Committed operator-host fixtures as “the” dogfood |
| How-tos that copy examples → temp/XDG | Mise tasks that assume a private fixture tree |

Operator dogfood lives in XDG (or `APP__CONFIG_DIR`), outside the shared repo—or gitignored local-only beside a checkout.

Do not expose `config example` as an operational subcommand unless there is a deliberate sample-dump feature; samples stay under `examples/`.

## Environment variable registry

Treat env documentation as a **[single source of truth](../glossary.md#single-source-of-truth-ssot)** with multiple **views**, not copy-pasted prose in every command `Long`.

### Two aligned declarations

| Surface | Role |
| --- | --- |
| In-code registry (e.g. `internal/envvars`) | Runtime help formatters + live value dumps |
| Static [`.appinfo/meta.toml`](../summaries/salotz-rfc-030-application-info.md) ([RFC 031](../summaries/salotz-rfc-031-application-env.md)) | Discovery without executing the binary |

Keep **names, type, policy, default story, and enum values** aligned ([RFC 032](../summaries/salotz-rfc-032-env-value-types.md)).
Name form: [RFC 027](../summaries/salotz-rfc-027-env-nexps.md).

### Help and live views

| Surface | Content |
| --- | --- |
| Root `--help` | Product blurb + **primary** tool env summary + pointers |
| `<cmd> --help` | Command prose + **primary** env vars for that command |
| `help envvars` (or equivalent) | **Full documentation** (global vs command-scoped) |
| `envvars` run | **Live values** for this process |

Rules:

1. No `--help-all` that fights the framework’s help exit path—use a dedicated full-reference command.
2. Do not merge product env walls into the flag help block.
3. Docs reference ≠ live dump.
4. Command packages choose **which formatter** to attach; they do not re-author env paragraphs.
5. External names (`HOME`, `XDG_*`, `PATH`) appear when the product reads them; mark them external.
6. Foreign prefixes (e.g. `PRJX__*`) get a short pointer to their spec—not a dump of every key—unless the product owns them.

## Application info and release metadata

Keep three identities distinct:

| Concern | Where | Not |
| --- | --- | --- |
| Usage context (products, commands, env registry) | `.appinfo/meta.toml` (RFC 030/031) | Release digests |
| Project ops metadata | PRJX `.config/_project-meta.toml` (RFC 28) | CLI product env |
| Signed install / supply chain | [packslip](https://packslip.dev/) (or equivalent) | Long-lived usage prose |

Do not merge packslip and appinfo.
Do not put tool `APP__*` under PRJX project env tables.

## Identity and addressing

### Stable ids independent of layout

- Prefer **layout-independent** identifiers (domain + name, or equivalent).
- Offer **one canonical machine form** (e.g. `app://domain/name[/instance]`) plus **bare** ergonomics for humans.
- Expand bare → canonical **internally**; never silently pick among ambiguous short names—**error with candidates**.
- Filesystem paths are **not** identifiers. Path → id is a separate **lookup** feature.

### Get vs lookup vs status

| Verb | Direction | Default job |
| --- | --- | --- |
| **get** | id → resource | One-resource detail |
| **lookup** | path → id (and optionally resource) | Stable handle from cwd/path; human default may be URI-only |
| **status** | selection → many rows | Multi-row presence/change view |

Ship **universal** verbs and, when nouns matter, **noun-scoped** verbs (`project get`, `replica lookup`) without forking business logic.

Do **not** auto-expand a project id into a default instance when resolving **identity**.
Choosing a default replica for a **probe** after the project is known is operation policy, not id expansion.

## Model-driven API resources

Pilot **kubectl-style resources** even without HTTP:

- Stable types: `apiVersion`, `kind`, documented fields, `json` tags.
- Map loaders → resources → printers.
- Enables `--output json|yaml|table`, later schema docs, and `get` without rewriting every command.

Boundaries:

- Config structs = **storage**
- API structs = **product**
- CLI rows = **presentation only** (prefer printing API types or thin column views)

New observed fields land on API types first, then printers.
Casual JSON key renames are breaking—bump `apiVersion` or release notes deliberately.

Resource `apiVersion` is **not** the product/binary version string.

## Output formats

Shared flag: **`--output`**.

| Value | Meaning |
| --- | --- |
| omit / empty | Command default human layout |
| `table` | Explicit alias for that human layout |
| `json` | One document (or array for multi); include `apiVersion` / `kind` on resources |
| `yaml` | Same field names as JSON tags |

- Unsupported values **error**.
- No requirement for TTY auto-json unless product-decided later.
- Multi-row human tables may print preamble lines; structured output should be **clean documents**.
- New read surfaces reuse the same vocabulary.

## CLI surface design

### Selection models

Define **one** selection model and reuse it:

- Explicit ids
- `--all` for opt-in full-catalog mutate
- Closed tag vocabulary + `--tag` (unknown tag → error; empty match → OK for reads, refuse for mutates)
- `--domain` / group filters as needed
- Mutually exclusive selectors on mutate commands (args XOR `--all` XOR `--tag` XOR …)

### Safe defaults

- **Refuse** clobbering non-empty destinations on materialize/create.
- **Ensure** creates only what the verb promises (e.g. workspace directory ≠ replica leaf).
- Network probes stay **opt-in**; local observation can be default.
- Prefer **change/presence detail on by default** for status-like commands when cheap, with a fast opt-out—rather than hiding the useful default behind a verbose flag.

### Bulk vs single

- Reads may default to “entire catalog.”
- Mutates that touch many targets require explicit bulk selectors (`--all` / `--tag` / …), not accidental full runs from bare invoke—unless deliberately documented.

## Documentation layout

Follow [Software Documentation](./documentation.md) (Diátaxis).

| Tree | Job |
| --- | --- |
| `docs/tutorials/` | First success path |
| `docs/how-to/` | Concrete operator tasks |
| `docs/reference/` | Commands, config keys, catalog schema, envvars overview |
| `docs/explanation/` | Concepts, status model, identity |
| `design/` | Goals, domain freeze, ADRs |
| `contributing/` | Bootstrap, build, test, release cut |
| Root `README.md` | Short hub + quick start—not the full manual |

Near-term docs may be **ad hoc Markdown** under Diátaxis folders without a site generator ([yerk ADR 006](https://github.com/salotz/yerk/blob/main/design/decisions/006-ad-hoc-docs-diataxis.md) pattern).
Publication tooling comes later and should **consume** that tree.

**CLI help is authoritative** for flags and env behavior in the binary.
Repo reference docs summarize and point; they must not become a second hand-maintained dump that drifts from the registry and `--help`.

## Versioning and release

### Three version surfaces

| Surface | Scheme (salotz default) |
| --- | --- |
| Product / binary | [Growth Versioning](../summaries/salotz-rfc-002-growth-versioning.md) `B.R.G`—SemVer-*shaped*, different meanings |
| API resources | `app/v1`-style generation; bump on resource schema breaks |
| `.appinfo` `version` | RFC 030 document version |

- Release tags: `vB.R.G`.
- Binary stamp: `B.R.G` without `v` (unless an ADR says otherwise).
- Dev builds: explicit dirty/dev marker (e.g. `0.0.0-dev`).
- While **B is 0**, treat user-facing breakages as **regressions** (bump R).

### Shipping

- Prefer a **standalone binary** when the audience should not install a language runtime to run the tool.
- Pin the toolchain in project mise (or equivalent); build via documented `mise run build`.
- Release automation: test → artifact → checksums → **signed install metadata** ([packslip](../summaries/packslip.md)) on the primary archive.
- Keep signing in a **stable workflow path** (moving the job file can change OIDC signer identity consumers pin).
- Document install as `mise use … packslip:…` (or project equivalent) after the first green release; publish packslip pin when available.

## Implementation checklist (agents)

1. Update **domain/ADR** if nouns, identity, or file roles change—not only code.
2. Extend **API resource types** before inventing CLI-private DTOs for new observed fields.
3. Register new env knobs in **both** the in-code registry and `.appinfo/meta.toml`.
4. Attach help via **formatters** (primary vs full); do not paste env essays into every `Long`.
5. Keep **examples** portable; never commit host-private catalogs as fixtures.
6. Add commands only with **real behavior**; no stubs.
7. Support `--output json` (and yaml when the product has it) on new read surfaces.
8. Tests: temp dirs only; inject IO; unit-test pure path/id/policy packages heavily.
9. Do not conflate product version bumps with `apiVersion` or appinfo document version.
10. Prefer adapter interfaces over scattering `exec.Command` in command handlers.

## Anti-patterns

- One TOML mixing host paths, tool defaults, and the portable project list
- Identity = “whatever folder we cloned into”
- Silent disambiguation of short names
- Status/get logic twice (once for table, once for JSON)
- Comprehensive env dump on every `--help`; `--help-all` bolted onto a framework that already exits in `HelpFunc`
- Stub `pull`/`push`/`sync` commands that only say “not implemented”
- Root `README` as the only docs surface
- Machine-specific dogfood committed as `examples/` or CI fixtures
- Library VCS rewrite mid-feature without an adapter-bound need
- Packslip / appinfo / PRJX collapsed into one “meta.toml”
- Shipping SemVer language while meaning Growth Versioning (or the reverse) without saying so

## Related

- [Software Guidelines](./guidelines.md) — git, comments, TDD, test layout
- [Software Documentation](./documentation.md) — Diátaxis
- [Project management and tooling](../project-management-and-tooling.md) — mise, bootstrap, tasks
- [Generic project template](../../templates/generic-project.md) — default stack
- Summaries: [RFC 002](../summaries/salotz-rfc-002-growth-versioning.md), [RFC 027](../summaries/salotz-rfc-027-env-nexps.md), [RFC 030](../summaries/salotz-rfc-030-application-info.md), [RFC 031](../summaries/salotz-rfc-031-application-env.md), [RFC 032](../summaries/salotz-rfc-032-env-value-types.md), [packslip](../summaries/packslip.md)
