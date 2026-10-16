---
name: proper-edits
description: How to modify files. Always use the harness's edit tool, never bash-invoked scripts. Invoke explicitly with /skill:proper-edits.
disable-model-invocation: true
---

# Editing files

- Modify files with the harness's edit tool, never with scripts run through
  bash (`sed -i`, `str.replace`/`index` heredocs, AST rewriters). Edits made
  through the tool are visible as they land, and an exact-match edit fails
  loudly where a script fails silently. This applies to tests too.
- Create files with the write tool, not with bash (`cat > file <<EOF`).
- Exception: a rename that is genuinely mechanical across many files. Assert
  every anchor was found, then lint and test. In `codemode`, do this with
  `tools.edit` (exact-match, fails loudly) rather than a bash script.
