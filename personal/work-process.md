# Work Process

Details regarding my preferred method of doing work with AI agents.

This covers work on and in projects. In other contexts (e.g. check the
weather for me, etc.) disregard this advice.

## New Work

New work is for doing undefined processes with the agent.

Prefer a plan-execute strategy.

Plans can span multiple unit-sized chunks. Execution should happen in
steps and default to one at a time.

A chunk is up to interpretation but it should be a self-contained and
identifiable change. Each step chunk should be committed as a new
commit.

Standard instructions for execution are "go" or "execute next step". By
default agents only execute one step at a time before returning to the
operator for feedback.

Unless instructed to, agents should not commit changes. Using a git
client with current changes is the operator's main interface for
reviewing changes (i.e. `magit` in Emacs).

After executing, the agent should provide in the chat buffer interface
and in a planning result document:

- A draft commit message for the operator to make commits with
- Commands or actions for the operator to use to test the changes that
  were made (with brief rationale). This can include setup harnesses for
  interactive experimentation as well.
- Console-formatted output of the test results from the agent running
  the tests themselves. These outputs should be truncated for clarity.

The operator may request further changes to the execution or make the
edits manually. If the operator commits the changes, that should be
interpreted as a successful end to the step.

When advancing to the next step after a commit, the agent can mark the
last one as complete.

If the operator says to advance without committing, the agent should
give a quick reminder before moving on.

In addition, there are additional guidelines for persisting and
communication channels for this process used in the planning stage.

### Planning Process

Applies when a plan lives under a project's `.agents/plans/`
([salotz RFC 22](https://github.com/salotz/rfcs/tree/master/rfcs/salotz.022_ai-coding-structure)
([summary](../shared/summaries/salotz-rfc-022-ai-coding-structure.md)))
and needs operator choices before or during execution.

Planning is a personal preference and so this organization should not
be applied repo-wide. However, it should be persisted in the repo
state (e.g. committed).

So prefer to make a namespace for your user to place plans under
(i.e. `.agents/plans/<username>`; see
[Personal information](./personal-info.md)).

#### File naming in plan folders

Match the rest of the plan tree: **lowercase**, hyphenated if needed
(`readme` is the usual `README.md` exception for directory indexes).

Examples: `decisions.md`, `checklist.md`, `background/`, owner-level
`todo.md`.
**Do not** use shouty all-caps names like `PROCESS.md` for new plan files.

#### Single inbox for Q&A

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

#### Decision record shape

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

#### Round protocol (agent)

1. Read `decisions.md` for Q&A state.
2. Update Status / Notes **in place**; append a **Changelog** line at the bottom.
3. Keep an **Open queue** table near the top of `decisions.md` (id, status, one-line need).
4. In chat: short checklist of open ids — tell the operator to edit `decisions.md`, not a new file.
5. New topics → next id in the same `decisions.md`.

#### Round protocol (operator)

1. Write under **Answer** (set Status to `locked` if you want).
2. Say “decisions updated” / “go” in chat.
3. Ignore any stray side review file; agent should remove or stub it toward this process.

#### Plan-local process docs

If a plan needs a short pointer to this workflow, use a **lowercase** file
(e.g. `process.md`) that only links here — or link from the plan `README.md`
with no extra file. Prefer not duplicating the full protocol in every plan.

#### Owner plan index (In Progress / Backlog)

Each owner directory (e.g. `.agents/plans/<username>/`) keeps a single
**index** file, usually `todo.md`. It answers only:

1. What plan folders are **in flight**?
2. What work should **become** a plan later (backlog)?

It is **not** an execute queue, checklist, or decision inbox. Those live
inside each plan folder.

##### File shape

Two second-level sections only:

```markdown
# <owner> plans

Optional one-line pointer to this work-process doc.

## In Progress

### <short-name>

One sentence. [link-to-folder/](./folder/)
Optional: minimal soft dependencies (a line or two).

## Backlog

### <short title>

One-sentence executive summary.

Minimal spawn context: enough to create the plan later without opening a
deleted tree. No checklist boxes (`- [ ]`).
```

##### Headings, not tasks

Use `###` items under each section. Do **not** use markdown checklist
elements (`- [ ]`) in the index.

##### In Progress means existing folders

Each In Progress item is a **live** plan directory. Keep it extremely
minimal: one sentence plus a link. Add soft dependencies only when they
help ordering.

##### Backlog means not a plan folder yet

Backlog items are enough to **spawn** a plan later. They are not a shadow
plan and not a second execute queue. **Never** put work that already has a
plan folder under Backlog — that belongs under In Progress.

##### Keep the index focused

Do **not** catalog “existing plans”, “not on this list”, or completed work
in extra sections.

##### Completed plans leave the index

Delete the plan folder when done (git history keeps it). **Before delete:**
put durable outcomes in main docs (`design/architecture/`, README, etc.) and
reflect any decisions worth keeping there — not only inside the plan tree
you are about to remove.

##### Removing from In Progress

The plan folder must be deleted **and** the operator must confirm that docs
and decision landing are adequate. Do not leave “done” ghosts under In
Progress.

##### No third status section

Do not add “Done”, “Blocked”, or “Existing plans” catalogs to this file.
Blocked state belongs in the plan’s own README or checklist.

##### Decisions stay in the plan

Multi-session Q&A lives only in that plan’s `decisions.md` (see above).

##### Moving work to the backlog

When the operator says to **move X to the backlog** (or equivalent):

1. **Add** a Backlog item in the owner `todo.md` with a one-sentence summary
   and **self-contained** spawn context (symptoms, options, soft deps,
   suggested folder name, safety constraints).
2. **Remove X from the source plan** — delete phase files, checklist
   sections, phase-order rows, and map entries. Do **not** leave “parked
   phase” stubs, “spawn later from phase N” links, or execute-queue ghosts.
3. **Decisions:** keep locked history if useful, but reword so the choice is
   “out of this plan / see owner backlog,” **without** links to deleted phase
   paths. Changelog the move.
4. **Cross-refs** inside the source plan (out-of-scope lists, risks): point
   at the **backlog by name**, not at removed files.
5. Backlog text must **survive deletion** of the originating plan: name the
   origin plan if helpful; **do not** depend on paths inside it
   (`phases/0N-….md`, decision anchors in that tree, etc.).

Parked-inside-the-same-plan is the **wrong** shape once the operator has
asked for backlog. Backlog owns the deferred work; the active plan’s
execute queue only has work still in scope.

##### Backlog item quality

A backlog entry should let a future session spawn a plan **without** the
old plan folder:

- One-sentence executive summary (why a separate plan)
- Origin by **name** only if useful (“came out of tooling-cleanup”)
- Concrete symptom / goal / constraints copied in full
- Approach options or open choices (if already known)
- Soft dependencies stated in plain language (not links into In Progress
  trees that will disappear)
- Suggested folder name; safety (preview-only, no deploy, etc.)

Avoid:

- Links to files under an In Progress plan that will be deleted when that
  plan completes
- “See phase 6 for details” with no details inlined
- Checklists or mini phase lists that belong in a real plan after spawn

##### Lifecycle

```text
Backlog item  --spawn-->  plan folder + In Progress line
                                │
                         execute (this work-process)
                                │
                    outcomes → architecture / project docs
                                │
              delete plan folder + drop In Progress line

In-plan work  --operator: move to backlog-->  remove from plan + Backlog item
```

##### Anti-patterns

- Listing every phase of an active plan in the index
- Duplicating locked decisions from `decisions.md` into the index
- Keeping finished plans “for reference” under In Progress
- Putting backlog items that already have folders under Backlog
- Leaving parked phases inside a plan after “move to backlog”
- Backlog entries that only link into another plan’s tree
- Side `review-*.md` questionnaires at the owner level
- Extra index sections (“Done”, “Not on this list”, “Existing plans”)


## Predefined Work Processes

Predefined work processes are tasks which may be multi-step but have
precedent on how to do them somewhere in context.

This should be written down in the repo (see guidance on specifically
where this is).

The above guidance on working in small chunks does not apply when
specifically doing work processes.

Predefined work processes may or may not be written out as "skills".
