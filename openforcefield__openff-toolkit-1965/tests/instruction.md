# Issue

**Title:** Fix nightly and conda CI

## Description

- [x]  by forcing use of the reference platform
- [x] Skip parmed-requiring example in conda tests since we can't build an env with parmed 3, rdkit 2024, and OFFTK 0.16.6 (see )
- [x] Update nagl tests to use a molecule that raises the bad bond error with nagl 0.5
- [ ] Downpin conda tests to use nagl 0.4 until the next release
- [ ] Disable slow tests before merging

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8aa96bcc62b4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.