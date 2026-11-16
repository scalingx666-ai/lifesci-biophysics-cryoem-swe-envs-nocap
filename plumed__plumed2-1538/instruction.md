# Issue

**Title:** Fix tree-based makeWhole reconstruction with MOLINFO WHOLE

## Description

Build the tree before using its indexes and pair each parent with the correct child. Add an analytical regression covering split coordinates, reordered subsets, and tree reuse without prior WHOLEMOLECULES reconstruction.

. The regression fails with the original implementation and with tree construction alone. The new regression, rt-emst, rt-ermsd, and rt-ermsd2 pass with both corrections.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a835edf5e55`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.