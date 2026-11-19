---
name: python
description: Python-specific rules on top of code-style. Apply when writing, changing or reviewing Python code.
---

# Python

## Typing

Python is dynamic; we don't write it as if it were statically typed.
Unless the project states otherwise:

- Annotate only the public interface of a library (exported functions,
  classes, methods). Internals, private helpers, scripts, applications and
  tests get no annotations.
- Don't chase a type checker: no `cast`, `# type: ignore` or contortions to
  silence it.
- When a type checker is needed, use `ty`, not mypy.

## Idioms

- No defensive `getattr`/`hasattr` (or `try: ... except AttributeError`)
  for attributes that are part of the object's contract. Write against the
  invariant; if a stub or double lacks the attribute, fix the double.
- No useless async wrappers: `async def f(x) -> T: return await g(x)`
  becomes `def f(x) -> Awaitable[T]: return g(x)`. In a class, a method
  that only forwards to a coroutine function and ignores `self` becomes
  `name = staticmethod(func)`.

## Performance

The cost model is CPython's: no inlining, expensive bytecode dispatch, cheap
work in C. Prefer the form that keeps work in C and bytecode short, when it
reads at least as well. On cold paths, never trade readability for speed.

- Push loops into C: builtins (`sum`, `any`, `all`, `min`/`max` with `key`,
  `sorted`, `str.join`), set/dict operations, `itertools`. When a Python
  loop is needed, prefer a comprehension over `for` + `.append`.
- Calls are expensive. Don't extract single-use helpers; prefer a longer
  function body to a chain of tiny functions, within reason.
- Dispatch on a value with a dict lookup, not an `if`/`elif` chain. `match`
  compiles to sequential comparisons: use it for readability (destructuring,
  several shapes), not speed.
- Membership against constants: `x in {a, b, c}`, not chained `or ==`.
- In hot loops, bind repeated attribute and global lookups to locals.
- Build strings with `str.join`, not `+=` in a loop.
- `__slots__` on classes with many instances.
- `try` costs nothing when nothing is raised: when failure is rare, prefer
  EAFP to a check before every access.
