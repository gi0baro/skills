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
- **Agents** (*overridable*): the default escalation list.
- **Concurrency**: a cap per agent, and optionally a total.
- **Isolation** (*overridable*): `none` (shared tree), `worktree` or
  `clone`, and where the directories go.
- **Merge-back** (*overridable*): how isolated work comes back, merged by
  the exec session or left on its branch for the maintainer.

### Units

Each unit has an id and:

- **Goal**: what the unit delivers.
- **Boundary**: the files and behaviours in scope.
- **Agents**: the escalation list, when it differs from the default.
- **Depends on**: the units that must pass first. Units with no dependency
  between them may run in parallel.
- **Acceptance**: how the exec session checks the result.

An escalation list is an ordered list of agent names (pidrei agent files)
with their attempts: `local-qwen x3, sonnet x1, fable x1`.

## Writing rules

- Acceptance is decidable without the maintainer: a command to run or a
  concrete criterion, never "looks right".
- Check at writing time that every agent named exists as an agent file, and
  that the files and code the units start from are as the plan describes.
