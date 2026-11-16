# Issue

**Title:** Additional fixes for the pre-YAML hooks to SAMS branch

## Description

* Fix `n_states` > `n_samplers`
* Bring ParallelTempering up to a working state again
* Auto-test ParallelTempering
* Add test for `n_states` > `n_samplers`
* Add test for `n_states` < `n_samplers`
* Change default state distribution among samplers to all conditions.
* Update setup.py for 0.21.0
* Merged the PDB fixer changes in master to this branch
* Added special condition travis branch to test PR's into `SAMS` branch (remove when done)

@jchodera if you want to give this a quick look over (changes in `repex.py` and `test_repex` only, the rest are all the PDBFIxer feature)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27dd169ec4a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.