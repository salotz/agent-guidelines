# Summary: salotz RFC 29 - Glossary Format

Source:
https://github.com/salotz/rfcs/blob/master/rfcs/salotz.029_glossary-format.md

## Purpose

Normative Markdown format for glossaries,
extracted from RFC 22 for reuse in projects, RFCs, and shared guideline repos.

## Format

- File name:
  `glossary.md` unless a consuming standard says otherwise
- Title:
  `# Glossary`
- One term per `## term` subheading (heading text is the canonical term)
- Short definition body under each heading
- Cross-link related terms with in-document anchors (`[other](#other)`)
- Do **not** use a table as the sole glossary structure (anchors/cross-links)
- Aliases:
  mention in the body;
  optional redirect headings if needed
- Order:
  free (alphabetical or thematic)

## Placement (informative)

| Context | Typical path |
|---------|--------------|
| Project design docs (RFC 22) | `design/glossary.md` |
| RFC supporting assets | `rfcs/<nexp>/glossary.md` |
| Shared agent guidelines | `shared/glossary.md` |

## Relationship

- RFC 22 requires project glossaries and defers **format** to RFC 29
- Shared `glossary.md` in this repo follows RFC 29
