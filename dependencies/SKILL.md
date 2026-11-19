---
name: dependencies
description: Language-agnostic rules on dependencies.
---

# Dependencies

Every dependency is a liability: code we don't control, can't easily fix,
and have to keep compatible. Unless the project states otherwise:

## Adding

- Before adding one, check the standard library and the project's existing
  dependencies.
- A project built on a specific stack (e.g. `trio`) uses that stack: its
  primitives over stdlib equivalents (`asyncio`), and never a competing
  library.
- No micro-dependencies: when only a small piece of functionality is needed,
  implement it, or bundle the relevant code.
- Don't reinvent substantial, well-established components either (an HTTP
  stack, a parser, a crypto primitive); depending on those is the right call.
- Never add a dependency without the maintainer's approval.

## Existing ones

- Don't update dependencies unless asked: what works today keeps working.
- Don't chase security advisories; automated tooling (e.g. Dependabot)
  already covers them.
