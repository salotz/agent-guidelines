# Writing for humans

Guidelines on how agents should write when communicating with human [operators](./glossary.md#operator).

This covers chat and prompt output,
plans written for operator review,
and other agent-to-human prose that is usually read as plain text rather than as a polished web page.

It shares some preferences with [technical writing](./technical-writing.md),
but the audience and medium differ:
technical writing targets a non-personal external or intra-org audience;
this document targets the operator in the loop.

For contributor-facing project docs, see [Writing Contributor Documentation](./writing-contributing.md).
For Markdown dialect and formatting mechanics, see the [Markdown style guide](./markdown-style-guide.md).

## Default assumptions

- Prefer prose that is easy to scan in a terminal, chat buffer, or plain-text editor.
- Assume the reader sees raw Markdown (or lightly rendered Markdown), not a designed HTML page.
- Keep output terse.
  Operators read much more slowly than agents;
  avoid [too much information (TMI)](./glossary.md#too-much-information-tmi).
- Favor structure that works without rich rendering:
  short sections, lists, and plain headings over dense emphasis and wide tables.

Table guidance that applies especially to human-to-agent and plan prose lives in [Technical writing: When to use tables](./technical-writing.md#when-to-use-tables).

## Abbreviations and initialisms

Agents often compress language with abbreviations and initialisms
(for example "SSOT", "ADR", "TMI").
That habit is convenient for the agent and expensive for the [operator](./glossary.md#operator).

Default stance:
**prefer full wording** in operator-facing chat, plans, and other human-read prose.
Use a short form only when it clearly improves communication
(for example a long proper name that will recur many times in one message,
or a term already established for this session).

### Define before use

If you use an abbreviation or initialism in a session,
**define it on first use** in that session unless it is already in a
**well-known, agreed shared context** with this operator.

Agreed shared context includes, in roughly this order of strength:

1. An entry in the active [glossary](./glossary.md) (or the project's `design/glossary.md`) that this session is expected to follow.
2. A term the operator already used the same way in **this** conversation.
3. Extremely common English or computing words the operator is unlikely to misread in context
   (for example "API", "HTTP", "CLI", "git", "TOML") —
   when unsure, still spell out once.

Do **not** treat "common in agent or industry chat" as agreed shared context.
Many operators do not share that jargon set.

First-use pattern (pick one; keep it light):

- "single source of truth (SSOT)"
- "SSOT — single source of truth — …"

After first definition in the session, the short form may be reused without re-expanding every time,
unless the operator asks for plain language again or the thread is long enough that a refresh helps.

### Prefer not to abbreviate

Even when a short form is defined,
prefer the full phrase when:

- the term appears only once or twice
- the short form is obscure, overloaded, or easy to confuse with another expansion
- the message is already dense with other jargon
- you are summarizing for a human decision (clarity beats compactness)

Write for a careful human reader, not for token budget theater.

### When a short form seems useful: ask, then offer vocabulary

If a recurring abbreviation would genuinely help
(long name, many repetitions, shared docs),
**ask the operator** whether they want that short form in this workstream.

If they agree (or they introduce the short form themselves),
offer to **record it in a vocabulary** and ask **which vocabulary and scope**:

- **Session-only** —
  no durable write; rely on the chat definition (one-off thread).
- **This project** —
  `design/glossary.md` (or the project glossary path per salotz RFC 22 / RFC 29)
  when the term is project domain language.
- **Shared agent guidelines** —
  this repo's [glossary](./glossary.md)
  when the term should be portable across projects for this operator set.
- **Host / personal** —
  personal guidelines or host agent context
  when the term is host- or operator-specific only.

Do not silently invent project- or guidelines-wide abbreviations.
Do not add glossary entries without operator confirmation unless the operator already asked you to capture the term.

When adding an entry, follow [salotz RFC 29](./summaries/salotz-rfc-029-glossary-format.md) glossary format:
one `## term` heading, short definition, cross-links, aliases noted in the body.

### Related

- Shared definitions: [Glossary](./glossary.md)
- Operator attention budget: [too much information (TMI)](./glossary.md#too-much-information-tmi)
- Durable project terms: promote important language out of ephemeral plans into glossaries or design docs (see Planning in [generic agent guidelines](./generic-agent-guidelines.md#planning))

## Patterns to avoid

### Overuse of bold and emphasis

Do not overuse bold and emphasis markers.

Reserve strong emphasis for rare cases where the medium is known to be rendered as a web page and the highlight carries real weight (for example a single warning label).

In default agent output, extra `*` / `**` markup is noise:
it is harder to parse in raw text and rarely improves comprehension.

Prefer plain wording and clear headings over decorating every key phrase.

### Unexplained abbreviations

Do not drop abbreviations or initialisms into operator-facing text without following
[Abbreviations and initialisms](#abbreviations-and-initialisms).
Unexplained short forms are a common source of [TMI](./glossary.md#too-much-information-tmi)-adjacent friction:
not too many words, but too little plain meaning.
