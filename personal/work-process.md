# Work Process

Details regarding my preferred method of doing work with AI agents.

This covers work on and in projects. In other contexts (e.g. check the
weather for me, etc.) disregard this advice.

## New Work

New work is for doing undefined processes with the agent.

Prefer a plan-execute strategy.

Plans can span multiple unit sized chunks. Execution should happen in steps and default to one at a time.

A chunk is up to interpretation but it should be a self-contained and identifiable change. Each step chunk should be committed as a new commit.

Standard instructions for execution are "go" or "execute next step". By default agents only execute one step at a time before returning to the operator for feedback.

Unless instructed to agents should not commit changes. Using a git client with current changes is the operator's main interface for reviewing changes (i.e. `magit` in emacs).

After executing agent should provide in the chat buffer interface and in a planning result document:

- A draft commit message for the operator to make commits with
- commands or actions for the operator to use to test the changes that were made (with brief rationale). This can include setup harnesses for interactive experimentation as well.
- A console formatted output of the test results fro the agent running the tests themselves. These outputs should be truncated for clarity.

The operator may request further changes to the execution or make the edits manually. If the operator commits the changes that should be interpreted as a successful end to the step.

When advancing to the next step after a commit the agent can mark the last one as complete.

If the operator says to advance without committing the agent should give a quick reminder before moving on.


## Predefined Work Processes

Predefined work processes are tasks which may be multi-step but have precedent on how to do them somewhere in context.

This should be written down in repo (see guidance on specifically where this is).

The above guidance on working in small chunks does not apply when specifically doing work processes.

Predefined work processes may or may not be written out as "skills".
