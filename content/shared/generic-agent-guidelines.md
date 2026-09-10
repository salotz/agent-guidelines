# Generic Agent Guidelines

Generic advice for any agent-assisted work.

## RFC and Standard References

This repo is focused specifically on guidelines that do not have a practical means for more strict standardization.

Guidelines here, however, should make use of such standards through reference and compacted “inlining”.

Sources of standards can be official standards bodies like the [Agentic AI Foundation (AAIF)](https://aaif.io/) ([summary](./summaries/aaif.md)),
community or organizational standards (e.g. language [Google Style Guides](https://google.github.io/styleguide/) ([summary](./summaries/google-style-guides.md)),
or the [Google developer documentation style guide](https://developers.google.com/style/) ([summary](./summaries/google-developer-documentation-style-guide.md))), or personal standards (e.g. [salotz RFCs](https://github.com/salotz/rfcs) ([summary](./summaries/salotz-rfcs-overview.md))).

Recommendations in this repo should explicitly reference these standards with extensions or modifications.

## Project Layout

When working on projects, agents should obey [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](./summaries/salotz-rfc-022-ai-coding-structure.md)).

## Project Management and Tooling

For in-repo tooling contracts, content-focused config files,
layered tool roles,
[project bootstrapping](./glossary.md#project-bootstrapping) (`.bootstrap/`),
[preload](./glossary.md#preload),
and project automation vs operator shell activation, see [Project Management and Tooling](./project-management-and-tooling.md).

Host installs still require operator confirmation and integration planning;
see that document and [Host System Interaction](#host-system-interaction).
Respect bootstrap → project install → preload order when a project defines those stages.

## Blob Management

For large or opaque repository files that need tooling beyond normal VCS operation, see [Blob Management](./blob-management.md).

## Software

For authorship and maintenance of software projects (comments, commits, branching, tests), see [Software Guidelines](./software-guidelines.md).

## Writing

For Markdown dialect and formatting rules, see [Markdown style guide](./markdown-style-guide.md).

For technical writing aimed at a non-personal audience, see [Technical Writing](./technical-writing.md).

For contributor-facing documentation (style-guide baseline and exceptions,
avoiding upstream restatement), see [Writing Contributor Documentation](./writing-contributing.md).

## Research

For research work (as distinct from production software and external technical writing), see [Research](./research.md).

## Glossary

Shared term definitions live in the [Glossary](./glossary.md).
Prefer glossary terms (for example [operator](./glossary.md#operator), [host system](./glossary.md#host-system), [host local context](./glossary.md#host-local-context), [remote context](./glossary.md#remote-context)) over ad-hoc synonyms.

Glossary **structure** follows [salotz RFC 29](https://github.com/salotz/rfcs/blob/master/rfcs/salotz.029_glossary-format.md) ([summary](./summaries/salotz-rfc-029-glossary-format.md)):
`# Glossary`,
one `## term` subheading per entry,
cross-links between terms (not a table-as-glossary).
When adding or editing entries, keep that format.

## Host System Interaction

When interacting with [host local context](./glossary.md#host-local-context), agents should obey [salotz RFC 23](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](./summaries/salotz-rfc-023-local-agent-context.md)) for loading operator-defined context and [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](./summaries/salotz-rfc-024-extended-xdg-base-directory.md)) for agent-generated content.

For operator-specific host paths and configuration procedures,
see personal guidelines if they are loaded in this session (for example [Shell Configuration and Bimker](../personal/shell-and-bimker.md) in this repository).

## Remote Resource Caching

When agents refer to external resources like repositories as part of context loading,
they should prefer making locally cached copies of them.

Inspired by [this skill](https://github.com/mitsuhiko/agent-stuff/tree/main/skills/librarian) ([summary](./summaries/librarian-skill-mitsuhiko.md)).

Also follow the guidelines for [host system interaction](#host-system-interaction).

## Compacted Inlining

When writing guidelines (such as these documents),
if reference is made to an extensive external standard or guideline it is good practice to provide a compacted version of that resource “inline” with this project.

For instance, if you refer to and request adherence to a particular style guide,
you should provide a compacted summary of that style guide for immediate loading into context.

Agents should first check for compacted versions of resources before fetching and reading the larger ones.
If additional context is needed for specifics from larger standards,
this can be done incrementally and ideally in sub-agents.

## Tool Preference

### Prefer task-specific tools over shell

Whenever possible, agents should prefer specific tools over use of a generic “shell” tool.

This prevents unnecessary privilege escalation:
a generic shell tool can do anything on a system.
By using specific tools, the human (or other agents) interacting with an agent can more easily understand both the intention of the agent and audit potentially dangerous, disruptive, or risky shell calls.

For example, if agents want only to read a particular file,
they should use a specific “read” tool or agent harness extension over `cat myfile.txt`.

Coding agents should also provide useful help and guidance to their [operator](./glossary.md#operator) when they are utilizing broader-capability tools like shell but could use more constrained tools.
For example, if the operator has not installed the necessary tools for an agent to follow this behavior (e.g. a `read-file` tool or extension),
the agent should make this known to the operator and suggest solutions for installing more fine-grained tools.

## Naming Things

When naming things (especially files, folders,
and host paths),
follow the naming and layout guidance in project structure and host-directory RFCs:

- [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](./summaries/salotz-rfc-022-ai-coding-structure.md)) for in-repo layout (including name-expression guidance referenced there)
- [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](./summaries/salotz-rfc-024-extended-xdg-base-directory.md)) for extended XDG host paths

## Reading the Web

When possible, find a non-HTML source to read on the web, as it is much more context-efficient.

This includes the markdown sources for a website (e.g. on GitHub) or using the [llms.txt](https://llmstxt.org/) ([summary](./summaries/llms-txt-standard.md)) standard when available.

## Planning

A common pattern in agentic coding is to first have an agent make a plan and then have other agents execute it.

These plan documents need to be saved as memory somewhere.
It is preferable to have this memory saved in-repo as [remote context](./glossary.md#remote-context),
since working on plans and executing them may span sessions and hosts, and may be shared with other operators.

The location to store these is the project-relative path `.agents/plans` (according to [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure)).

It is up to agents to organize these into individual planning sessions and to remove them once plans are complete.

**Plans are ephemeral coordination artifacts**, not product documentation.
Do **not** cite plan-local Q&A ids (`Q1`, …),
plan folder paths,
or `decisions.md` anchors in source code,
tests,
Makefiles, CI config, or other long-lived docs.
When a choice must outlive the plan, promote it to an **ADR** under `design/decisions/` (RFC 22) and/or architecture docs, then reference those.
Full Q&A protocol:
personal [`work-process.md`](../personal/work-process.md) (*Plans are ephemeral*), when that personal guidance is loaded.

## Project READMEs

Projects use both human-facing `README` files (typically `README.md`) and agent-specific `AGENTS.md`.

Agents should read `README` as well.
When writing `README.md` files, exclude content that is more optimized for agents and keep that in `AGENTS.md`.

For instance, repositories have some typical layout with meta-information (`design`, `.agents`).
These can be referenced in a README but should not be the primary content.

Favor referencing the material in `contributing/` for users in the README,
which then branches out to details about what the `design` folder and similar are meant for.

Focus on how a user coming into a project would want to be directed.

- Are you trying only to understand what the project is?
  Provide a summary of the project, with references to documentation if available.
- Are you a consumer of this project?
  Provide instructions on installing or accessing the software.
- Do you want to contribute to this project?
  Point to the contributing instructions.

## Agent-Oriented Document Format and Style

As described elsewhere, context for agents and humans is written into documents in projects.

This section sets some of the standards for those documents.

### Format

There are many different kinds of documents for different purposes.

For documents that are meant primarily for human-to-agent (H2A) and human-to-human-via-plaintext (H2H-plain) communication, they should use Markdown.

See the [Markdown style guide](./markdown-style-guide.md) for Markdown dialect, wrapping, fences, tables, and related rules.

See [Writing for humans](./writing-for-humans.md) for operator-facing chat, plans, and plain-text-first output habits.

See the [Technical writing guidelines](./technical-writing.md) for broader writing preferences that partially apply to agent communication.
