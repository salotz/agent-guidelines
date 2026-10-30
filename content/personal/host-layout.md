# Host Layout

My personal hosts have a specific layout that agents should be aware of and follow.

This adds more constraints to these existing RFCs:

- [salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure) ([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md))
- [salotz RFC 23](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.023_local-agent-context) ([summary](../shared/summaries/salotz-rfc-023-local-agent-context.md))
- [salotz RFC 24](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.024_extended_xdg_base_directory) ([summary](../shared/summaries/salotz-rfc-024-extended-xdg-base-directory.md))
- [salotz RFC 25](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.025_host-domain-organization) ([summary](../shared/summaries/salotz-rfc-025-host-domain-organization.md))
- [salotz RFC 26](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.026_domain-local-configuration) ([summary](../shared/summaries/salotz-rfc-026-domain-local-configuration.md))

Of most importance is [salotz RFC 25](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.025_host-domain-organization) ([summary](../shared/summaries/salotz-rfc-025-host-domain-organization.md)).

Under the `~/tree` directory I have domains for `personal` and `examol`.
Examol is my company I run and can be considered "work".

## Important locations

In each domain I have more or less this layout:

- `admin` or `org`
- `devel`
- `projects`

### Admin Folder

This contains repos which contain generic information,
notes,
and other information that does not fit well into a project.

The most important is **org**: the personal notes silo and digital work board.
Treat it as a standard host object.
Details: [Org](./org.md).
Daily notes live under that project's `notes/journal/` (not a monolithic `notebook.org`).

### Devel

The `devel` folder specifically contains repositories for software projects.
Software projects are grouped together because they typically share local configuration (i.e. [salotz RFC 26](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.026_domain-local-configuration) ([summary](../shared/summaries/salotz-rfc-026-domain-local-configuration.md))).

### Projects

Projects are more free-form than devel.
They typically are tracked by git but may use other mechanisms for maintaining remote repositories.

## Relevance to agents

When working in one repository I may have references to other repositories or locations on the host that I want the agent to be aware of.
For instance I may prompt with something like "Check the other devel project X for a pattern I just used and copy it to this one."

How work is collected and executed on this host: [Work processes](./work-processes/).
