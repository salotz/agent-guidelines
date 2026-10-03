# Summary: salotz RFC 031 — Application environment registry

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.031_application-env

## Overview

Environment-variable **declaration** tables for [RFC 030](./salotz-rfc-030-application-info.md) documents (`.appinfo/meta.toml`).

Humans and agents discover what a product reads without executing it.

## Shape

- Shared `[env]` and optional per-product `[products.<id>.env]`
- `env_prefix` for the product namespace
- Per-name subtables under `.vars` (one table per full env name)
- Fields focus on prose plus optional value metadata from [RFC 032](./salotz-rfc-032-env-value-types.md): `type`, `policy`, `default`, enum `values` / `aliases`
- `external` marks borrowed names (`HOME`, `XDG_*`, …)

## Alignment

Runtime help registries (in-code) **must stay aligned** with appinfo for tool-owned and documented platform names: same type, policy, default story, enums.

Name form remains [RFC 027](./salotz-rfc-027-env-nexps.md).

## Related

- [CLI and application patterns](../software/cli-applications.md) — help views vs live dumps
