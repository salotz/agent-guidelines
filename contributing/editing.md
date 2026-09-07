# Editing

As an editor you are responsible for checking the work of the author.

## Check links

All links should be checked that they are valid.

## Spelling and grammar

Spelling and grammar should be fixed where obvious.
Larger changes should request input from the author.

## Consistency

Is the usage of terms and guidelines consistent with each other?

Are there terms that should be defined in the [shared glossary](../shared/glossary.md)?
New or edited glossary entries must follow [salotz RFC 29](https://github.com/salotz/rfcs/blob/master/rfcs/salotz.029_glossary-format.md) ([summary](../shared/summaries/salotz-rfc-029-glossary-format.md)).

Is portable guidance under `shared/` and host-specific guidance under `personal/`?

Is **human operator** guidance (sandboxing how-to,
host hardening patterns) under `operator/`,
with a local table of contents —
not mixed into agent bootloaders?

Do new cross-cutting shared topics get their own `shared/*.md` file plus a thin hub link (for example from `generic-agent-guidelines.md` and `README.md`), instead of bloating the generic hub?

Do new operator topics get a sibling `operator/*.md` plus links from `operator/README.md` and the root `README.md`?
