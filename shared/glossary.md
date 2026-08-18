# Glossary

This glossary defines key terms and provides cross-references between them.

## operator

The human person that is controlling an agentic aided process.

## project

A self-contained unit of work and content. Usually materialized as specific folder or directory on a [host system](#host-system).

Almost always a [repository](#repository), but not necessarily.

## host system

The computer and operating system for which the primary data sources (e.g. [projects](#project)) and compute are based on.

Simply, the laptop or machine and your operating system as an [operator](#operator) you interact with.

## repository

Refers to a specific sub-directory tree which is treated as a self-contained unit of content.

Most commonly a git repository.

## host home

The subtree of a host system which refers to the space for a particular [operator](#operator) of the host to use.

Especially, the literal "home" directory on a Unix-like system (like `$HOME` or `~/`).

Distinguished from the system managed directories on your host (i.e. `/` in Unix-likes).

## host local context

Refers to agent context that lives only on a particular host.

For example, for a given [host home](#host-home) this would be configuration which is not exclusively tracked remotely. I.e. contents of `~/.*` and `~/.config/*` content.

## remote context

This is context which is fetched from remote sources and is not particular to a specific [host local context](#host-local-context).

Commonly this is context in a [project](#project) [repository](#repository).

Remote context should be generic over hosts and should be assumed to be shared across many hosts and [operators](#operator).

## Integration Environment

A computational environment which provides all systems put together to mimic a production environment.

## Integration Tests

Tests that run in an integration environment.

## codetag

A specially formatted comment tag (e.g. TODO, FIXME) placed in source code to add machine-searchable semantic meaning beyond freeform comments. Defined by [salotz RFC 6](./summaries/salotz-rfc-006-codetags.md).

## Project tooling entrypoint

The documented command used by humans, agents, and CI to run project tools with pinned versions and env (for example `mise exec -- …`), without requiring interactive shell activation. See [Project Management and Tooling](./project-management-and-tooling.md).

## task runner

The project layer that exposes named recipes (the **menu and wiring**) for
common work: run a CLI with fixed flags, chain a few one-liners, or depend on
other recipes. Examples: mise tasks, Make, just.

Actual task running tools often are multifunctional, including build
systems (e.g. `make`), package or tool managers, etc. We refer to them
as task runners only with reference to those capabilities.

## task

One named unit of work in a [task runner](#task-runner) (for example
`mise run validate`, a Make target, or a just recipe).

## blob

Abbreviation for "binary large object". Colloquially, any file that is too
large or opaque to handle well with a code VCS tool like git. See
[Blob Management](./blob-management.md).


## Too much information (TMI)

An observation that something is too verbose and providing information or context that is irrelevant in the current context.

An operator may simply express "TMI" to an agent and the agent should then attempt to be more focused.

This doesn't mean leave out important points just to cut word counts, but to optimize for limited attention.
