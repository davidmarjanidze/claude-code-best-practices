---
id: T-000
title: Short title
status: Draft # Draft | Approved | In progress | Blocked | Done | Dropped
created: YYYY-MM-DD
approved:
completed:
roadmap: # M1 | —
adrs: [] # e.g. [0003]
---

# T-000 · Short title

<!--
Copy to active/T-<id>-<slug>.md. Delete guidance comments and any subsections that don't apply.
Phases: Requirements → Design → Tasks → (human approval) → implementation.
-->

## Overview

<!-- The problem, who has it, and why now. 2–5 sentences. -->

### Goals

-

### Non-goals

-

## Requirements

<!--
Number every requirement and acceptance criterion so tasks, tests and commits can reference them.
Criteria must be observable and testable. Use EARS or Given/When/Then.
-->

### R1 · Requirement name

**Story:** As a <role>, I want <capability>, so that <benefit>.

**Acceptance criteria:**

- [ ] **R1.1** WHEN <trigger> THE SYSTEM SHALL <response>.
- [ ] **R1.2** IF <error condition> THEN THE SYSTEM SHALL <response>.

## Design

### Approach

<!-- The chosen solution in a few paragraphs or a diagram. Significant choices → ADR, linked here. -->

### Affected areas

<!-- Modules, files, services that change. -->

### Data model & migrations

### API / interfaces

### UI / UX

### Security & privacy

### Edge cases & error handling

### Testing strategy

<!-- Which tests prove which acceptance criteria. -->

### Rollout

<!-- Feature flags, migration order, backwards compatibility, rollback plan. -->

### Alternatives considered

## Tasks

<!--
Small, ordered, independently verifiable steps. Each one names the criteria it satisfies and how to verify it.
Tick a box only after verifying it.
-->

- [ ] **1.** Task description. _(R1.1)_ Verify: <test / command / manual check>
- [ ] **2.** Task description. _(R1.2)_ Verify: …

## Dependencies

<!-- Other items (T-…), services, people, or external decisions this depends on. -->

## Open questions

<!-- All resolved or explicitly deferred before status → Approved. -->

- [ ] **Q1** …

## Progress log

<!-- Dated, append-only: deviations from the plan, discoveries, decisions made while building. -->

- YYYY-MM-DD: Spec created.

## Outcome

<!--
Fill in when completing, before moving to completed/:
- What shipped (and what didn't)
- Deviations from the original spec, and why
- Lessons learned / production notes
- Follow-ups: new backlog IDs
-->
