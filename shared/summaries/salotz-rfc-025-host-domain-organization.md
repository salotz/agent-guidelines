# Summary: salotz RFC 25 - Host Domain Organization

Source: https://github.com/salotz/rfcs/tree/master/rfcs/salotz.025_host-domain-organization

## Purpose
Guidelines for organizing project work on host systems. Distinguishes local (host-only) vs remote (shared/persisted) work, and organizes remote work by domain (e.g. personal vs work).

## Core Concepts
- **Local work**: host-only; not shared or persisted across hosts.
- **Remote work**: shared/persisted (typically git); primary form of durable work.
- **Scratch**: ephemeral local experiments; safe to discard.
- **Domains**: contexts of work on one host (e.g. `personal`, `work`).
- **Inbox / staging (outbox)**: incoming files and outbound staging areas.

## Directory Layout (recommended)

### Local
- `~/scratch` — ephemeral scratch
- `~/local/work` — local project work
- `~/local/resources` — reusable local resources without a standard XDG home
- `~/local/inbox`, `~/local/outbox` — optional non-web inbox/outbox
- `~/local` and `~/scratch` backed up with the host; migrate `~/local`, not `~/scratch`

### Remote
- `~/tree/<domain>/` — all remote work by domain
- Under each domain (recommendations):
  - `devel/` — software projects
  - `projects/` — non-software projects
  - `admin/` — domain meta-content
  - optional `inbox/`, `outbox/`

### Inbox defaults
- Primary inbox: `~/Downloads`
- Alternates: `~/local/inbox`, `~/tree/<domain>/inbox`
- Outbox mirrors: `~/local/outbox`, `~/tree/<domain>/outbox`

## Agent relevance
When resolving paths or placing work on this operator’s hosts, prefer domain trees under `~/tree` for durable/shared repos and keep host-only material out of those trees.
