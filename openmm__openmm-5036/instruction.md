# Issue

**Title:** Convert C++ types to Python types in API docs

## Description

.

The Python API is generated automatically from the C++ API.  An unfortunate consequence is that in the Python API docs, function signatures list the argument types and return types as the original C++ types, not the Python types they get translated into.  This adds code to translate them.

The result is still not perfect, but it's a big improvement.  Consider the argument to `context.setPositions()`.  In C++ its type is `const vector<Vec3>&`.  With this PR, it now gets translated to `Sequence[Vec3]` in the Python API docs.  That is indeed an accepted type for that argument, but it also accepts other types, such as a sequence of sequences, or a 2D array, or a `Quantity` that wraps any of those types and adds distance units to it.  So it still doesn't express the full range of accepted types, but it's a lot better than listing it as `vector<Vec3>` which isn't even a Python type.

The other big issue in the Python API docs is that all the code examples are still in C++.  Fixing that will be a separate project.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2af004b652dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.