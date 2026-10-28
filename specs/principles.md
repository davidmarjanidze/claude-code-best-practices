# Principles

<!--
The stable rules for building this project. Every agent reads this file at the start of every session,
so keep it short and specific. Change it rarely, and record significant changes as ADRs.
-->

## Product

- **What:** <!-- one sentence -->
- **For whom:** <!-- primary users -->
- **Core problem:** <!-- one sentence -->

## Tech stack

| Layer | Choice | ADR |
| ----- | ------ | --- |
|       |        |     |

## Commands

```bash
# install:
# dev:
# test:
# lint:
# build:
```

## Project structure

<!-- Key directories and what belongs where. -->

## Conventions

- **Code style:** <!-- formatter/linter config, notable rules -->
- **Naming:** <!-- files, components, variables -->
- **Branches:** `<type>/T-<id>-<slug>`
- **Commits / PRs:** `<type>(T-<id>): <summary>` (Conventional Commits)

## Quality bar

- **Testing:** <!-- what must be tested, which kinds of tests, coverage expectations -->
- **Performance / accessibility / security:** <!-- concrete thresholds, if any -->

## Definition of Done (project additions)

<!-- Extra items on top of the default in .claude/skills/implement-spec/SKILL.md, e.g. "docs updated", "deployed to staging". -->

-

## Agent boundaries

- **Always:** run tests and lint before marking a task done; follow the active spec.
- **Ask first:** adding dependencies; changing schemas, public APIs, CI or infra; deleting files.
- **Never:** commit secrets; force-push; modify `completed/` specs or accepted ADRs.
