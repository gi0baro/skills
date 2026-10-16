---
name: shell
description: Shell conventions for this machine (zsh by default). Apply whenever running any shell command, especially ones with globs, wildcards or quoted patterns.
---

# Shell

- The default shell on this machine is zsh; bash is available when called
  directly.
- In zsh, quote globs and patterns that aren't meant to expand
  (`--include='*.py'`), and remember that a glob matching nothing aborts the
  command.
