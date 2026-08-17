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

## Multi-session plans and decision back-and-forth

Applies when a plan lives under a project’s `.agents/plans/` (RFC 22) and
needs operator choices before or during execution.

### File naming in plan folders

Match the rest of the plan tree: **lowercase**, hyphenated if needed
(`readme` is the usual `README.md` exception for directory indexes).

Examples: `decisions.md`, `checklist.md`, `background/`.  
**Do not** use shouty all-caps names like `PROCESS.md` for new plan files.

### Single inbox for Q&A

| Place | Role |
| --- | --- |
| **`decisions.md`** in the plan folder | **Only** place for prompts, operator **Answer**s, status, short notes |
| `background/` | Optional long research; **no** “answer here” |
| Plan `README.md` | Index, phase order, link to open-queue — not a second questionnaire |
| Chat | Short ping only (“need A3 in decisions.md”) — not the answer archive |

**Do not** create side questionnaires for the operator to fill in
(`review-*.md`, `follow-ups-*.md`, `questions-*.md`, dated review dumps that
mix research + new prompts). If research is long, put it under `background/`
and link from the decision id.

### Decision record shape

Stable ids (`A1`, `A2`, …). In `decisions.md`:

```markdown
## A3 — short title

Status: open | proposed | locked

### Prompt
What we need chosen (options if any).

### Context
(optional) short, or link to background/….md

### Answer
<!-- operator writes here -->

### Notes
(optional) agent lock rationale — not a new question list
```

- **open** — needs operator text under Answer  
- **proposed** — agent drafted a default; operator accepts or edits Answer  
- **locked** — implement against this; change only deliberately  

### Round protocol (agent)

1. Read `decisions.md` for Q&A state.  
2. Update Status / Notes **in place**; append a **Changelog** line at the bottom.  
3. Keep an **Open queue** table near the top of `decisions.md` (id, status, one-line need).  
4. In chat: short checklist of open ids — tell the operator to edit `decisions.md`, not a new file.  
5. New topics → next id in the same `decisions.md`.

### Round protocol (operator)

1. Write under **Answer** (set Status to `locked` if you want).  
2. Say “decisions updated” / “go” in chat.  
3. Ignore any stray side review file; agent should remove or stub it toward this process.

### Plan-local process docs

If a plan needs a short pointer to this workflow, use a **lowercase** file
(e.g. `process.md`) that only links here — or link from the plan `README.md`
with no extra file. Prefer not duplicating the full protocol in every plan.
