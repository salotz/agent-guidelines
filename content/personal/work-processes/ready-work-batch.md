# Ready work batch

A predefined process for churning many **independent**, implementation-ready work-board items.

This is cheaper than a tracker-backed issue system and is meant for a pile of mostly unrelated cards, not for cooperatively building one large thing.

Hub: [Work processes](./).
Board object: [Org](../org.md).
Implementation sessions still follow [New work](./new-work.md) once the operator starts them.

The operator is fast at capturing problem reports, workflow ideas, and project features.
The bottleneck is organizing and starting that pile.
This process is the collect-and-initialize loop.
It does **not** drive every implementation session.

## When to use

The operator asks to complete a bunch of TODOs that have no more design work (or equivalent: “start completing ready todos”, “collect”, “batch ready work”).

Use this when:

- Items live on the org digital work board (`notes/todo/current/`, `notes/todo/backlog/`)
- Each item has a target project (often Bimker, which may hold several unrelated topics)
- Design is done or nearly done; remaining work is initialize + implement

Do not use this for:

- Items still in a design phase (`notes/todo/ideas/`, research threads, open architecture)
- One large cooperative build that needs a single plan
- The agent starting implementation sessions on its own

## Roles

- **Hub session** (this process):
  collect, classify, initialize replicas/plans, keep the board in sync, surface operator attention.
- **Operator**:
  veto items, start implementation sessions in the target replicas, come back to the hub for overall progress.
- **Implementation sessions**:
  one (or a few related) cards in the target replica.
  Follow [New work](./new-work.md).
  Do not implement from the org note.

## Dialogue (happy path)

1. Operator: wants to complete a bunch of TODOs with no more design work.
2. Agent: offers to list items in org `current/` and `backlog/` that are ready for implementation or have only a few blockers, and to initialize work / surface attention. Asks whether to start.
3. Operator: yes, collect.
4. Agent: presents the collection (see [Collection](#collection)). Asks to start initializing.
5. Operator: yes, with optional vetoes (“don’t work on X; it is not ready”).
6. Agent: initializes allowed items (replicas, plans). Reports done. Hands back a checklist.
7. Operator: starts implementation sessions. Returns to this hub to check overall progress.
8. Agent (hub): keeps that checklist in sync with the org board notes.

## Collection

Scan only:

- Org `notes/todo/current/`
- Org `notes/todo/backlog/`

Exclude:

- `ideas/`, `incoming/`, `finished/`, `graveyard/`
- Items still significantly in design (research-only, unlocked architecture, “do not implement from this note” when that means the whole card is design)
- Physical / errand work

**Ready** means: enough locked intent to implement, or only a few named blockers the operator can clear quickly.

For each candidate, resolve the **target** from the note (yerk id, path, or project name).
Do not guess on-disk paths; use yerk (`yerk get`, `yerk path`, `yerk lookup`, `yerk status`).
See the host bootloader `yerk://…` section.

### Status to surface

Present a scannable list (sections or a short status matrix, not a prose table of descriptions).
For each item include title, board path or Denote id, target, and which of these apply:

- Plan in the target repo (path if found)
- Replica created (yerk replica present)
- Partially implemented
- Changes in a tree (often the default replica) that are not spun into a session replica
- Open decisions that still need the operator (`decisions.md` open/proposed)

Group so the operator can veto quickly.
Call out items that need operator attention (open decisions, dirty default replica, missing target).

A target like Bimker often has several independent topics.
Keep those as separate cards / replicas.
Do not fold them into one workspace because they share a repo.

## Initialize (after “start them”)

Honor vetoes.
Do not initialize excluded items.

For each allowed item, only the setup the card still needs:

1. Confirm the target project is catalogued.
   Materialize the main replica if it is missing (`yerk materialize`).
2. Create a session replica when the work should not land on the default checkout (`yerk replica create <project-id> <replica>`).
   Choose a distinguisher from the note or plan name.
   Do not reuse a dirty unrelated tree.
3. Ensure a plan exists in that replica under `.agents/plans/` (or the project’s existing `.agents/plan/`).
   Copy or update from whatever the note already points at.
   Do not invent a second plan if one is named.
4. If the default replica already has uncommitted work for this topic, say so.
   Prefer spinning that into the session replica over mixing topics on `main` / `master`.
5. Update the org board note with replica id, plan path, and init state.
   Do not duplicate the plan body into org.

Bimker remains special: propose replica/plan actions; do not refactor Bimker.
See [Shell and Bimker](../shell-and-bimker.md).

Stop after initialize.
Do not start coding in this hub session unless the operator explicitly asks for one item here.

## Checklist handoff

After initialize, give a checklist the operator can operate:

- Item title and org note
- Target yerk id and replica path
- Plan path
- Remaining operator attention (open decisions, vetoed, blocked)
- What is ready for an implementation session

Tell the operator they start those sessions.
They can return to this hub for overall progress.
The hub agent keeps the checklist aligned with the org notes (and with plan/replica state when re-checked).

## Progress sync

When the operator returns to the hub:

- Re-read the board notes and, as needed, `yerk status` / plan `decisions.md` / checklists in the replicas
- Update the hub checklist and the org notes to match
- Do not silently move notes to `finished/` without operator confirmation

## Anti-patterns

- Implementing every card from the hub session
- Starting implementation sessions without the operator
- Including design-phase or idea-column work
- Mixing several Bimker (or other) topics in one replica because they share a repo
- Treating org as the implementation checkout
- Guessing replica paths instead of asking yerk
- Creating a skill or extra questionnaire files for this process
