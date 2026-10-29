# Part 1: Price optimization

Keep this file under 200 lines. It is sent on every turn.
Task-specific procedures belong in `.claude/skills/`.

## Commands

```bash
./claude-code/scripts/install.sh      # once per clone: git hooks + jq check
./claude-code/scripts/cost-audit.sh   # what this repo adds to every request
./claude-code/scripts/test-hooks.sh   # smoke-test the Claude Code hooks
```

## Conventions

- Keep every source file under 5,000 tokens (~20 KB). A pre-commit hook enforces this.
- Prefer small, single-purpose modules over large files.
- Prefer CLIs (`gh`, `aws`, `supabase`, `netlify`) over MCP servers.

## Workflows (loaded on demand as skills)

- Reviewing a pull request: use the `pr-review` skill.
- Writing or running database/schema migrations: use the `migrations` skill.
- Ending or pausing a long task: use the `handoff` skill.

## Cost rules for agents

- Use Explore/Plan subagents for searching and planning (they run on Haiku).
- Run tests, lint, and builds through the `test-runner` subagent when the
  output may be long.
- Grep before you Read. Read only the line ranges you need (offset/limit);
  a hook blocks full reads of files over ~10,000 tokens.
- Do not paste whole files or full command output into the conversation.
- Test/lint/build output is filtered to errors automatically; append `# raw`
  to a command only if the filtered output is not enough.
- Keep replies short: no restating the plan, no summaries of unchanged code.
- Never edit lockfiles or generated code by hand.

# Part 2: Other best practices

## Project Overview

## Commands

```bash
npm run prettier   # format everything
```

## Code Style

## Specs

This project uses spec-driven development. The rules live in the
`implement-spec` skill (`.claude/skills/implement-spec/`); use it for any spec
or backlog work.

- At the start of every session, read `specs/principles.md` and `specs/progress.md`.
- Only write code for an Approved spec in `specs/active/` or a quick-track item
  in `specs/backlog.md`.
- Keep the spec and `specs/progress.md` in sync with what you actually build.

## Subagents

Project agents live in `.claude/agents/`. Delegate when the process is large but the result is small; do targeted or context-dependent work inline.

- `oss-researcher`: before implementing an unfamiliar feature (see Workflow).
- `code-reviewer`: after finishing a change, before committing.
- `test-runner`: to run tests/lint/format checks without flooding context.

## Workflow

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
- Always keep `README.md` and `CLAUDE.md` split into two parts: Part 1 price optimization, Part 2 other best practices. Put new content in the matching part.
