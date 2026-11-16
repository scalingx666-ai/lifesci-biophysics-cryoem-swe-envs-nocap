# Issue

**Title:** add flaky to a few test_distance tests

## Description

A few tests were failing on one specific configuration (one python version, macos) and require manual restarting. I added `pytest-rerunfailures` as a pytest plugin, which would automatically rerun the tests 3 times before officially declaring a failure. 

https://github.com/jeremyleung521/mdtraj/actions/runs/13057906340/job/36433449570
https://github.com/mdtraj/mdtraj/actions/runs/12101259345/job/33740887047



Also had to restart `test_superpose_refinds` FOUR TIMES to get it working. Not sure what that was about. Leaving that be for now.
https://github.com/mdtraj/mdtraj/actions/runs/12966061236/job/36247585956

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de18b88c4722`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.