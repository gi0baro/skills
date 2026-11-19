---
name: orchestration-exec
description: Execute an orchestration plan autonomously, delegating the work to subagents.
disable-model-invocation: true
---

# Orchestration exec

This session executes a plan written beforehand in the
`orchestration-plan` format. It doesn't plan or redesign: it runs the units
and delegates the work to subagents, and stops when reality and the plan
diverge.

## Start

- Ask the maintainer for the plan file(s) before anything else.
- Read the plan and the docs in its Context, then validate it: Context,
  Rules and Units are present; every unit has goal, boundary, depends on,
  acceptance, and an escalation list (its own or the default); every agent
  named is available; dependencies refer to existing units and have no
  cycles. Any failure is a hard stop.
- If the plan already has a log, resume from it (see Log).

## Running units

- A unit starts when the units it depends on have passed and its agent has
  a free slot under the concurrency caps; until then it waits.
- Write each brief from the unit: goal, boundary, acceptance, what the
  units it depends on produced, and the report format. The brief never
  extends or contradicts the unit.
- Run the unit in its isolation, through the `subagent` tool's `cwd`.
- Ask for a short report in a fixed format: files changed, how acceptance
  was checked, anything off-plan.
- On a result, check acceptance yourself. If it passes, merge back as the
  Rules say and mark the unit done. If it fails, re-brief with the failure
  attached; move to the next agent in the escalation list once the current
  one's attempts are used. A cancelled job counts as a failed attempt.
- Parallel units that overlap (same files, shared state) and merge
  conflicts are judged case by case: recover if you can do it cleanly,
  stop otherwise.

## Monitoring

- Completions arrive on their own when the session is idle. Never wait with
  `subagent_result`.
- While any job runs, keep a `timer` armed. There is one timer per session
  and setting one replaces it: one timer covers all running jobs.
- On each wake-up, check every job with `subagent_status`. A job that made
  no progress since the last check gets steered (`subagent_steer`); still
  stuck at the next check, it gets cancelled (`subagent_cancel`). A job
  near its context limit gets steered to wrap up and report. Then re-arm
  the timer.

## Context

The session has to last the whole plan; keep its own context minimal.

- Don't read subagent work in full: check it with commands and targeted
  reads.
- State lives in the log, not in the conversation.

## Log

- Keep `<plan name>.log.md` next to the plan, updated on every event: unit
  state changes, each attempt (agent, outcome), every judgement call with
  its reason, stops.
- Jobs don't survive quit or reload. When resuming, units logged as running
  were interrupted: check what they left before running them again.

## Hard stops

- The plan fails validation at start.
- Reality doesn't match the plan: files or code differ from what it
  assumes, or a unit can't be done within its boundary.
- A unit's escalation list is used up.
- The tooling misbehaves: tool calls keep failing, harness errors,
  subagents crashing.
- A decision the plan doesn't cover.
- An overlap or merge conflict that can't be recovered cleanly.

On a hard stop, launch nothing new. Let running jobs that are independent
of the cause finish; cancel the others. Log the stop, then report.

## Report

At a hard stop or at the end, built from the log:

- what stopped the run, if anything, and what it needs from the maintainer;
- each unit's state, with its attempts and agents;
- every judgement call made along the way.
