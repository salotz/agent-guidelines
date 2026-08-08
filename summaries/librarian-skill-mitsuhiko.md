# Summary: librarian skill (mitsuhiko/agent-stuff)

Source: https://github.com/mitsuhiko/agent-stuff/tree/main/skills/librarian

## Purpose (from context)
A skill for caching external repositories locally to avoid excessive network calls when loading context for agents.

## Key Idea
When agents reference external resources (e.g. other repos or standards), prefer making locally cached copies.

This aligns with "Remote Resource Caching" guidance: agents should prefer local cached copies of referenced material.

See the original skill for implementation details if needed for building similar caching behavior.