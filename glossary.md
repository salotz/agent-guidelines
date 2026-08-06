# Glossary

References between

## project

A self-contained unit of work and content. Usually materialized as specific folder or directory on a [host system](#host-system).

Almost always a [repository](#repository), but not necessarily.

## host system

The computer and operating system for which the primary data sources (e.g. [projects](#project)) and compute are based on.

Simply, the laptop or machine and your operating system as a user you interact with.

## repository

Refers to a specific sub-directory tree which is treated as a self-contained unit of content.

Most commonly a git repository.

## host home

The subtree of a host system which refers to the space for a particular user of the host to use.

Especially, the literal "home" directory on a Unix-like system (like `$HOME` or `~/`).

Distinguised from the system managed directories on your host (i.e. `/` in Unix-likes).

## host local context

Refers to agent context that lives only on a particular host.

For example for my current [host home](#host-home) (e.g. `salotz@mantis`) this would be configuration which is not exclusively tracked remotely. I.e. contents of `~/.*` and `~/.config/*` content.

## remote context

This is context which is fetched from remote sources and is not particular to a specific [host local context](#host-local-context).

Commonly this is context in a [project](#project) [repository](#repository).

Remote context should be generic over hosts and should be assumed to be shared across many hosts and users.
