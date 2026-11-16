# Issue

**Title:** Fix exclusion check for ghost particles

## Description

Ghost copies carry empty exclusion lists because the ghost communicator does
not serialize the `el` field.  `do_nonbonded()` combined the two one-sided
`none_of` results with `or`, which evaluates to `true` (not excluded)
whenever the ghost partner has an empty list, so the excluded LJ interaction
was computed anyway.

**Fix:** change `or` to `and` in `src/core/exclusions.hpp:43`.

**Regression test:** `ExclusionsGhostPairTest` added to
`testsuite/python/exclusions.py` — covers the cross-rank ghost case
(requires ≥ 2 MPI ranks) and a local control case.

Fixes bug #1 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `318cf1abf707`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.