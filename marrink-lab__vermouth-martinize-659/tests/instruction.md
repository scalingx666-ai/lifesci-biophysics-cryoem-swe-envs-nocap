# Issue

**Title:** change posres_fc format

## Description

as discussed in #639 

- write global #ifndef at top of itps
- change value of posres_fc to global
- add some tests for the posres processor
- add documentation page for things vermouth writes that are applied via gromacs preprocessor

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `41aabed70f62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.