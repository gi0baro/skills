---
name: code-style
description: Language-agnostic code style rules for writing, changing and reviewing code (comments, tests, readability).
---

# Code style

Rules against the usual agent habits: over-commenting and low-value tests.
When in doubt, write less.

## Comments

Unless the project states otherwise, comments and docstrings are useful
only when they convey non-trivial things. This applies to what you write;
leave existing comments alone. Thus:

- Never describe what the code does, narrate steps, or repeat a
  signature or its types.
- No banners, dividers, or history/changelog comments ("added for X");
  history belongs in git.

## Tests

Unless the project states otherwise, we don't target coverage (it's just a
number and says nothing about code quality) or add a test case for every
change. Thus:

- Don't test the language, the standard library, or third-party
  dependencies; their behaviour is taken for granted.
- Don't test identities: parametrizing a condition just to check that an
  `if` takes each branch restates the code and verifies nothing.
- Don't write several near-identical tests; merge them into one.
- Never shape production code around tests (test-only parameters, flags,
  branches or hooks); use mocks, overrides or patching from the test.
- Avoid tests that depend on timing; they are a source of flakiness.
- Prefer unit tests; e2e tests are costly to maintain and prone to
  flakiness.

## Readability

- Prefer early returns and guard clauses over deep nesting.
- No magic numbers or strings; name them once.
