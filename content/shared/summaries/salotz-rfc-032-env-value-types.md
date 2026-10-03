# Summary: salotz RFC 032 — Environment variable value types

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.032_env-value-types

## Overview

Small **type system** and **value grammar** for environment variable **strings** (value side; names are RFC 027).

## Types (normative names)

- `boolean`
- `enum`
- `string`
- `null`
- `nullable-enum` (enum or typed null only—not general unions)

## Missing vs set

- **Missing**: absent **or** empty string (same)
- **Set**: non-empty body
- Empty is **not** typed `null`

## Value policy (per variable)

| Policy | Role |
| --- | --- |
| `silent` | Soft handling; limited reporting |
| `warn` | Default when unspecified |
| `strict` | Hard fail on invalid |
| `required` | Must be set; no default |

Non-`required` variables declare a **default** (literal and/or resolution prose).

## Related

- Registry embedding: [RFC 031](./salotz-rfc-031-application-env.md)
- [CLI and application patterns](../software/cli-applications.md)
