# Issue

**Title:** rstbx: open crystal_orientation reference file in binary mode

## Description

metric_subgroups.parse_reference reads the 'crystal_orientation' file written by labelit.store_crystal_orientation, which pickles the reference orientation and subgroup list in binary mode. Opening it with mode 'r' makes pickle.load raise UnicodeDecodeError under Python 3; the broad except then leaves reference_subgroups as None, so the reindexing branch in change_of_basis_op_to_best_cell has silently never run since the Python 3 conversion.

With binary mode, labelit_regression/reindex/test_reindex.py (disabled in 2021 as failing on Python 3) passes again: a re-indexed dataset's monoclinic/orthorhombic best-cell choice once more matches the stored reference dataset.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37c5bece7f3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.