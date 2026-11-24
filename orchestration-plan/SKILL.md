---
name: orchestration-plan
description: Write an execution plan for an autonomous orchestration-exec session.
disable-model-invocation: true
---

# Orchestration plan

The plan is the only input of an `orchestration-exec` session, which runs it
unattended and stops only on a hard stop. Whatever the plan leaves unclear
becomes a guess or a stop: write it to be executed without the maintainer.

## Format

A Markdown file with three sections: Context, Rules, Units.

### Context

- The overall goal.
- The docs and files the exec session reads before starting.

### Rules

Session-wide. The items marked *overridable* can be overridden per unit.

- **Git**: what may be committed, where, and when.
- **Commands**: lint, tests and any other checks.
- **Skills**: what the orchestrator and the subagents load.
- **Agents** (*overridable*): the default lanes, and optionally a time cap per
  attempt.
- **Decisions**: what the exec session may decide on its own when the plan
  doesn't cover something, which agent gives a second opinion when it is
  unsure, and what always stops for the maintainer.
- **Review**: whether a unit is reviewed after its acceptance, by which agent,
  and which findings block.
- **Brief clauses** (optional): standing text every brief carries.
- **Concurrency**: a cap per agent, and optionally a total.
- **Isolation** (*overridable*): `none` (shared tree), `worktree` or
  `clone`, and where the directories go.
- **Merge-back** (*overridable*): how isolated work comes back, merged by
  the exec session or left on its branch for the maintainer.

### Units

Each unit has an id and:

- **Goal**: what the unit delivers.
- **Boundary**: the files and behaviours in scope.
- **Agents**: the lanes, when they differ from the default.
- **Depends on**: the units that must pass first. Units with no dependency
  between them may run in parallel.
- **Acceptance**: how the exec session checks the result.

A lane is an agent name (a pidrei agent file) with its attempts: `sonnet x2`.
Lanes separated by commas are tried in that order; lanes separated by `|` are
alternatives the exec session picks from, each keeping its own attempt count:
`local-qwen x2, sonnet x2 | fable x2`.

## Writing rules

- Acceptance is decidable without the maintainer: a command to run or a
  concrete criterion, never "looks right".
- For each acceptance, name a wrong result that would still pass: close the
  gap, or list it as left for the maintainer.
- The plan carries decisions, not open questions: anything with more than one
  reasonable reading is settled with the maintainer before the plan is
  written.
- Check at writing time that every agent named exists as an agent file, and
  that every fact the plan states holds: the files and code the units start
  from, and any reference an acceptance compares against. For a reference,
  say which side is wrong when the two disagree.
- A fact that only work can establish is the deliverable of a unit, and the
  units relying on it depend on that unit.
