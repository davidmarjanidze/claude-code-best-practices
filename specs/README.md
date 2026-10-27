# Specs

`specs/` is the single source of truth for what this project is, what is being
built, and why. Humans and AI agents work from it.

If the code and the specs disagree, one of them is wrong. Fix that before
continuing.

---

## Quick start (new project)

1. Copy this folder into the project root as `specs/`.
2. Fill in `principles.md` (product, stack, commands, conventions, agent
   boundaries) and `roadmap.md` (vision, milestones).
3. Add work items to `backlog.md`.
4. Paste the [agent snippet](#agent-snippet) into the project's `CLAUDE.md` /
   `AGENTS.md`.

---

## Structure

```text
specs/
├── README.md        # The rules (this file)
├── principles.md    # Stable rules: product, stack, commands, conventions, DoD, agent boundaries
├── roadmap.md       # Direction: vision, non-goals, milestones
├── backlog.md       # Prioritized work that has not started
├── progress.md      # What is happening right now + session handoff
├── changelog.md     # What shipped (append-only)
├── active/          # One spec per item being specified or built
│   └── _template.md
├── completed/       # Archived specs (read-only)
└── decisions/       # Architecture Decision Records (append-only)
    └── _template.md
```

Each file holds one kind of information. Link to other files instead of
copying from them.

---

## Lifecycle

```text
Idea
  │
  ├─► roadmap.md              (only if it is strategic / a milestone)
  ▼
backlog.md                    T-012 · Backlog
  │
  ├─ quick track ─► implement ─► changelog.md, remove from backlog
  │
  ▼ spec track
active/T-012-<slug>.md        Draft ─► Approved ─► In progress ─► Done
  │                           (tracked in progress.md while active)
  ▼
completed/T-012-<slug>.md     + changelog.md entry
```

ADRs (`decisions/`) can be written at any stage.

### Statuses

| Status      | Location     | Meaning                                                          |
| ----------- | ------------ | ---------------------------------------------------------------- |
| Backlog     | `backlog.md` | Prioritized, not started.                                        |
| Draft       | `active/`    | Spec being written. No production code yet.                      |
| Approved    | `active/`    | A human approved requirements, design and tasks. Code may start. |
| In progress | `active/`    | Being implemented.                                               |
| Blocked     | `active/`    | Cannot continue. The reason is in `progress.md`.                 |
| Done        | `completed/` | Meets the Definition of Done.                                    |
| Dropped     | `completed/` | Abandoned. The reason is recorded in the spec.                   |

**The Approved gate is the core of the workflow.** Nobody, human or agent, starts
implementing a spec that is still in Draft.

### IDs and naming

- Backlog items get a permanent ID: `T-001`, `T-002`, … Take the next number
  from `Next ID` in `backlog.md` and increment it. Never reuse an ID.
- An item keeps its ID for life: `active/T-012-user-login.md` →
  `completed/T-012-user-login.md`.
- ADRs are numbered separately: `decisions/0001-use-postgres.md`.
- Put the ID in branch names, commits and PR titles, e.g.
  `feat(T-012): add login form`. That links code history back to the spec.

### Two tracks: quick vs. spec

Not every change needs a full spec. Use the **quick track** only when all of
these are true:

- It is small: about a day of work or less, touching a few files.
- It adds no new API, schema, dependency or architectural choice.
- The backlog item has clear acceptance criteria.

Quick track steps: implement directly from the backlog item → verify the
acceptance criteria → add a changelog entry → remove the item from the backlog.

Everything else goes on the **spec track**. If a quick task turns out to be
bigger than expected, stop and write a spec.

---

## File responsibilities

| File             | Answers                         | Update when                                                          |
| ---------------- | ------------------------------- | -------------------------------------------------------------------- |
| `principles.md`  | "How do we build here?"         | Stack, conventions, commands or boundaries change (rarely).          |
| `roadmap.md`     | "Where are we going?"           | Priorities or long-term direction change. No implementation details. |
| `backlog.md`     | "What's next, in what order?"   | Work is found, re-prioritized, started or dropped.                   |
| `progress.md`    | "What is happening right now?"  | Continuously, and at the end of every work session.                  |
| `changelog.md`   | "What shipped, and when?"       | Every completed item. Append-only; never rewrite history.            |
| `active/*.md`    | "What exactly are we building?" | Before and during implementation. Must always match reality.         |
| `completed/*.md` | "Why was it built this way?"    | Only once, when archiving. Read-only afterwards.                     |
| `decisions/*.md` | "Why did we choose X over Y?"   | A significant technical decision is made. Append-only.               |

---

## Writing an active spec

Copy `active/_template.md` to `active/T-<id>-<slug>.md`. Remove the item from
`backlog.md` and add it to `progress.md`.

A spec moves through three phases in one file:

1. **Requirements: what and why.** Write user stories plus numbered, testable
   acceptance criteria. Use EARS
   (`WHEN <trigger> THE SYSTEM SHALL <response>`) or Given/When/Then. List
   non-goals explicitly.
2. **Design: how.** Include only the subsections that apply. Link ADRs instead
   of repeating them.
3. **Tasks: the plan.** Write a checklist of small steps, each one verifiable
   on its own. Each task names the requirements it satisfies (`R1.2`) and how
   to verify it.

Before setting status to **Approved**:

- Every requirement has acceptance criteria.
- Every acceptance criterion is covered by at least one task.
- Open Questions are resolved or explicitly deferred.

### Changing a spec during implementation

- Update the spec **before** you deviate from it, and add a dated line to its
  Progress Log.
- A small design or task change only needs a log entry.
- A change to requirements or scope sends the spec back to **Draft** for
  re-approval.

---

## Completing work

### Definition of Done

This is the default. `principles.md` can add project-specific items.

- [ ] All tasks are checked, and every acceptance criterion is verified (by
      automated tests where practical).
- [ ] Tests, lint and build pass.
- [ ] The spec describes what was actually built.
- [ ] A changelog entry is added under `Unreleased`.

### Archive steps

1. Fill in the spec's **Outcome** section: what shipped, deviations, lessons,
   follow-ups. Follow-ups become new backlog items with new IDs.
2. Set `status: Done` and `completed: <date>`.
3. Move the file from `active/` to `completed/`.
4. Remove the item from `progress.md`.

Completed specs are read-only. Later changes to a shipped feature get a new
backlog item and spec that links to the old one.

---

## Architecture Decision Records

Write an ADR when a decision is significant and costly to reverse: a framework,
library, datastore, protocol, architectural pattern, or a cross-cutting
convention.

- Copy `decisions/_template.md` to `decisions/NNNN-<slug>.md`.
- Use `Proposed` while the decision is under discussion and `Accepted` once it
  is made.
- Never edit an accepted ADR's content. To change direction, write a new ADR
  and set the old one's status to `Superseded by NNNN`.
- Add accepted choices to the stack table in `principles.md`, linked to the ADR.

---

## Releases

`changelog.md` follows [Keep a Changelog](https://keepachangelog.com). Record
finished work under `Unreleased`. On release, rename that section to the version
and date, then start a new `Unreleased` section. Use
[SemVer](https://semver.org) if the project is versioned; otherwise use the date
only.

---

## AI agent rules

**At the start of a session**

1. Read `principles.md` and `progress.md`.
2. Read the active spec for the current item and any ADRs it links.
3. Read `roadmap.md` and `backlog.md` only when planning or choosing work.

**Before writing code**

- The work must trace to an **Approved** spec or a quick-track backlog item. If
  neither exists, propose one. Don't write code.
- If the spec is still in Draft, help complete it, then stop and ask for
  approval.
- If the spec is ambiguous or contradicts the code, add the problem to Open
  Questions and ask. Never guess about anything that changes behavior.

**While implementing**

- Work through the tasks in order. Tick a task only after verifying it.
- Update the spec before you deviate from it.
- Keep `progress.md` current. At the end of a session, overwrite its Handoff
  section: where you stopped, what is half-done, what comes next.
- For a new significant technical choice, draft an ADR as `Proposed` and ask.

**Never**

- Implement scope that isn't in the spec. Add it to the backlog instead.
- Edit `completed/` specs, accepted ADRs (beyond their status line), or past
  changelog entries.
- Mark work done without verifying it.

---

## Agent snippet

Paste into the project's `CLAUDE.md` / `AGENTS.md`:

```markdown
## Specs

This project uses spec-driven development. The rules are in `specs/README.md`;
follow them.

- At the start of every session, read `specs/principles.md` and `specs/progress.md`.
- Only write code for an Approved spec in `specs/active/` or a quick-track item
  in `specs/backlog.md`.
- Keep the spec and `specs/progress.md` in sync with what you actually build.
```

---

## Principles

- The specs are the source of truth, and code follows them. When reality
  changes, update the spec first.
- Write specs for the reader: concise, concrete, testable.
- One fact lives in one place. Link to it instead of copying it.
- History is append-only: changelog, completed specs, ADRs.
- Every piece of work has a traceable lifecycle, from ID to spec to commits to
  changelog.
