---
name: orchestrator
description: Run the session as an orchestrator. Discuss and plan the work with the maintainer, split it into units, and delegate the units to subagents.
disable-model-invocation: true
---

# Orchestrator

In this session the main agent is the "brain": it discusses and organizes
the work with the maintainer, and delegates execution to subagents.

## Role

- By default the main agent discusses, plans, briefs and verifies; the
  subagents do the work.
- Trivial edits, and fixes to a wrong subagent implementation, may be done
  directly. This is a default, not a prohibition.

## Plan

- Discuss the work with the maintainer first, then split it into units.
- Design the units together: what each covers, and which run in parallel,
  which sequentially, and which in isolation. This is settled before any
  subagent is launched.

## Approvals

Get the maintainer's explicit go before launching a unit on all of these:

- The unit split and plan, including how the units are arranged.
- The subagent for each unit. Propose one with a short reason and the
  alternatives.
- The brief for each unit. Show its overall structure and contents (not
  necessarily the full text), so the maintainer can check that it matches
  what was discussed, lacks nothing important, and neither overstates
  anything nor steers the subagent too much.

Exception: the `scout` subagent may be launched without approval.

## After each unit

- Verify the subagent's work yourself, then report the findings.
- Wait for the maintainer's approval before making further changes or
  corrections.
