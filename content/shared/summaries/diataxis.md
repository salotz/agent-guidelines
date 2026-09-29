# Summary: Diátaxis

- **Canonical**:
  [https://diataxis.fr/](https://diataxis.fr/)
- **Author**:
  Daniele Procida
- **Also known as**:
  Divio documentation system
  ([historical write-up](https://docs.divio.com/documentation-system/))
- **Fetched**:
  2026-09-28

## What it is

A systematic approach to technical documentation **content**, **style**, and **architecture**.

Diátaxis (Ancient Greek: *dia* “across” + *taxis* “arrangement”) starts from **user needs**, not from the product’s internal structure or the author’s outline habits.

It is lightweight, tool-agnostic, and widely adopted (for example internal and open-source docs at organizations that cite it as an IA north star).

## The four types

| Type | Orientation | Serves |
| --- | --- | --- |
| **Tutorials** | learning | acquire skill through a guided lesson |
| **How-to guides** | task / goal | accomplish a concrete job |
| **Reference** | information | look up accurate description (APIs, facts, contracts) |
| **Explanation** | understanding | grasp why, context, and design |

### Tutorials

- Learning-oriented; beginner succeeds by following a path.
- Controlled lesson; learning by doing.
- Avoid option overload, side quests, and comprehensive coverage.

### How-to guides

- Goal-oriented directions for someone who already knows the basics.
- Named problems and practical steps.
- Not a first lesson; not an exhaustive catalog.

### Reference

- Dry, consistent, complete-enough description of the system as it is.
- Optimized for lookup and correctness.
- Structure often mirrors the product (commands, types, config keys).

### Explanation

- Clarifies concepts, rationale, and connections.
- Discursive where needed; still distinct from procedure and from pure reference.
- Answers “why” and “how should I think about this?”

## Application ideas (from the framework)

- Organize docs around the four needs; use navigation and page purpose that make the type obvious.
- Prefer splitting mixed pages over maintaining “one doc that does everything.”
- Improve iteratively: Diátaxis is often applied by refactoring existing docs toward clearer types rather than a big-bang rewrite.
- Quality comes from fitness to need (right type, right form), not only from prose polish.

## In this repository

Agent-facing application for software projects:
[Software Documentation](../software/documentation.md).

Editorial style remains under [Technical Writing](../technical-writing.md) and related writing guides.
