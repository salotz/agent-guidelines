# Writing Contributor Documentation

Guidelines for documentation aimed at project contributors (for example
material under `contributing/`).

For writing aimed at a non-personal audience more generally, see
[Technical Writing](./technical-writing.md).

## Style guide

Follow the
[Google developer documentation style guide](https://developers.google.com/style/)
([summary](./summaries/google-developer-documentation-style-guide.md))
with these exceptions:

- Do not follow the accessibility guidelines strictly. More accessible
  documentation is good, but it should not make the final product more
  complicated than necessary.
- Do not follow the guidelines on inclusive language.
- Use the word list as a baseline, not as a strict language subset.
- Developer documentation within a project does not need such strict
  rules around excessive disclosure. The audience is relatively
  restricted, so extra context is often warranted, whereas that style
  guide usually forbids it.

## Don't recapitulate upstream documentation

When writing documentation, don't restate what upstream documentation
already tells a user.

Focus on project-specific choices and design of the tool.

If extra reminders or summaries are needed, keep them in agent-specific
context files.

Keep documents as terse and readable as possible. Humans read much more
slowly than agents, so avoid
[too much information (TMI)](./glossary.md#too-much-information-tmi).
