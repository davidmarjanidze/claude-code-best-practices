# Claude Code best practices

A starter kit for working with [Claude Code](https://docs.claude.com/en/docs/claude-code):
a `CLAUDE.md` with workflow rules, a spec-driven `specs/` folder, and an
`implement-spec` skill that drives it.

## What's here

| Path                             | Purpose                                                               |
| -------------------------------- | --------------------------------------------------------------------- |
| `CLAUDE.md`                      | Project memory loaded every session: commands, style, workflow rules. |
| `specs/`                         | Source of truth: principles, roadmap, backlog, progress, specs, ADRs. |
| `.claude/skills/implement-spec/` | Skill Claude loads when you ask it to implement or plan a spec task.  |

Use it in a new project by copying `CLAUDE.md`, `specs/` and `.claude/` into
the project root, then filling in `specs/principles.md` and `specs/roadmap.md`.

## Best practices

### Context: `CLAUDE.md`

- Keep it short and specific. Every line is loaded into every session, so
  include only what Claude can't infer from the code: commands, conventions,
  gotchas, boundaries.
- Write rules as instructions ("Run `npm test` before committing"), not prose.
- Grow it from real corrections: when you fix the same mistake twice, add a
  rule. Prune rules that no longer matter.
- Use nested `CLAUDE.md` files in subdirectories for area-specific rules, and
  `CLAUDE.local.md` (gitignored) for personal preferences.
- Move long, situational procedures into skills so they load only when needed.

### Skills, commands and agents

- **Skills** (`.claude/skills/<name>/SKILL.md`): repeatable procedures. The
  `description` decides when Claude picks the skill up — list the phrases users
  actually say. Keep `SKILL.md` lean and put lookup material in sibling files.
- **Subagents** (`.claude/agents/`): delegate broad searches or independent
  reviews so the main context stays focused.
- **Hooks** (`.claude/settings.json`): for anything that must happen every
  time (format on save, block edits to protected paths). Instructions in
  `CLAUDE.md` are advice; hooks are guaranteed.

### Workflow

- **Explore → plan → code.** Ask Claude to read the relevant code first, then
  use plan mode (Shift+Tab) for non-trivial changes before implementing.
- **Give it a way to check its work**: tests, a typecheck, a linter, a
  screenshot. Claude is far more reliable when it can run the feedback loop
  itself.
- **Be specific.** Name files, functions and the expected behavior; paste the
  error. Vague prompts get generic code.
- **Test first for bugs**: have Claude write a failing test, confirm it fails,
  then fix.
- **Course-correct early.** Press Esc to interrupt, and double-Esc to rewind to
  an earlier point instead of arguing a bad direction forward.
- **Manage context.** Use `/clear` between unrelated tasks and `/compact` on
  long ones. A fresh session with a good handoff (`specs/progress.md`) beats a
  bloated one.
- **Review the diff yourself** before committing; use `/code-review` for a
  second pass.

### Permissions and safety

- Allowlist safe, frequent commands in `.claude/settings.json` (or run
  `/fewer-permission-prompts`) instead of approving them one by one.
- Keep destructive operations (push, deploy, delete) behind a prompt.
- Never put secrets in `CLAUDE.md`, specs, or prompts.

### Parallel work

- Use git worktrees to run several Claude sessions on separate branches without
  conflicts.
- For large changes, split work into independent spec tasks and run them in
  separate sessions.

## Spec-driven development

Ask Claude things like _"implement T-001"_, _"work on the next backlog item"_ or
_"write a spec for password reset"_. The `implement-spec` skill takes over:
it picks the quick or spec track, writes the spec, **stops for your approval**,
then implements task by task and updates `specs/progress.md` and
`specs/changelog.md`. See
[`.claude/skills/implement-spec/SKILL.md`](.claude/skills/implement-spec/SKILL.md).
