# Summary: salotz RFC 030 — Application info

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.030_application-info

## Overview

Static **usage context** for software products so humans, agents, and tools can discover products and commands without scraping READMEs or running binaries.

Default path: `${PROJECT_ROOT}/.appinfo/meta.toml`

## Core shape

- Document `version`
- Optional `[project]` labels
- `[products.<id>]` first-class products (name, command, summary, …)
- **Open extensions**: new concerns add named tables (not a pyproject-style `[tool.*]` dump)

## Boundaries

| Surface | Answers |
| --- | --- |
| This RFC (appinfo) | Usage context, products, commands |
| [packslip](./packslip.md) | Signed release / install artifacts |
| [RFC 28 PRJX](./salotz-rfc-028-prjx.md) | Project root, replicas, `PRJX__*` project env |
| [RFC 031](./salotz-rfc-031-application-env.md) | Env variable **registry** tables inside appinfo |

Do not merge packslip release identity into appinfo.
Do not replace PRJX project metadata with appinfo.

## Related

- [CLI and application patterns](../software/cli-applications.md)
