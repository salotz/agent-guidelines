# Summary: llms.txt Standard

Source: https://llmstxt.org/

## Purpose
Proposal to standardize a `/llms.txt` markdown file at the root of websites (or subpaths) to provide concise, LLM-friendly information for use at inference time.

## Background
- Full websites are hard for LLMs: large HTML, navigation, ads, JS.
- Context windows are limited.
- Need curated, expert-level, plain-text content in one place.

## Core Proposal
- Add a `/llms.txt` file containing:
  - H1 with project/site name (required)
  - Optional blockquote summary
  - Detailed sections (no extra headings at top level)
  - H2 sections with lists of links to detailed content
- For pages with useful info, also provide a clean `.md` version at the same URL + `.md` (or `index.html.md` for directories).

## Format
Example structure:
```markdown
# Title

> Optional short description

Some details here.

## Section

- [Link title](https://url): Optional notes

## Optional

- [Secondary link](https://url)
```

The special `## Optional` section can be skipped for shorter contexts.

## Relationship to Other Standards
- Complements (does not replace) `robots.txt` and `sitemap.xml`.
- Focused on *inference* (helping LLMs use the site now), not primarily training.
- Provides curated, LLM-optimized views rather than exhaustive lists.

## Usage Guidance (for agents)
- When reading web content, prefer non-HTML sources (e.g. GitHub raw markdown) or `llms.txt` + linked `.md` files when available.
- This is much more context-efficient.

## Integrations & Tools
- Various plugins and converters exist (CLI, VitePress, Docusaurus, etc.).
- Directories listing sites that provide `llms.txt` are available.

See the site for full spec, examples (including FastHTML), and community resources.