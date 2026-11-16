# Issue

**Title:** Adds Nightly Memcheck job

## Description

* Some of the memcheck tests are getting expensive and taking more time than the other jobs. Moved them to their own separate jobs where it seemed like the memcheck was not urgent
* Moved the optimized mtm test to nightly since it is relatively slow and we don't use this code much in practice
* Fixes up some warnings from pytest with the changes I had made in  that would have eventually turned into a pytest failure
* Reduces current CI from an hour 30 to about an hour 10. Nightlies stay around 6 hours

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ddf5caabe29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.