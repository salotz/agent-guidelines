# Summary: packslip

Source:
https://packslip.dev/

## Overview

Signed **release / install metadata** for distributing CLI (and similar) artifacts—complementary to usage-context files like `.appinfo` (RFC 030).

Answers: which artifact, which bin path, digests, and **who signed** this release—so tools such as [mise](./mise.md) can install with verification.

## Typical product flow

1. CI builds platform archives (e.g. `tool-VERSION-linux-x64.tar.gz` with binary at archive root).
2. Upload release assets + checksums.
3. Generate and sign a packslip bundle (often OIDC/keyless in GitHub Actions via `jdx/packslip`).
4. Consumers install with a packslip-aware backend (e.g. `mise use … packslip:github.com/org/proj`).
5. After first release, publish `packslip pin` fingerprint beside install docs.

## Boundaries

| Concern | Owner |
| --- | --- |
| Signed artifacts + install bin | packslip |
| Usage context, env registry | [RFC 030/031 appinfo](./salotz-rfc-030-application-info.md) |
| Project root / replicas | [RFC 28 PRJX](./salotz-rfc-028-prjx.md) |

Do not merge packslip into `.appinfo/meta.toml`.
Keep the signing workflow path stable; moving the job file can change the OIDC signer identity pins refer to.

## Related

- [CLI and application patterns](../software/cli-applications.md) — release section
- [mise](./mise.md)
