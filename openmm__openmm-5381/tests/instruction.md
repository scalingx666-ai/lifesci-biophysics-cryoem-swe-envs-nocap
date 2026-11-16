# Issue

**Title:** Minor performance improvements to Modeller

## Description

The following changes are some minor performance improvements to reduce the time it takes to build systems, primarily looking at solvent systems. In some cases I was able to reduce the time to constructing a system by ~5 seconds (a large membrane system).

## Changes
* Use list comprehensions where appropriate (IE where the size of the list isn't unbounded and thus would have memory implications) in the Modeller. See [this](https://switowski.com/blog/for-loop-vs-list-comprehension/) for how list comprehensions end up being faster than generators.
* Exit early from _addIons if there aren't any ions to be added, avoiding a createSystem call that is redundant.
* Re-order a set of nested loops to avoid repeatedly looping through the same residue multiple times to get an oxygen index.

The changes were benchmarked using [Pyspy](https://github.com/benfred/py-spy) and this should produce identical systems as the current changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ff926904720`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.