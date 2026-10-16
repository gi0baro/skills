---
name: pytest
description: How to run pytest. Apply whenever running the test suite or any subset of it, including repeated runs to chase an intermittent failure.
---

# Running pytest

## Timeouts

- Never run pytest with a timeout above 60 seconds. A hang is diagnosed
  (narrow it down, `-v`, faulthandler, minimal repro), never waited out.

## Output

- Never filter pytest output in the pipe (`| tail`, `| grep`, `| head`,
  `> /dev/null`). Every run writes its full output to its own log file
  outside the repo (`pytest ... > {scratch}/pytest-{label}-{n}.log 2>&1;
  echo "exit=$?"`), then the file is read or grepped
  (`grep -E "^FAILED|^E "`).
- Loops of runs write one file per run, and logs are kept for the session.
- An intermittent failure is only useful if its name and traceback are
  captured the first time it happens; a summary line loses them.
