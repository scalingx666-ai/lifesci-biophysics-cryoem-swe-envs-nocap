# Issue

**Title:** Add 'atffparams' as guaranteed attribute from PDB

## Description

Small fix: The 'atffparams' wasn't added as a guaranteed returned value of `load_one` in PDB, so I just added it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7dbf77313598`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.