# Summary: Google Developer Documentation Style Guide

Source: https://developers.google.com/style/

## What it is

Editorial style guidelines for clear, consistent technical documentation
aimed at software developers and other technical practitioners. This is
**not** the same as the language [Google Style Guides](https://google.github.io/styleguide/)
([summary](./google-style-guides.md)) used for source code.

## Reference hierarchy

1. Project-specific style (exceptions and product-local terms win).
2. This style guide.
3. Third-party references when still unresolved:
   - Spelling: Merriam-Webster
   - Nontechnical style: *Chicago Manual of Style* (17th ed.)
   - Technical style: Microsoft Writing Style Guide (apply only when it fits)

Clarity and domain consistency may justify deliberate deviations.

## Highlights (most-used defaults)

**Tone and content**

- Conversational and friendly without being frivolous.
- Don't pre-announce unreleased features in docs.
- Use descriptive link text.
- Write accessibly and for a global audience.

**Language and grammar**

- Second person ("you"), active voice.
- Standard American spelling and punctuation.
- Put conditions before instructions.
- Check the [word list](https://developers.google.com/style/word-list) for
  preferred terms.

**Formatting and structure**

- Sentence case for titles and headings.
- Numbered lists for sequences; bullets otherwise; description lists for
  paired data.
- Serial commas.
- Code font for code-related text; bold for UI elements.
- Unambiguous date formats.
- Alt text for images; prefer high-resolution or vector art when practical.

## Topics called out in this repo's exceptions

This repository's contributor-doc rules intentionally relax or skip some
sections. See [Writing Contributor Documentation](../writing-contributing.md):

- [Accessibility](https://developers.google.com/style/accessibility)
- [Inclusive language](https://developers.google.com/style/inclusive-documentation)
- Word list used as baseline, not a closed vocabulary
- Stricter public-doc limits on extra context / disclosure (related pages
  include [excessive claims](https://developers.google.com/style/excessive-claims))

## Agent notes

- Prefer the project summary plus listed exceptions over loading the full
  guide into context.
- Do not confuse this guide with language code style guides under
  `google.github.io/styleguide`.
