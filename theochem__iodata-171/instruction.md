# Issue

**Title:** Add load_one function for CHARMm crd files.

## Description

This PR is associated with #36  to support CHARmm crd files. Particularly, segment id, residue id and weight columns (used to store a mass or charge) are stored in extra dict. Perhaps for the latter, it is convenient to apply a conditional and store them in atmassess or atcharges as appropriate.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `774a6ede1407`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.