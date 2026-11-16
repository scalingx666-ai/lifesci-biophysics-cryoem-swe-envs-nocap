# Issue

**Title:** Fix change_reaction_constant rejecting odd reaction_ids

## Description

`change_reaction_constant` had a contradictory guard:
`if (reaction_id % 2 == 1) throw "Only forward reactions can be selected"`.
This interpreted `reaction_id` as a raw index into the doubled
`m_reactions` container, but `get_reaction_index()` (and the public
docstring) treat it as a logical index and multiply by 2 internally.
Every odd logical `reaction_id` (the 2nd, 4th, … added reaction) was
wrongly rejected.

**Fix:** remove the `% 2 == 1` guard from
`src/script_interface/reaction_methods/ReactionAlgorithm.cpp`.
Range validation is performed by `get_reaction_index()`.

Also updated the existing `test_exceptions` in
`testsuite/python/reaction_methods_interface.py` to expect the correct
`IndexError` from `get_reaction_index()` for an out-of-range id.

**Regression test:** `test_change_constant_of_second_reaction` added to
`ReactionMethods` in `testsuite/python/reaction_methods_interface.py`.

Fixes bug #36 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f37298b11562`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.