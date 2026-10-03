# Summary: salotz RFC 027 — Environment variable name expressions

Source:
https://github.com/salotz/rfcs/tree/master/rfcs/salotz.027_env-nexps

## Overview

Naming rules for UNIX-style environment variables (names only, not values).
Adapts name-expression ideas to screaming snake case in the global process environment.

## Form

- Regex shape: `^[A-Z_][A-Z0-9_]*$`
- Single `_` separates **words** inside a symbol
- Double underscore `__` (“dunder”) separates **fields** (tuple-like segments)

Examples:

```text
MY_VARIABLE
PREFIX__FIELD_A__FIELD_B
YERK__CONFIG_DIR
```

## Conventions

- Prefer word separators over run-on tokens (`MY_EXAMPLE` not `MYEXAMPLE`).
- Leading underscore patterns mark special/internal classes per the RFC; do not invent conflicting “hidden” schemes casually.
- Do not reuse POSIX, XDG, and other reserved names/prefixes for unrelated product meaning.

## Related

- Value types and policies: [RFC 032](./salotz-rfc-032-env-value-types.md)
- Product env **registry** shape: [RFC 031](./salotz-rfc-031-application-env.md)
- [CLI and application patterns](../software/cli-applications.md)
