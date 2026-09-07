# Operator Guidelines

These documents are written for the **[operator](../shared/glossary.md#operator)** —
the human who installs, configures, and supervises agent systems — not for
agents to execute as standing work rules.

## Documents

- [Sandboxing agents](./sandboxing.md) — why default-on sandboxing, principles,
  threat model, escape hatches.
- [Implementing a sandboxed agent CLI](./implementing-sandbox.md) — host bring-up
  of the landrun + opt + PATH wrapper pattern.

## How to use these guidelines

1. Read [sandboxing.md](./sandboxing.md) first for the *why* and non-negotiables.
2. Follow [implementing-sandbox.md](./implementing-sandbox.md) when bringing up
   or hardening a host.
3. Keep durable *decisions* for a specific host config repo in ADRs under that
   repo’s `design/decisions/` (see
   [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure)
   ([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md))).
   These operator docs stay generic enough to reimplement elsewhere.
4. Keep agent bootloaders (`AGENTS.md`, `~/.agents/AGENTS.md`) pointed at
   `shared/` / `personal/` unless you intentionally want an agent to implement
   sandboxing work from this tree.
