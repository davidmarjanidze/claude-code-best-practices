# Project Overview

# Commands

```bash
npm run prettier   # format everything
```

# Code Style

# Specs

This project uses spec-driven development. The rules live in the
`implement-spec` skill (`.claude/skills/implement-spec/`); use it for any spec
or backlog work.

- At the start of every session, read `specs/principles.md` and `specs/progress.md`.
- Only write code for an Approved spec in `specs/active/` or a quick-track item
  in `specs/backlog.md`.
- Keep the spec and `specs/progress.md` in sync with what you actually build.

# Subagents

Project agents live in `.claude/agents/`. Delegate when the process is large but the result is small; do targeted or context-dependent work inline.

- `oss-researcher`: before implementing an unfamiliar feature (see Workflow).
- `code-reviewer`: after finishing a change, before committing.
- `test-runner`: to run tests/lint/format checks without flooding context.

# Workflow

- For unfamiliar features, don't write from scratch. First search GitHub for open-source implementations, compare approaches, then pick one and start. Summarize which approach you chose and why before implementing.
- Explore before editing: read the relevant files and existing patterns first. Reuse existing helpers instead of adding near-duplicates.
- For anything touching more than a few files, write a short plan (or use plan mode) and get agreement before coding.
- Write or update a failing test first when fixing a bug, so the fix is proven.
- Never claim something works without running it. If you couldn't verify, say so explicitly.
- Fix root causes, not symptoms. Don't silence errors, skip tests, or loosen types to make a check pass.
- Keep diffs focused on the task. No drive-by refactors, renames, or reformatting of untouched code — note them as follow-ups instead.
- Ask before: adding dependencies, changing public APIs/schemas/CI, deleting files, or any git push / force operation.
- When stuck after two failed attempts at the same approach, stop, explain what you tried, and propose alternatives rather than looping.
- Commit in logical units with Conventional Commit messages; never commit secrets or generated artifacts.
- When a correction from the user is likely to recur, propose adding it to this file so it sticks.
