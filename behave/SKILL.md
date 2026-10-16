---
name: behave
description: Working agreements for how to collaborate with the maintainer on process, review and communication. Apply at the start of every session and before any implementation, commit, or report back.
disable-model-invocation: true
---

# Working agreements

How agents work with the maintainer. These are about process, review and
communication.

## Agree before implementing

- "Let's address X" or "let's look at X" means *discuss*, not implement.
  Propose the change as a concrete diff or code sketch with its consequences,
  then wait for an explicit go ("go ahead", "implement it").
- Larger work is split into units. Report the plan before executing a unit,
  and report after each one; the maintainer reviews before the next starts.
- Design before code: architecture questions are settled in discussion
  before any implementation, never by writing code and seeing.

## Stay inside the agreed boundary

- Fix the boundary (files, behaviours) before editing, and implement only
  that.
- The moment something outside the boundary breaks or needs deciding, stop.
  Report the finding and the options, and change nothing further until the
  maintainer rules. This includes a failing test whose cause is the agreed
  mechanism itself.
- Keep adjacent problems found along the way as separate items; don't fold
  them into the current change.
- When a unit exposes something the existing rulings don't cover, stop and
  ask rather than extending the design alone.

## git

- Never commit, stage or push unless explicitly asked; leave every change
  uncommitted. The maintainer commits on their own cadence.
- Don't report git state (uncommitted, staged, clean) in summaries.

## Reporting

- Verify before relaying. Check every finding (your own or a subagent's)
  against the code and against the intended behaviour before presenting it.
  Drop "working as designed" and out-of-scope items; label anything
  unverified explicitly and keep it out of the headline list.
- Lead with the verification state: what ran, what passed, what failed, with
  the output.
- Surface every judgement call (deviations, drops, gaps) for review rather
  than burying it.
- When a rule was broken during the work, say so.
