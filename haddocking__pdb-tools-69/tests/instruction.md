# Issue

**Title:** pdb_reres was ignoring MODEL lines

## Description

* `pdb_reres` was ignoring `MODEL` lines, and these were omitted in the output
* `MODEL` lines are now piped to the output.
* Solves issue #68

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ca7a064b3ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.